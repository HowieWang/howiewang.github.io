---
layout: post
title:  【Howie玩python】-FTPplus诞生记
category:
-   python

---

* content
{:toc}


## 从alex的代码学起

终于要做一个有点实用价值的小工具了。做个简单的ftp自己上传下载文件用。  

古人云，站在巨人的肩膀上，让他带你装逼带你飞。于是，我选择了alex的示例代码，先仔细的撸一撸，然后在此基础上，做个FTPplus。

ftp主要分为服务器端和客户端。从源码上可以看出，alex把这两部分分别做了一个包，然后各写了一个类去使用。下面，我就把里面关键部分做些注释学习一下。

### ftpServer

    class MyTCPHandler(SocketServer.BaseRequestHandler): # 继承BaseRequestHandler用于实现多线程
        """
        The RequestHandler class for our server.

        It is instantiated once per connection to the server, and must
        override the handle() method to implement communication to the
        client.
        """
        exit_flag = False
        def handle(self):  # 必须从写这个方法，这是入口函数，socketServer的基类初始化时候就会调用
            # self.request is the TCP socket connected to the client

            while not self.exit_flag: # 等于while Ture，exit_flag并没有什么卵用
                msg = self.request.recv(1024) # 开始接收客户端的请求，默认1024
                if not msg: # 如果为空，就直接跳出循环
                    break
                msg_parse = msg.split("|") # 采用管道符作为自定义命令的分隔符，所以这里要开始分解命令
                msg_type = msg_parse[0]    # 得到第一个命令，下面采用反射的方式调用类中方法，和谁名字一样就用谁！
                if hasattr(self,msg_type):
                    #print "---allowcate msg to method %s ---" % msg_type,msg_parse
                    func = getattr(self,msg_type)
                    func(msg_parse)
                else:
                    print "--\033[31;1mWrong msg type:%s\033[0m--" % msg_type # 都不一样，说明没有这个方法
        def ftp_authentication(self,msg):
            #print '----auth----'
            auth_res = False
            if len(msg) == 3: # split("|") -->list
                msg_type,username,passwd = msg # 得到具体参数
                if account.accounts.has_key(username): # 从字典中判断用户名，密码是否合法
                    if account.accounts[username]['passwd'] == passwd:
                        auth_res = True
                        self.login_user = username # 认证通过，保存用户名和相应路径
                        self.cur_path = '%s/%s' %(os.path.dirname(__file__),account.accounts[username]['home'])
                        self.home_path = '%s/%s' %(os.path.dirname(__file__),account.accounts[username]['home'])
                    else:
                        #print '---wrong passwd---'
                        auth_res = False
                else:
                    auth_res == False
            else:
                auth_res == False

            if auth_res:
                msg = "%s::success" % msg_type
                print '\033[32;1muser:%s has passed authentication!\033[0m' % username

            else:
                msg = "%s::failed" % msg_type
            self.request.send(msg) # 通知客户端验证结果
        def has_privilege(self,path): # 验证用户是否有目录权限，用于文件传输
            abs_path = os.path.abspath(path) # 判断绝对路径是不是相应用户名下面的路径
            if abs_path.startswith(self.home_path):
                return True
            else:
                return  False
        def file_transfer(self,msg):
            #print msg
            transfer_type = msg[1] # 判断第二个参数是put或get
            filename = '%s/%s' %(self.cur_path,msg[2]) # 拼出来当前文件路径
            self.has_privilege(filename) # 判断权限， 这句多余
            if transfer_type == 'get': #download from server
                if os.path.isfile(filename) and self.has_privilege(filename): # 这里判断文件是否存在，和权限

                    file_size = os.path.getsize(filename) # 得到文件总大小
                    confirm_msg = "file_transfer::get_file::send_ready::%s" % file_size # 通知客户端准备接受多大的文件
                    self.request.send(confirm_msg)
                    client_confirm_msg = self.request.recv(1024) # 得到客户端的确认通知
                    if client_confirm_msg == "file_transfer::get_file::recv_ready": # 准备好啦！开始吧！
                        f = file(filename,'rb') # 读的方式打开文件，开始发送
                        size_left = file_size # 用剩余多少来做标志
                        #print "--size left:",size_left
                        while size_left >0: # 只要有剩下的就继续发
                            if size_left < 1024: #不到1024，就有多少发多少
                                self.request.send(f.read(size_left))
                                size_left = 0
                            else:
                                self.request.send(f.read(1024)) # 每次发1024，剩下的大小也就少了1024
                                size_left -= 1024

                        else:
                            print "send file done...."
                else:#file doesn't exist
                    err_msg = "file_transfer::get_file::error::file does not exist or is a directory"
                    self.request.send(err_msg) #有错误当然也要通知客户端
            elif transfer_type == 'put':#upload file to server 上传文件是put
                #print '-->', msg
                filename,file_size = msg[-2],int(msg[-1]) # 名字字符串最后两位是名字和大小
                filename = '%s/%s' %(self.cur_path,filename) # 拼出来工作路径
                print 'filename:',filename
                if os.path.isfile(filename): # 如果文件已经存在，就多个.0来保存
                    f = file('%s.0' % (filename),'wb')
                else:
                    f = file("%s" %(filename) ,'wb') # 不存在就开始写吧
                confirm_msg = "file_transfer::put_file::recv_ready"
                self.request.send(confirm_msg) # 告诉客户端，我们可以开始接受文件了
                recv_size = 0
                while not recv_size == file_size: # 只要没接受完，就继续
                    data = self.request.recv(1024) # 每次收1024
                    recv_size += len(data) # 大小就增加收到的文件大小
                    f.write(data) # 收到就写
                else:
                    print "--\033[32;1mReceiving file:%s done\033[0m--" %(filename) # 接受完成
                    #print len(file_content), file_content[-100:]
                    f.close()
        def delete_file(self,msg): # 删除文件，命令写死了，不好玩
        def list_file(self,msg): # 文件列表，我要自己来判断，直接写命令，判断是否是合法路径就ok

### ftpClient

    class Client(object):

        def __init__(self,host,port): # 初始化时候就连接服务器
            self.sock = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
            self.sock.connect((host,port))
            self.exit_flag = False
            if self.auth(): # 验证通过，就可以交互了
                self.interactive()

                def interactive(self):
            '''allowcate task to different function according to msg type'''
            try:
                while not self.exit_flag: # 就是True， 然后在用户名和路径下，输入命令，从定义好的字典里面去取命令
                    cmd = raw_input("[\033[;32;1m%s:\033[0m%s]>>:" % (self.username,self.cur_path) ).strip()
                    if len(cmd) == 0:continue
                    cmd_parse = cmd.split()
                    msg_type = cmd_parse[0]
                    #print 'msg_type::',msg_type
                    if self.func_dic.has_key(msg_type):
                        func = getattr(self,self.func_dic[msg_type]) # 存在命令的话，就用反射去调用相应方法
                        func(cmd_parse)
                    else:
                        print "Invalid instruction,type [help] to see available cmd list"
            except KeyboardInterrupt: # 过滤键盘输入错误
                self.exit('exit')
            except EOFError:
                self.exit('exit')
        def put_file(self,msg):
            #print '---uploading file----',msg
            if len(msg) == 2:# put local_filename 上传文件
                if os.path.isfile(msg[1]): # 第二个参数是文件名，判断存在才继续

                    file_size = os.path.getsize(msg[1]) # 先计算大小，然后告诉服务器准备收多大的文件
                    instruction_msg = "file_transfer|put|send_ready|%s|%s" %(msg[1],file_size)
                    self.sock.send(instruction_msg) # 发送准备接收的命令和文件大小
                    feedback = self.sock.recv(1024) # 收到回复，服务器准备好了，就开始发送
                    print '==>',feedback
                    progress_percent = 0
                    if feedback.startswith("file_transfer::put_file::recv_ready"): # 开始上传
                        f = file(msg[1], "rb") # 读的方式打开文件
                        sent_size = 0
                        while not sent_size == file_size: # 发送的文件大小和总大小不相等，就继续
                            if file_size - sent_size <= 1024: # 剩下的小于1024，就剩下多少发多少
                                data = f.read(file_size - sent_size)
                                sent_size += file_size - sent_size # 已经发送的部分，就加上这些大小
                            else:
                                data = f.read(1024) # 够1024的就一次次的发
                                sent_size += 1024
                            self.sock.send(data) # 发送到服务器
                            #print '-->',file_size,sent_size

                            cur_percent = int(float(sent_size) / file_size * 100) # 进度条，已经发送/总大小，放大100作为整数去显示
                            if cur_percent > progress_percent: # 当前大小比已经显示的大小大，就继续
                                progress_percent = cur_percent
                                self.show_progress(file_size,sent_size,progress_percent) # 调用进度条方法
                        else:
                            print "--send file:%s done---" % msg[1]
                        f.close()

                else:
                    print "\033[31;1mFile %s doesn't exist on local disk\033[0m" % msg[1]
        def get_file(self,msg): # 类似服务器端
            pass


## 开始优化

纵观代码，我想优化下面几个地方：

- 命令模式， 客户端启动时候就显示命令帮助，然后通过命令行+文件名的方式进行上传下载。  
- 服务器端执行命令，可以随意一些，但是都限定到固定用户目录下  ，显示列表，增删文件等。
- 上传文件重复，可以选择是否覆盖，下载就无所谓了，因为我就想下载服务器最新的
- 添加md5文件验证，判断文件传递前后的一致性  
- 服务器端添加日志，客户端可以读取日志

### 命令模式  

由于服务器端ip地址不确定，这里假装我这个ftp是跨平台无所不能啊！要不然怎么能叫FTPplus?! 所以我决定把客户端的启动方式写成`python file.py ip port`的形式。  如果不写ip和port就弹出帮助文档，告诉用户怎么使用。

输入命令当然是用sys自带的argv了。简要代码如下：  这里通过try的异常处理，来判断是否采用正确的姿势启动客户端。

    def usage():
        print("Usage:")
        print("python "+__file__+" <host> <port>")
        sys.exit()


    if __name__ == '__main__':
        try:
            host = sys.argv[1]
            port = int(sys.argv[2])
            print "connect to server--%s:%s" %(host, port)

        except:
            usage()


### 服务器端    

为了实现多线程，继承SocketServer和重写handle方法是必须要办的。  

    class FTPplus(SocketServer.BaseRequestHandler): # 继承BaseRequestHandler用于实现多线程
        """
        The RequestHandler class for our server.

        It is instantiated once per connection to the server, and must
        override the handle() method to implement communication to the
        client.
        """
        def handle(self):  # 必须从写这个方法，这是入口函数，socketServer的基类初始化时候就会调用
            pass


服务器端作为老大，我觉得可以加个新建用户的功能。就用json写个db吧。保存用户名和密码，让客户端验证使用。  
这里我就简单建立一个用户好了。根据输入，然后写成db.json。这样启动服务器时候可以顺手建立一个，并生成相应的用户文件夹，客户端连接时候也就知道去用谁登陆了。

    # make user for ftp server
    def make_user():
        user_dict = {}
        info_dict = {}
        user = raw_input(">user:")
        cmd = "mkdir %s" %user
        subprocess.Popen(cmd, shell=True)
        pwd = raw_input(">pwd:")
        info_dict["pwd"] = pwd
        user_dict[user] = info_dict
        f = file('db.json', 'w')
        json.dump(user_dict, f, sort_keys=True, indent=4, separators=(',', ': '))
        f.close()

服务器端发送文件文件比较简单，就分两部分好了，一部分按照1024去发，剩下的不足1024单独发一下：  

    f = file(file_full_name,'rb') # 读的方式打开文件，开始发送
                    size_left = file_size # 用剩余多少来做标志
                    #print "--size left:",size_left
                    while size_left >0: # 只要有剩下的就继续发
                        if size_left < 1024: #不到1024，就有多少发多少
                            self.request.send(f.read(size_left))
                            size_left = 0
                        else:
                            self.request.send(f.read(1024)) # 每次发1024，剩下的大小也就少了1024
                            size_left -= 1024

接收文件时候, 有个关键地方是，每次收1024，还是有多少收多少？


        while not recv_size == file_size: # 只要没接受完，就继续
            data = self.request.recv(1024) # 每次收1024 或者 recv(file_size-size_recv) ？
            recv_size += len(data) # 大小就增加收到的文件大小
            f.write(data) # 收到就写

### 文件覆盖  

如果服务器端文件已经存在，上传时候就需要考虑是否覆盖。需要服务器和客户端多一次消息的传递。  客户端可以添加类似代码 ：

                if feedback.startswith("push-file-exist"):
                    still_push = raw_input("file exists: <yes> to rewrite or <no> to cancel:")
                    if still_push == "yes":
                        self.sock.send("push-yes")
                        feedback = self.sock.recv(1024) # 收到回复，服务器准备好了，就开始发送
                    else:
                        exit_msg =  "file exists, you said cancel...stop push file"
                        self.exit(exit_msg)
                        return

服务器端对应添加：  

        if os.path.isfile(filename): # 如果文件已经存在，是否覆盖？
            msg_exist = 'push-file-exist'
            self.request.send(msg_exist)
            client_msg = self.request.recv(1024)
            if client_msg.split("-")[-1] == "yes":
                f = file("%s" %(filename) ,'wb') # 覆盖就开始写吧
                print "file exists, but rewrite it"
            else:
                print "file exists, return"
                return

### md5的文件验证  

添加如下方法，然后分别在服务器端和客户端验证就好了。这里需要`import hashlib`。这里调试时候发现有意思的：上传的文件md5一致，下载的就不一样了，正在检查一下为什么。

    def check_md5(self, filename, md5):
        new_md5 = self.hash_file(filename)
        print "old file md5= %s" %md5
        print "new file md5= %s" %new_md5
        if md5 == new_md5:
            print "file md5 is the same!"
            return True
        else:
            print "md5 is not the same, error in transfer"
            return False

    def hash_file(self, filename):
        hasher = hashlib.md5()
        with open(filename, 'rb') as afile:
            buf = afile.read()
            hasher.update(buf)
        return hasher.hexdigest()

### 添加日志  

日志是应用管理中的利器啊！我们当然要添加一个，就放在服务器端，客户端根据需要可以查看。  



## 怎么变得更好玩


## 参考索引  

- [alex的ftp示例]()  
- [wusir的超级引导]()  

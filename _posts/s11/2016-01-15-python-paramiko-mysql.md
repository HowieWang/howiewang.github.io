---
layout: post
title:  【Howie玩python】-堡垒机的雏形-堡垒蛋
category: 
- python
- mysql

---

* content
{:toc}


> 我觉得我注定是可以写出几个牛逼的项目的！
认真的练习，认真的练习，认真的练习多了，也就成了实战。

国内的运维开发行业越来越火，开源项目也很多。有俩堡垒机不知你听说过没？一个是Crazyeye，一个是Jumpserver。还都挺好玩的。

我们是要拿来直接用呢？还是改吧改吧再用呢？还是自己写一个呢？

为了更多个性化需求，后两项是必须的。但是，从头写个堡垒机其实还挺麻烦的，要不，简单一点，今天写个堡垒蛋吧，早晚会长成堡垒机（鸡）的。

## 堡垒机执行流程

- 管理员为用户在服务器上创建账号（将公钥放置服务器，或者使用用户名密码）    
- 用户登陆堡垒机，输入堡垒机用户名密码，现实当前用户管理的服务器列表    
- 用户选择服务器，并自动登陆    
- 执行操作并同时将用户操作记录

*注：配置.brashrc实现ssh登陆后自动执行脚本，如：/usr/bin/python /home/wupeiqi/menu.py*

流程示意图如下：  

![](http://images2015.cnblogs.com/blog/425762/201601/425762-20160103100859651-138631517.png)

## 堡垒蛋执行流程

作为一只早晚会变成堡垒机的堡垒蛋，我们的执行流程，其实，和上面，是一样的。TT

## 相关技术

### paramiko作为钥匙和通行证

既然要远程登陆并管理服务器，当然首选`paramiko`了。安装路径下面的`demo.py, interactive.py`可都是超级大钥匙和通行证啊，让我们在远程连接服务器的道路上畅行无阻。

远程连接与执行命令我们可以类似这样操作：

    tran = paramiko.Transport((hostname, port,))
    tran.start_client()
    default_path = os.path.join(os.environ['HOME'], '.ssh', 'id_rsa')
    key = paramiko.RSAKey.from_private_key_file(default_path)
    tran.auth_publickey('wupeiqi', key)
     
    # 打开一个通道
    chan = tran.open_session()
    # 获取一个终端
    chan.get_pty()
    # 激活器
    chan.invoke_shell()
     
    #########
    # 利用sys.stdin,肆意妄为执行操作
    # 用户在终端输入内容，并将内容发送至远程服务器
    # 远程服务器执行命令，并将结果返回
    # 用户终端显示内容
    #########
     # 获取原tty属性
    oldtty = termios.tcgetattr(sys.stdin)
    try:
        # 为tty设置新属性
        # 默认当前tty设备属性：
        #   输入一行回车，执行
        #   CTRL+C 进程退出，遇到特殊字符，特殊处理。

        # 这是为原始模式，不认识所有特殊符号
        # 放置特殊字符应用在当前终端，如此设置，将所有的用户输入均发送到远程服务器
        tty.setraw(sys.stdin.fileno())
        chan.settimeout(0.0)

        while True:
            # 监视 用户输入 和 远程服务器返回数据（socket）
            # 阻塞，直到句柄可读
            r, w, e = select.select([chan, sys.stdin], [], [], 1)
            if chan in r:
                try:
                    x = chan.recv(1024)
                    if len(x) == 0:
                        print '\r\n*** EOF\r\n',
                        break
                    sys.stdout.write(x)
                    sys.stdout.flush()
                except socket.timeout:
                    pass
            if sys.stdin in r:
                x = sys.stdin.read(1)
                if len(x) == 0:
                    break
                chan.send(x)

    finally:
        # 重新设置终端属性
        termios.tcsetattr(sys.stdin, termios.TCSADRAIN, oldtty)

    chan.close()
    tran.close()

本来自己打算写个类去执行各种远程操作，但是想起来以前在github撸到一个不错的封装，所以就打算直接来用来。这个名字起的也不错：anyhost


### mysql作为数据中心

mysql在it公司里已经使用相当广泛了。结合python使用，效果也非常的强大。

首先我们简单说一下安装。这里以ubuntu14为例：

ubuntu下mysql-python模块的安装

> 安装步骤：

1、sudo apt-get install python-setuptools

2、sudo apt-get install libmysqld-dev

3、sudo apt-get install libmysqlclient-dev

4、sudo apt-get install python-dev

5、sudo easy_install mysql-python

> 测试下：

在python交互式窗口，import MySQLdb 试试，不报错的话，就证明安装好了。

> 使用举例

**SQL基本使用**

1、数据库操作

    show databases;
    use [databasename];
    create database  [name];

2、数据表操作

    show tables;
     
    create table students
        (
            id int  not null auto_increment primary key,
            name char(8) not null,
            sex char(4) not null,
            age tinyint unsigned not null,
            tel char(13) null default "-"
        );

3、数据操作

    insert into students(name,sex,age,tel) values('alex','man',18,'151515151')
     
    delete from students where id =2;
     
    update students set name = 'sb' where id =1;
     
    select * from students

**Python MySQL API**

插入数据

    import MySQLdb
      
    conn = MySQLdb.connect(host='127.0.0.1',user='root',passwd='1234',db='mydb')
      
    cur = conn.cursor()
      
    reCount = cur.execute('insert into UserInfo(Name,Address) values(%s,%s)',('alex','usa'))
    # reCount = cur.execute('insert into UserInfo(Name,Address) values(%(id)s, %(name)s)',{'id':12345,'name':'wupeiqi'})
      
    conn.commit()
      
    cur.close()
    conn.close()
      
    print reCount

我们还可以插入列表：

    li =[
         ('alex','usa'),
         ('sb','usa'),
    ]
    reCount = cur.executemany('insert into UserInfo(Name,Address) values(%s,%s)',li)

删除数据和修改数据，也都是修改execute中的命令字符串就好！

    reCount = cur.execute('delete from UserInfo')
    reCount = cur.execute('update UserInfo set Name = %s',('alin',))

查数据主要有3个方法：  

-  fetchone    
- fetchmany(num)    
- fetchall

使用举例：

    reCount = cur.execute('select * from UserInfo')
    print cur.fetchone()
    # nRet = cur.fetchall()


## 开始造蛋

### 主要流程

### mysql造几个表


    def create_user_table(self, tb_name='tb_user', user_list=['root',]):
        #如果表已经存在，则删除·
        self.cursor.execute("""DROP TABLE IF EXISTS %s""" % (tb_name,))
        #创建表
        self.cursor.execute("""CREATE TABLE IF NOT EXISTS %s(
                            id int(11) not null auto_increment,
                            name char(20) not null,
                            PRIMARY KEY(id)
                            )ENGINE=MyISAM;
            """ % (tb_name,))
        self.tb_user = tb_name
        #插入数据
        reCount = self.cursor.executemany('insert into tb_user (Name) values(%s)', user_list)
        self.conn.commit()

        return reCount


    def create_hosts_table(self, tb_name='tb_hosts', host_list=[('127.0.0.1','DBA',1),]):
        #如果表已经存在，则删除·
        self.cursor.execute("""DROP TABLE IF EXISTS %s""" % (tb_name,))
        #创建表
        self.cursor.execute("""CREATE TABLE IF NOT EXISTS %s(
                            id int(11) not null auto_increment,
                            host_ip char(20) not null,
                            host_group char(20) not null,
                            user_id int(11) not null,
                            PRIMARY KEY(id)
                            )ENGINE=MyISAM;
            """ % (tb_name,))

        self.tb_hosts = tb_name

        #插入数据
        reCount = self.cursor.executemany('insert into tb_hosts(host_ip,host_group, user_id) values(%s, %s, %s)', host_list)
        self.conn.commit()

        return reCount

-----------

    mysql> show tables
        -> ;
    +----------------+
    | Tables_in_test |
    +----------------+
    | tb_mytest      |
    | tb_user        |
    +----------------+
    2 rows in set (0.00 sec)

    mysql> select * from tb_user
        -> ;
    +----+-------+
    | id | name  |
    +----+-------+
    |  1 | howie |
    |  2 | alex  |
    +----+-------+
    2 rows in set (0.00 sec)
    --------------------
    使用字典输出：

    {'id': 1L, 'name': u'howie'}
    {'id': 2L, 'name': u'alex'}
    {'host_group': u'DBA', 'user_id': 1L, 'id': 1L, 'host_ip': u'192.168.1.106'}
    {'host_group': u'DEV', 'user_id': 2L, 'id': 2L, 'host_ip': u'127.0.0.1'}



### 使用paramiko

    def connect(self,hostname,username,password,port=22):
            self.hostname = hostname
            self.username = username
            self.password = password
     
            self.ssh = paramiko.SSHClient()
            #Don't use host key auto add policy for production servers
            self.ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
            self.ssh.load_system_host_keys()
            try:
                self.ssh.connect(hostname,port,username,password)
                self.transport=self.ssh.get_transport()
            except (socket.error,paramiko.AuthenticationException) as message:
                print "ERROR: SSH connection to "+self.hostname+" failed: " +str(message)
                sys.exit(1)
            return  self.transport is not None

    def run_cmd(self, cmd):
            port=22
            username='root'
            file=open('ip.list')
            for line in file :
                print line
                msg = line.split('|')
                hostname=str(msg[0]).strip()
                username =str(msg[1]).strip()
                passwd=str(msg[2]).strip()
                print "#"*20, hostname, 20*"#"
                print hostname, username, passwd
                self.connect(hostname, username, passwd)
                stdin,stdout,stderr=self.ssh.exec_command(cmd)
                print stdout.read()
                print stderr.read()

            file.close()

        def download_file(self, remote_path, local_path, host) :
           sftp = self.ssh.open_sftp()
           remote_dir = os.path.dirname(remote_path)
           try:
               sftp.chdir(remote_dir)  # Test if remote_path exists
           except IOError:
               print >> sys.stderr, "remote dir not exists"
               return 
           local_path = local_path + "." + host
           sftp.get(remote_path, local_path)
           sftp.close()

           
----
## 参考

-[ubuntu安装mysql-python](http://www.cnblogs.com/coser/archive/2012/01/11/2319125.html)    
-[堡垒机与数据库](http://www.cnblogs.com/wupeiqi/articles/5095821.html)    

---
写代码时候找背景音乐，发现这个podcast不错
可以一边学python一边学英语了。哈哈！

强烈推荐：

<div class="talk-python-embedded-player"
     data-episode-id="39"
     data-show-title="Getting your first dev job as a Python developer (part 1)"
     data-guest="Panelists"></div>
<script src="https://talkpython.fm/static/js/embed/player.js" type="text/javascript"></script>

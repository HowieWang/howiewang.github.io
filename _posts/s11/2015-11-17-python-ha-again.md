---
layout: post
title:  【Howie玩python】-再战ha配置文件继续找对象
category: 
- python  
---

* content
{:toc}

day4
===

## 再来一次

这周修改ha配置文件的小练习WU.SIR不是很满意，特意又花了三小时带我们做了一遍。主要强调了要用标志位，然后博客里还给出了完整代码，这周再写这个作业就是让大家自己做一遍，再优化一下，同时可以用以后的知识show一下。

我擦，我show啥？

- 标志位，写了  
- 类，用了  
- 面向对象，找到了  
- 函数，必须的  
- 附加题，完成了  

真的啊，不蒙你，不信去看day3的源码，虽然细节处理可能还有问题，但是真的发挥了我上周六最好状态。
那么，问题来了？这次怎么搞？
坐在地铁里，我开始莫名的思考，WU.SIR用了更简洁的方法去单独操作backend记录的list，那么干脆我就给他封个类好了。
好的，开始吧！说干就干！

## 分割小函数
读写部分在添加和删除里，都有一大段代码，而且几乎一样，咱还是把他们直接拉出来溜溜吧。
于是有了下面这几个函数。


## backend来个类
这个类很简单，就两成员变量，一个是title的字符串，一个是记录的列表。好像分别来个get和set就好了。找加删三个功能好像也可以做到里面？试试吧！

然后看看配置文件，可能有多个backend啊，咋办？

直接再来个list保存个backend的类对象，咋样？让我们试试看！


首先是Backend的类：

    class Backend(object):
        """Backend title and records"""
        def __init__(self, title):
            self.title = title
            self.record_list = []

然后是对backend进行操作的类：

    class ConfigBackend(object):
        """modify the haproxy config file,
            only the backend node.
        """
        def __init__(self, config_path):
            self.config_path = config_path
            self.config = self.read_config()
            self.backup_config()
            self.backend_list = self.read_backend_list()
            ...

这里最重要的就是把所有backend找出来的方法：  

     def read_backend_list(self):
            backend_list = []
            title = ''
            record_list = []
            flag = False
            with open(self.config_path + '.bk') as read_obj, open (self.config_path, 'w') as write_obj:
                for line in read_obj:
                    if len(line.strip().split()) > 1: # get title line
                        if line.strip() and line.strip().split()[0] == 'backend':
                            if title: # get another backend title
                                backend = Backend(title)
                                backend.record_list = record_list
                                backend_list.append(backend)
                                record_list = []
                            flag = True
                            title = line.strip().split()[1]
                            continue
                        if flag and line.strip():
                            record_list.append(line.strip())
                    if not flag: # only write lines before backend
                        write_obj.write(line)
            backend = Backend(title)
            backend.record_list = record_list
            backend_list.append(backend) # add the last one
            return backend_list

从上面代码可以看到我同时打开两个文件，一个是备份的原文件，一个是重新写一个原文件，重新写出来的原文件其实是不包含backend部分的。因为backend所有内容都要根据后面的添加和修改进行更新，最后再追加到原文件中。

添加函数主要代码如下：  

    if fetch_list: # exist title
                if current_record in fetch_list:
                    pass
                else:
                    fetch_list.append(current_record) # add to list
                    for item in self.backend_list:
                        if item.title == backend_title:
                            item.record_list = fetch_list
            else:
                backend = Backend(backend_title)
                backend.record_list.append(current_record)
                self.backend_list.append(backend)
            self.write_backend_list()

删除函数过程类似，只不过需要把相同title的项删除掉。

再添加和删除前，有一点需要注意，就是需要更新一下当前的`backend_list`。

最后是追加文件，这就比较简单了。

        def write_backend_list(self):
            with open(self.config_path, 'a') as f:
                for backend in self.backend_list:
                    if backend.record_list: # only write when record exists
                        f.write('backend %s\n' % backend.title)
                    for record in backend.record_list:
                        f.write('%s %s \n' %(' '*8, record))
            print 'success!'


完整源文件请查看[github](https://github.com/HowieWang/pythonStudy/tree/master/s11/day4)
>>>>>>> 053c28c60af20d92179c57739a387b765237ef62

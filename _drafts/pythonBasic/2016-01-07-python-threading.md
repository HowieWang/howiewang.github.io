---
layout: post
title:  【De8ug玩python】-多线程从0到1到澡堂子洗澡
category:
-   python

---

* content
{:toc}

> 我觉得我注定是可以写出几个牛逼的项目的！
认真的练习，认真的练习，认真的练习多了，也就成了实战。

今天我们来好好研究一下python里面的多线程。

## 从标准库学起  

### 线程与锁  

从根源来学，当然要从threading学起。

启动一个线程就是把一个函数传入并创建Thread实例，然后调用start()开始执行：

    import time, threading

    # 新线程执行的代码:
    def loop():
        print 'thread %s is running...' % threading.current_thread().name
        n = 0
        while n < 5:
            n = n + 1
            print 'thread %s >>> %s' % (threading.current_thread().name, n)
            time.sleep(1)
        print 'thread %s ended.' % threading.current_thread().name

    print 'thread %s is running...' % threading.current_thread().name
    t = threading.Thread(target=loop, name='LoopThread')
    t.start()
    t.join()
    print 'thread %s ended.' % threading.current_thread().name

执行结果如下：

    thread MainThread is running...
    thread LoopThread is running...
    thread LoopThread >>> 1
    thread LoopThread >>> 2
    thread LoopThread >>> 3
    thread LoopThread >>> 4
    thread LoopThread >>> 5
    thread LoopThread ended.
    thread MainThread ended.

由于任何进程默认就会启动一个线程，我们把该线程称为主线程，主线程又可以启动新的线程，Python的threading模块有个current_thread()函数，它永远返回当前线程的实例。主线程实例的名字叫MainThread，子线程的名字在创建时指定，我们用LoopThread命名子线程。名字仅仅在打印时用来显示，完全没有其他意义，如果不起名字Python就自动给线程命名为Thread-1，Thread-2……

多线程中，所有变量都由所有线程共享，所以，任何一个变量都可以被任何一个线程修改，因此，线程之间共享数据最大的危险在于多个线程同时改一个变量，把内容给改乱了。

由于计算机技术飞速发展，硬盘读写速度加快，内存增加，cpu的核数也越来越多。其中cpu的运行速度飞快，相对而言，IO的速度就很慢，为了充分利用IO过程中的时间，所以需要采用多任务的方式处理任务。也就出现了多线程和多进程。

### 多线程  

多线程类似一个教室里，所有学生同时在学习一门课程（相当于一个线程），如果有一个学生不及格，整个班级的及格率就有影响。
或许还可以比喻成在澡堂子洗澡。澡堂子本身是一个进程，里面每个喷头就是个线程。想去洗澡的必须在门口领牌子（锁），才能进去洗澡。里面有多少个喷头也就有多少个线程，每个喷头都有人用了，也就不能再继续进人洗澡了，当然，这里咱们不考虑有人非要和别人挤一个喷头的情况，等电脑硬件支持了咱们在讨论也不迟。澡堂子里一开始设置好多少个喷头是固定的，也就是一个进程里面有多少个线程是固定的。来的人多了，就只好在外面等着。等到有人出来了，再进去一个。后面的实例里，我们会讨论这个东东，也就是所谓的线程池。
写个伪代码也许可以这样：   

    澡堂子里安装了8个喷头，营业开始：（线程池启动）  
    张1，张2，张3，张4来了，每个人领个牌子进去洗澡；  
    李1，李2，李3，李4来了，每个人领个牌子进去洗澡；    
    王五来了，被管理员关在外面，喷头用完了，等着  
    （线程用完了，等着）  
    张3出来了，空闲了一个喷头  
    王五领个牌子，进去洗澡。。。


因为Python的线程虽然是真正的线程，但解释器执行代码时，有一个GIL锁：Global Interpreter Lock，任何Python线程执行前，必须先获得GIL锁，然后，每执行100条字节码，解释器就自动释放GIL锁，让别的线程有机会执行。你可以想象成别人洗澡出来了，你领个牌子进去。这个GIL全局锁实际上把所有线程的执行代码都给上了锁，所以，多线程在Python中只能交替执行，即使100个线程跑在100核CPU上，也只能用到1个核。

由于GIL的存在，CPython不能有效的利用多核处理器。表现为任意时间一个进程只有一个线程在跑，而IO密集型运算，多数是在IO读写将线程堵塞掉了，这个时候线程切换是很合理的，反正线程只是单纯地等待，在这个等待的时候去做其他的事情，资源利用率就上去了。  

简单点解释就是，python的多线程只能使用一个cpu核心。io密集型应用，本来cpu占用率就很低，单个cpu核心就够了。用多进程给它多几个核心也不能提升性能。

**IO密集型:**涉及到网络、磁盘IO的任务都是IO密集型任务，这类任务的特点是CPU消耗很少，任务的大部分时间都在等待IO操作完成（因为IO的速度远远低于CPU和内存的速度）。对于IO密集型任务，任务越多，CPU效率越高，但也有一个限度。常见的大部分任务都是IO密集型任务，比如Web应用。

IO密集型任务执行期间，99%的时间都花在IO上，花在CPU上的时间很少，因此，用运行速度极快的C语言替换用Python这样运行速度极低的脚本语言，完全无法提升运行效率。对于IO密集型任务，最合适的语言就是开发效率最高（代码量最少）的语言，脚本语言是首选，C语言最差。

### 多进程

多进程类似同时有几个班级的学生在学习，某个班级学生不及格，但是不会影响到其他班级，因为各个进程都是独立的。

与多线程相比，多进程比较稳定。但是也有缺点，就是创建时候需要消耗很多资源，在Unix/Linux系统下，使用用fork调用。

多进程可以有效的利用cpu多核的特点，处理计算密集型任务。  

**计算密集型任务**的特点是要进行大量的计算，消耗CPU资源，比如计算圆周率、对视频进行高清解码等等，全靠CPU的运算能力。这种计算密集型任务虽然也可以用多任务完成，但是任务越多，花在任务切换的时间就越多，CPU执行任务的效率就越低，所以，要最高效地利用CPU，计算密集型任务同时进行的数量应当等于CPU的核心数。

python已经有很成熟的多进程处理模块，使用示例如下，  

    from multiprocessing import Process
    import threading
    import time
      
    def foo(i):
        print 'say hi， process-',i
      
    for i in range(10):
        p = Process(target=foo,args=(i,))
        p.start()

和使用线程的方法类似，更多内容可以查看官方帮助。下面我们主要讨论一下python中多线程的实现。   
让我们从前人的研究成果上学起。

## 研究优秀博客  

### 多线程与同步  

vamei用一个买票的例子讨论了多线程与同步。  
在当今网络时代，每个服务器都会接收到大量的请求。服务器可以利用多线程的方式来处理这些请求，以提高对网络端口的读写效率。  

我们使用mutex (也就是Python中的Lock类对象) 来实现线程的同步:

    # A program to simulate selling tickets in multi-thread way
    # Written by Vamei
     
    import threading
    import time
    import os
     
    # This function could be any function to do other chores.
    def doChore():
        time.sleep(0.5)
     
    # Function for each thread
    def booth(tid):
        global i
        global lock
        while True:
            lock.acquire()                # Lock; or wait if other thread is holding the lock
            if i != 0:
                i = i - 1                 # Sell tickets
                print(tid,':now left:',i) # Tickets left
                doChore()                 # Other critical operations
            else:
                print("Thread_id",tid," No more tickets")
                os._exit(0)              # Exit the whole process immediately
            lock.release()               # Unblock
            doChore()                    # Non-critical operations
     
    # Start of the main function
    i    = 100                           # Available ticket number 
    lock = threading.Lock()              # Lock (i.e., mutex)
     
    # Start 10 threads
    for k in range(10):
        new_thread = threading.Thread(target=booth,args=(k,))   # Set up thread; target: the callable (function) to be run, args: the argument for the callable 
        new_thread.start()                                      # run the thread

我们使用了两个全局变量，一个是i，用以储存剩余票数；一个是lock对象，用于同步线程对i的修改。此外，在最后的for循环中，我们总共设置了10个线程。每个线程都执行booth()函数。线程在调用start()方法的时候正式启动 (实际上，计算机中最多会有11个线程，因为主程序本身也会占用一个线程)。Python使用threading.Thread对象来代表线程，用threading.Lock对象来代表一个互斥锁 (mutex)。

### 线程池1号  -threadpool

这个是几年前别人实现好的一个模块。用法很简单。  

    pool = threadpool.ThreadPool(10)  #建立线程池，控制线程数量为10
    reqs = threadpool.makeRequests(get_title, data, print_result)  #构建请求，get_title为要运行的函数，data为要多线程执行函数的参数，最后这个print_result是可选的，是对前两个函数运行结果的操作
    [pool.putRequest(req) for req in reqs]  #多线程一块执行
    pool.wait()  #线程挂起，直到结束

可以在pypi上（https://pypi.python.org/pypi/threadpool#downloads）下载文件安装。


### 线程池2号  -multiprocessing.dummy

multiprocessing.dummy 模块与 multiprocessing 模块的区别： dummy 模块是多线程，而 multiprocessing 是多进程， api 都是通用的。 所有可以很方便将代码在多线程和多进程之间切换。

下面是个获取网页内容的例子，除了dummy好用，其中map也是个亮点啊。

    # from multiprocessing import Pool
    from multiprocessing.dummy import Pool as ThreadPool
    import time
    import urllib2

    urls = [
        'http://www.baidu.com',
        'http://home.baidu.com/',
        'http://tieba.baidu.com/',
        'http://zhidao.baidu.com/',
        'http://music.baidu.com/',
        'http://image.baidu.com/',
        'http://python-china.org/',
        'http://python-china.org/node/about',
        'http://python-china.org/node/',
        'http://python-china.org/account/signin',
        'http://python-china.org/account/signup',
        'http://www.qq.com',
        'http://www.youku.com',
        'http://www.tudou.com'
    ]

    start = time.time()
    results = map(urllib2.urlopen, urls)
    print 'Normal:', time.time() - start

    start2 = time.time()
    # 开8个 worker，没有参数时默认是 cpu 的核心数
    pool = ThreadPool(processes=8)
    # 在线程中执行 urllib2.urlopen(url) 并返回执行结果
    results2 = pool.map(urllib2.urlopen, urls)
    pool.close()
    pool.join()
    print 'Thread Pool:', time.time() - start2

执行结果:

    Normal: 12.5460000038
    Thread Pool: 2.0680000782

### 线程池3号  -用Queue和threading实现

找资源时候无意中发现了一个很不错的博客：**http://www.the5fire.com/**
推荐你也关注一下，我是直接就写到rss了。  

代码有点长，不贴了，感兴趣的到最下面的参考链接去撸吧。（因为我也要仿照写啊！）

记录主要思路：  

整个代码只有两个类：WorkManager和Work，前者确实如命名所示，是一个管理者，管理线程池和任务队列，而后者就是具体的一个线程。

它的整个运行逻辑就是，给WorkManager分配制定的任务量和线程数，然后每个线程都从任务队列中获取任务来执行，直到队列中没有任务。

无独有偶，有个老外也用Queue写了个类似的，也是分类两个类，一个是worker，一个是threadpool，后面我就照着这个学习了。


### 线程池4号-ThreadPoolExecutor

在python cookbook的第三版书中，还介绍了新的线程池模块。

12.7. Creating a Thread Pool
 >Problem  
You want to create a pool of worker threads for serving clients or performing other kinds of work.
Solution  
The  concurrent.futures library has a  ThreadPoolExecutor class that can be used for this purpose. Here is an example of a simple TCP server that uses a thread-pool to serve clients:
    
    from socket import AF_INET, SOCK_STREAM, socket
    from concurrent.futures import ThreadPoolExecutor
    ...

用的时候好简单，直接`pool = ThreadPoolExecutor(128)`类似这样的写法就好了。  
这个需要单独安装，看包的名字后面有个futures，特意记录一下，以后估计大有可为。


## 自己造轮子  

好了，看了这么多，轮子还是要自己造一个的。

赵猫画虎也好啊！

学习有个721原则，学习占10%，反馈占20%，剩下的70%要用实践来完成。这样长期下去，学习的知识才会变成我的能力。
这种编程的学习更是如此。撸完别人的代码，自己是必须要敲一敲，再敲一敲才过瘾！

线程池采用Queue和Thread完成。测试时候采用个洗澡场景，记录完成率和每人洗澡时间。

    #!/usr/bin/env python
    # -*- coding:utf-8 -*-

    __author__ = 'SothisDe8ug'

    # standard library modules
    from Queue import Queue
    from threading import Thread

    class ThreadWorker(Thread):
        """Thread executing tasks from a given tasks queue"""
        def __init__(self, tasks):
            Thread.__init__(self)
            self.tasks = tasks # 任务队列
            self.daemon = True # 必须在start之前
            self.start()

        def run(self):
            while True:
                func, args, kargs = self.tasks.get() # 取出来一个任务
                try:
                    func(*args, **kargs) # 执行任务函数
                except Exception as e:
                    print e
                self.tasks.task_done()


    class MyThreadPool:
        """Pool of threads consuming tasks from a queue"""
        def __init__(self, num_threads):
            self.tasks = Queue(num_threads)
            for _ in range(num_threads):
                ThreadWorker(self.tasks) # 启动每一个任务

        def add_task(self, func, *args, **kargs):
            """Add a task to the queue"""
            self.tasks.put((func, args, kargs)) # 放入队列

        def wait_completion(self):
            """Wait for completion of all the tasks in the queue"""
            self.tasks.join()


    if __name__ == '__main__':
        from random import randrange
        delays = [randrange(1, 10) for i in range(100)] # 随机有100以内的人去洗澡

        from time import sleep
        def bath_time(d):
            print 'take a bath for (%d)sec' % d # 每个人洗澡时间1-10s
            sleep(d)

        # 1) Init a Thread pool with the desired number of threads
        pool = MyThreadPool(20) # 假设有20个喷头

        for i, d in enumerate(delays):
            # print the percentage of tasks placed in the queue
            print 'bath finish percent of total number: %.2f%c' % ((float(i)/float(len(delays)))*100.0,'%') #有多少人洗完了

            # 2) Add the task to the queue
            pool.add_task(bath_time, d)

        # 3) Wait for completion
        pool.wait_completion()


## 多线程应用  

如前文所说，多线程要用在IO操作很多的场景。比如浏览器后台的读写，用户交互界面不能卡住，读写数据就可以用多线程和服务器进行传输。

## 参考  

- [知乎-对CPU密集型计算和IO密集型运算，应该选择多进程还是多线程？](https://www.zhihu.com/question/29927678)  
- [廖雪峰python-线程与进程](http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/001397567993007df355a3394da48f0bf14960f0c78753f000)    
- [Python 快速教程（标准库08）：多线程与同步 (threading包)](http://www.cnblogs.com/vamei/archive/2012/10/11/2720042.html)    
- [Python多线程简易版 - 线程池threadpool](http://www.zhidaow.com/post/python-threadpool)    
- [使用 multiprocessing.dummy 执行多线程任务](https://mozillazg.com/2014/01/python-use-multiprocessing-dummy-run-theading-task.html)    
- [python线程池](http://www.the5fire.com/python-thread-pool.html)    
- [Python Thread Pool (Python recipe)](http://code.activestate.com/recipes/577187-python-thread-pool/)    
- [A Better Model for Day to Day Threading Tasks-使用 multiprocessing.dummy ](http://chriskiehl.com/article/parallelism-in-one-line/)    
- []()    

---
> 人生苦短，python的世界里感谢有你。


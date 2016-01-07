---
layout: post
title:  【Howie玩python】-多线程从0到1
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

因为Python的线程虽然是真正的线程，但解释器执行代码时，有一个GIL锁：Global Interpreter Lock，任何Python线程执行前，必须先获得GIL锁，然后，每执行100条字节码，解释器就自动释放GIL锁，让别的线程有机会执行。这个GIL全局锁实际上把所有线程的执行代码都给上了锁，所以，多线程在Python中只能交替执行，即使100个线程跑在100核CPU上，也只能用到1个核。

由于GIL的存在，CPython不能有效的利用多核处理器。表现为任意时间一个进程只有一个线程在跑，而IO密集型运算，多数是在IO读写将线程堵塞掉了，这个时候线程切换是很合理的，反正线程只是单纯地等待，在这个等待的时候去做其他的事情，资源利用率就上去了。  

简单点解释就是，python的多线程只能使用一个cpu核心。io密集型应用，本来cpu占用率就很低，单个cpu核心就够了。用多进程给它多几个核心也不能提升性能。

**IO密集型:**涉及到网络、磁盘IO的任务都是IO密集型任务，这类任务的特点是CPU消耗很少，任务的大部分时间都在等待IO操作完成（因为IO的速度远远低于CPU和内存的速度）。对于IO密集型任务，任务越多，CPU效率越高，但也有一个限度。常见的大部分任务都是IO密集型任务，比如Web应用。

IO密集型任务执行期间，99%的时间都花在IO上，花在CPU上的时间很少，因此，用运行速度极快的C语言替换用Python这样运行速度极低的脚本语言，完全无法提升运行效率。对于IO密集型任务，最合适的语言就是开发效率最高（代码量最少）的语言，脚本语言是首选，C语言最差。

### 多进程

多进程类似同时有几个班级的学生在学习，某个班级学生不及格，但是不会影响到其他班级，因为各个进程都是独立的。

与多线程相比，多进程比较稳定。但是也有缺点，就是创建时候需要消耗很多资源，在Unix/Linux系统下，使用用fork调用。

多进程可以有效的利用cpu多核的特点，处理计算密集型任务。  

**计算密集型任务**的特点是要进行大量的计算，消耗CPU资源，比如计算圆周率、对视频进行高清解码等等，全靠CPU的运算能力。这种计算密集型任务虽然也可以用多任务完成，但是任务越多，花在任务切换的时间就越多，CPU执行任务的效率就越低，所以，要最高效地利用CPU，计算密集型任务同时进行的数量应当等于CPU的核心数。


## 研究优秀博客  


## 自己造轮子  


## 多线程应用  


## 参考  

-[知乎-对CPU密集型计算和IO密集型运算，应该选择多进程还是多线程？](https://www.zhihu.com/question/29927678)  
-[廖雪峰python-线程与进程](http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/001397567993007df355a3394da48f0bf14960f0c78753f000)    
-[]()    
-[]()    
-[]()    
-[]()    
-[]()    
-[]()    
-[]()    

---
> 人生苦短，python的世界里感谢有你。


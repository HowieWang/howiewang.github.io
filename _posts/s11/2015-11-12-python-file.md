---
layout: post
title:  【Howie玩python】-文件操作
category: 
- python  
---

* content
{:toc}


**操作文件时，一般需要经历如下步骤：**

- 打开文件
- 操作文件

### 打开文件

`文件句柄 = file('文件路径', '模式')`

> 注：python中打开文件有两种方式，即：open(...) 和  file(...) ，本质上前者在内部会调用后者来进行文件操作，推荐使用 open。

打开文件时，需要指定文件路径和以何等方式打开文件，打开后，即可获取该文件句柄，日后通过此文件句柄对该文件操作。

打开文件的模式有：

>r，只读模式（默认）。
w，只写模式。【不可读；不存在则创建；存在则删除内容；】
a，追加模式。【可读；   不存在则创建；存在则只追加内容；】
"+" 表示可以同时读写某个文件
r+，可读写文件。【可读；可写；可追加】
w+，无意义
a+，同a

"U"表示在读取时，可以将 \r \n \r\n自动转换成 \n （与 r 或 r+ 模式同使用）
`
rU
r+U`

"b"表示处理二进制文件（如：FTP发送上传ISO镜像文件，linux可忽略，windows处理二进制文件时需标注）
`
rb
wb
ab
`
### 操作文件

```
class file(object):
  
    def close(self): # real signature unknown; restored from __doc__
        关闭文件
        """
        close() -> None or (perhaps) an integer.  Close the file.
         
        Sets data attribute .closed to True.  A closed file cannot be used for
        further I/O operations.  close() may be called more than once without
        error.  Some kinds of file objects (for example, opened by popen())
        may return an exit status upon closing.
        """
 
    def fileno(self): # real signature unknown; restored from __doc__
        文件描述符  
         """
        fileno() -> integer "file descriptor".
         
        This is needed for lower-level file interfaces, such os.read().
        """
        return 0    
 
    def flush(self): # real signature unknown; restored from __doc__
        刷新文件内部缓冲区
        """ flush() -> None.  Flush the internal I/O buffer. """
        pass
 
 
    def isatty(self): # real signature unknown; restored from __doc__
        判断文件是否是同意tty设备
        """ isatty() -> true or false.  True if the file is connected to a tty device. """
        return False
 
 
    def next(self): # real signature unknown; restored from __doc__
        获取下一行数据，不存在，则报错
        """ x.next() -> the next value, or raise StopIteration """
        pass
 
    def read(self, size=None): # real signature unknown; restored from __doc__
        读取指定字节数据
        """
        read([size]) -> read at most size bytes, returned as a string.
         
        If the size argument is negative or omitted, read until EOF is reached.
        Notice that when in non-blocking mode, less data than what was requested
        may be returned, even if no size parameter was given.
        """
        pass
 
    def readinto(self): # real signature unknown; restored from __doc__
        读取到缓冲区，不要用，将被遗弃
        """ readinto() -> Undocumented.  Don't use this; it may go away. """
        pass
 
    def readline(self, size=None): # real signature unknown; restored from __doc__
        仅读取一行数据
        """
        readline([size]) -> next line from the file, as a string.
         
        Retain newline.  A non-negative size argument limits the maximum
        number of bytes to return (an incomplete line may be returned then).
        Return an empty string at EOF.
        """
        pass
 
    def readlines(self, size=None): # real signature unknown; restored from __doc__
        读取所有数据，并根据换行保存值列表
        """
        readlines([size]) -> list of strings, each a line from the file.
         
        Call readline() repeatedly and return a list of the lines so read.
        The optional size argument, if given, is an approximate bound on the
        total number of bytes in the lines returned.
        """
        return []
 
    def seek(self, offset, whence=None): # real signature unknown; restored from __doc__
        指定文件中指针位置
        """
        seek(offset[, whence]) -> None.  Move to new file position.
         
        Argument offset is a byte count.  Optional argument whence defaults to
        0 (offset from start of file, offset should be >= 0); other values are 1
        (move relative to current position, positive or negative), and 2 (move
        relative to end of file, usually negative, although many platforms allow
        seeking beyond the end of a file).  If the file is opened in text mode,
        only offsets returned by tell() are legal.  Use of other offsets causes
        undefined behavior.
        Note that not all file objects are seekable.
        """
        pass
 
    def tell(self): # real signature unknown; restored from __doc__
        获取当前指针位置
        """ tell() -> current file position, an integer (may be a long integer). """
        pass
 
    def truncate(self, size=None): # real signature unknown; restored from __doc__
        截断数据，仅保留指定之前数据
        """
        truncate([size]) -> None.  Truncate the file to at most size bytes.
         
        Size defaults to the current file position, as returned by tell().
        """
        pass
 
    def write(self, p_str): # real signature unknown; restored from __doc__
        写内容
        """
        write(str) -> None.  Write string str to file.
         
        Note that due to buffering, flush() or close() may be needed before
        the file on disk reflects the data written.
        """
        pass
 
    def writelines(self, sequence_of_strings): # real signature unknown; restored from __doc__
        将一个字符串列表写入文件
        """
        writelines(sequence_of_strings) -> None.  Write the strings to the file.
         
        Note that newlines are not added.  The sequence can be any iterable object
        producing strings. This is equivalent to calling write() for each string.
        """
        pass
 
    def xreadlines(self): # real signature unknown; restored from __doc__
        可用于逐行读取文件，非全部
        """
        xreadlines() -> returns self.
         
        For backward compatibility. File objects now include the performance
        optimizations previously implemented in the xreadlines module.
        """
        pass
```

### with

为了避免打开文件后忘记关闭，可以通过管理上下文，即：

```
with open('log','r') as f:
     
    ...
    ```
如此方式，当with代码块执行完毕时，内部会自动关闭并释放文件资源。

在Python 2.7 后，with又支持同时对多个文件的上下文进行管理，即：

```
with open('log1') as obj1, open('log2') as obj2:
    pass
    ```
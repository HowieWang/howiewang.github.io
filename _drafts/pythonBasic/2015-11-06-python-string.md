---
layout: post
title:  【De8ug玩python】-无敌字符串
category: 
- python  
---

* content
{:toc}


## 帮助三剑客  

- dir  
- help  
- google

`help(str)` 就可以看见下面的源代码了。


    class str(basestring):
        """
        str(object='') -> string
        
        Return a nice string representation of the object.
        If the argument is a string, the return value is the same object.
        """
        def capitalize(self):  
            """ 首字母变大写 """
            """
            S.capitalize() -> string
            
            Return a copy of the string S with only its first character
            capitalized.
            """
            return ""

        def center(self, width, fillchar=None):  
            """ 内容居中，width：总长度；fillchar：空白处填充内容，默认无 """
            """
            S.center(width[, fillchar]) -> string
            
            Return S centered in a string of length width. Padding is
            done using the specified fill character (default is a space)
            """
            return ""

        def count(self, sub, start=None, end=None):  
            """ 子序列个数 """
            """
            S.count(sub[, start[, end]]) -> int
            
            Return the number of non-overlapping occurrences of substring sub in
            string S[start:end].  Optional arguments start and end are interpreted
            as in slice notation.
            """
            return 0

        def decode(self, encoding=None, errors=None):  
            """ 解码 """
            """
            S.decode([encoding[,errors]]) -> object
            
            Decodes S using the codec registered for encoding. encoding defaults
            to the default encoding. errors may be given to set a different error
            handling scheme. Default is 'strict' meaning that encoding errors raise
            a UnicodeDecodeError. Other possible values are 'ignore' and 'replace'
            as well as any other name registered with codecs.register_error that is
            able to handle UnicodeDecodeErrors.
            """
            return object()

        def encode(self, encoding=None, errors=None):  
            """ 编码，针对unicode """
            """
            S.encode([encoding[,errors]]) -> object
            
            Encodes S using the codec registered for encoding. encoding defaults
            to the default encoding. errors may be given to set a different error
            handling scheme. Default is 'strict' meaning that encoding errors raise
            a UnicodeEncodeError. Other possible values are 'ignore', 'replace' and
            'xmlcharrefreplace' as well as any other name registered with
            codecs.register_error that is able to handle UnicodeEncodeErrors.
            """
            return object()

        def endswith(self, suffix, start=None, end=None):  
            """ 是否以 xxx 结束 """
            """
            S.endswith(suffix[, start[, end]]) -> bool
            
            Return True if S ends with the specified suffix, False otherwise.
            With optional start, test S beginning at that position.
            With optional end, stop comparing S at that position.
            suffix can also be a tuple of strings to try.
            """
            return False
    ```

很多，咱们就不全贴上来了。

如果要看主要那些函数，就直接`dir`一下

```
>>> dir(str)
['__add__', '__class__', '__contains__', '__delattr__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getnewargs__', '__getslice__', '__gt__', '__hash__', '__init__', '__le__', '__len__', '__lt__', '__mod__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmod__', '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '_formatter_field_name_split', '_formatter_parser', 'capitalize', 'center', 'count', 'decode', 'encode', 'endswith', 'expandtabs', 'find', 'format', 'index', 'isalnum', 'isalpha', 'isdigit', 'islower', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'partition', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']
>>> 
```

## 用法举例  

字符串最需要主要的问题有两个：编码格式和格式化。  

### 编码格式

因为Python的诞生比Unicode标准发布的时间还要早，所以最早的Python只支持ASCII编码，普通的字符串'ABC'在Python内部都是ASCII编码的。Python提供了ord()和chr()函数，可以把字母和对应的数字相互转换：

        >>> ord('A')
        65
        >>> chr(65)
        'A'

Python在后来添加了对Unicode的支持，以Unicode表示的字符串用u'...'表示，比如：

        >>> print u'中文'
        中文
        >>> u'中'
        u'\u4e2d'

写u'中'和u'\u4e2d'是一样的，\u后面是十六进制的Unicode码。因此，u'A'和u'\u0041'也是一样的。

两种字符串如何相互转换？字符串'xxx'虽然是ASCII编码，但也可以看成是UTF-8编码，而u'xxx'则只能是Unicode编码。

把u'xxx'转换为UTF-8编码的'xxx'用encode('utf-8')方法：

        >>> u'ABC'.encode('utf-8')
        'ABC'
        >>> u'中文'.encode('utf-8')
        '\xe4\xb8\xad\xe6\x96\x87'

英文字符转换后表示的UTF-8的值和Unicode值相等（但占用的存储空间不同），而中文字符转换后1个Unicode字符将变为3个UTF-8字符，你看到的\xe4就是其中一个字节，因为它的值是228，没有对应的字母可以显示，所以以十六进制显示字节的数值。len()函数可以返回字符串的长度：

        >>> len(u'ABC')
        3
        >>> len('ABC')
        3
        >>> len(u'中文')
        2
        >>> len('\xe4\xb8\xad\xe6\x96\x87')
        6

反过来，把UTF-8编码表示的字符串'xxx'转换为Unicode字符串u'xxx'用decode('utf-8')方法：

        >>> 'abc'.decode('utf-8')
        u'abc'
        >>> '\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')
        u'\u4e2d\u6587'
        >>> print '\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')

> 中文
由于Python源代码也是一个文本文件，所以，当你的源代码中包含中文的时候，在保存源代码时，就需要务必指定保存为UTF-8编码。当Python解释器读取源代码时，为了让它按UTF-8编码读取，我们通常在文件开头写上这两行：

        #!/usr/bin/env python
        # -*- coding: utf-8 -*-


### 格式化

```
>>> help(str.format)
Help on method_descriptor:

format(...)
    S.format(*args, **kwargs) -> string
    
    Return a formatted version of S, using substitutions from args and kwargs.
    The substitutions are identified by braces ('{' and '}').
```

从上面的两个参数可以看出，python的字符串格式化相当的灵活。
在Python中，采用的格式化方式和C语言是一致的，用%实现，举例如下：

        >>> 'Hello, %s' % 'world'
        'Hello, world'
        >>> 'Hi, %s, you have $%d.' % ('Michael', 1000000)
        'Hi, Michael, you have $1000000.'

你可能猜到了，%运算符就是用来格式化字符串的。在字符串内部，%s表示用字符串替换，%d表示用整数替换，有几个%?占位符，后面就跟几个变量或者值，顺序要对应好。如果只有一个%?，括号可以省略。

常见的占位符有：

- %d  整数
- %f  浮点数
- %s  字符串
- %x  十六进制整数

其中，格式化整数和浮点数还可以指定是否补0和整数与小数的位数：

        >>> '%2d-%02d' % (3, 1)
        ' 3-01'
        >>> '%.2f' % 3.1415926
        '3.14'

如果你不太确定应该用什么，%s永远起作用，它会把任何数据类型转换为字符串：

        >>> 'Age: %s. Gender: %s' % (25, True)
        'Age: 25. Gender: True'

对于Unicode字符串，用法完全一样，但最好确保替换的字符串也是Unicode字符串：

        >>> u'Hi, %s' % u'Michael'
        u'Hi, Michael'

有些时候，字符串里面的%是一个普通字符怎么办？这个时候就需要转义，用%%来表示一个%：

        >>> 'growth rate: %d %%' % 7
        'growth rate: 7 %'

## 实际工程应用

## 参考

- [liangxuefeng-python 2.7](http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/001386819196283586a37629844456ca7e5a7faa9b94ee8000)  
-

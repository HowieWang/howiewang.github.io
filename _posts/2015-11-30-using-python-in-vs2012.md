---
layout: post
title:  python in vs2012
category: 
- cpp  

---

* content
{:toc}

Open Visual Studio and create a new C++ “Win32 Console Application” project.  Before you can call Python code you need to tell Visual Studio how to find Python.  Before doing this change the build mode from “Debug” to “Release”.  If you attempt to build a Visual Studio project that calls Python code in Debug mode the build will fail because, in debugging mode, Visual Studio will look for a library file that does not exist.  The reason why this file did not exist on my machine is because the Windows Python installer did not install a version of Python compiled in Debug mode.  It is possible to get a version of Python built in Debug mode, but you will have to download the Python source code and build it manually.  For the purposes of this blog post we will ignore this and just change to Release mode.

Next you need to add the locations of the Python header and library files to the project properties.  My Python installation directory was “C:\Python27”, so I added “C:\Python27\include” to the list of include directories and “C:\Python27\libs” to the list of library directories.  After making these changes to the properties you should be able to add the line “#include <Python.h>” to the C++ source file and build the project without error.



1. 把python的include和libs目录分别加到vc的include和lib directories中去。 
2. 另外，由于python没有提供debug lib，具体地说，就是没有提供python25_d.lib了,所以默认只能在Release下运行。 
3. 可以自己编译python的源代码来得到python25_d.lib。 
4. 或者，想要在debug下运行程序的话，你要把pyconfig.h（在python25/include/目录下）的大概是在283行， 
5. 把pragma comment(lib,"python25_d.lib")改成pragma comment(lib,"python25.lib")，让python都使用非debug lib.
---
layout: post
title:  【Howie玩docker】-使用mono编译c#程序
category: 
- docker  

---

* content
{:toc}

>根据前面的方法，在windows和Linux共享文件夹，然后就可以开发了！

### Start up an Ubuntu container

	$ docker run -it ubuntu bash


###Update apt-get inside the container


	$ apt-get update


###Install mono-complete

	root@47b0e0464825:/# apt-get install mono-complete


###Sanity test – run mono

	root@47b0e0464825:/# mono
	Usage is: mono [options] program [program-options]

	Development:
	    --aot[=<options>]      Compiles the assembly to native code
	...
	    --gc=[sgen,boehm]      Select SGen or Boehm GC (runs mono or mono-sgen)




###Suspend container, back to host shell

	$ <Control-P><Control-Q>
	root@47b0e0464825:/# [root@localhost share]#
	[root@localhost share]# docker ps
	CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
	47b0e0464825        ubuntu              "bash"              About an hour ago   Up About an hour                        furious_payne


### Commit changes to new Docker image

	[root@localhost share]# docker commit 47b0e0464825 howie/monodev
	87f7b9c4f3f5e809e3141b78117d6d8b984935a6b1023cbb9756409bdaad0cb4


###Run new Docker image

	root@localhost share]# docker images
	REPOSITORY                             TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
	howie/monodev                          latest              87f7b9c4f3f5        3 minutes ago       589.8 MB


###Create a hellomono.cs file

`$ vim hellomono.cs`

	[root@localhost share]# ls
	
> hellomono.cs  
	
	public class HelloMono
	{
	    static public void Main()
	    {
	        System.Console.WriteLine("Hello Mono!");
	    }
	}

###compile

	[root@localhost share]# docker run -it --rm -v $(pwd):/mono -w /mono howie/monodev mcs hellomono.cs
 
###Run

	[root@localhost share]# docker run -it --rm -v $(pwd):/mono -w /mono howie/monodev mono hellomono.exe
	
>Hello Mono!


参考文章：http://dotnetliberty.com/index.php/2015/10/04/mono-and-c-sharp-on-docker-hello-world-in-15-steps/?_tmc=JnJW_22cvahemGz-VrPpV-o6AeShsAY4R6CsqV6i5V4&mkt_tok=3RkMMJWWfF9wsRonuqTMZKXonjHpfsX57uQtXa%2BzlMI%2F0ER3fOvrPUfGjI4ASsBiI%2BSLDwEYGJlv6SgFQ7LMMaZq1rgMXBk%3D

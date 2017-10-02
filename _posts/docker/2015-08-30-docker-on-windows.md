---
layout: post
title:  【Howie玩docker】-windows下玩docker和go语言
category: 
- docker  

---

* content
{:toc}

**Windows下安装toolbox一直没成功，于是投机取巧，用虚拟机手工打造玩docker的方法。**  

### 步骤： 

- 安装虚拟机，安装centos
- 在win下建立共享文件夹，假如是` f:/share`
- 在centos里的mnt路径下，建立个挂载点：` /mnt/share/`
- mount添加win下的共享文件夹

### 命令： 

	
	[root@localhost ~]# mount -t cifs -o username=user,password=pass //192.168.1.103/share /mnt/share/
	[root@localhost ~]# cd /mnt/share/
	[root@localhost share]# ls
	1.txt 2.txt


### 应用-来个go的helloworld

在share文件夹，写个go文件`helloworld.go`

	package main
	import "fmt"

	func main() {
	    fmt.Println("hello,docker world in go! this is by daocloud or aluda!");
	}

shell中cd到mnt/share


	
	[root@localhost share]# docker run -it --rm -v $(pwd):/go -w /go index.alauda.cn/dockerlibrary/golang go build -o helloworld.out
	#或者
	[root@localhost share]# docker run -it --rm -v $(pwd):/go -w /go daocloud.io/library/golang:latest  go build -o helloworld.out
	#然后执行
	[root@localhost share]# ./helloworld.out



---
参考：http://www.ilanni.com/?p=6978

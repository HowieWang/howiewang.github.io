---
layout: post
title:  【Howie玩docker】-命令行只显示-bash-4.1#
category: 
- docker  

---

* content
{:toc}


灵雀云上面用docker建了个centOS的实例，首个免费，正好当云主机来玩。
但是，打开有个问题，**命令行不显示当前用户和路径**。
只显示：

```
	-bash-4.1#
```

简单，配置文件不全而已。
下面对其重新设置，需要设置两个文件：`~/.bashrc和~/.bash_profile `

###bashrc 
在当前目录下新建.bashrc文件： 
```
	# touch ~/.bashrc 
	# vi ~/.bashrc 
```
并输入以下数据
```
	# .bashrc 
	# Source global definitions 
	if [ -f /etc/bashrc ]; then 
	. /etc/bashrc 
	fi 
	# User specific aliases and functions
```
source以下使得其生效： 
	`# source ~/.bashrc `

###bash_profile 
在当前目录下新建.bash_profile文件： 
```
	# touch ~/.bash_profile 
	# vi ~/.bash_profile 
```
并输入以下数据 
```
		# .bash_profile 
		# Get the aliases and functions 
		if [ -f ~/.bashrc ]; then 
		. ~/.bashrc 
		fi 
```
source以下使得其生效： 
	`# source ~/.bash_profile `
可以看到已经能成功显示当前用户和路径了。 

---
如果懒得自己写，你可以直接`wget`我的文件:
```
	http://howie.wang/.bashrc
	http://howie.wang/.bash_profile
```

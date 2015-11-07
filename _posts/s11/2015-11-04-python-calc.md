---
layout: post
title:  【Howie玩python】-做个计算器不容易
category: 
- python  
---

* content
{:toc}


用python写个能解析运算表达式的计算器，大概分几步呢？
 我一开始朴素的想法是这样的，不管计算表达式多长，最终应该都能写成a+b的形式，也就是俩个数字中间带个运算符。   
 经过我的初步研究，大概要分如下步：

## 递归算法  
 

 递归算法有个经典的阶乘例子：
 
		 def factorial(n):
		    if n == 0 or n == 1: return 1
		    else: return (n * factorial(n - 1))
		    
**递归有两个特点：**  

 - 递归就是在过程或函数里调用自身；   
 - 在使用递归策略时，必须有一个明确的递归结束条件，称为递归出口。也就是要把返回的所有情况都考虑到，否则可能会因为python默认返回None引起奇怪的问题。

**递归算法一般用于解决三类问题：**    

- 数据的定义是按递归定义的。（比如Fibonacci函数）   
- 问题解法按递归算法实现。（回溯）    
- 数据的结构形式是按递归定义的。（比如树的遍历，图的搜索）    

## 模式匹配

一般说来，数据的“定义”有多少种情况，用来处理它的“模式”就有多少情况。比如算术表达式有两种情况，数字或者 (op e1 e2)。所以用来处理它的 match 语句就有两种模式。“你所有的情况，我都能处理”，这就是“穷举法”。穷举的思想非常重要，你漏掉的任何一种情况，都非常有可能带来麻烦。所谓的“数学归纳法”，就是这种穷举法在自然数的递归定义上面的表现。因为你穷举了所有的自然数可能被构造的两种形式，所以你能确保定理对“任意自然数”成立。

那么模式是如何工作的呢？比如 '(,op ,e1 ,e2) 就是一个模式（pattern），它被用来匹配输入的 exp。模式匹配基本的原理就是匹配与它“结构相同”的数据。比如，如果 exp 是 '(+ 1 2)，那么 '(,op ,e1 ,e2) 就会把 op 绑定到 '+，把 e1 绑定到 '1，把 e2 绑定到 '2。这是因为它们结构相同：

	'(,op ,e1 ,e2)
	'( +   1   2)

## 正则表达    
模式匹配需要把输入的表达式的各个项目放到特定的数据结构中，这就需要把一个完整的表达式，通过正则分析拆解成数字，符号的形式了。

## 做个解释器
思考了一晚上，感觉还是写个解释器吧，就针对四则运算定制一个就好。网上资料一堆一堆的，跟着做就好，为啥要假装自己学不会呢。说干就干，今天动手。预计两天的业余时间完工！
跟着这里做：[Let’s Build A Simple Interpreter. Part 1.](http://ruslanspivak.com/lsbasi-part1/)

**目标**  

	> 3 * 5 /-2 -(8*3/(20+3/2-5) + 4 /(3-2) * -3 )
	> 3.04545

嗯，两天过去了，木有做出来，因为我偷偷看了这个系列最后一篇文章的答案，并不能计算出上面那个等式。于是，还是自己造轮子吧。  

自己造轮子也是需要原料的。感谢强大的谷歌，让我找到俩不错的材料，如下：    

- [how-to-write-a-calculator-in-50-python-lines-without-eval](http://blog.erezsh.com/how-to-write-a-calculator-in-50-python-lines-without-eval/)    
- [how-to-write-a-calculator-in-70-python-lines-](http://blog.erezsh.com/how-to-write-a-calculator-in-70-python-lines-by-writing-a-recursive-descent-parser/)    

都是一个作者3年前的作品，从中我学会不错的数据结构，也就是元组和词典去把一个数学表达式符号化。    
	
	Token = collections.namedtuple('Token', ['name', 'value'])
	token_map = {'+':'ADD', '-':'ADD', '*':'MUL', '/':'MUL', '(':'LPAR', ')':'RPAR'}


在70行里面那个算法其实已经非常完美解决我的问题了，但是数学功底差，看了两天才看懂个七八成，所以还是继续自己造轮子。  
造轮子遇到最大障碍是负号的处理，研究了好几个小时才弄出来这个函数：    
	
	# 过滤负号，比如/-，*-
	    def remove_neg(self):
	        tokens = []
	        #tokens[:2] = self.tokens[:2]
	        next_pos = 0
	        for x in range(len(self.tokens)):
	            if x < next_pos:
	                continue
	            else:
	                next_pos = x
	          #  if x < 2:
	            #    continue
	            if self.tokens[x].name == 'MUL'and self.tokens[x+1].name == 'ADD':
	                name = 'NUM'
	                value = '-' + self.tokens[x+2].value
	                token = Token(name, value)
	                tokens.append(self.tokens[x])
	                tokens.append(token)
	                next_pos += 3
	            else:
	                tokens.append(self.tokens[x])
	        self.tokens_remove_neg = tokens
	        self.current_token = tokens[0]

	        return tokens


又折腾一天，才将就写出来个计算器，但是还是不能正确计算上面那个表达式。具体原因已经找到，就是我在处理计算顺序遇到括号流程时候算法有bug，正在debugging...
这个可以测试出bug

	expr = '4 /(3-2) * -3'

算法代码放到[github](https://github.com/HowieWang/pythonStudy/blob/master/s11/day2/calc_exp.py)，欢迎你来帮我完善。



---

## 参考资料：
* [Python递归（recursion）专题](http://www.cnblogs.com/balian/archive/2011/02/11/1951054.html)
* [怎样写解释器](http://www.cnblogs.com/heyonggang/archive/2012/12/18/2823177.html)

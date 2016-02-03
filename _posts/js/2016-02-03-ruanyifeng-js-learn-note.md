---
layout: post
title:  ruanyifeng的javascript教程学习笔记1-基本语法
category: 
-   js

---

* content
{:toc}

> http://javascript.ruanyifeng.com/


## 基本语法  

### 语句  
语句以分号结尾，一个分号就表示一个语句结束。多个语句可以写在一行内。

    var a = 1 + 3 ; var b = 'abc';
分号前面可以没有任何内容，JavaScript引擎将其视为空语句。

    ;;;
上面的代码就表示3个空语句。

### 变量  
变量是对“值”的引用，使用变量等同于引用一个值。每一个变量都有一个变量名。

    var a = 1;

JavaScript允许在变量赋值的同时，省略var命令声明变量。也就是说，var a = 1与a = 1，这两条语句的效果相同。但是由于这样的做法很容易不知不觉地创建全局变量（尤其是在函数内部），所以建议总是使用var命令声明变量。

> 严格地说，var a = 1 与 a = 1，这两条语句的效果不完全一样，主要体现在delete命令无法删除前者。不过，绝大多数情况下，这种差异是可以忽略的。

可以在同一条var命令中声明多个变量。

    var a, b;
JavaScirpt是一种动态类型语言，也就是说，变量的类型没有限制，可以赋予各种类型的值。

    var a = 1;
    a = 'hello';
上面代码中，变量a起先被赋值为一个数值，后来又被重新赋值为一个字符串。第二次赋值的时候，因为变量a已经存在，所以不需要使用var命令。

** 变量提升**
JavaScript引擎的工作方式是，先解析代码，获取所有被声明的变量，然后再一行一行地运行。这造成的结果，就是所有的变量的声明语句，都会被提升到代码的头部，这就叫做变量提升（hoisting）。

    console.log(a);
    var a = 1;
上面代码首先使用console.log方法，在控制台（console）显示变量a的值。这时变量a还没有声明和赋值，所以这是一种错误的做法，但是实际上不会报错。因为存在变量提升，真正运行的是下面的代码。

    var a;
    console.log(a);
    a = 1;
最后的结果是显示undefined，表示变量a已声明，但还未赋值。

请注意，变量提升只对var命令声明的变量有效，如果一个变量不是用var命令声明的，就不会发生变量提升。

    console.log(b);
    b = 1;
上面的语句将会报错，提示“ReferenceError: b is not defined”，即变量b未声明，这是因为b不是用var命令声明的，JavaScript引擎不会将其提升，而只是视为对顶层对象的b属性的赋值。


### 标识符与保留字

> JavaScript有一些保留字，不能用作标识符：arguments、break、case、catch、class、const、continue、debugger、default、delete、do、else、enum、eval、export、extends、false、finally、for、function、if、implements、import、in、instanceof、interface、let、new、null、package、private、protected、public、return、static、super、switch、this、throw、true、try、typeof、var、void、while、with、yield。

有三个词虽然不是保留字，但是因为具有特别含义，也不应该用作标识符：Infinity、NaN、undefined。


### 标签（label）
JavaScript语言允许，语句的前面有标签（label），相当于定位符，用于跳转到程序的任意位置，标签的格式如下。

    label:
      statement
标签可以是任意的标识符，但是不能是保留字，语句部分可以是任意语句。

标签通常与break语句和continue语句配合使用，跳出特定的循环。

    top:
      for (var i = 0; i < 3; i++){
        for (var j = 0; j < 3; j++){
          if (i === 1 && j === 1) break top;
          console.log('i=' + i + ', j=' + j);
      }
    }
    // i=0, j=0
    // i=0, j=1
    // i=0, j=2
    // i=1, j=0 
上面代码为一个双重循环区块，break命令后面加上了top标签（注意，top不用加引号），满足条件时，直接跳出双层循环。如果break语句后面不使用标签，则只能跳出内层循环，进入下一次的外层循环。


### 数据类型


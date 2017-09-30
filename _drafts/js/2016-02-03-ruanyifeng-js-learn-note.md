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


## 数据类型

JavaScript语言的每一个值，都属于某一种数据类型。JavaScript的数据类型，共有六种。（ES6又新增了第七种Symbol类型的值，本教程不涉及。）

    数值（number）：整数和小数（比如1和3.14）
    字符串（string）：字符组成的文本（比如”Hello World”）
    布尔值（boolean）：true（真）和false（假）两个特定值
    undefined：表示“未定义”或不存在，即此处目前没有任何值
    null：表示空缺，即此处应该有一个值，但目前为空
    对象（object）：各种值组成的集合

通常，我们将数值、字符串、布尔值称为原始类型（primitive type）的值，即它们是最基本的数据类型，不能再细分了。而将对象称为合成类型（complex type）的值，因为一个对象往往是多个原始类型的值的合成，可以看作是一个存放各种值的容器。至于undefined和null，一般将它们看成两个特殊值。

对象又可以分成三个子类型。

    狭义的对象（object）
    数组（array）
    函数（function）

狭义的对象和数组是两种不同的数据组合方式，而函数其实是处理数据的方法。JavaScript把函数当成一种数据类型，可以像其他类型的数据一样，进行赋值和传递，这为编程带来了很大的灵活性，体现了JavaScript作为“函数式语言”的本质。

这里需要明确的是，JavaScript的所有数据，都可以视为广义的对象。不仅数组和函数属于对象，就连原始类型的数据（数值、字符串、布尔值）也可以用对象方式调用。

### typeof运算符  
JavaScript有三种方法，可以确定一个值到底是什么类型。

    typeof运算符
    instanceof运算符
    Object.prototype.toString方法

instanceof运算符和Object.prototype.toString方法，将在后文相关章节介绍。这里着重介绍typeof运算符。

typeof运算符可以返回一个值的数据类型，可能有以下结果。

- 原始类型:数值、字符串、布尔值分别返回number、string、boolean。  
- 函数:函数返回function。  
- undefined    
- 除此以外，其他情况都返回object

实际编程中，这个特点通常用在判断语句。

    // 错误的写法
    if (v) {
      // ...
    }
    // ReferenceError: v is not defined

    // 正确的写法
    if (typeof v === "undefined") {
      // ...
    }


    typeof window // "object"
    typeof {} // "object"
    typeof [] // "object"
    typeof null // "object"

从上面代码可以看到，空数组（[]）的类型也是object，这表示在JavaScript内部，数组本质上只是一种特殊的对象。

另外，null的类型也是object，这是由于历史原因造成的。1995年JavaScript语言的第一版，所有值都设计成32位，其中最低的3位用来表述数据类型，object对应的值是000。当时，只设计了五种数据类型（对象、整数、浮点数、字符串和布尔值），完全没考虑null，只把它当作object的一种特殊值，32位全部为0。这是typeof null返回object的根本原因。

为了兼容以前的代码，后来就没法修改了。这并不是说null就属于对象，本质上null是一个类似于undefined的特殊值。

### null和undefined  

首先，null像在Java里一样，被当成一个对象。但是，JavaScript的数据类型分成原始类型和合成类型两大类，Brendan Eich觉得表示”无”的值最好不是对象。

其次，JavaScript的最初版本没有包括错误处理机制，发生数据类型不匹配时，往往是自动转换类型或者默默地失败。Brendan Eich觉得，如果null自动转为0，很不容易发现错误。

因此，Brendan Eich又设计了一个undefined。他是这样区分的：null是一个表示”无”的对象，转为数值时为0；undefined是一个表示”无”的原始值，转为数值时为NaN。

    Number(undefined) // NaN
    5 + undefined // NaN

但是，这样的区分在实践中很快就被证明不可行。目前null和undefined基本是同义的，只有一些细微的差别。

null的特殊之处在于，JavaScript把它包含在对象类型（object）之中。

    typeof null // "object"

上面代码表示，查询null的类型，JavaScript返回object（对象）。

**用法和含义** 

对于null和undefined，可以大致可以像下面这样理解。

null表示空值，即该处的值现在为空。典型用法是：

    作为函数的参数，表示该函数的参数是一个没有任何内容的对象。
    作为对象原型链的终点。

undefined表示不存在值，就是此处目前不存在任何值。典型用法是：

    变量被声明了，但没有赋值时，就等于undefined。
    调用函数时，应该提供的参数没有提供，该参数等于undefined。
    对象没有赋值的属性，该属性的值为undefined。
    函数没有返回值时，默认返回undefined。
    
    var i;
    i // undefined

    function f(x){console.log(x)}
    f() // undefined

    var  o = new Object();
    o.p // undefined

    var x = f();
    x // undefined


### if判断  

空数组（[]）和空对象（{}）对应的布尔值，都是true。

    if ([]) {
      console.log(true);
    }
    // true

    if ({}) {
      console.log(true);
    }
    // true


## 参考  

[http://javascript.ruanyifeng.com/](http://javascript.ruanyifeng.com/grammar/basic.html#toc0)
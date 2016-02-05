---
layout: post
title:  ruanyf的js教程学习笔记2-基本语法注意点
category: 
-   js

---

* content
{:toc}

> http://javascript.ruanyifeng.com/


## 函数

### 全局变量和局部变量

注意，对于var命令来说，局部变量只能在函数内部声明，在其他区块中声明，一律都是全局变量。

    if (true) {
      var x = 5;
    }
    console.log(x);  // 5


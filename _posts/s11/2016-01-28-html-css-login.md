---
layout: post
title:  【可爱的前端】-从一个登录学前端基础
category: 
-  html
- css

---

* content
{:toc}

## 为什么学前端

为什么？自己做网站啊！
现在移动应用这么流行，网站已经不简单是原来傻傻的pc端的浏览器显示的内容，已经变成一个个的web应用。比如weibo，比如各种邮箱，比如在线的office，ps，甚至操作系统！

html5的广泛使用，已经使得做一个网站可以跨越各种平台，满足各种业务场景，因此，学习前端刻不容缓。

网络的世界里，用别人的同时，还要做自己的！

## html基础

html是所有网站的主要内容，他到各种标签构成了网站的骨架。

HTML是英文Hyper Text Mark-up Language(超文本标记语言)的缩写，他是一种制作万维网页面标准语言（标记）。相当于定义统一的一套规则，大家都来遵守他，这样就可以让浏览器根据标记语言的规则去解释它。

浏览器负责将标签翻译成用户“看得懂”的格式，呈现给用户！(例：djangomoan模版引擎)

下面主要说HTML5!

HTML5 将成为 HTML、XHTML 以及 HTML DOM 的新标准。
HTML 的上一个版本诞生于 1999 年。自从那以后，Web 世界已经经历了巨变。
HTML5 仍处于完善之中。然而，大部分现代浏览器已经具备了某些 HTML5 支持。

HTML5 是如何起步的？

HTML5 是 W3C 与 WHATWG 合作的结果。

### HTML5新特性    
HTML5 中的一些有趣的新特性：

- 用于绘画的 canvas 元素    
- 用于媒介回放的 video 和 audio 元素    
- 对本地离线存储的更好的支持    
- 新的特殊内容元素，比如 article、footer、header、nav、section   
- 新的表单控件，比如 calendar、date、time、email、url、search    

        <!DOCTYPE HTML>
        <html>
        <body>

        <video width="320" height="240" controls="controls">
          <source src="movie.ogg" type="video/ogg">
          <source src="movie.mp4" type="video/mp4">
        Your browser does not support the video tag.
        </video>

        </body>
        </html>

具体标签列表如下：

[HTML5_element_list](https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/HTML5/HTML5_element_list)

![](http://www.wfuns.com/wp-content/uploads/2014/11/HTML51.gif)

![](http://www.html5party.com/wp-content/uploads/2013/07/yuyi.gif)

## css基础

CSS 用于控制网页的样式和布局。
CSS3 是最新的 CSS 标准。
使用 CSS3 中的新特性，我们可以做出来更丰富多彩的网站应用。

### CSS 的格式

CSS 样式元素的结构很简单：

    html-tag-name {
        css-property-key-1: css-value-1;
        css-property-key-2: css-value-2;
    }

>其中 html-tag-name 可以是您能在 HTML 代码中找到的任何标记（比如 <a>、<div>、<li> 或 <label>）。除了 HTML 标记，在 CSS 代码中它也可以是前面带有井号（#）的 ID 引用，如下所示：

    #id-of-html-tag {
        …
    }

或者在 CSS 代码中这个标记可以是一个前面带有点/句点（.）的类引用：

    .class-of-html-tag {
        …
    }

CSS 的这些部分（html-tag-name、id-of-html-tag 或 class-of-html-tag）称为简单选择器，可嵌套（使用空格分开）使用以在 HTML 中实现更高的粒度，如下所示：

    outer-html-tag-name inner-html-tag-name { … }

或者作为一个列表来将一种设计元素应用到多个选择器：

    1st-html-tag-name, 2nd-html-tag-name { … }


## 登录框示例


## 参考

- [thecodeplayer-超级推荐](http://thecodeplayer.com/)      
- [很酷的设计](http://designscrazed.org/css-html-login-form-templates/)          
- [again cool](https://colorlib.com/wp/html5-and-css3-login-forms/)     
- [web desing blog--queness](http://www.queness.com/post/256/vertical-scroll-menu-with-jquery-tutorial)          
- [100-great-css-menu-tutorials](http://www.noupe.com/essentials/freebies-tools-templates/100-great-css-menu-tutorials.html)

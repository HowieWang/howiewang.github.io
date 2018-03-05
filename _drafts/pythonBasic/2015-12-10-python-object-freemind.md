---
layout: post
title:  【De8ug玩python】-python对象的思维导图
category:
-   python

---

* content
{:toc}

<script type="text/javascript" src="{{"/static/marktree.js" }}"></script>
<link rel="stylesheet" href="/static/treestyles.css" type="text/css" />

> 一切皆对象（续篇），用个思维导图整理一下python面向对象的边边角角吧！

<div class="basetext" id="base"><ul>
    <li class="col" id="FMID_1437991969FM"><div class="nodecontent">Python面向对象编程</div>
        <ul class="subexp">
    <li class="col"><div class="nodecontent">类的特殊成员</div>
        <ul class="subexp">
    <li class="col"><div class="nodecontent">__doc__</div>
        <ul class="subexp">
    <li class="basic"><div class="nodecontent">类的描述信息</div></li></ul></li>
    <li class="col"><div class="nodecontent">__module__</div>
        <ul class="subexp">
    <li class="basic"><div class="nodecontent">表示当前操作的对象在哪个模块</div></li></ul></li>
    <li class="col"><div class="nodecontent">__class__</div>
        <ul class="subexp">
    <li class="basic"><div class="nodecontent">表示当前操作的类</div></li></ul></li>
    <li class="col"><div class="nodecontent">__init__</div>
        <ul class="subexp">
    <li class="basic"><div class="nodecontent">构造方法</div></li></ul></li>
    <li class="col"><div class="nodecontent">__del__</div>
        <ul class="subexp">
    <li class="basic"><div class="nodecontent">析构方法</div></li></ul></li>
    <li class="basic"><div class="nodecontent">__call__</div></li>
    <li class="col"><div class="nodecontent">__dict__</div>
        <ul class="subexp">
    <li class="col"><div class="nodecontent">对象.__dict__</div>
        <ul class="subexp">
    <li class="basic" id="FMID_1649255792FM"><div class="nodecontent">类的普通字段</div></li></ul></li>
    <li class="col"><div class="nodecontent">类.__dict__</div>
        <ul class="subexp">
    <li class="basic" id="FMID_543247045FM"><div class="nodecontent">类中的静态字段和方法</div></li></ul></li></ul></li>
    <li class="col"><div class="nodecontent">__str__</div>
        <ul class="subexp">
    <li class="basic"><div class="nodecontent">定义打印对象时的返回值</div></li></ul></li>
    <li class="col"><div class="nodecontent">索引操作</div>
        <ul class="subexp">
    <li class="basic"><div class="nodecontent">__getitem__</div></li>
    <li class="basic"><div class="nodecontent">__setitem__</div></li>
    <li class="basic"><div class="nodecontent">__delitem__</div></li></ul></li>
    <li class="col"><div class="nodecontent">切片操作</div>
        <ul class="subexp">
    <li class="basic"><div class="nodecontent">__getslice__</div></li>
    <li class="basic"><div class="nodecontent">__setslice__</div></li>
    <li class="basic"><div class="nodecontent">__delslice__</div></li></ul></li>
    <li class="col"><div class="nodecontent">__iter__</div>
        <ul class="subexp">
    <li class="basic"><div class="nodecontent">迭代器</div></li></ul></li>
    <li class="col" id="FMID_1078365995FM"><div class="nodecontent">type类</div>
        <ul class="subexp">
    <li class="basic" id="FMID_4ecc64b6043c65f0307bcd909888f1d9FM"><div class="nodecontent">__new__</div></li>
    <li class="basic" id="FMID_1981831219FM"><div class="nodecontent">__metaclass__</div></li>
    <li class="basic" id="FMID_1558534152FM"><div class="nodecontent">obj对象是Foo类的一个实例，Foo类对象是 type 类的一个实例</div></li></ul></li></ul></li>
    <li class="col"><div class="nodecontent">类成员修饰符</div>
        <ul class="subexp">
    <li class="col" id="FMID_722320589FM"><div class="nodecontent">公有</div>
        <ul class="subexp">
    <li class="basic" id="FMID_136709211FM"><div class="nodecontent">对象可以访问；类内部可以访问；派生类中可以访问</div></li></ul></li>
    <li class="col"><div class="nodecontent">私有</div>
        <ul class="subexp">
    <li class="basic" id="FMID_674046490FM"><div class="nodecontent">定义方法:  __name</div></li>
    <li class="basic"><div class="nodecontent">只能内部访问</div></li>
    <li class="basic"><div class="nodecontent">特殊的访问方式: 对象._类__成员</div></li></ul></li></ul></li>
    <li class="col" id="FMID_415996465FM"><div class="nodecontent">参考</div>
        <ul class="subexp">
    <li class="basic" id="FMID_384032483FM"><div class="nodecontent"><a href="http://www.cnblogs.com/wupeiqi/articles/5017742.html">cnblog-wupeiqi</a> <a href="http://www.cnblogs.com/wupeiqi/articles/5017742.html"><img src="Python面向对象编程.html_files//ilink.png" alt="User Link" style="border-width:0" /></a></div></li>
    <li class="basic" id="FMID_1899239181FM"><div class="nodecontent">石鹏的第一版</div></li></ul></li>
    <li class="col"><div class="nodecontent">类成员</div>
        <ul class="subexp">
    <li class="col"><div class="nodecontent">字段</div>
        <ul class="subexp">
    <li class="basic" id="FMID_1488375139FM"><div class="nodecontent">普通字段属于对象</div></li>
    <li class="basic" id="FMID_1602174316FM"><div class="nodecontent">静态字段属于类</div></li></ul></li>
    <li class="col"><div class="nodecontent">方法</div>
        <ul class="subexp">
    <li class="col"><div class="nodecontent">普通方法</div>
        <ul class="subexp">
    <li class="basic"><div class="nodecontent">由对象触发</div></li>
    <li class="basic" id="FMID_612967687FM"><div class="nodecontent">至少一个self参数</div></li></ul></li>
    <li class="col"><div class="nodecontent">类方法</div>
        <ul class="subexp">
    <li class="basic"><div class="nodecontent">由类触发</div></li>
    <li class="basic" id="FMID_720494650FM"><div class="nodecontent">只有一个cls参数</div></li></ul></li>
    <li class="col"><div class="nodecontent">静态方法</div>
        <ul class="subexp">
    <li class="basic"><div class="nodecontent">由类触发</div></li>
    <li class="basic" id="FMID_1357985895FM"><div class="nodecontent">无默认参数</div></li></ul></li></ul></li>
    <li class="col" id="FMID_494792115FM"><div class="nodecontent">属性</div>
        <ul class="subexp">
    <li class="basic"><div class="nodecontent">普通属性</div></li>
    <li class="col"><div class="nodecontent">定义方法</div>
        <ul class="subexp">
    <li class="basic"><div class="nodecontent">@property</div></li>
    <li class="basic"><div class="nodecontent">Data = property(方法名)</div></li>
    <li class="col"><div class="nodecontent">新式类中特有的定义方法</div>
        <ul class="subexp">
    <li class="basic"><div class="nodecontent">@property</div></li>
    <li class="basic"><div class="nodecontent">方法名.setter</div></li>
    <li class="basic"><div class="nodecontent">方法名.deleter</div></li></ul></li></ul></li>
    <li class="basic" id="FMID_1333418290FM"><div class="nodecontent">意义：访问属性时可以制造出和访问字段完全相同的假象</div></li></ul></li></ul></li>
    <li class="col" id="FMID_655924589FM"><div class="nodecontent">类、对象、内存图</div>
        <ul class="subexp">
    <li class="basic" id="FMID_1798281308FM"><div class="nodecontent">面向对象编程其实就是对 “类” 和 “对象” 的使用</div></li>
    <li class="basic" id="FMID_222393983FM"><div class="nodecontent">类就是一个模板，模板里可以包含多个函数，函数里实现一些功能</div></li>
    <li class="basic" id="FMID_668682682FM"><div class="nodecontent">对象则是根据模板创建的实例，通过实例对象可以执行类中的函数</div></li>
    <li class="basic" id="FMID_371281316FM"><div class="nodecontent">内存--类以及类中的方法在内存中只有一份</div></li>
    <li class="basic" id="FMID_1142577334FM"><div class="nodecontent">类对象指针-指向当前对象的类</div></li></ul></li>
    <li class="col" id="FMID_892668702FM"><div class="nodecontent">三大特性</div>
        <ul class="subexp">
    <li class="basic" id="FMID_77b65ff3c0f934a08bb21dbe97fd8b3dFM"><div class="nodecontent">多态[基本没用]</div></li>
    <li class="col" id="FMID_1302719320FM"><div class="nodecontent">继承</div>
        <ul class="subexp">
    <li class="basic"><div class="nodecontent">基类，派生类</div></li>
    <li class="basic"><div class="nodecontent">多继承</div></li>
    <li class="basic"><div class="nodecontent">新式类，经典类</div></li>
    <li class="basic" id="FMID_706918671FM"><div class="nodecontent">广度优先--新式类（object）</div></li>
    <li class="basic" id="FMID_106025472FM"><div class="nodecontent">深度优先--经典类：</div></li></ul></li>
    <li class="col" id="FMID_464114259FM"><div class="nodecontent">封装</div>
        <ul class="subexp">
    <li class="basic"><div class="nodecontent">多个方法共用一组变量,变量封装到对象</div></li>
    <li class="basic"><div class="nodecontent">通过一个Template创建多个对象</div></li>
    <li class="col" id="FMID_552323121FM"><div class="nodecontent">调用</div>
        <ul class="subexp">
    <li class="basic" id="FMID_499053471FM"><div class="nodecontent">通过对象直接调用</div></li>
    <li class="basic" id="FMID_1770903579FM"><div class="nodecontent">self间接调用</div></li></ul></li></ul></li></ul></li>
    <li class="col" id="FMID_1144057488FM"><div class="nodecontent">其他</div>
        <ul class="subexp">
    <li class="basic" id="FMID_1042393526FM"><div class="nodecontent">isinstance(obj, cls)</div></li>
    <li class="basic" id="FMID_1948364102FM"><div class="nodecontent">issubclass(sub, super)</div></li>
    <li class="col" id="FMID_982276589FM"><div class="nodecontent">异常处理</div>
        <ul class="subexp">
    <li class="basic" id="FMID_850340252FM"><div class="nodecontent">增加友好性一个提示的页面</div></li>
    <li class="col" id="FMID_1062909272FM"><div class="nodecontent">异常种类</div>
        <ul class="subexp">
    <li class="basic" id="FMID_169298454FM"><div class="nodecontent">AttributeError</div></li>
    <li class="basic" id="FMID_34794202FM"><div class="nodecontent">IOError</div></li>
    <li class="basic" id="FMID_1459120530FM"><div class="nodecontent">ImportError</div></li>
    <li class="basic" id="FMID_779511784FM"><div class="nodecontent">IndentationError</div></li>
    <li class="basic" id="FMID_212792344FM"><div class="nodecontent">IndexError</div></li>
    <li class="basic" id="FMID_617658432FM"><div class="nodecontent">...</div></li></ul></li>
    <li class="col" id="FMID_1165543065FM"><div class="nodecontent">万能异常</div>
        <ul class="subexp">
    <li class="basic" id="FMID_1834252555FM"><div class="nodecontent">Exception</div></li>
    <li class="basic" id="FMID_273053104FM"><div class="nodecontent">可以捕获任意异常</div></li>
    <li class="basic" id="FMID_996401634FM"><div class="nodecontent">对于特殊处理或提醒的异常需要先定义，最后定义Exception来确保程序正常运行</div></li></ul></li></ul></li>
    <li class="col" id="FMID_343902850FM"><div class="nodecontent">反射</div>
        <ul class="subexp">
    <li class="basic" id="FMID_1323882280FM"><div class="nodecontent">hasattr</div></li>
    <li class="basic" id="FMID_687353051FM"><div class="nodecontent">getattr</div></li>
    <li class="basic" id="FMID_906523227FM"><div class="nodecontent">setattr</div></li>
    <li class="basic" id="FMID_360477057FM"><div class="nodecontent">delattr</div></li>
    <li class="basic" id="FMID_230536532FM"><div class="nodecontent">反射是通过字符串的形式操作对象相关的成员</div></li></ul></li></ul></li></ul></li></ul></div>
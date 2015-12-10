---
layout: post
title:  【Howie玩python】-python对象的思维导图
category:
-   python

---

* content
{:toc}

> 一切皆对象（续篇），用个思维导图整理一下python面向对象的边边角角吧！

<div class="basetop"><a onclick="expandAll(document.getElementById('base'))" href="#">Expand</a> -
<a onclick="collapseAll(document.getElementById('base'))" href="#">Collapse</a></div><div class="basetext" id="base"><ul>
    <li class="col" id="FMID_842188119FM"><div class="nodecontent">Python面向对象编程</div>
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
    <li class="basic"><div class="nodecontent">对象的所有成员</div></li></ul></li>
    <li class="col"><div class="nodecontent">类.__dict__</div>
        <ul class="subexp">
    <li class="basic"><div class="nodecontent">类的所有成员</div></li></ul></li></ul></li>
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
    <li class="col"><div class="nodecontent">type类</div>
        <ul class="subexp">
    <li class="basic" id="FMID_4ecc64b6043c65f0307bcd909888f1d9FM"><div class="nodecontent">__new__</div></li>
    <li class="basic"><div class="nodecontent">__metaclass__</div></li></ul></li></ul></li>
    <li class="col"><div class="nodecontent">类成员修饰符</div>
        <ul class="subexp">
    <li class="basic"><div class="nodecontent">公有</div></li>
    <li class="col"><div class="nodecontent">私有</div>
        <ul class="subexp">
    <li class="basic"><div class="nodecontent">定义方法:  __Class</div></li>
    <li class="basic"><div class="nodecontent">只能内部访问</div></li>
    <li class="basic"><div class="nodecontent">特殊的访问方式: 对象._类__成员</div></li></ul></li></ul></li>
    <li class="col"><div class="nodecontent">封装</div>
        <ul class="subexp">
    <li class="basic"><div class="nodecontent">多个方法共用一组变量,变量封装到对象</div></li>
    <li class="basic"><div class="nodecontent">通过一个Template创建多个对象</div></li></ul></li>
    <li class="col"><div class="nodecontent">类成员</div>
        <ul class="subexp">
    <li class="col"><div class="nodecontent">字段</div>
        <ul class="subexp">
    <li class="basic"><div class="nodecontent">普通字段</div></li>
    <li class="basic"><div class="nodecontent">静态字段</div></li></ul></li>
    <li class="col"><div class="nodecontent">方法</div>
        <ul class="subexp">
    <li class="col"><div class="nodecontent">普通方法</div>
        <ul class="subexp">
    <li class="basic"><div class="nodecontent">由对象触发</div></li>
    <li class="basic"><div class="nodecontent">失少一个self参数</div></li></ul></li>
    <li class="col"><div class="nodecontent">类方法</div>
        <ul class="subexp">
    <li class="basic"><div class="nodecontent">由类触发</div></li>
    <li class="basic"><div class="nodecontent">只有一个cls参数</div></li></ul></li>
    <li class="col"><div class="nodecontent">静态方法</div>
        <ul class="subexp">
    <li class="basic"><div class="nodecontent">由类触发</div></li>
    <li class="basic"><div class="nodecontent">任意个参数</div></li></ul></li></ul></li>
    <li class="col"><div class="nodecontent">属性</div>
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
    <li class="basic"><div class="nodecontent">方法名.deleter</div></li></ul></li></ul></li></ul></li></ul></li>
    <li class="basic"><div class="nodecontent">类、对象、内存图</div></li>
    <li class="col"><div class="nodecontent">继承</div>
        <ul class="subexp">
    <li class="basic"><div class="nodecontent">基类，派生类</div></li>
    <li class="basic"><div class="nodecontent">多继承</div></li>
    <li class="basic"><div class="nodecontent">新式类，经典类</div></li>
    <li class="basic"><div class="nodecontent">广度优先</div></li>
    <li class="basic"><div class="nodecontent">深入优先</div></li></ul></li>
    <li class="col"><div class="nodecontent">三大特性</div>
        <ul class="subexp">
    <li class="basic"><div class="nodecontent">封装</div></li>
    <li class="basic"><div class="nodecontent">继承</div></li>
    <li class="basic" id="FMID_77b65ff3c0f934a08bb21dbe97fd8b3dFM"><div class="nodecontent">多态</div></li></ul></li></ul></li></ul></div>
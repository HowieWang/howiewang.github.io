---
layout: post
title:  【姜戈带你飞-2】-django的template
category: 
- django  
---

* content
{:toc}

## 为啥要用模板
使用 django.http.HttpResponse() 来输出"Hello World！"。该方式将数据与视图混合在一起，不符合Django的MVC思想。MVC也就是模型，视图，控制器。用模板来添加修改网页的表现内容，更有利于大型工程的开发。

## 第一个hello模板

模板本质就是个文本文件。用来实现数据与视图分离。

在 HelloWorld 目录底下创建 templates 目录并建立 hello.html文件，整个目录结构如下：

    HelloWorld/
    |-- HelloWorld
    |   |-- __init__.py
    |   |-- __init__.pyc
    |   |-- settings.py
    |   |-- settings.pyc
    |   |-- urls.py
    |   |-- urls.pyc
    |   |-- view.py
    |   |-- view.pyc
    |   |-- wsgi.py
    |   `-- wsgi.pyc
    |-- manage.py
    `-- templates
        `-- hello.html

hello.html 文件代码如下：

    <h1>{{ hello }}</h1>

从模板中我们知道变量使用了双括号。

接下来我们需要向Django说明模板文件的路径，修改HelloWorld/settings.py，修改 TEMPLATES 中的 DIRS 为 [BASE_DIR+"/templates",]，如下所示:

    TEMPLATES = [
        {
            'BACKEND': 'django.template.backends.django.DjangoTemplates',
            'DIRS': [BASE_DIR+"/templates",],
            'APP_DIRS': True,
            'OPTIONS': {
                'context_processors': [
                    'django.template.context_processors.debug',
                    'django.template.context_processors.request',
                    'django.contrib.auth.context_processors.auth',
                    'django.contrib.messages.context_processors.messages',
                ],
            },
        },
    ]

我们现在修改 view.py，增加一个新的对象，用于向模板提交数据：

    # -*- coding: utf-8 -*-

    #from django.http import HttpResponse
    from django.shortcuts import render

    def hello(request):
        context          = {}
        context['hello'] = 'Hello World!'
        return render(request, 'hello.html', context)

可以看到，我们这里使用render来替代之前使用的HttpResponse。render还使用了一个字典context作为参数。

context 字典中元素的键值 "hello" 对应了模板中的变量 "{{ hello }}"。

## 模板变量  

上文的{{hello}}就是个模板变量，用两个大括号来表示。  
需要和view里面的字典的键值相对应。



## 模板继承

模板可以用继承的方式来实现复用。

接下来我们先创建之前项目的 templates 目录中添加 base.html 文件，代码如下：

        <html>
          <head>
            <title>Hello World!</title>
          </head>

          <body>
            <h1>Hello World!</h1>
            {% block mainbody %}
               <p>original</p>
            {% endblock %}
          </body>
        </html>

以上代码中，名为mainbody的block标签是可以被继承者们替换掉的部分。

        所有的 {% block %} 标签告诉模板引擎，子模板可以重载这些部分。

hello.html中继承base.html，并替换特定block，hello.html修改后的代码如下：

        {% extends "base.html" %}

        {% block mainbody %}
        <p>继承了 base.html 文件</p>
        {% endblock %}

第一行代码说明hello.html继承了 base.html 文件。可以看到，这里相同名字的block标签用以替换base.html的相应block。

----

## 参考：

- [1](http://www.phperz.com/article/15/0814/148615.html)  
- []()  
- []()  
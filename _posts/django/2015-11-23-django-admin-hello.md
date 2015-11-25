---
layout: post
title:  【姜戈带你飞-1】-django的helloworld
category: 
- django  
---

* content
{:toc}

## 安装django  
如果你还未安装Python环境需要先下载Python安装包。

- Python 下载地址：https://www.python.org/downloads/
- Django 下载地址：https://www.djangoproject.com/download/

##　Django 管理工具
django有个统一的项目管理工具叫`django-admin.py`, 可以用来创建和管理项目。 

    # django-admin.py
    Usage: django-admin.py subcommand [options] [args]

    Options:
      -v VERBOSITY, --verbosity=VERBOSITY
                            Verbosity level; 0=minimal output, 1=normal output,
                            2=verbose output, 3=very verbose output
      --settings=SETTINGS   The Python path to a settings module, e.g.
                            "myproject.settings.main". If this isn't provided, the
                            DJANGO_SETTINGS_MODULE environment variable will be
                            used.
      --pythonpath=PYTHONPATH
                            A directory to add to the Python path, e.g.
                            "/home/djangoprojects/myproject".
      --traceback           Raise on exception
      --version             show program's version number and exit
      -h, --help            show this help message and exit

    Type 'django-admin.py help <subcommand>' for help on a specific subcommand.

##　创建helloworld项目

在任意目录下，运行命令：

    django-admin.py startproject HelloWorld

进入这个目录，可以看见相应的目录结构如下：  

    [root@solar ~]# cd HelloWorld/
    [root@solar HelloWorld]# tree
    .
    |-- HelloWorld
    |   |-- __init__.py
    |   |-- settings.py
    |   |-- urls.py
    |   `-- wsgi.py
    `-- manage.py

目录说明：

>HelloWorld: 项目的容器。  
manage.py: 一个实用的命令行工具，可让你以各种方式与该 Django 项目进行交互。  
HelloWorld/__init__.py: 一个空文件，告诉 Python 该目录是一个 Python 包。  
HelloWorld/settings.py: 该 Django 项目的设置/配置。  
HelloWorld/urls.py: 该 Django 项目的 URL 声明; 一份由 Django 驱动的网站"目录"。  
HelloWorld/wsgi.py: 一个 WSGI 兼容的 Web 服务器的入口，以便运行你的项目。  
接下来我们进入 HelloWorld 目录输入以下命令，启动服务器：  

    python manage.py runserver 0.0.0.0:8000

>0.0.0.0让其它电脑可连接到开发服务器，8000为端口号。如果不说明，那么端口号默认为8000。

打开浏览器，访问8000端口，如果看见“It worked!”,则说明正常启动。（`ctrl+c`结束运行）。

## 编写view文件和url

在先前创建的 HelloWorld 目录下的 HelloWorld 目录新建一个 `view.py` 文件，并输入代码：

    from django.http import HttpResponse

    def hello(request):
        return HttpResponse("Hello world ! ")

接着，绑定 URL 与视图函数。打开 urls.py 文件，删除原来代码，将以下代码复制粘贴到 urls.py 文件中：

    from django.conf.urls import *
    from HelloWorld.view import hello

    urlpatterns = patterns("",
        ('^hello/$', hello),
    )

整个目录结构如下：

    [root@solar HelloWorld]# tree
    .
    |-- HelloWorld
    |   |-- __init__.py
    |   |-- __init__.pyc
    |   |-- settings.py
    |   |-- settings.pyc
    |   |-- urls.py              # url 配置
    |   |-- urls.pyc
    |   |-- view.py              # 添加的视图文件
    |   |-- view.pyc             # 编译后的视图文件
    |   |-- wsgi.py
    |   `-- wsgi.pyc
    `-- manage.py

完成后，启动 Django 开发服务器，并在浏览器访问打开浏览器并访问：`http://localhost:8000/hello/`

可以看见我们自己写的"Hello world ! "。

**注意：此时服务器会自动跟踪代码的改动，然后刷新页面就可以看到，不需要从新启动服务器。**

## 创建hello应用

### startapp
上面是创建个项目，但其实项目里面创建n个项目才是实际用法。  

        django-admin.py startapp hello

这之后会自动在项目路径下创建一个hello文件夹。

### settings.py

在项目的settings.py中，添加应用名称，注意后面的逗号！

        # Application definition

        INSTALLED_APPS = (
            'django.contrib.admin',
            'django.contrib.auth',
            'django.contrib.contenttypes',
            'django.contrib.sessions',
            'django.contrib.messages',
            'django.contrib.staticfiles',
            'hello',
        )

###　templates

在应用hello中创建templates文件夹，添加html模板。假如是 `hello.html`

        <h2>hi {{name}}</h2>

### views.py

在应用的view文件中，添加如下代码  ：

        from django.template import loader, Context
        from django.http import HttpResponse
        # Create your views here.

        def index(request):
            t = loader.get_template('hello.html')  # load template
            c = Context({'name': 'howie'}) # context, add data to template to render
            html = t.render(c) # str
            return HttpResponse(html)


###　urls

修改工程的urls.py文件，添加路由

        urlpatterns = [
            url(r'^admin/', include(admin.site.urls)),
            url(r'^index/$', 'hello.views.index')
        ]


###　run

        python manage.py runserver

看见正确的输出，说明我们这个hello应用就正常工作了。



----

## 参考：

- [1](http://www.phperz.com/article/15/0814/148616.html)  
- []()  
- []()  
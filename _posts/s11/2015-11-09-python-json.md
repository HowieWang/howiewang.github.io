---
layout: post
title:  【Howie玩python】-外交官json
category: 
- python  
---

* content
{:toc}


## 帮助三剑客  

- dir  
- help  
- google

    >>> dir(json)
    ['JSONDecoder', 'JSONEncoder', '__all__', '__author__', '__builtins__', '__doc__', '__file__', '__name__', '__package__', '__path__', '__version__', '_default_decoder', '_default_encoder', 'decoder', 'dump', 'dumps', 'encoder', 'load', 'loads', 'scanner']
    >>> 

    >>> help(json.dump)
    Help on function dump in module json:

    dump(obj, fp, skipkeys=False, ensure_ascii=True, check_circular=True, allow_nan=True, cls=None, indent=None, separators=None, encoding='utf-8', default=None, sort_keys=False, **kw)

## 用法举例  

json其实就是文件序列话的一种方式，这种方式有这么几个好处：


- 可读  
- 节省内存 
- 各种语言通用，所以我叫她外交官

简单用法：  

### 序列化

        import json

        dic = {'name': 'alex', 'age':16}
        teacher = {'teacher': dic}
        f = file('test.json', 'w')
        json.dump(teacher, f, sort_keys=True, indent=4, separators=(',', ': '))
        f.close()

这里注意`dump`后面几个参数的用法，具体查看上面的`help`。

生成的文件为：  

        {
            "teacher": {
                "age": 16,
                "name": "alex"
            }
        }

### 反序列化

        import json

        f = file('test.json')
        data = json.load(f)
        print data

        f.close()

输出：

> {u'teacher': {u'age': 16, u'name': u'alex'}}


## 实际工程应用  

很多啦，比如网站数据，Google浏览器插件，sublime的插件...不信你在自己电脑搜一下 `*.json`

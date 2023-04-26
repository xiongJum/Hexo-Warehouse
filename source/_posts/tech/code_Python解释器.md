---
title: Python修饰器
date: 2020-04-23 08:00:05
tags: 
- python
- 修饰器
photos: https://cdn.jsdelivr.net/gh/xiongJum/Picture/img/82169888_p0.png
---

### **修饰器的作用**

1.  修饰器本质上是一个Python函数，它可以让其他函数在不需要做任何代码变动的前提下增加额外功能
2.  python的修饰器是一种语法糖（Syntactic Sugar）

<!--more-->

+   实例

    ```python
    # 示例1
    @funA
    def funB():
        pass
    # 示例2， 实例1是示例2的简写，即示例1=示例2
    def funB():
        pass
    funB =funA(funB)
    ```


### **修饰函数**

修饰器的用法之一是修饰新定义的函数，修饰器通常用于<font color="red">扩展函数的行为或属性</font>

1.  示例（函数作为修饰器）

    ```python
    def log(func):
        def wraper():
            print("INFO: String {}".format(func.__name__))
            func()
            print("INFO: Finishing 	{}".format(func.__name__))
        return wraper
    
    @log
    def run():
        print("Running run...")
    run()
    -------------------------------------------------------------------------------------------------
    >>>
    INFO: String run
    Running run...
    INFO: Finshing run
    ```

### **修饰类**

1.  示例(函数作为修饰器)

    ```python
    from time import time, sleep
    
    def timer(Cls):
        def wraper():
            s = time()
            obj = Cls()
            e = time()
            print("Cost {:.4f}s to init.".format(e -s))
            return obj
        return wraper
    
    @timer
    class Obj:
        def __init__(self):
            print("Hello")
            sleep(2)
            print("Obj")
    
    Obj()
    --------------------------------------------------------------------------------------------------
    >>>
    Hello
    Obj
    Cost 2.0476s to init.
    ```

### **类作为修饰器**

只有函数才可以被调用，除了函数以外也可以定义被调用的类，只要添加 <font color="red">\__call__</font> 方法即可

1.  示例

```python
class HTML(object):
    """
    Baking HTML Tags!
    修饰HTML标签

    """
    def __init__(self, tag='p'):
        print("LOG: Baking Tag <{}>".format(tag))
        self.tag = tag
    def __call__(self, func):
        return lambda: "<{0}>{1}</{0}>".format(self.tag, func())

@HTML("html")
@HTML("body")
@HTML("div")
def body():
    return "Hello, World" 
    
print(body())
--------------------------------------------------------------
>>>
LOG: Baking Tag <html>
LOG: Baking Tag <body>
LOG: Baking Tag <div>
<html><body><div>Hello, World</div></body></html>
```

2.  函数解析

```python
lambda: "<{0}>{1}</{0}>".format(self.tag, func())
# lambda 匿名函数，“:”左边为返回值，
# format 格式化函数，
# {} 不指定参数位置 eg："{} {}".format("Hello", "World")
# {x} 指定参数位置 eg: "{0} {1}".format("Hello", "World")
```

### **传递参数**

在实际的使用过程中可能需要想修饰器传递参数，可能需要想被修饰的函数（或类）传递参数，如上方示例中

<font color='orange'>@HTML('html')</font> 

1.  示例

    ```python
    RULES = {}
    def route(rule):
        def decorator(hand):
            RULES.update({rule: hand})
            return hand
        return decorator
    
    @route("/")
    def index():
        print("XIONG JUM")
    
    def home():
        print("Hello, World!")
    home = route("/home")(home)
    
    index()
    home()
    print(RULES)
    print(index)
    --------------------------------------------------------------------------------------------------
    >>>
    XIONG JUM
    Hello, World!
    {'/': <function index at 0x000001C605A35B80>, '/home': <function home at 0x000001C605A35AF0>}
    <function index at 0x000001C605A35B80>
    ```

2.  函数解析

    ```python
    RULES.update({rule: hand})
    # update 字典的方法，更新字典的值，直接替换字典中相同键的值
    # <function index at 0x000001C605A35B80> 为函数index的储存位置
    ```

    向被修饰的函数传递参数，若不进行处理，则只需要将其原模原样的返回即可

    ```python
    @route("/login")
    def login(user='user', pwd='pwd'):
        print("DB.findOne({{{}, {}}})".format(user, pwd))
    login("hail", "python")
    --------------------------------------------------------------------------------------------------
    >>>
    DB.findOne({hail, python})
    ```

    如果需要在修饰器内执行，见<font color='red'>修饰函数的示例</font>

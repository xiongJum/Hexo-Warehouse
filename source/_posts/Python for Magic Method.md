---
title: Python学习之Magic Method(浅析)
date: 2020-07-15 11:00:00
categories: 
- Python浅析
tag: 
- 入门
- python
- Magic Method
- 描述器
---

##### 前言

本文基本摘自 两点水 的[草根学Python](),本文仅作记录备份或者本人查阅方便，

##### 了解Magic Method(魔术方法)

+ 双下划线包起来的方法，都统称为"魔术方法"，如"__init__"
+ 使用魔术方法可以构造出优美的代码，将复杂的逻辑封装成简单的方法
+ 使用dir()列出类中所有的魔术方法，如下例


```python
class User(object):
    pass
if __name__ == '__main__':
    print(dir(User()))
```

    ['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__']


##### 构造(__new__)和初始化(__init__)
+ 实际上创建类的过程是分为两步的，一步是创建类的对象，还有一部就是对类进行初始化
+ 下方的例子是对类进行初始化操作


```python
class User(object):
    def __init__(self, name, age):
        self.name = name
        self.age = age
user = User('XiongJum', 24)
```

+ __new__ 是用来创建类并返回这个类的实例，而 __init__ 只是将传入的参数来初始化该实例
+ __new__ 在创建一个实例的过程中必定会被调用， 但 __init__ 就不一定
+ 如通过 pickle.load 的方式反序化一个实例时就不会调用 __init__ 方法 <font color=red>反序列化？</font>
+ 具体示例如下


```python
class User(object):
    def __new__(cls, *args, **kwargs):
        # 打印 __new__ 方法中的相关信息
        print('调用了 def __new__ 方法')
        print(args)
        # 最后返回父类的方法
        return super(User, cls).__new__(cls)
    
    def __init__(self, name, age):
        print("调用了 def __init__ 方法")
        self.name = name
        self.age = age
        
if __name__ == '__main__':
    user = User('XiongJum', 23)
```

    调用了 def __new__ 方法
    ('XiongJum', 23)
    调用了 def __init__ 方法


+ 通过上图示例可得知，创建一个类，先是调用了 __new__ 方法来创建一个对象，把参数传给 __init__，并进行实例化
+ 但是在实际的开发中，很少会用到__new__ 方法，除非希望能够控制类的创建。

##### 属性的访问控制
+ \__getattr__(self, name): 
    + 定义了试图访问不存在的属性时的行为
    + 所以重载该方法可以实现捕获错误拼写然后进行重定向，或者对一些废弃的属性进行警告
+ \__setattr__(self, name, value):
    + 定义了对属性进行赋值和修改操作时的行为。
    + 不管对象的某个属性是否存在，都允许为该属性进行赋值(如下例的attr1)，但要避免"无限递归"的错误
+ \__delattr__(self, name):
    + __delattr__ 与 __setattr__ 很像，但它只定义的时你删除属性时的行为
    + 和 __setattr__ 一样，要避免"无限递归"的错误
+ \__getattribute__(self, name):
    + 定义了你的属性被访问时的行为，相比与只有该属性不存在时才会起作用的 __getattr__，在支持 __getattribute__ 的Python版本中，调用 \__getattr__ 前必定会调用 __getattribute__。
    + 如果在 __getattribute__(self, name) 方法下存在通过self.name 访问属性, 则会出现无限递归错误，
    + 所以和 __setattr__ 一样，要避免"无限递归"的错误


```python
# def __setattr__(self, name, value):
#     self.name = value
    
# def __setattr__(self, name, value):
#     self.__dict__[name] = value
    
class User(object):
    def __getattr__(self, name):
        print("调用了 __getattr__ 方法")
        return super(User, self).__getattr__(name)
    def __setattr__(self, name, value):
        print("调用了 __setattr__ 方法")
        return super(User, self).__setattr__(name, value)
    def __delattr__(self, name):
        print("调用了 __delattr__ 方法")
        return super(User, self).__delattr__(name)
    def __getattribute__(self, name):
        print("调用了 __getattribute__ 方法")
        return super(User, self).__getattribute__(name)
    
if __name__ == '__main__':
    user = User()
    # 设置属性值，会调用 __setattr__
    user.attr1 = True
    # 属性存在，只有 __getattribute__ 调用
    user.attr1
    try:
        # 属性不存在 先调用 __getattribute__，后调用 __getattr__
        user.attr2
    except AttributeError:
        pass
    # 调用 __delattr__
    del user.attr1
```

    调用了 __setattr__ 方法
    调用了 __getattribute__ 方法
    调用了 __getattribute__ 方法
    调用了 __getattr__ 方法
    调用了 __delattr__ 方法


##### 对象的描述器
+ 定义和简介
    + 一般地，一个描述器是一个包含"绑定行为"的对象,对其属性的访问被描述器协议中定义的方法覆盖。
    + 这些方法有: __get__(), __set__(), __delete__().
    + 如果某个对象中定义了这些方法中的任意一个，那么这个对象就可以被称为一个描述器
    + <font color=red>下例中 MyClass 是如何传入值的？</font>
+ 描述器协议
    + descr.__get__(self, obj, type=None) --> value
    + descr.__set__(self, obj, value) --> None
    + descr.__delete__(self, obj) -->None
    + 定义这些方法中的任何一个对象被视为描述器，并在被作为属性时覆盖其默认行为


```python
class User(object):
    def __init__(self, name='XiongJum', sex='男'):
        self.sex = sex
        self.name = name
    def __get__(self, obj, objtype):
        print('获取 name 的值')
        return self.name
    def __set__(self, obj, val):
        print('设置 name 的值')
        self.name = val
class MyClass(object):
    x = User('XiongJum', '男')
    y = 5
    
if __name__ == '__main__':
    m = MyClass()
    print(m.x, '\n')
    
    m.x = '酒明'
    print(m.x, '\n')
    
    print(m.x, '\n')
    
    print(m.y)
```

    获取 name 的值
    XiongJum 
    
    设置 name 的值
    获取 name 的值
    酒明 
    
    获取 name 的值
    酒明 
    
    5



```python
class User(object):
    def __init__(self, value=0.0):
        self.value = float(value) 
    def __get__(self, instance, owner):
        return self.value
    def __set__(self, instance, value):
        self.value = float(value)
class Dis(object):
    user = User()

if __name__ == '__main__':

    d = Dis()
    print(d.user)
```

##### 自定义容器
+ 常见的容器类型
    + 可变容器：dict，list
    + 不可变容器：tuple， string
+ 可变容器和不可变容器的区别
    + 不可变容器一旦赋值后，就无法对某个元素进行修改
    + 只能重新赋值或者重新覆盖全部的元素
+ 自定义容器
    + 自定义不可变容器类型： 需要定义 '\__len__' 和 '\__getitem__' 方法
    + 自定义可变类型容器： 在不可以容器的基础上，增加定义 '\__setitem__' 和 '\__delitem__'
    + 自定义的数据类型需要迭代：需要定义 '\__iter__'
    + 返回自定义容器的长度：需要实现 '\__len__'(self)
    + 自定义容器可以调用 self(key)，如果 key 类型错误，抛出 TypeError, 如果没法返回 key 对应的数字时，则应该抛出 ValueError：
      需要实现 '\__getitem__(self, key)'
    + 当执行 self\[key\] = value 时 ： 调用 '\__setitem__(self, key, value)' 方法
    + 当执行 del self\[key\] 方法：调用的方法时 '\__delitem__'(self, key)
    + 当你想你的容器可以执行 for x in container：或者使用 iter(container) 时：需实现 '\__iter__(self)',该方法返回的是一个迭代器


```python
class FunctionalList:
    '''实现内置类型list的功能，并丰富了一些其余的方法：head, init, last, drop, take'''
    def __init__(self, values=None):
        if values is None:
            self.values = []
        else:
            self.values = values
    
    def __len__(self):
        return len(self.values)
    
    def __getitem__(self, key):
        return self.values[key]
    
    def __setitem__(self, key, value):
        self.values[key] = value
    
    def __delitem__(self, key):
        del self.values[key]
    
    def __iter__(self):
        return iter(self.values)
    
    def __reversed__(self):
        return FunctionalList(reversed(self.values))
    
    def append(self, value):
        self.values.append(value)
    
    def head(self):
        # 获取第一个元素
        return self.values[0]
    
    def init(self):
        # 获取最后一个元素之外的所有元素
        return self.values[:-1]
    
    def last(self):
        # 获取最后一个元素
        return self.values[-1]
    
    def drop(self, n):
        # 获取第n个元素之后的元素
        return self.values[n:]
    
    def take(self, n):
        # 获取第n个元素之前的元素
        return self.values[:n]
if __name__ == '__main__':
    num = FunctionalList([0,2,4,5])
    print(len(num), num[0])
    print(num.head(), num.init(), num.last(), num.init(), num.drop(2), num.take(1))
```

    4 0
    0 [0, 2, 4] 5 [0, 2, 4] [4, 5] [0]


##### 运算符相关的魔术方法
+ 比较运算符
    + __ cmp __(self, other). 
    + __ eq __(self, other)：定义了比较操作符 == 的行为
    + __ ne __(self, other)：定义了比较操作符 != 的行为
    + __ it __(self, other)：定义了比较操作符 <  的行为
    + __ gt __(self, other)：定义了比较操作符 >  的行为
    + __ le __(self, other)：定义了比较操作符 <= 的行为
    + __ ge __(self, other)：定义了比较操作符 >= 的行为
+ 算数运算符
    + __ add __(self, other)：实现加法运算
    + __ sub __(self, other)：实现减法运算
    + __ mul __(self, other)：实现乘法运算
    + __ floordiv __(self, other)：实现//运算
    + __ div __(self, other)： python3中已废弃
    + __ truediv __(self, other)：
    + __ mod __(self, other)：实现取余运算
    + __ divmod __(self, other)：实现 divmod() 内建函数
    + __ pow __(self, other)：实现 **/'N' 次方操作
    + __ lshift __(self, other)：实现位/<'<'操作
    + __ rshift __(self, other)：实现位/'>'>操作
    + __ and __(self, other)：实现位/'&'操作
    + __ or __(self, other)：实现位/'`'操作
    + __ xor __(self, other)：实现位/'^'操作
    
    


```python
class Number(object):
    '''比较运算符相关的魔术方法'''
    
    def __init__(self, value):
        self.value = value
        
    def __eq__(self, other):
        print('__eq__')
        return self.value == other.value
    
    def __ne__(self, other):
        print('__ne__')
        return self.value != other.value
    
    def __it__(self, other):
        print('__it__')
        return self.value < other.value
    
    def __gt__(self, other):
        print('__gt__')
        return self.value > other.value
    
    def __le__(self, other):
        print('__le__')
        return self.value <= other.value
    
    def __ge__(self, other):
        print('__ge__')
        return self.value >= other.value
    
    '''算数运算符像概念的魔术方法'''
    def __add__(self, other):
        print('__add__')
        return self.value + other.value
    
if __name__ == '__main__':
    num1 = Number(2)
    num2 = Number(3)
    print('num1 == num2 ? --> {}\n'.format(num1 == num2))
    print('num1 != num2 ? --> {}\n'.format(num1 != num2))
    print('num1 <  num2 ? --> {}\n'.format(num1 < num2))
    print('num1 >  num2 ? --> {}\n'.format(num1 > num2))
    print('num1 <= num2 ? --> {}\n'.format(num1 <= num2))
    print('num1 >= num2 ? --> {}\n'.format(num1 >= num2))
    print('num1 + num2 ? --> {}\n'.format(num1 + num2))
    
```

    __eq__
    num1 == num2 ? --> False
    
    __ne__
    num1 != num2 ? --> True
    
    __gt__
    num1 <  num2 ? --> True
    
    __gt__
    num1 >  num2 ? --> False
    
    __le__
    num1 <= num2 ? --> True
    
    __ge__
    num1 >= num2 ? --> False
    
    __add__
    num1 + num2 ? --> 5



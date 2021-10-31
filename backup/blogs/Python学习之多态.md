---
title: 【浅析】python的多态
cover: https://cdn.jsdelivr.net/gh/xiongJum/Picture//img/77253241_p0.png
date: 2020-07-10 16:00:00
categories: 
- Python浅析
tag:
- python
- 多态
---

##### Ⅰ，理解

+   指对不同类型的变量进行操作，根据对象（或类）类型的不同而表现出不同的行为

##### Ⅱ，常用的多态

~~~python
>>> print(1+2)
3
>>> print('1'+'2')
'12'
~~~

+   不同的对象（或类）调用‘’+”，“+”会做出不同的响应

##### Ⅲ，对象所属的类之间有继承关系


```python
class User(object):
    def __init__(self, name):
        self.name = name
    def printUser(self):
        print('你好！' + self.name)
        
class UserVip(User):
    def printUser(self):
        print('你好！尊敬的会员用户' + self.name)
        
class UserGeneral(User):
    def printUser(self):
        print('你好！尊敬的用户' + self.name)
        
def printUserInfo(user):
    user.printUser()
    
if __name__ == "__main__":
    # 实例化User类
    user = User('121')
    printUserInfo(user)
    # 实例化Vip类
    userVip = UserVip('xiongJum')
    printUserInfo(userVip)
    # 实例化General类
    userGeneral = UserGeneral('酒明')
    printUserInfo(userGeneral)
    
        
```

```python
你好！121
你好！尊敬的会员用户xiongJum
你好！尊敬的用户酒明
```

##### Ⅳ，对象所属的类之间没有继承关系

+ 调用同一个函数，传入不同的参数，以达到不同的功能


```python
class Duck(object):
    def fly(self):
        print('鸭子沿着地面飞起来了')
class Swan(object):
    def fly(self):
        print('天鹅飞越了喜马拉雅山的最高峰')
class Plane(object):
    def fly(slef):
        print('space X 回收成功')
def fly(vehicle):
    vehicle.fly()
    
duck = Duck()
fly(duck)
swan = Swan()
fly(swan)
plane = Plane()
fly(plane)
```

```python
鸭子沿着地面飞起来了
天鹅飞越了喜马拉雅山的最高峰
space X 回收成功
```


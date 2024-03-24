---
title: 枚举类
date: 2020-07-15 21:00:00
tags: ["Python3", "编程基础"]
categories: ["编程"]
---
##### 枚举类的使用

+ 当需要大量定于变量时，可以使用枚举类(Enum)来实现这个功能
    + 当我们定义一个Class类型时，每个常量都是class里面的唯一实例
    + 方式： Enum(类名, (tuple参数))
    + 枚举类通过 __ members __ 方法遍历所有的成员
    + <font color=red>而且 Enum 的成员均为单例（Singleton），并且不可实例化，不可更改</font>
<!--more-->

```python
from enum import Enum

Month = Enum('MONTH',('Jan', 'Feb','Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'))

for name, member in Month.__members__.items():
    # member.value 自动赋给成员的 int 类型的常量， 默认从1开始
    print(name, '---------', member, '--------', member.value)
    
print('\n', Month.Jan, '\n', Month.Nov.value)
```

    Jan --------- MONTH.Jan -------- 1
    Feb --------- MONTH.Feb -------- 2
    Mar --------- MONTH.Mar -------- 3
    Apr --------- MONTH.Apr -------- 4
    May --------- MONTH.May -------- 5
    Jun --------- MONTH.Jun -------- 6
    Jul --------- MONTH.Jul -------- 7
    Aug --------- MONTH.Aug -------- 8
    Sep --------- MONTH.Sep -------- 9
    Oct --------- MONTH.Oct -------- 10
    Nov --------- MONTH.Nov -------- 11
    Dec --------- MONTH.Dec -------- 12
    
     MONTH.Jan 
     11


##### 自定义类型的枚举
+ 枚举模块定义了具有迭代和比较功能的枚举类型
+ 可以用来为值创建明确定义的符号，而不是使用具体的整数或者字符串
+ 在自定义类型的枚举中，可以使用装饰器@unique来检查有没有重复值


```python
from enum import Enum, unique

# @unqiue 装饰器可以帮助我们用来检查有没有重复值，可选
@unique
class Month(Enum):
    Jan = 1
    Feb = 'Ⅱ'
    Mar = '1'

if __name__ == '__main__':
    print(Month.Jan, '---', Month.Jan.name, '---', Month.Jan.value)
    
    for name, member in Month.__members__.items():
    # member.value 自动赋给成员的 int 类型的常量， 默认从1开始
        print(name, '---------', member, '--------', member.value)
    
    print(type(Month.Jan.value))
```

    Month.Jan --- Jan --- 1
    Jan --------- Month.Jan -------- 1
    Feb --------- Month.Feb -------- Ⅱ
    Mar --------- Month.Mar -------- 1
    <class 'int'>


##### 枚举的比较
+ 枚举成员不是有序的即
    + 只能通过标识和相等性进行比较
    + 如 == 和 is


```python
Twowater = Month.Jan
XiongJum = Month.Mar

print(Twowater == XiongJum, Twowater == Month.Jan)
print(Twowater is XiongJum, Twowater is Month.Jan)

try:
    print('\n'.join(' ' + s.name for s in sorted(Month)))
except TypeError as err:
    print(' Error: {}'.format(err))
```

    False True
    False True
     Error: '<' not supported between instances of 'Month' and 'Month'



```python
+ 可以使用 IntEnum 类进行枚举，进而支持比较功能
    + 但仅支持int类型
```


```python
from enum import IntEnum

class User(IntEnum):
    Xiongjum = 12
    Jumxiong = 18
    Onetwoone = 24

try:
    print('\n'.join(' ' + s.name for s in sorted(User)))
except TypeError as err:
    print(' Error: {}'.format(err))
```

     Xiongjum
     Jumxiong
     Onetwoone


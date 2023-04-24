---
title: 正则表达式
date: 2020-07-06
photos: [https://cdn.jsdelivr.net/gh/xiongJum/Picture/img/78405585_p0.png]
tags: 
  - 正则表达式
  - code
---

##### 初识正则表达式
+ 使用python的内置函数

```python
# 设置一个常量
name = 'XiongJum|酒明|JumXiong|初学者'

# 判断是否有字符串XiongJum
print('是否含有XiongJum这个字符串：{}'.format(name.index('XiongJum') < 1))
print('是否含有XiongJum这个字符串：{}'.format('XiongJum' in name))
```

    是否含有XiongJum这个字符串：True
    是否含有XiongJum这个字符串：True
<!-- more -->

+ 初识正则表达式re
    + Python内置函数能简单解决的事情，就没有必要使用正则表达式，如下例

```python
import re

findall = re.findall('Jum', name)
print(findall)

if len(findall) > 0:
    print('name含有Jum这个字符串')
else:
    print('name不含有Jum这个字符串')
```

    ['Jum', 'Jum']
    name含有Jum这个字符串



```python
# 选择 name 里面的所有小写英文字母

re_findall = re.findall('[a-z]', name)
print(re_findall)
```

    ['i', 'o', 'n', 'g', 'u', 'm', 'u', 'm', 'i', 'o', 'n', 'g']


##### 字符集
+ 字符集是由一对方括号"[]"括起来的字符集合，使用字符集合可以匹配多个字符中的一个
    + 如 C[ET]O,匹配到为CEO或CTO
    + [0-9a-fA-F]，匹配单个的十六进制数字
+ 字符和范围定义的先后循序对匹配的结果是没有任何影响的


```python
a = 'uav,ubv,ucv,ucv,uov,uzb'

# 取'u' 和'v'中间为‘a’或‘b’的字符
findbat = re.findall('u[ab]v', a)
print(findbat)

# 如果是连续的字符，可以使用-代替
# 取'u' 和'v'中间为‘a’至‘b’的字符
findall = re.findall('u[a-c]v', a)
print(findall)

# [^string]为取反字符集
# 取'u' 和'v'中间不为‘a’或‘b’的字符
findall = re.findall('u[^ab]', a)
print(findall)
```

    ['uav', 'ubv']
    ['uav', 'ubv', 'ucv', 'ucv']
    ['uc', 'uc', 'uo', 'uz']



```python
num = '171-2119-2606-A4-A5&A6'

# 概括字符集

# \d, 相当于[0-9]
# \D, 相当于[^0-9]
# \w, 相当于[A-Za-z0-9], 相当于匹配所有单词字符

notnum = re.findall('\D', num)
stringall = re.findall('\w', num)
num = re.findall('\d', num)

print(notnum, '\n', num, '\n', stringall)
```

    ['-', '-', '-', 'A', '-', 'A', '&', 'A'] 
     ['1', '7', '1', '2', '1', '1', '9', '2', '6', '0', '6', '4', '5', '6'] 
     ['1', '7', '1', '2', '1', '1', '9', '2', '6', '0', '6', 'A', '4', 'A', '5', 'A', '6']


##### 数量词
+ 词法：{min, max}, min和max 都是非负整数
+ \b[1-9]\[0-8]\{2,3}\b, 表示为 100-9888之间的数字, \b 表示的是边界


```python
stringnum = 'python@java@C#123@C++XiongJumm'
# 匹配4-7位的连续字母
findall =re.findall('[a-zA-Z]{4,7}', stringnum)
print(findall)

# 匹配 0-7位的连续字母
findall = findall =re.findall('[a-zA-Z]{,7}', stringnum)
print(findall)

# 匹配 2-正无穷的连续字母
findall = findall =re.findall('[a-zA-Z]{2,}', stringnum)
print(findall)

# 匹配连续两位的连续字母
findall = findall =re.findall('[a-zA-Z]{2}', stringnum)
print(findall)
```

    ['python', 'java', 'XiongJu']
    ['python', '', 'java', '', 'C', '', '', '', '', '', 'C', '', '', 'XiongJu', 'mm', '']
    ['python', 'java', 'XiongJumm']
    ['py', 'th', 'on', 'ja', 'va', 'Xi', 'on', 'gJ', 'um']


+ 贪婪模式
    + 一次性的读入整个字符串，如果不匹配就丢弃最右边的一个字符，如此循环，直到字符串的长度为0，如上例
+ 懒惰模式
    + 从字符串的最有左边开始，从0开始读取，若失败则多读一个字符，如此循环，直到读取完成，如下例


```python
# 须在后方加入一个“？”，相当于{4}
findall = re.findall('[a-zA-Z]{4,7}?', stringnum)
print(findall)
```

    ['pyth', 'java', 'Xion', 'gJum']


##### 边界匹配符和组

+ 一般边界匹配符有一下几种

| 语法 | 描述 |
|:---- |:---- |
|   ^  | 匹配字符串开头（在有多行的情况中匹配每行的开头） |
|   $  | 匹配字符串末尾（在有多行的情况下匹配每行的开头） |
|  \A  | 仅匹配字符串的开头                               |
|  \Z  | 仅匹配字符串的末尾                               |
|  \b  | 匹配\w和\W之间                                   |
|  \B  | \b                                               |

+ 分组
    + 被括号括起来的的表达式就是分组。
    + 分组表达式（...）其实就是把这部分字符作为一个整体
    + 可以有多分组的情况，每遇到一个分组，编号就会+1，而且分组后面也是可以加数量词的

##### 替换字符串
+ re.sub
    + 一些项目中我们需要替换字符串中的字符，这是后就可以使用函数 def sub()
    + 具体参数如下
    
|参数|描述|
|:----|:----|
|pattern|必选，表示正则中的模式字符串|
|repl   |必选，被替换的字符串|
|string |必选，表示要被处理，要被替换的那个string|
|count  |可选，对于pattern中部分匹配到的结果，count可以对前几个group进行替换|
|flags  |可选，正则表达式修饰符|


```python
a = 'Python$Android*Java-121'

# 将"*"或"$"替换为"&"
sub1 = re.sub('[\*$]','&',a)
print(sub1)

# 将字符串中第一个"&"替换成 "*"
sub2 = re.sub('\&', '*', sub1, 1)
print(sub2)

# 将字符串中 "*" 替换为"!", ""& 替换为"-"

# 定义一个函数

def convert(value):
    group = value.group()
    if (group == '*'):
        return '!'
    elif (group == '&'):
        return '-'

    
    
sub3 = re.sub('[\*&]', convert, sub2)
print(sub3)
```

    Python&Android&Java-121
    Python*Android&Java-121
    Python!Android-Java-121


##### re.match 和 re.search
+ re.match函数
    + 语法：re.match(pattern, string, flag=0)
    + re.match 尝试从字符串的起始位置匹配一个模式，如果不是其实位置匹配成功的话， match() 就返回None
+ re.search()函数
    + re.search(pattern, string, flag=0)
    + 扫描整个字符串并返回第一个成功的匹配
+ re.match() 和 re.search() 的具体描述如下

|参数|描述|
|:---|:---|
|pattern|匹配的正则表达式|
|string |要匹配的字符串|
|flags  |标志位，用于控制正则表达是的匹配方式，如:是否区别大小写|

|函数|区别|
|:---|:---|
|re.match() |只匹配字符串的开始。如果字符串开始不符合正则表达式，则匹配失败，函数返回None|
|re.search()|匹配整个字符串，直到找到一个匹配|


```python
img = '<img src="http://static.xiaobu.hk/cityact/application/images/20200612/2QCXwH6pej.JPG">'

# 使用re.search()
search = re.search('<img src="(.*)">', img)
# group(0)是一个完整的分组
print(search.group(0))
print(search.group(1))

# 使用re.findall()
search = re.findall('<img src="(.*)">', img)
# group(0)是一个完整的分组
print(search)

# 多个分组的使用
search = re.search('<(.*) src="(.*)">', img)

print("打印字符串 img:", search.group(1))

print("打印图片地址", search.group(2))

print("以元组的形式打印 img和图片地址:", search.group(1,2))

print("使用 groups:",search.groups())
```

    <img src="http://static.xiaobu.hk/cityact/application/images/20200612/2QCXwH6pej.JPG">
    http://static.xiaobu.hk/cityact/application/images/20200612/2QCXwH6pej.JPG
    ['http://static.xiaobu.hk/cityact/application/images/20200612/2QCXwH6pej.JPG']
    打印字符串 img: img
    打印图片地址 http://static.xiaobu.hk/cityact/application/images/20200612/2QCXwH6pej.JPG
    以元组的形式打印 img和图片地址: ('img', 'http://static.xiaobu.hk/cityact/application/images/20200612/2QCXwH6pej.JPG')
    使用 groups: ('img', 'http://static.xiaobu.hk/cityact/application/images/20200612/2QCXwH6pej.JPG')


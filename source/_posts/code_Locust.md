---
title: 性能测试 for Locust
date: 2020-05-28
updated: 2023-04-21 11:56:22
photos : https://share.lain.buzz/api/name/Locust.jpeg?path=/🖼%20Picture/图床/博客/Locust.jpeg
tags: 
  - 性能测试
  - Python
  - Locust
  - 测试
---

locast 是一種易於使用、可編寫脚本且可擴展的性能測試工具。

在常規 Python 代碼中定義用戶的行爲。

開始使用 [Local](https://locust.io/)
<!--more-->


#### 安装 Locust

安装 locast 和 geventhttpclient。
```
pip3 install locast, geventhttpclient

# 查看是否安装成功
locust -v
```

>若安装失败，[点击这排查问题](https://github.com/locustio/locust/wiki/Installation)

#### locust 快速入门

##### 编写执行文本

导入 locust.HttpUser 即可编写简单的 locust 文件。

```python
# mylocast.py

from locust import HttpUser, between

class updateVesion(Httpset):
  
    # 若在类中申明了host属性，则不需要再命令行中另行申明--host,若在命令行中另行申明了-host，则不再使用该host属性
    host = 'http://118.190.124.69:7080'
    @task
    def updateTest(self):
        self.client.port(url,data)
        self.client.get(url)
```

在命令行执行locust文件

```
# 执行 locust 文件
locust -f mylocut.py
# 执行文件中没有申明 host 或者覆盖执行文件中的host
locust -f mylocut.py --host=http://118.190.124.69:7080
```

使用 @task() [装饰器](https://docs.python.org/zh-cn/3/library/typing.html#functions-and-decorators) 传入参数，设置单个类中多个函数的执行效率；设置 weight [#变量](/tags/变量) 则可以设置多个 locust 的执行效率；数字越高则代表执行效率越高
若需要设置等待时间，则需要导入  between 或  constant 模块，设置等待区间或者具体的等待时间， 

```python
from locust import HttpUser, task, between, constant
import requests

"""设置多个 locust 类，变更设置执行效率""""
class updateVesion(HttpUser):
    wait_time = constant(1)
    weight = 3
    @task(1)
    def updateTest(self):
        
        self.client.post(url=url, json=data, headers=headers)

	@task(3)
    def login(self):
        self.client.post(url=login_url, json=data, headers=headers)
		
class loginTest(HttpUser):
    wait_time = between(1.3, 2.1)
    weight = 1
	
    def logout(self):
        headers = {"Content-Type": "application/json"}
        url = "/city-api/wx/logout?token=tangsx"
        data = {"USERNAME":"17134025282"}
     	self.client.post(url=url, json=data, headers=headers)
```

一般浏览网页时会一级级的向下点击，为模拟用户的真实操作，可以使用TaskSet 进行嵌套处理

```python
from locust import HttpUser, task, between, TaskSet
import requests

class logIn(TaskSet):
    
    @task
    def login(self):
        self.client.post(url=url,json=data, headers=headers)
		
class update(TaskSet):
    tasks = [logIn]
    
    @task
    def updateTest(self):
        self.client.post(url=url, json=data, headers=headers)
        
class runLog(HttpUser):
    # tasks是python的可调用函数，他们接受一个参数——正在执行任务的TaskSet类实例
    
    tasks = [logOut]
    wait_time = between(1,2)
```

两个Locust类并行，并设置执行比例

```python

class logIn(TaskSet):
...
...
...
        
class runLog(HttpUser):
    # tasks是python的可调用函数，他们接受一个参数——正在执行任务的TaskSet类实例
    
    tasks = {logIn:3, update: 1}
    wait_time = between(1,2)
```

设置 Locust 的 csv 文件写入速度

```python
import locust.stats
locust.stats.CSV_STATS_INTERVAL_SEC = 5 # 默认为2s
```

##### 命令行命令

设置无界面运行

```shell
locust -f [文件名] --host[网址] --headless [无Web_UI] -u [模拟用户数] -r [用户孵化数] -t [运行时间]
# 设置无界面运行的关键参数
##  --headless
# 若执行文件没有写明，以下为必填参数
## -r 用户孵化数，模拟的真实的用户
## -u 模拟用户数
# 选填参数
## -t 设置运行时间，单位时(h)、分(m)、秒(s), 和 --headless 进行关联
## --csv=[文件名]  以 csv 格式储存当前测试数据
## -L 级别为[DEBUG/INFO/WARNING/ERROR/CRITICAL], 默认为INFO
## --logfile=[路径]  log日志的储存路径，若没有设置则在stdout/stderr

```


多机器分布测试

主机和分机需要在同一个局域网下，或者设置的ip为外网；主机只显示测试数据，不执行性能测试，且主机和分机的执行文件需要一致

主机的执行的命令行

```shell
Locust -f [文件名] --master
# 可选参数
# --master-bind-host = [IP] 将主机绑定到特定的网络上，若不填写则为本机默认的ipv4网络
# --master-bind-prot = [port] 设置主机的监听端口，默认为5557， 会占用两个端口，为指定端口+1和指定端口
# --expect-workers = [数量] 等待X个分机连接后，进行测试（命令行模式下使用）
```

 分机执行的命令行

```shell
locust -f [文件名] --worker
# --master-host = [ip] 值和主机设置的值一致；必填
# --master-port = [port] 值和主机设置的值一致；若主机没有特殊指定，则分机不需要特殊设置
```

#### 提高Http请求性能

通过FastHttpLocust提高Locust的Http请求性能

>1. 通常情况下我们只需要使用 requests 来实现HTTP请求，若执行脚本时花费了大量的CPU时间，可以切换到 FastHttpLocust
>2. FastHttpLocust 无法完全替代 HttpLocust。

示例代码

```python
from locust import TaskSet, task, between
from locust.contrib.fasthttp import FastHttpLocust

class MyTaskSet(TaskSet):
    @task
    def index(self):
        response = self.client.get("/")
    
class MyLocust(FastHttpLocust):
    tasks = [MyTaskSet]
    wait_time = between(1, 2)
```

---
#### 参考

[1] [阿西河博客](https://www.axihe.com/tools/locust/home.html)
[2] [Locust IO文档](https://docs.locust.io/)


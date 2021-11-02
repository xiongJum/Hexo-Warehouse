---
title: Locust
date: 2020-05-28 09:00:00
updated: 2021-11-02
photos : [https://cdn.jsdelivr.net/gh/xiongJum/Picture/img/78405585_p0.png]
tags: 
  - 并發
  - 性能測試
  - Http
categories: 
  - 測試
  - 性能測試
---

locast 是一種易於使用、可編寫脚本且可擴展的性能測試工具。

在常規 Python 代碼中定義用戶的行爲。

開始使用 [Local](https://docs.locust.io/en/stable/installation.html#installation)


<!--more-->
# 1. 安裝

1, 安裝Locast
```
pip3 install locast
```
2, 驗證安裝是否成功, 若安裝失敗，請查看[Wiki](https://github.com/locustio/locust/wiki/Installation)

```
locust -V
```


# 2. locust快速入门

## 2.1 简单的locust的执行文件

1, 編寫簡單的執行文檔

```python
# mylocast.py

from locust import HttpUser, task, between

class updateVesion(Httpset):
    # 模拟每个用户在每个人执行后等待1~2s的等待时间
    wait_time =between(1,2) 
  
    # 若在类中申明了host属性，则不需要再命令行中另行申明--host,若在命令行中另行申明了-host，则不再使用该host属性
    host = 'http://118.190.124.69:7080'
    @task
    def updateTest(self):
        self.client.port(url,data)
        self.client.get(url)
```
2, 在命令行执行locust文件

```
locust -f mylocut.py
```

ps: 若执行文件没有申明host或想覆盖执行文件中的host

```
locust -f mylocut.py --host=http://118.190.124.69:7080
```

## 2.2 设置执行效率

```python
from locust import HttpUser, task, between, constant
import requests

class updateVesion(HttpUser):
    
    # 模拟用户在执行完操作后等待1s的时间
    wait_time = constant(1)
    # 执行多个locust类时，可设置weight,数字越高代表执行率越高，3是1的三倍效率
    weight = 3
    @task(1)
    def updateTest(self):
        # 常用的移动端头部信息
        headers = {"Content-Type": "application/json"}
        url = "/city-api/system/app_version/check/v2?token=tangsx"
        data = {
                "MOBILE_MODEL":1,
                "VERSION":"1.1.1",
                "OAUTH_NONCE":"761e9187-8877-4cd3-952e-08ae1b78bc41"
                }
        self.client.post(url=url, json=data, headers=headers)
	# task(number),模拟用户的执行效率，如login是updateVesion的3倍执行效率
    @task(3)
    def login(self):
        login_url = "/city-api/user/login?token=tangsx"
        headers = {"Content-Type": "application/json"}
        login_data = {"USERNAME":"17134025282",
                    "PASSWORD_MD5":"cffbad68bb97a6c3f943538f119c992c"}
        self.client.post(url=login_url,
                json=login_data, headers=headers)
class loginTest(HttpUser):
    
    wait_time = between(1.3, 2.1)
    weight = 1
    @task
    def logout(self):
        headers = {"Content-Type": "application/json"}
        logout_url = "/city-api/wx/logout?token=tangsx"
        logout_data = {"USERNAME":"17134025282"}
     	self.client.post(url=logout_url, json=logout_data, headers=headers)
```

## 2.3 代码优化(嵌套)

1, 一般浏览网页时会一级级的向下点击，为模拟用户的真实操作，可以使用TaskSet进行嵌套处理

```python
from locust import HttpUser, task, between, TaskSet
import requests

class logIn(TaskSet):
    
    @task
    def login(self):
        headers = {"Content-Type": "application/json"}
        login_url = "/city-api/user/login?token=tangsx"
        login_data = {"USERNAME":"17134025282",
                      "PASSWORD_MD5":"cffbad68bb97a6c3f943538f119c992c"}
        self.client.post(url=login_url,
                json=login_data, headers=headers)
class update(TaskSet):
    tasks = [logIn]
    
    @task
    def updateTest(self):
        headers = {"Content-Type": "application/json"}
        url = "/city-api/system/app_version/check/v2?token=tangsx"
        data = {
                "MOBILE_MODEL":1,
                "VERSION":"1.1.1",
                "OAUTH_NONCE":"761e9187-8877-4cd3-952e-08ae1b78bc41"
                }
        self.client.post(url=url, json=data, headers=headers)
        
class runLog(HttpUser):
    # tasks是python的可调用函数，他们接受一个参数——正在执行任务的TaskSet类实例
    
    tasks = [logOut]
    wait_time = between(1,2)
```

## 2.4 两个Locust类并行，并设置测试比例

```python
from locust import HttpUser, task, between, TaskSet
import requests

class logIn(TaskSet):
    
    @task
    def login(self):
        headers = {"Content-Type": "application/json"}
        login_url = "/city-api/user/login?token=tangsx"
        login_data = {"USERNAME":"17134025282",
                      "PASSWORD_MD5":"cffbad68bb97a6c3f943538f119c992c"}
        self.client.post(url=login_url,
                json=login_data, headers=headers)
class update(TaskSet):
    
    @task
    def updateTest(self):
        headers = {"Content-Type": "application/json"}
        url = "/city-api/system/app_version/check/v2?token=tangsx"
        data = {
                "MOBILE_MODEL":1,
                "VERSION":"1.1.1",
                "OAUTH_NONCE":"761e9187-8877-4cd3-952e-08ae1b78bc41"
                }
        self.client.post(url=url, json=data, headers=headers)
        
class runLog(HttpUser):
    # tasks是python的可调用函数，他们接受一个参数——正在执行任务的TaskSet类实例
    
    tasks = {logIn:3, update: 1}
    wait_time = between(1,2)
```

# 3. 命令行

## 3.1 无web_UI 运行Locust

```shell
locust -f [文件名] --host[网址] --headless [无Web_UI] -u [模拟用户数] -r [用户孵化数] -t [运行时间]
"""若执行文件写明host、模拟用户数以及用户孵化数，这该参数为选填项"""
# -r [用户孵化数]：真实模拟的用户， 无web_ui下必填参数
# -t [运行时间] 单位时(h)、分(m)、秒(s), 选择项，若需要定时关闭可以设置该参数，该参数需要关联[--headless]
# --headless 无Web_UI下的决定参数
# -u 非web_UI 下必填参数
## 选填项
# --csv=[文件名]， 以csv格式储存当前测试数据
# -L [log日志]， 级别为[DEBUG/INFO/WARNING/ERROR/CRITICAL], 默认为INFO
# --logfile=[文件名] log日志的储存路径，若没有设置则在stdout/stderr

```

## 3.2 自定义csv的写入速度

```python
import locust.stats
locust.stats.CSV_STATS_INTERVAL_SEC = 5 # 默认为2s
```



## 多机器分布测试

1, 主机和分机需要在同一个局域网下，或者设置的ip为外网
2, 主机只显示测试数据，不执行性能测试，且主机和分机的执行文件需要一致
3, 主机的执行的命令行

```shell
Locust -f [文件名] --master
# 可选参数
# --master-bind-host = [IP] 将主机绑定到特定的网络上，若不填写则为本机默认的ipv4网络
# --master-bind-prot = [port] 设置主机的监听端口，默认为5557， 会占用两个端口，为指定端口+1和指定端口
# --expect-workers = [数量] 等待X个分机连接后，进行测试（命令行模式下使用）
```

4, 分机执行的命令行

```shell
locust -f [文件名] --worker
# --master-host = [ip] 值和主机设置的值一致；必填
# --master-port = [port] 值和主机设置的值一致；若主机没有特殊指定，则分机不需要特殊设置
```

# 4. 提高Http请求性能

## 4.1 通过FastHttpLocust提高Locust的Http请求性能

1, 通常情况下我们只需要使用requests来实现HTTP请求，若执行脚本时花费了大量的CPU时间，可以切换到FastHttpLocust

2, 安装 geventhttplocust 的python包
```
pip3 install geventhttpclient
```

3, 或 使用清华源 加快下载速度
```
pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple geventhttpclient
```

## 示例代码

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

+   注意：FastHttpLocust可能无法完全替代HttpLocust。

---
# 5，参考文件

1, [阿西河博客](https://www.axihe.com/tools/locust/home.html)
2, [Locust IO文档](https://docs.locust.io/)


---
title : 接口自动化测试
date: 2021-12-26
updated : 2023-04-21 13:06:54
photos: 
tags: 
- 接口自动化测试
- 自动化测试
- Python
- requests
- unittest
---

基于 requests 库 和 unittest 库，编写的脚本 demo，还存在不成熟的地方。

<!--more-->


# 准备

准备库 [requests](https://docs.python-requests.org/zh_CN/latest/) 和 [unittest](https://docs.python.org/zh-cn/3/library/unittest.html)

```python
pip install requests
```

# 编写测试脚本

## 封装公共参数

封装公共参数，重复调取结果，减少代码的编写量，降低理解难度。

设置接口的必填参数，并使用 *args 传入接口的非必填参数；使用 \**kwargs 设置一些条件。
可以将一些公共参数写入到单独的文件中，使用时引用即可；一些隐私的数据可以写入到[#环境变量中](/tags/环境变量)中，如测试 api 或者测试账号。

```python
import requests
import unittest

class RequestTest(unittest.TestCase):
	
 	# 封装公共参数
	 def common(self, name, password, *args, **kwargs):
		url = f"{config.ENVIRONMENT}/auth/login"
		param = {"name":name, "password":password,}
		
		if args: # 如果 args 不为空 则将参数传入添加到请求参数中
			 for arg in args:
				 param.update(**arg)
		self.response = requests.post(url, headers=self.headers, json=param, 
									  verify=False,)

		 # 如果 kwargs 不为 False 则进行相应的操作
		if kwargs:
			for key, value in kwargs.items():
				if key == 'del_' and value : # 如果 key 为 del_ 则删除本次测试所产生的数据
					self.db.commit_data(f"delete from mp_org where phone='{phone}'")

		return self.response

if __name__ == '__main__':
	unittest.main(verbosity=2)
```

调取公共参数，请求项目的外部接口获取请求结果, 并根据结果进行断言

```python

class RequestTest(unittest.TestCase):
	
	... skip
	
	def test_login(self):
		self.common(name='admin', password='passwd')
		
		# 断言
		self.assertEqual(self.response.status_code, 200)
 		self.assertEqual(self.response.json()['code'], 0, msg='返回的状态码不为0')
		
if __name__ == '__main__':
	unittest.main(verbosity=2)
```

## 编写装饰器

将相同的断言放入 [#装饰器](https://docs.python.org/zh-cn/3/library/typing.html#functions-and-decorators)中，以供重复使用

```python
# decorators.py

def lsgot_success(f):

	@wraps(f)
	def decorator(self, *args, **kwargs):
		f(self, *args, **kwargs)
		responseJSON = self.response.json()
		print(responseJSON)
		self.assertEqual(self.response.status_code, 200,)
		self.assertEqual(responseJSON['code'], 0, msg='返回的状态码不为0')
		return f(self, *args, **kwargs)
	return decorator
```

修改测试方法

```python
from decorators import lsgot_success

class RequestTest(unittest.TestCase):
	
	... skip
	
	@lsgot_success
	def test_login(self):
		self.common(name='admin', password='passwd')
		
if __name__ == '__main__':
	unittest.main(verbosity=2)
```

若接口需要 cookie 之类的凭证，可以编写单独的方法，在运行测试之前写入到 cookie 或者 headers 中。

```python
def login():

	response = requests.post(url=url, headers=config.CONFING.LOGIN_HEADERS, 
							 data=param, verify=False)
	
	# 获取登录结果返回的响应， 并将 Authorization 写入到 headers 中。
	headers = {
		"Authorization": f"Bearer {response.json()['access_token']}",
		"Content-Type": "application/json;charset=UTF-8"
		}
	return headers

class RequestTest(unittest.TestCase):
	
	def setUp(self):
		self.headers = login()

... skip

if __name__ == '__main__':
	
	unittest.main(verbosity=2)
```

如果在测试过程中需要访问数据库，访问这篇文章[在 Python 中访问 SQL]()

谢谢

# 参考

[1] 由于链接消失，暂时无法展示，以后找到后补上


---
title: web 端自動化測試
# photos: https://cdn.jsdelivr.net/gh/xiongJum/Picture/img/77247188_p0.jpg
date: 2020-06-29 09:00:00
categories:
  - 測試
  - 自動化測試
tag: 
- python
- selenium
- web 自動化測試
---


Selenium 是支持 web 浏览器自动化的一系列工具和库的综合性项目，它提供了扩展来模拟用户与浏览器的交互，用于扩展浏览器分配的分发服务器， 以及用于实现 [W3C WebDriver](https://www.w3.org/TR/webdriver/) 规范的基础结构， 该规范允许您为所有主要 Web 浏览器编写可互换的代码.

Selenium 汇集了浏览器供应商，工程师和爱好者，以进一步围绕 Web 平台自动化进行公开讨论。 该项目组织了一次年度会议，以教学和培养社区.

Selenium 的核心是 [WebDriver](https://www.selenium.dev/zh-cn/documentation/webdriver/)，这是一个编写指令集的接口，可以在许多浏览器中互换运行

<!--more-->


# 1. 准备

1, 下载selenium库

```shell
pip3 install selenium
```
ps 若下载时间过慢，可以跟换国内pip源进行下载

2, 下载浏览器驱动，下面用例以火狐浏览器举例

将[chrome](http://chromedriver.chromium.org/downloads)驱动放在目录 [python的安装目录]\Python\Python(版本号)下，或者放在执行和执行文件同路径下

# 2. 編寫脚本文件

## 2.1 编写基礎的web自动化脚本

1, 編寫执行脚本文件

```python
from selenium import webdriver
import unittest

class seleniumTest(unittest.TestCase):
    def setUp(self):
        # 添加驱动，设置隐形等待时间、最大化浏览器窗口、输入测试地址
        self.driver = webdriver.Chrome()	
        self.driver.implicitly_wait(10)
        self.driver.maximize_window()
        self.driver.get("http://118.190.124.69:8080/bts-2.4.3/")
    def test_login(self):
        # 查找账号、密码输入框，并输入内容
        self.driver.find_element_by_id("username").send_keys('xiongceshi')
        self.driver.find_element_by_id("password").send_keys("xiongceshi121")
        # 查找并点击登录按钮
        self.driver.find_element_by_id("login_submit").click()
	def tearDown(self):
        # 关闭浏览器并退出脚本
        self.driver.quit()

if __name__ == "__main__":
    # 运行全部以test_开头的方法，vrrbosity为打印等级，默认为2，exit为退出条件，默认为True
    unittest.main(verbosity=2, exit = False)
```

2, 执行脚本文件

```shell
test_login (__main__.seleniumTest) ... ok
-----------------------------------------------------------------------------------------
Ran 1 test in 12.749s

OK
```

## 2.2 增加断言，判断是否符合测试要求

```python
from selenium import webdriver
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By
import unittest

class seleniumTest(unittest.TestCase):

    @classmethod
    def setUp(cls):
        cls.driver = webdriver.Firefox()	
        cls.driver.implicitly_wait(10)
        cls.driver.maximize_window()
        cls.driver.get("http://118.190.124.69:8080/bts-2.4.3/")
    def test_a_login(self):
        """正确的账号密码进行登录"""
        -------skip  
    def test_b_login(self):
        """错误的账号密码进行登录，并判断提示文案是否正确"""       
        self.driver.find_element_by_id("username").send_keys('错误账号')
        self.driver.find_element_by_id("password").send_keys("错误密码")
        self.driver.find_element_by_id("login_submit").click()
        
        """查找错误提示文案，并判断文案是否错误"""
        # 设置显式等待时间，强制等待2秒查找元素
        errorMsg = WebDriverWait(self.driver, 2) .until \
        	(EC.visibility_of_element_located((By.ID,"errorMsg"))).text
        errorText = '账户或密码错误，请重新输入呀！'
        # 断言，比较两者的的字符串是否一致，若一致则为True，不一致则为Flase并抛出错误
        self.assertEqual(errorMsg, errorText, '提示文案错误')
    @classmethod
    def tearDown(cls):
        cls.driver.quit()

if __name__ == "__main__":
    unittest.main(verbosity=2, exit = False)
```

执行测试结果

```shell
test_a_login (__main__.seleniumTest)
正确的账号密码进行登录 ... ok
test_b_login (__main__.seleniumTest)
错误的账号密码进行登录，并判断提示文案是否正确 ... FAIL

======================================================================
FAIL: test_b_login (__main__.seleniumTest)
错误的账号密码进行登录，并判断提示文案是否正确
----------------------------------------------------------------------
Traceback (most recent call last):
  File "c:\Users\jum\Desktop\临时文件夹\changzhou\changzhou_run\testEdit.py", line 27, in test_b_login
    self.assertEqual(errorMsg, errorText, '提示文案错误')
AssertionError: '账户或密码错误，请重新输入' != '账户或密码错误，请重新输入呀！'
+ 账户或密码错误，请重新输入！ : 提示文案错误

----------------------------------------------------------------------
Ran 2 tests in 41.063s

FAILED (failures=1)
```

## 2.3 增加功能，截取产生有bug的界面

```python
--------------------skip---------------------------------------
        # 判断文案是否错误，并截取用例执行错误时的界面
        errorText = '账户或密码错误，请重新输入！'     
        
        # 错误代码块，当try为True时，跳过except，当try为Flase时，执行except
        try:
            self.assertEqual(errorMsg, errorText)
        except:
            # 截取当前界面，字符串中的内容为保存路径
            self.driver.save_screenshot('./test.png')
            # 断言，无条件的创造失败，字符串为错误说明
            self.fail("登录失败时，提示文案错误")

    @classmethod
    def tearDown(cls):
        cls.driver.quit()

if __name__ == "__main__":
    unittest.main(verbosity=2, exit = False)
```

# 3. 优化代码结构，产生测试报告

1, 文件1，储存配置以及通用方法
```python
# seleniumconfig.py
import unittest
from selenium import webdriver
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

class seleniumConfig(unittest.TestCase):
	"""储存重复的方法，以及通用配置"""
    @classmethod
    def setUp(cls):
        cls.driver = webdriver.Firefox()	
        cls.driver.implicitly_wait(10)
        cls.driver.maximize_window()
        cls.driver.get("http://118.190.124.69:8080/bts-2.4.3/")
    @classmethod
    def tearDown(cls):
        # 关闭浏览器并退出脚本
        cls.driver.quit()  
    def find_element_try(self, real, demand, explana):
        """判断实际内容和需求是否一致，若不一致则截取图片，并返回错误说明"""
		try:
            self.assertEqual(real, demand)
		except:
            self.fail(explana)
            self.driver.save_screenshot('./test.png')   
    def find_element_wait(self, wait_time, type_element, element):
        """检查元素是否存在，若不存在则截取图片并返回错误说明"""
        try:
            return WebDriverWait(self.driver, wait_time).until \
                (EC.visibility_of_element_located((type_element,element)))
        except:
            self.fail("{}查找失败，请检查元素是否正确或产品出现缺陷".format(element))
```

2, 设计测试流程、设计测试用例

```python
# seleniumtest.py
from seleniumconfig import seleniumConfig
from selenium.webdriver.common.by import By

class seleniumTest(seleniumConfig):
	"""设计118环境web端登录的测试用例"""
    def test_a_login(self):
        """正确的账号密码进行登录"""
        -------skip  
    def test_b_login(self):
        """错误的账号密码进行登录，并判断提示文案是否正确"""
        self.driver.find_element_by_id("username").send_keys('错误账号')
        self.driver.find_element_by_id("password").send_keys("错误密码")
        self.driver.find_element_by_id("login_submit").click()
        # 获取错误提示文案
        errorMsg = self.find_element_wait(2,By.ID,"errorMsg").text
        errorText = '账户或密码错误，请重新输入！'
        explana = '登录失败时，提示文案错误'      
       # 判断错误提示文案和需求是否一致
        self.find_element_try(errorMsg, errorText, explana)
```

# 4. 生成测试报告

```python
# seleniumrun.py
import unittest
import HTMLTestRunner
import os
import time
from seleniumtest import seleniumTest

# 获取测试用例，创建测试套件(若有多份测试用例，可以创建多份测试套件)
web_login = unittest.TestLoader().loadTestsFromTestCase(seleniumTest)
kts_login = unittest.TestSuite([web_login])
"""
# 若有多份测试用例时
import seleniumtestone import seleniumTestOne
import seleniumtestone import seleniumTestTwo
web_logout = unittest.TestLoader().loadTestsFromTestCase(seleniumTestOne)
web_log = unittest.TestLoader().loadTestsFromTestCase(seleniumTestTwo)
kts_login = unittest.TestSuite([web_login, web_logout, web_log])
"""

#设置报告的写入路径和名称
dir = os.getcwd()
nowTime = time.strftime('%Y%m%d%H%M%S',time.localtime(time.time()))
outPath = './outFile/webTestReport{}.html'.format(nowTime)
outFile = open(dir + outPath, 'wb')

# 创建测试内容，执行测试套件
runner = HTMLTestRunner.HTMLTestRunner(
    stream= outFile,
    title= '测试报告',
    description= 'WEB支撑平台测试_UI自动化回归测试',
    verbosity=2)

runner.run(kts_login)
# 不生成测试报告
# unittest.TextTestRunner(verbosity=2).run(kts_login)
```

执行测试用例，获取测试报告

~~~shell
py seleniumrun.py
# ps如果执行失败请检查缩进是否正常，以及文件是否创建
~~~


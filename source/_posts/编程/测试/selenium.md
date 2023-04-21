---
title: 自动化测试 for selenium
photos: https://share.lain.buzz/api/name/selenium.jpeg?path=/🖼%20Picture/图床/博客/selenium.jpeg
date: 2020-06-29 09:00:00
updated: 2023-04-21 11:58:06
categories: 
- [测试, web自动化测试]
tag: 
- Python
- selenium
- web
- 自动化测试
---


Selenium 是支持 web 浏览器自动化的一系列工具和库的综合性项目，它提供了扩展来模拟用户与浏览器的交互，用于扩展浏览器分配的分发服务器， 以及用于实现 [W3C WebDriver](https://www.w3.org/TR/webdriver/) 规范的基础结构， 该规范允许您为所有主要 Web 浏览器编写可互换的代码.

Selenium 汇集了浏览器供应商，工程师和爱好者，以进一步围绕 Web 平台自动化进行公开讨论。 该项目组织了一次年度会议，以教学和培养社区.

Selenium 的核心是 [WebDriver](https://www.selenium.dev/zh-cn/documentation/webdriver/)，这是一个编写指令集的接口，可以在许多浏览器中互换运行

<!--more-->

# 准备所需文件

1. 下载selenium库,
2. 下载浏览器驱动，并将浏览器驱动放入项目的根目录下，并在执行文件中设置路径地址；或者将其所在目录加入到环境变量中；也可以直接在脚本中安装驱动

```shell
pip3 install selenium
```

chrome http://chromedriver.chromium.org/downloads


# 开始编写测试脚本

## 基础的 selenium 的脚本文件

编写执行脚本文件

通过使用方法 driver.implicitly_wait(10) 可以设置隐式的等待时间，但是不建议；设置窗口最大化，可以调用方法 driver.maximize_window() 

```python
from selenium import webdriver
	
# 添加浏览器驱动，并设置测试地址
driver = webdriver.Chrome()
driver.get("http://blog.xcumin.top")
# 通过 id 定位测试元素控件，并在控件中输入测试文本
driver.find_element_by_id("username").send_keys('xiongceshi')
# 退出浏览器
driver.quit()

```

可以通过python自带的测试库 [unittest](https://docs.python.org/zh-cn/3/library/unittest.html) 调整结构，并引用测试断言

```python
import unittest
from selenium import webdriver

class SeleniumTest(unittest.TestCase):
def setUp(self): # 测试前置方法，在所有的方法之前进行运行
	self.driver = webdriver.Chrome()
	self.driver.implicitly_wait(10)
	self.driver.get('http://test.lsgot.com:8181/#/login')

def tearDown(self): # 测试后置方法，在所有测试方法之后运行
	self.driver.quit()

def test_login(self):
	"""登录软件"""
	self.driver.find_element(By=id('loginUsername')).send_keys('WWGS')
	self.driver.find_element_by_id('loginPassword').send_keys('123456')
	self.driver.find_element_by_id('loginButton').click()
	
if __name__ == "__main__":
    unittest.main()
```

## 增加断言，测试输出是否符合条件

使用 unittest.TestCase 模块，判断结果是否符合预期，并在命令行中显示测试结果

```python
from selenium import webdriver
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By
import unittest

class seleniumTest(unittest.TestCase):
		
    def testlogin(self):
        """错误的账号密码进行登录，并判断提示文案是否正确"""       
		... skip
        
        # 设置显式等待时间，强制等待2秒查找元素
        errorMsg = WebDriverWait(self.driver, 2) .until \
        	(EC.visibility_of_element_located((By.ID,"errorMsg"))).text
        errorText = '账户或密码错误，请重新输入呀！'
        # 断言，比较两者的的字符串是否一致，若一致则为True，不一致则为Flase并抛出错误
        self.assertEqual(errorMsg, errorText, '提示文案错误')

if __name__ == "__main__":
    unittest.main()
```

## 增加功能，截取产生有bug的界面

使用 save_screenshot() 截取错误页面，并保存到设置的路径中

```python
class seleniumTest(unittest.TestCase):
		··· skip  
        
        # 错误代码块，当try为True时，跳过except，当try为Flase时，执行except
        try:
            self.assertEqual(errorMsg, errorText)
        except:
            # 截取当前界面，字符串中的内容为保存路径
            self.driver.save_screenshot('./test.png')
            # 断言，无条件的创造失败，字符串为错误说明
            self.fail("登录失败时，提示文案错误")

if __name__ == "__main__":
    unittest.main()
```

# 优化代码结构，产生测试报告

## 储存配置以及通用方法

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
		... skip

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
            self.fail(f"{element}查找失败，请检查元素是否正确或产品出现缺陷") #格式化字符串
```

## 设计测试流程、设计测试用例

```python
# seleniumtest.py
from seleniumconfig import seleniumConfig
from selenium.webdriver.common.by import By

class seleniumTest(seleniumConfig):

    def test_b_login(self):
        """错误的账号密码进行登录，并判断提示文案是否正确"""
		... skip
		
        errorMsg = self.find_element_wait(2,By.ID,"errorMsg").text
     
       # 判断错误提示文案和需求是否一致
        self.find_element_try(errorMsg, '账户或密码错误，请重新输入！', 
							  '登录失败时，提示文案错误')
		
```

## 生成测试报告

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

#设置报告的写入路径和名称
dir = os.getcwd()
nowTime = time.strftime('%Y%m%d%H%M%S',time.localtime(time.time()))
outPath = f'./outFile/webTestReport{nowTime}.html'
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

执行 seleniumrun.py ，获取测试报告

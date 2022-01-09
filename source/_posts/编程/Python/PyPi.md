---
title : PyPi
date: 2021-12-25
updated : 2021-12-25 21:14
categories: 
- [Python]
tags:
- pip
- Python
---
#### 国内源
+ 豆瓣:：https://pypi.douban.com/simple/
+ 清华：https://pypi.tuna.tsinghua.edu.cn/simple

##### 临时使用
```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple some-package
```

##### 设置为默认值

```
# 更新 pip
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pip -U

# 更换 pip 源
pip config set https://pypi.tuna.tsinghua.edu.cn/simple
```

##### 参考

[1] [清华大学开源软件镜像站](https://mirror.tuna.tsinghua.edu.cn/help/pypi/)
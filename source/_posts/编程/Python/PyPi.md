---
title : pip
date: 2021-12-25
updated : 2022-01-22
categories: 
- [Python]
tags:
- pip
- Python
---

由于某些发行版（如 pacman）会去除 pip 包管理器，则需要以下方法进行 pip的安装。
国内的下载速度过于缓慢，所以需要将下载源更换为国内源，来提升pip的下载速度，这里首先推荐豆瓣源。

安装详情请见 [pip 文档](https://pip.pypa.io/en/stable/installation/)

<!--more-->

# 1. 安装pip
从以下途径安装 python，则会自动安装 pip。
+ 在虚拟环境中工作
+ 从 (python.org)[https://www.python.org/] 中下载的 Python

否则使用以下方法进行 pip 的安装

## 1.1 ensurepip

```bash
# Linux
python -m ensurepip --upgrade
# Windows
py -m ensurepip --upgrade
```
## 1.2 get-pip.py
从 https://bootstrap.pypa.io/get-pip.py 下载脚本。
若从linux 的命令行模式下下载，则需执行以下语句

```bash
wget https://bootstrap.pypa.io/get-pip.py
```
请参阅 [ensurepip](https://docs.python.org/3/library/ensurepip.html#module-ensurepip) 获取更详细的信息。

### Linux 下安装
```bash
# (**如果需要全局安装，请执行下一条语句**)安装到当前用户下
python get-pip.py
# 全局安装pip
sudo python get-pip.py
```
若不是全局安装 pip ，则需要将 pip 添加到环境变量中
```vi
# ~/.bashrc 或 ~/.zshrc
export PATH="$HOME/.local/:$PATH"
```
重新进入环境变量
```bash
source ~/.bashrc
# 或
source ~/.zshrc
```

### Windows 下安装

```cmd
py get-pip.py
```
更多请参考文档 [pypa/get-pip](https://github.com/pypa/get-pip)
> 此链接为 github，若需要良好的访问体验，请魔法上网或者其余方式

# 2. 更新 pip

Linux
```bash
python -m pip install --upgrade pip
```

Windows
```
py -m pip install --upgrade pip
```
若需要下载速度过慢可参考下文，更换为国内源


# 3. 更换 pip 的下载源

## 3.1 国内源
+ 豆瓣:：https://pypi.douban.com/simple/
+ 清华：https://pypi.tuna.tsinghua.edu.cn/simple

> 建议使用豆瓣源

## 3.2 更换 pipy 源
### 临时使用
```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple some-package
```

### 设置为默认值

```
# 更换 pip 源
pip config set global.index-url https://pypi.douban.com/simple/
```

##### 参考

[1] [清华大学开源软件镜像站](https://mirror.tuna.tsinghua.edu.cn/help/pypi/)
[2] [pip 文档](https://pip.pypa.io/en/stable/installation/)

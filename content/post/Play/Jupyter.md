---
title : JupyterLab
date: 2022-01-19
updated : 2023-04-26 21:07:08
categories: ["玩机"]
tags: ["编辑器", "Jupyter"]
---

JupyterLab 是 Project Jupyter 的下一代基于 Web 的用户界面。
JupyterLab 使您能够以灵活、集成和可扩展的方式处理文档和活动，例如 Jupyter 笔记本、文本编辑器、终端和自定义组件.

以下适用于 jupyter3.0 

<!--more-->

# <center>Install Jupyter Lab

## Install

准备隔离的环境(可选)
使用 pip 进行安装

```bash
# 创建虚拟环境
python3 -m venv jupyter
# 进入虚拟环境
source jupyter/bin/activate
# 使用 pip 安装 JupyterLab
pip install jupyterlab
# 为当前用户安装
## 需要将jupyter 添加至环境变量中去
pip install --user jupyterlab
export PATH=~/.local/bin/jupyterlab:PATH
```

## Localization

> 从 3.0 版本开始，JupyterLab 提供了设置用户界面显示语言的能力。

### 安装语言包
显示新的语言，您需要安装语言包，访问[语言包列表](https://github.com/jupyterlab/language-packs/tree/master/language-packs) 
```bash
pip install jupyterlab-language-pack-zh-CN
```
点击 Settings >> Language 更改显示语言, 并点击 Ok 确认更改语言，重新载入或者刷新页面即可显示此示例中的简体中文。

# <center>Install jupyterLab extend

## 1. [install nodejs and npm](../tools-install-nodejs) 


## 2. 安装 jupyterlab 扩展

###  [jupyterlab-lsp](https://github.com/jupyter-lsp/jupyterlab-lsp)

> 提供语言的自动补全功能 

功能
1. 将鼠标悬停在任何一段代码上， 如果出现下滑线，您可以按下 Ctrl 以获取带有函数/类签名、模块文档或语言服务器（或其他信息工具）的提示信息
2. 严重错误带有红色下滑线，警告为橙色。将鼠标悬停在带有下滑线上可以查看更加详细的信息

```bash
# 安装
pip install 'jupyterlab>=3.0.0,<4.0.0a0' jupyterlab-lsp
# 为选择的语言安装 LSP 服务器
pip install 'python-lsp-server[all]'
```

### [jupyterlab-kite](https://github.com/kiteco/jupyterlab-kite)

> Kite 是一个 AI 驱动的编程助手，可帮助您在 JupyterLab 中编写 Python 代码。Kite 通过节省击键并在正确的时间向您显示正确的信息来帮助您更快地编写代码。在https://kite.com/integrations/jupyter/了解更多关于 Kite 如何增强 JupyterLab 编辑器功能的信息。

**安装 Kite 需要 CPU 支持 AVX**， 详情请参考 https://help.kite.com/article/101-kite-requires-avx-support

```bash
# 安装
# linux
bash -c "$(wget -q -O - https://linux.kite.com/dls/linux/current)"
pip install "jupyterlab-kite>=2.0.2"
```

# 参考
[1] [阿里云社区](https://developer.aliyun.com/article/760687)

---
title : jupyterlab
date: 2022-01-19
updated : 2022-01-19
photos: https://share.lain.buzz/api/name/jupyter-logo.svg?path=/🖼%20Picture/图床/博客/jupyter-logo.svg
categories: 
- [文本编辑器]
tags:
- Python
- ide
- jupyter
---

JupyterLab 是 Project Jupyter 的下一代基于 Web 的用户界面。
JupyterLab 使您能够以灵活、集成和可扩展的方式处理文档和活动，例如 Jupyter 笔记本、文本编辑器、终端和自定义组件.

以下适用于 jupyter3.0 

<!--more-->

# 1 安装jypyter

## 1.1 准备隔离的环境

1. 创建 python 虚拟环境

```python
python3 -m venv jupyter
```

2. 进入虚拟环境

```bash
source jupyter/bin/activate
```

## 1.2 安装 jupyterlab

### 1.2.1 使用 pip 安装本体

```bash
# 使用 pip 安装
pip install jupyterlab
```

如果使用 *pip install --user* 安装， 需要将 *jupyter lab* 加入至环境变量中去，
*export PATH="&amp;HOME/.local/bin\:&amp;PATH"*

### 1.2.2 本地化和语言

从 3.0 版本开始，JupyterLab 提供了设置用户界面显示语言的能力。

#### 语言包

显示新的语言，您需要安装语言包，访问[语言包列表](https://github.com/jupyterlab/language-packs/tree/master/language-packs) 

#### 安装语言包

```bash
pip install jupyterlab-language-pack-zh-CN
```

点击 Settings >> Language 更改显示语言, 并点击 Ok 确认更改语言，重新载入或者刷新页面即可显示此示例中的简体中文。

### 1.2.3 安装 nodejs

[Node.js](https://nodejs.org/zh-cn/)® 是一个基于 Chrome V8 引擎 的 JavaScript 运行时环境。

#### Ubuntu 下安装 nodejs [1]

可以从 Ubuntu 软件软件源中安装 Node.js 和 npm，但是此方法安装的 node.js 版本过低，故采用第二种方法

##### 从 NodeSource 中安装 Node.js 和 npm

1. 使用 sudo 用户身份运行下面的命令，下载并执行 NodeSource 安装脚本

```bash
sudo curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
```

这个脚本将会添加 NodeSource 的签名 key 到你的系统，创建一个 apt 源文件，安装必备的软件包，并且刷新 apt 缓存。
如果你需要另外的 Node.js 版本，例如12.x，将setup_14.x修改为setup_12.x

2. NodeSource 源启用成功后，安装 Node.js 和 npm

```bash
sudo apt install nodejs
```

3. 验证 Node.js 和 npm 是否正确安装。打印它们的版本号：

```bash
npm --version
node --version
```

#### Arch 下安装 nodejs [2]

在 Arch Linux 下安装 Node.js 非常的简单, 只需要运行 **sudo pacman -S nodejs npm** 即可。
使用 Arch Linux 仓库中 Node.js 长期支持版本（LTS） 的包， 防止 node.js 不兼容部署在旧版本中的代码。

1. 安装 lts(长期支持版) node.js

```bash
sudo pacman -Ss nodejs-lts

>>>
community/nodejs-lts-erbium 12.22.8-1
    Evented I/O for V8 javascript (LTS release: Erbium)
community/nodejs-lts-fermium 14.18.2-1
    Evented I/O for V8 javascript (LTS release: Fermium)
community/nodejs-lts-gallium 16.13.1-1
    Evented I/O for V8 javascript (LTS release: Gallium)

sudo pacman -S nodejs-lts-gallium
```

2. 验证 Node.js 和 npm 是否正确安装。打印它们的版本号：

```bash
npm --version
node --version
```

# 2 安装 jupyterlab 扩展

## 2.1 [jupyterlab-lsp](https://github.com/jupyter-lsp/jupyterlab-lsp)

Language Server Protocol integration for Jupyter(Lab)。
提供语言的自动补全功能

### 安装

```bash
pip install 'jupyterlab>=3.0.0,<4.0.0a0' jupyterlab-lsp
```

### 为选择的语言安装 LSP 服务器

```bash
pip install 'python-lsp-server[all]'
```

### 特性

#### 提示

将鼠标悬停在任何一段代码上， 如果出现下滑线，您可以按下 Ctrl 以获取带有函数/类签名、模块文档或语言服务器（或其他信息工具）的提示信息

#### 诊断

严重错误带有红色下滑线，警告为橙色。将鼠标悬停在带有下滑线上可以查看更加详细的信息

## 2.2 [jupyterlab-kite](https://github.com/kiteco/jupyterlab-kite)

Kite 是一个 AI 驱动的编程助手，可帮助您在 JupyterLab 中编写 Python 代码。Kite 通过节省击键并在正确的时间向您显示正确的信息来帮助您更快地编写代码。在https://kite.com/integrations/jupyter/了解更多关于 Kite 如何增强 JupyterLab 编辑器功能的信息。

**安装 Kite 需要 CPU 支持 AVX**， 详情请参考 https://help.kite.com/article/101-kite-requires-avx-support

### 在 Linux 下安装

```bash
bash -c "$(wget -q -O - https://linux.kite.com/dls/linux/current)"

pip install "jupyterlab-kite>=2.0.2"
```


# 参考
[1] [阿里云社区](https://developer.aliyun.com/article/760687)

[2] [zzz.buzz](https://zzz.buzz/zh/)

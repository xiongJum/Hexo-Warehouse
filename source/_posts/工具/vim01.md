---
title: VIM 插件管理器-Vundle  
date: 2021-11-27 13:03:33
updated: 2022-01-09
categories: 
- [工具] 
tags:
- vim
- 编辑器
- 插件
- Vundle
---

Vim 是一个高度可配置的文本编辑器，旨在高效地创建和更改任何类型的文本。它作为“vi”和“vim”包含在大多数 类UNIX 和Linux 中。
<!--more-->
# 1. 安装插件管理器

## 1.1 下载 Vundle

1. Vundle插件也是提供一个Vundle.vim文件，其下载地址为 [Vim bundle](https://github.com/VundleVim/Vundle.vim.git) 然后将下载后的文件存放到 ~/.vim/bundle 
2. 或者可以直接从 Github 上拉取文件
~~~
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
~~~

## 1.2 配置 Vundle

1. 修改 Vim 的配置文件 ~/.vimrc, 若没有此文件可以直接进行创建， 以下是配置文件。

~~~
set nocompatible               "去除VIM一致性，必须
filetype off                   

set rtp+=~/.vim/bundle/Vundle.vim "设置包括vundle和初始化相关的运行时路径

call vundle#begin()
"在此增加其他插件，安装的插件需要放在vundle#begin和vundle#end之间"
"安装github上的插件格式为 Plugin '用户名/插件仓库名'"

Plugin 'VundleVim/Vundle.vim'  "启用vundle管理插件，必须"

call vundle#end()           

filetype plugin indent on     "加载vim自带和插件相应的语法和文件类型相关脚本，必须
~~~

## 1.3 安装 Vundle 

1. 需要打开配置文件 ~/.vimrc， 在命令模式下输入 **:PluginInstall**
2. 或者在终端命令行下通过命令 **vim + PluginInstall + qall(用户名/插件仓库名)** 直接安装

## 1.4 使用 Vundle 修改插件

1. 安装插件， 同1.3
2. 删除插件，编辑 Vim 配置文件 .vimrc, 删除想要移除插件所对应的 Plugin 一行， 然后打开 vim 在 vim命令行模式中执行命令 **:BundleClean**
3. 其余操作方法可以见[原文档](https://github.com/VundleVim/Vundle.vim/blob/v0.10.2/doc/vundle.txt#L319-L360)

# 2. Markdown 扩展插件

## 2.1 vim-markdown 
原始 Markdown 的语法高亮、匹配规则和映射, 项目地址[vim-markdown](https://github.com/plasticboy/vim-markdown)

### 2.1.1 安装

1. 使用 Vundle 安装，将以下两行添加至您的 .vimrc 文件中
~~~
' 安装 vim-markdown
Plugin 'godlygeek/tabular'         '依赖
Plugin 'plasticboy/vim-markdown'   '本体
~~~
保存后，直接在 .vimrc 文件的命令行模式键入命令 :PluginInstall

### 2.1.2 基本用法

1. 使用 :help fold-expr 和 :help fold-commands 了解详情
2. 默认情况下为标题启用折叠
+ zr 降低整个缓冲区的折叠级别 1~6，即六级标题
+ zR 打开所有的折叠
+ zm 增加折叠级别
+ zM 折叠所有
+ za 展开当前光标所在的位置
+ zc 折叠光标所在的位置
+ zA 递归展开
+ zC 递归折叠

## 2.2 markdown-preview
通过浏览器实时预览 Markdown 文件

### 2.2.1 安装
1. 打开 .vimrc 添加以下几行文本
~~~
Plugin 'iamcco/markdown-preview.nvim'
~~~
2. 然后在命令行模式中运行
~~~
:source %
:PluginInstall
:call mkdp#util#install()
~~~

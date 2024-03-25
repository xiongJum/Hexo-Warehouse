---
title: 安装 Node.js
date: 2023-04-26 21:16:04
categories: ["玩机"]
tags: ["node", "npm", "安装"]
toc: false
---

# 安装

## Ubuntu 下安装 nodejs
下载并执行 NodeSource 安装脚本
```bash
sudo curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash 
```
安装
```bash
sudo apt install nodejs
```

## Arch 下安装 nodejs

安装长期支持版
```bash
sudo pacman -S nodejs-lts-gallium
```

## 检验是否安装成功

打印他们的版本号, 验证是否安装正确
```bash
npm --version
node --version
```


## 说明
1. [Node.js](https://nodejs.org/zh-cn/)® 是一个基于 Chrome V8 引擎 的 JavaScript 运行时环境。

2. Ubuntu 软件软件源中安装 Node.js 和 npm，但是此方法安装的 node.js 版本过低, 不建进行包管理器安装



## 参考
[zzz.buzz](https://zzz.buzz/zh/)
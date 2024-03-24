---
title: SHELL(其一)
date: 2020-06-28 09:00:00
categories: ["玩机"]
tags: ["shell", "系统", "Linux"]
---
linux

##### (1). bash

+ 参考/引用：[鸟哥的Linux私房菜-第十一章](http://cn.linux.vbird.org/linux_basic/0320bash_1.php)

+ 命令编修能力

```shell
# 查看不同用户使用的shell,其记录在文件/etc/passwd中
cat /ect/passwd
# 查看bash的历史命令,其记录在文件~/.bash_history中
# 其只记录前一次登录系统时使用的命令，当前登录运行的命令被记录在缓存中
cat ~/.bash_history
```
<!--more-->
+ 命令与文件补全能力[tab]

  + [tab]接在命令之后为[命令补全]
  + [tab] 接在命令-文件之后为[文件补齐]
  + 连续按两次[tab]显示所有可运行的命令
  + c[tab] [tab]显示以c开头的所有可运行的命令

+ 命令别名配置功能（alias）

  + ```shell
    alias lm='ls -al'
    # lm 等同于命令 ls -al
    # 查看目前命名的所有别名（alias） 
    alias
    ```

+ 工作控制、前景背景控制（<font color=red>待补充</font>）

  + [ctrl] + z 将当前任务放入后台

+ 程序化脚本（shell scripts)

  + （<font color=red>待补充</font>）

+ 通配符

  + 如 "*" 

+ Bash shell的内嵌\内建命令 (type)

  + ```shell
    # 查看内嵌命令的说明文档，如bash
    man bash
    # 查看命令是偶为内嵌命令 type
    type cd
    >>
    cd is a shell builtin => cd 是 shell 内嵌
    ```

+ 命令的下达

  + 如 “\ [Enter]” 使 [Enter]不再具有[开始运行]的功能，具有转义的作用，并将命令在下一行继续输入
  + （<font color=red>待补充</font>）


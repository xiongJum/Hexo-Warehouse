---
title: 【Linux】文件管理和帮助命令
date: 2020-07-28 15:00:00
photos: https://cdn.jsdelivr.net/gh/xiongJum/Picture/img/83109676.jpg
tag:
- linux
- 浅析
- 文件管理
- system
---
### 帮助命令

~~~shell
# man
man find # 查询命令的说明文档
man -f find # 显示更多的信息
>>> 
find (1)             - search for files in a directory hierarchy
man -k find # 关键词查找
~~~
<!--more-->


#####   查找程序的安装路径

~~~shell
# which
which mysql
>>>
/usr/bin/mysql
~~~

#####   查看程序的搜索路径

+   当系统中安装了同一软件的多个版本时，不确定使用的是哪个版本时，这个命令就能派上用场

~~~shell
# whereis
$ whereis mysql
>>>
mysql: /usr/bin/mysql /usr/lib64/mysql /usr/share/mysql /usr/share/man/man1/mysql.1.gz
~~~

### 文件管理

#####   创建和删除文件

~~~shell
# 创建文件
mkdir # 创建文件夹文件
touch # 创建普通文件
# 删除 rm
rm #删除普通文件和空目录文件
rm -rf file # 删除非空目录文件
# 复制 cp
cp file ./path/ # 复制普通文件
cp -r file ./path # 复制目录文件
# 删除和移动文件
mv file # 删除文件
mv name newname # 文件重命名
~~~

#####   查看当前目录文件下的文件个数

~~~shell
find ./ | wc -l
~~~

#####   目录切换

~~~shell
# cd
pwd # 显示当前路径
cd # 切换到home目录 或者cd ~
cd - # 切换到上一个工作目录
cd path # 切换到指定目录
~~~

#####   列出文件

~~~shell
# ls
# 如果不写路径 则表示查看当前目录
ls path # 查看path路径下的文件
ls | cat -n path # 在每项文件前面增加一个编号
ls -a path # 查看隐藏的文件
~~~

#####   查找文件

~~~shell
# find 实时查找
find / -name '*.o' # 查找所有以".o"结尾的文件
find ./ -name "core*" | xargs file # 查找以"core"开头文件，并标注文件类型
find ./ -name "*.o" -exec rm {} \; # 递归当前目录及子目录并删除所有".o"文件
# locate 需要定时更新数据库
locate add # 寻找含有add文件的路径
# centos安装locate
sudo yum -y install mlocate  # 安装mlocate
sudo updatedb # 初始化
~~~

#####   查看文件内容

~~~shell
# cat
cat file # 显示文件内容，显示每行的行号 cat -n file
# head
head -2 /path/** # 显示指定路径下所有文件的前两行，若不指定路径则为当前工作路径
head -2 /path/file # 显示指定文件的前两行内容
# tail
tail -2 /path/file  # 显示文件倒数两行的内容
tail -f file # 动态显示当前文件内容
tail -fn 200 file # 动态显示最近200行的文件内容
~~~

#####   查找文件内容

~~~shell
# diff
diff file1 file2 # 比较两个文件的差别
# egrep
egrep 'xion' file # 在file文件中查找"xion"

~~~

#####   文件的权限修改

~~~shell
# chown 改变文件的拥有者
chown -R mysql@group /path # 将path文件修改为group组下的mysql用户
# chmod 改变文件的读、写、执行权限
chmod 777 file
chmod a+x file # 增加文件的可执行权限
~~~

#####   增加文件别名

~~~shell
# 软链接
ln -s cc ccTo # 删除源文件，另一个将无法使用。cc为源文件
# 硬链接
ln cc ccAotu # 删除其中一个，另一个文件不受影响
~~~

#####   设置命令别名

~~~shell
# alias
alias lsl='ls -lrt'
alias lm='ls -al | more'
~~~
---
title: V2ray搭建教程（基础篇）
cover: https://cdn.jsdelivr.net/gh/xiongJum/Picture//img/82601928_p0.jpg
tag: 
- 服务器
- vpn
- V2ray
---

----

#### 1. 服务器

+   可以使用[linode](https://www.linode.com/) 或者[搬瓦工](https://bandwagonhost.cn/)vps服务器

+   本次推荐Google Cloud(谷歌云)，搭建v2ray或者其余学习用途

+   ![](https://cdn.jsdelivr.net/gh/xiongJum/Picture/img/scr_GoogleCloud.png)

    （1）.优点

    +   也无需担心和AWS一样会莫名其妙的扣费，在没有升级为付费用户之前是不会扣除信用卡的金额
    +   新用户会赠送等同于$300 的当地货币使用，有效期为12个月
    +   有香港等亚太地区的服务器，使用延迟会好上不少

    （2）.缺点

    +   搭建过程中需要使用魔法上网

+   本教程暂介绍服务器申请

#### 2. 服务端配置

+   本次教程选择的面板为[sprov065](https://github.com/sprov065)@github用户的[v2-ui](https://github.com/sprov065/v2-ui) 

    +   特点：
    +   可视化操作，查看方便、学习门槛较低
    +   由于是网页端所以管理会比较方便
    +   该面板可以配置多用户
+   安装&升级v2ray

~~~
bash <(curl -Ls https://blog.sprov.xyz/v2-ui.sh)
~~~



+   输入v2-ui，进入面板设置

    +   <img src="https://cdn.jsdelivr.net/gh/xiongJum/Picture/scr/03.png" style="zoom:50%;" />

+   查看或者重新设置面板的端口号

    ~~~shell
    # 查找v2-ui的服务进程
    ps -ef | grep v2-ui
    # 根据PID查找端口号
    netstat -nap | grep [PID]
    ~~~

+   ![](https://cdn.jsdelivr.net/gh/xiongJum/Picture/Scr/03.png)

+   输入当前服务器IP:端口 和账号密码admin，进入设置面板

+   ![](https://cdn.jsdelivr.net/gh/xiongJum/Picture/Scr/04.png)

+   1.  点击账号列表，点击左上角的+号，进入添加账号页面，备注为非填项，其余默认即可，可以按照自己的需求进行修改，本次教程选择的协议为kcp
    2.  kcp消耗流量比较多，但延迟较低，对于延迟较高的IP，选择kcp会一定程度上改善网络环境
    3.  如果想选择其余协议，可以参考[Blog](https://toutyrater.github.io/advanced/wss_and_web.html)
    4.  若想使用Nginx+w2+TLS进行网络代理，本教程中协议传输配置选择w2，不要选择TLS。TLS在Nginx中配置即可，但客户端中要选择TLS，端口为Nginx中代理的端口，非在本面板中设置的端口

+   ![](https://cdn.jsdelivr.net/gh/xiongJum/Picture/Scr/05.png)

+   点击添加-二维码-复制连接，即可获得v2Ray的连接

#### 3. 客户端配置

+   下载地址
    +   PC端：[windows](https://github.com/2dust/v2rayN/releases)、[MacOS](https://github.com/Cenmrev/V2RayX/releases)
    +   移动端：[Android](https://github.com/2dust/v2rayNG/releases)
+   配置：windows
    +   下载并解压[v2RayN](https://github.com/2dust/v2rayN/releases/download/3.19/v2rayN.zip)和[v2rayN-Core](https://github.com/2dust/v2rayN/releases/download/3.19/v2rayN-Core.zip)
    +   点击v2rayN.exe文件，右击右下角的v2rayN的图标选择{从剪贴板导入批量Url}
    +   双击右下角的v2ray图标进入显示面板，可以发现有刚刚导入的链接
    +   右击右下角的图标选择{Http代理-PAC模式}

----

本文结束

---

#### 4. 扩展

+   模式说明
    +   全局是代理所有的网络包括局域网和国内的网络如百度，和国外的网络如Google
    +   PAC是通过配置文件选择代理部分网络
    +   v2ray支持GFWfilt地址和本地pac文件同时生效，若GFWList地址中没有代理你需要的网络，你可以在用户PAC设置中手动添加要代理的ip或者域名
    +   ![](https://cdn.jsdelivr.net/gh/xiongJum/Picture//Scr/08.png)
+   pixiv的PAC代理地址
    +   www.pixiv.net,
    +   api.booth.pm,(存疑)
    +   i.pximg.net,
+   局域网代理配置（如switch配置）
    +   双击v2ray-参数设置，进入设置页面，点击基础设置查看本地监听端口
    +   http代理默认即可，你也可以根据自己的选择进行不同的配置
    +   本地会开启两个端口号（设置的端口号X和X+1）
    +   按下快捷键win+R，在运行输入框中输入cmd
    +   在cmd界面输入ipconfig
    +   ![](https://cdn.jsdelivr.net/gh/xiongJum/Picture//Scr/09.png)
    +   在要代理的机器中（如switch）ip设置为上图的ipv4、端口号为设置的端口号+1
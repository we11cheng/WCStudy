## Google Cloud Platform免费申请&一键搭建SSR & BBR加速教程


- 修改防火墙规则

![](https://gitee.com/he11oworld/picBed/raw/master/20210219174457.png)

![](https://gitee.com/he11oworld/picBed/raw/master/20210219174518.png)

注意IP地址范围是0.0.0.0/0, 别写错了

有遇到过有人写成0.0.0.0, 结果后来怎么试都不成功的


- 创建计算引擎 

或登录GCP控制台后, 点击计算引擎 – 创建实例. 如图:

![](https://gitee.com/he11oworld/picBed/raw/master/20210219174655.png)

```
名称: 随便填
地区: 建议选asia-east1-c
asia-east1-a, asia-east1-b, asia-east1-c 机房都在中国台湾彰化县, 实测c区更好.
asia-south1 机房在印度孟买
asia-northeast1 机房在日本东京
别选错了跑来说怎么延迟不一样….
机器类型: 选微型(一个共享vCPU)
0.6G内存, 一般加速上网, 看视频, 玩游戏都够用了, 看你自己需求.
启动磁盘: 推荐选CentOS 7.

```

- 生成静态IP，点击网络选项卡—外部IP—创建IP地址

![](https://gitee.com/he11oworld/picBed/raw/master/20210219174757.png)

![](https://gitee.com/he11oworld/picBed/raw/master/20210219174814.png)

![](https://gitee.com/he11oworld/picBed/raw/master/20210219174833.png)

```
补充说明
关于IP会有静态和临时两个选项
推荐选择静态, 选择临时的话机器重启后会重新分配IP地址
```

### 原文链接

![Google Cloud Platform免费申请&一键搭建SSR & BBR加速教程](https://www.wmsoho.com/wp-content/uploads/google_cloud_platform_BBR_SSR.png "Google Cloud Platform免费申请&一键搭建SSR & BBR加速教程")


### **1\. Google Cloud Platform (GCP) 简介**

Google Cloud Platform (以下简称GCP)是Google提供的云平台, 可以用来搭建加速服务, 网站和存储数据等等, 本文将介绍如何申请GCP一年的免费试用、Linux服务器环境搭建、配置BBR加速以及[安装BBR](https://www.wmsoho.com/install-shadowsocks/ "安装BBR")等服务端. 网上关于如何搭建GCP的教程很多, 不过都不是太详尽. 如果你是没基础的新手, 或者像下图这位童鞋踩到坑不知所措的话, 希望这篇教程能帮到你 🙂

![Google Cloud Platform搭建问题](https://www.wmsoho.com/wp-content/uploads/a03153be4d2016acc0076b8350dc04f1.png "Google Cloud Platform搭建问题")

### **2\. Google Cloud Platform 优势**

大家用得比较多的可能是[Linode](https://www.wmsoho.com/go/linode "Linode"), 搬瓦工, Vultr等VPS, 收费$5-$20美金左右.  
最近Linode IP段也封得比较多, 基本上开十多台机器都难找到能PING通的IP.  
另外还有各种VP/恩基本都挂了.  
至于XX路由器, 本质上只是刷了个固件内置了SS相关服务而已, 略过不提.

GCP的优势在于:  
– **新用户可以免费获得300美元赠金**  
Google云服务平台对新用户赠送300美元, 可以免费使用1年.  
并且到期后如果不打算续费, 也不会额外收取费用 (像亚马逊AWS就直接扣你费用了).  
当然如果你还想继续免费用, 也不是没有办法 🙂  
用来搭SS的话, 最低配置的机型$5/月, 出口大陆流量1T以内为0.23$/1G, 算下来每个月可用80多G的流量, 足够用了, 当然你还可以顺便搭个网站之类的 🙂  
![GCP 300美元一年免费试用](https://www.wmsoho.com/wp-content/uploads/82beee88bc32c71d236ec6c6763e3f58.png "GCP 300美元一年免费试用")

+   **速度快**  
    Google GCP提供美国, 亚洲, 欧洲等区域的机房, 而亚洲的台湾机房就在厦门正对面的台湾省彰化县, TTL只有40ms左右，快到飞起, 而Linode、Vultr等通常是200多；  
    \> 后来出的香港机房也行, 电信直连, 联通移动据说会绕路. 新加坡也可以. 具体自己测试.

### **3\. 需要准备的东西**

+   `Gmail账号`
+   `双币信用卡` (淘宝购买的虚拟卡无法使用, 不要浪费钱)
+   `Xshell` SSH客户端 (也可用Google自带的在线SSH, 以后比较方便).  
    Xshell不建议用绿色版, 某些电脑没装运行库的话可能无法打开.  
    另外之前官方5.0 Build 1322版本存在后门, 所以还是去[官网](http://www.netsarang.com/ "官网")下载最新的安装版吧, 目前家庭与学校用户可免费使用. 填写姓名和邮箱并提交表单后, 邮箱会收到下载链接.
+   `puttygen` SSH密钥生成工具. 官网下载地址: [puttygen 32位](https://the.earth.li/~sgtatham/putty/latest/w32/puttygen.exe "puttygen 32位") , [puttygen 64位](https://the.earth.li/~sgtatham/putty/latest/w64/puttygen.exe "puttygen 64位")  
    \> 当然Xshell也能直接生成密钥, 这个Wordpress编辑器太难用了, 我懒得写了 🙂
+   `能访问Google` (额, 这貌似是个悖论, 你可以先找其他临时方案)
+   需要一定动手能力以及Linux系统基础知识, 想省事或遇到问题的, [加群找群主](https://jq.qq.com/?_wv=1027&k=5mZal27 "加群找群主"), 人工付费服务…

**废话不多说,下面直接上教程**

### **4\. 申请Google Cloud Platform**

#### **4.1 注册**

申请地址: [https://cloud.google.com/free/](https://cloud.google.com/free/ "https://cloud.google.com/free/")

![Google Cloud Platform 免费试用](https://www.wmsoho.com/wp-content/uploads/5c866781753c179106c2b5dce9313946.png "Google Cloud Platform 免费试用")

点击`免费试用`, 登入你的Google账号或Gmail账号 (通用).

> 什么? Gmail你没有? 那先去注册一个吧. 手机号码无法通过验证, 可通过下载QQ邮箱APP或者网易邮箱大师APP, 添加Gmail邮箱账号, 在打开的页面注册, 直接访问, 不用F.

地区选`中国`.

> 现在没有中国选项了, 可以随便选. 比如美国, 地址等信息请百度”美国地址生成”  
> ![](https://www.wmsoho.com/wp-content/uploads/9652bd93a8987a23c25d9734200cfe83.png)

账号类型选`个人`, 接下来输入你的账单地址以及信用卡信息. Google会从信用卡扣除1美元用于验证, 验证完成后会退还到账户.

点击`开始免费试用`.

#### **4.2 激活结算账号**

注册成功后**可能**并不能马上使用 (国内注册的基本都需要激活, 当然也不排除你人品好的情况), Gmail邮箱会受到一封标题为`紧急通知：您的结算帐号 XXXXX 已经被暂停使用`的邮件, 你需要点击邮件中的链接, 上传身份证和信用卡照片完成验证 (信用卡照片可以遮住或PS掉敏感信息, 只保留最后四位, 自己看页面提示), 一般提交后10分钟左右就能收到通过的邮件, 验证完成后就可以正常使用了.

> 这一步很多人会忽略, 结果就是创建实例的时候会不停提示你Enable Billing Account, 但是指引界面又没有明确说明要怎么激活, 即使是进到账号管理界面也无法自己开启. 所以别忘了去喵一眼你的邮箱 🙂

![邮件: 紧急通知：您的结算帐号 XXXXX 已经被暂停使用](https://www.wmsoho.com/wp-content/uploads/e1a3f40e874b67a2524311294b6e6f70.png "邮件: 紧急通知：您的结算帐号 XXXXX 已经被暂停使用")

结算账号验证成功后会显示消息:

![结算账号验证成功消息](https://www.wmsoho.com/wp-content/uploads/cc724243ba9a51c05e3f9bde67e540a4.png "结算账号验证成功消息")

#### **4.3 修改防火墙规则**

有的文章是先创建实例再改规则.  
我们先修改防火墙规则, 后面会省事一点.

直接访问 [Google Cloud Platform 防火墙规则](https://console.cloud.google.com/networking/firewalls/list "Google Cloud Platform 防火墙规则")

![Google Cloud Platform 防火墙规则](https://www.wmsoho.com/wp-content/uploads/a3519c51b23bcd48b95b704f99d1c6ef.png "Google Cloud Platform 防火墙规则")

点击`创建防火墙规则`, 按下图设置:

![Google Cloud Platform 创建防火墙规则](https://www.wmsoho.com/wp-content/uploads/3c31361b2a2051761dd253801c9cde23.png "Google Cloud Platform 创建防火墙规则")

注意IP地址范围是`0.0.0.0/0`, 别写错了.  
有遇到过有人写成`0.0.0.0`, 结果后来怎么试都不成功的.

#### **4.4 准备SSH密钥**

密钥在Xshell连接时需要用到.  
当然, 你也可以直接用Google自带的在线SSH, 那这一步可忽略.  
不过, 还是建议使用Xshell之类的SSH客户端,直接本地连接服务器进行管理, 不用每次都打开浏览器后台. 另外还可以预先避免可能的连接错误, 否则搭建好后却ping不通或者连不上, 那就白费力气了.

#### **4.4.1 生成密钥**

打开puttygen.exe, 直接点`生成` (英文版点`generate`), 在空白区域移动鼠标以生成随机数据.  
![Puttygen点击Generate生成密钥](https://www.wmsoho.com/wp-content/uploads/7bc2741a71508d173c5175a3a1ceb524.png "Puttygen点击Generate生成密钥")

![在空白区域移动鼠标生成随机数据](https://www.wmsoho.com/wp-content/uploads/988f9bdd9f59bdcc4e06368f656c0667.png "在空白区域移动鼠标生成随机数据")  
![Putty密钥注释](https://www.wmsoho.com/wp-content/uploads/ef5a320568bc4e5d52e1c003a6779735.png)

**密钥注释**: 填写你的Gmail账户名, 如邮箱username@gmail.com, 则填写username.  
**密码**: 不填也没事.

我用的puttygen是中文版, 如果你是官网下载的英文版请参考下图, 个别选项不一样.

![Putty选项](https://www.wmsoho.com/wp-content/uploads/d528c121fa0053a5f14f9ec728127ca3.png)

#### **4.4.2 保存私钥**

点击`转换`(`Conversions`) – `导出openssh密钥` (`Export Open SSH Key`). 等会Xshell连接VPS会用到.

生成器窗口先不要关闭.

![puttygen导出私钥](https://www.wmsoho.com/wp-content/uploads/679b6fce135edadbce7565b65b64a679.png "puttygen导出私钥")

#### **4.5 创建计算引擎**

#### **4.5.1 创建实例**

本文来源: [外贸SOHO笔记](https://www.wmsoho.com"/ "外贸SOHO笔记"). 未经允许不得转载.

直接访问 [https://console.cloud.google.com/compute/instances](https://console.cloud.google.com/compute/instances%22 "https://console.cloud.google.com/compute/instances")  
或登录[GCP控制台](https://console.cloud.google.com/home/dashboard%22 "GCP控制台")后, 点击`计算引擎` – `创建实例`. 如图:

![title](https://www.wmsoho.com/wp-content/uploads/c5c0eeedec98f78036978a9360c1a5c7.png)

+   `名称`: 随便填
+   `地区`: 建议选`asia-east1-c`  
    asia-east1-a, asia-east1-b, asia-east1-c 机房都在中国台湾彰化县, 实测c区更好.  
    asia-south1 机房在印度孟买  
    asia-northeast1 机房在日本东京  
    **别选错了跑来说怎么延迟不一样…**.
+   `机器类型`: 选微型(一个共享vCPU)  
    0.6G内存, 一般加速上网, 看视频, 玩游戏都够用了, 看你自己需求.
+   `启动磁盘`: 推荐选CentOS 7.  
    \> 本文命令都是基于CentOS 7, 如选Debian, Ubuntu等其他系统, 命令会稍有不同, 自行解决.

#### **4.5.2 生成静态IP**

点击`网络选项卡`—`外部IP`—`创建IP地址`.

![Google运服务器-创建实例-网络选项卡](https://www.wmsoho.com/wp-content/uploads/b362c2281b1cc0ff39c3f25ccd9fb35c.png "Google运服务器-创建实例-网络选项卡")

![外部IP,创建IP地址](https://www.wmsoho.com/wp-content/uploads/d4cc9b6d438c178ee448f114c8d474c4.png "外部IP,创建IP地址")

![创建IP地址](https://www.wmsoho.com/wp-content/uploads/da7c9c2ceb994556c3e93dc2c9690250.png "创建IP地址")

然后会提示输入名称, 任意输入即可(小写字母开头,不能为大写字母)

![保留新的静态IP地址](https://www.wmsoho.com/wp-content/uploads/f00330ef68ffb91cb939d26f5804ce98.png "保留新的静态IP地址")

> **补充说明**  
> 关于IP会有`静态`和`临时`两个选项.  
> 推荐选择静态, 选择临时的话机器重启后会重新分配IP地址.

更多信息可以去Dashboard中的`网络` – `VNC网络` – `外部IP地址`或直接点击[https://console.cloud.google.com/networking/addresses/list](https://console.cloud.google.com/networking/addresses/list%22 "https://console.cloud.google.com/networking/addresses/list")查看.

把创建好的IP记下来, 用Xshell连接时会用到.

> 静态IP一定记得绑定到实例.  
> 已分配但未被使用的IP, 会按$0.01/小时计费.

#### **4.5.3上传SSH公钥**

点击`SSH密钥`选项卡

> 现在是”安全”选项卡

![Google Cloud Platform - 创建实例 - SSH密钥选项卡](https://www.wmsoho.com/wp-content/uploads/f3b828f34d58070841103acd06c68bce.png "Google Cloud Platform - 创建实例 - SSH密钥选项卡")

在上图文本框内填入4.4.1步骤中puttygen中生成的`公钥`, 注意是下图中**蓝色**部分的内容, 直接`全选`并`复制`, 再`粘贴`过来即可.

![https://www.wmsoho.com/wp-content/uploads/dbf0857c554022a8e4df45821d858b69.png](https://www.wmsoho.com/wp-content/uploads/dbf0857c554022a8e4df45821d858b69.png)

最后点击最底部的`创建`按钮, 创建实例.

页面左下角会提示正在创建实例, 等待10秒左右即可创建完成并启动.

![正在创建GCP VM实例](https://www.wmsoho.com/wp-content/uploads/fe9636153a74c0421be510dd4eea6726.png "正在创建GCP VM实例")

VM实例管理界面第一列中绿色的对勾图标表示该实例正在运行, 第五列为实例的外部IP, 也就是下一步用Xshell连接时需要用到的主机IP地址.

![https://www.wmsoho.com/wp-content/uploads/8c4d333bb2ac96542dbd1fe7995235ca.png](https://www.wmsoho.com/wp-content/uploads/8c4d333bb2ac96542dbd1fe7995235ca.png)

### **5\. 一键安装最新内核并开启BBR脚本**

> BBR要求系统内核版本为4.9以上, 本文采用teddysun的脚本, 支持一键自动判断和安装所需内核版本并开启BBR.

#### **5.1 连接XShell**

通过Xshell连接到Google Cloud VPS.

`主机`: 之前步骤创建的静态IP, 也可在[GCP VM实例的管理界面](https://console.cloud.google.com/compute/instances%22 "GCP VM实例的管理界面")查看.  
`端口`: 默认的22端口  
`用户名`: 如你的gmail邮箱是username@gmail.com, 则用户名是username

> 这里的用户名和你生成密钥时填的注释是对应的.

`密钥`: 使用之前puttygen生成的私钥, 导入即可.

有的小伙伴不会用Xshell, 补充几张图片:  
![https://www.wmsoho.com/wp-content/uploads/e0383a23a109e5c165186f17e1c40627.png](https://www.wmsoho.com/wp-content/uploads/e0383a23a109e5c165186f17e1c40627.png)

![https://www.wmsoho.com/wp-content/uploads/4529c8d4ce0958b64bd5fc3f80f4cc18.png](https://www.wmsoho.com/wp-content/uploads/4529c8d4ce0958b64bd5fc3f80f4cc18.png)

Google也自带了在线SSH, 实例创建好后, 在[列表界面](https://console.cloud.google.com/compute/instances%22 "列表界面")点击`SSH` – `在浏览器窗口中打开`即可进入, 见下图.  
不过出于个人习惯, 用Xshell本地进行连接管理更方便, 不用每次都打开浏览器进GCP后台.

![https://www.wmsoho.com/wp-content/uploads/79c7450fea289883af56f624aaf2907d.png](https://www.wmsoho.com/wp-content/uploads/79c7450fea289883af56f624aaf2907d.png)

Xshell连接成功, 会显示如下的信息:  
Last login: Sun Feb 23 06:17:40 2020 from 18.138.172.57″  
\[root@instance-1 ~\]#

> Tips  
> root是你当前登录的用户名  
> instance-1是实例名  
> ~是当前工作路径  
> 最后都是以 $ 或者 # 号结束，普通用户以 $ 号结束，只有root用户以 # 结束。

这时才可以在服务器端进行后续的操作. 如果Xshell没连上, 显示的是`[C:\~]$`之类的

![https://www.wmsoho.com/wp-content/uploads/Snipaste_2020-02-26_14-22-21.jpg](https://www.wmsoho.com/wp-content/uploads/Snipaste_2020-02-26_14-22-21.jpg)

#### **5.2 运行BBR脚本**

通过脚本一键升级内核并安装BBR加速.

Linux系统中root用户拥有最高权限, 出于安全考虑, GCP默认是以普通用户登录的, 我们需要先切换到root用户, 否则运行某些命令时会提示无权限.

Xshell连上服务器后, 先要切换到root用户.  
输入命令:  
`sudo -i`

切换后, 命令前面的提示符中也会显示root, 如图:  
![](https://www.wmsoho.com/wp-content/uploads/438f91b7c41f67689ce28eae3a467cd9.png)

依次运行以下4条命令

安装wget  
`yum install -y wget`

下载shell脚本  
`wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh`

给脚本赋予执行权限  
`chmod +x bbr.sh`

执行脚本  
`./bbr.sh`

安装完成后，脚本会提示需要重启VPS，输入 `y` 或 `Y` 并`回车`, 实例会重启, 这时Xshell连接也会自动断开。

VPS重启可能需要几分钟, 耐心等待一下.  
重启完成后，再用Xhell重新连接VPS, `sudo -i`切换到root用户.

现在验证一下是否成功安装了最新内核并开启BBR.  
输入以下命令：

`uname -r`  
查看内核版本，如果返回值含有4.13或以上版本, 就表示OK了.

![https://www.wmsoho.com/wp-content/uploads/48cb97a23c97981d6c51bbb9e97a569b.png](https://www.wmsoho.com/wp-content/uploads/48cb97a23c97981d6c51bbb9e97a569b.png)

`sysctl net.ipv4.tcp_available_congestion_control`  
返回值一般为：  
net.ipv4.tcp\_available\_congestion\_control = bbr cubic reno

`sysctl net.ipv4.tcp_congestion_control`  
返回值一般为：  
net.ipv4.tcp\_congestion\_control = bbr

`sysctl net.core.default_qdisc`  
返回值一般为：  
net.core.default\_qdisc = fq

`lsmod | grep bbr`  
返回值有 tcp\_bbr 模块即说明bbr已启动。

### **6\. 安装SSR**

本文采用秋水逸冰的Shadowsocks一键安装脚本(四合一).  
依次运行以下3条命令:

> 2020.2.24更新: SS现在不稳, 建议换其他方式.  
> 2017.12.3更新: 记得先用`sudo -i`命令切换到root用户, 不然会显示This script must be run as root! 见下图:

![](https://www.wmsoho.com/wp-content/uploads/8a096bb81ccd41d685e1b6df34895c00.png)

下载Shell脚本

```auto
wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh

```

> `-O`是大写的字母O, 不是零. 如果你是手动输入, 记得看清楚哦. 复制粘贴更省事. 另外请留意看屏幕上的提示信息.

赋予脚本执行权限  
`chmod +x shadowsocks-all.sh`

> 命令执行完不会显示结果, 是正常的

执行脚本  
`./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log`

然后是选择SSR版本, 设置端口、密码、加密方式等等，之前的 [SSR安装教程](https://www.wmsoho.com/install-shadowsocks/%22 "SSR安装教程")有详细步骤， 此处略过.

安装好之后即可通过客户端进行连接与测速.  
各个版本的客户端, QQ群内有.

最后附上测速:  
![https://www.wmsoho.com/wp-content/uploads/6d0ecce17a8c73fd857c9b0e33b2b4fa.png](https://www.wmsoho.com/wp-content/uploads/6d0ecce17a8c73fd857c9b0e33b2b4fa.png)

小飞机客户端的使用请参考:

[ShadowsocksR客户端 各种隐藏使用技巧说明](https://www.wmsoho.com/how-to-use-shadowsocksr/%22 "ShadowsocksR客户端 各种隐藏使用技巧说明")  
[SSR 安卓 & 苹果 Iphone, Ipad一键设置教程](https://www.wmsoho.com/ssr-for-android-iphone-tutorial/%22 "SSR 安卓 & 苹果 Iphone, Ipad一键设置教程").

### **7\. 关于流量与扣费**

Google Cloud Platform 计算引擎是按小时收费，网络按流量收费.  
我们掏出笔来算一算:

注册赠送了300美金, 一年免费使用期.  
主机我们选的是微型(最低配置机型), $5/月.  
还剩300-5\*12=$240, 用于抵扣流量的费用.  
谷歌云服务器出口大陆流量1T以内价格约为0.23$/1G.  
那么每个月可用流量 = 240/12/0.23 ≈ 86G, 一般日常使用绝对够用, 当然你还可以顺便搭个网站之类的 🙂

#### **8\. 如何查看使用量及余额**

进入结算概览页面: [https://console.cloud.google.com/billing/](https://console.cloud.google.com/billing/%22 "https://console.cloud.google.com/billing/")

可以看到剩余的赠金和天数.

> 如果一开始你区域选的是香港, 这里会显示余额有2000多美金. 不要惊讶, 那是港币~ 港币也是美元符号$

点击左侧交易, 以及图中下方相关联的项目名称, 可以查看使用量和扣费明细.

![结算概览查看使用量明细和赠金余额](https://www.wmsoho.com/wp-content/uploads/2b9e52e7517b57a2f70ef03b2fa599d2.png "结算概览查看使用量明细和赠金余额")

### **9\. 增加Swap交换分区与调优**

GCP默认没有Swap交换分区, 如果0.6G内存不够用, 可以手动增加Swap交换分区并且调整对应的内核参数. 参考文章: [Google Cloud Platform 增加Swap交换分区与调优](https://www.wmsoho.com/google-cloud-platform-add-swap-and-tuning/%22 "Google Cloud Platform 增加Swap交换分区与调优")

### **10\. 如何开启root登录 (非必须)**

出于安全因素, Google Cloud Platform默认是以普通用户密钥认证的方式登录的, 禁止root登录. 如需要root权限可以使用命令`sudo -i`进行切换.  
如果需要直接使用root登录,按如下设置:

先切换到root  
`sudo -i`

创建.ssh目录并修改权限, 然后复制普通用户的密钥文件到root相应目录下. 用户名注意换成你自己的.  
`mkdir .ssh &amp;&amp; chmod 700 .ssh &amp;&amp; cp /home/你的gmail用户名/.ssh/authorized_keys .ssh`

修改SSH配置文件  
`vim /etc/ssh/sshd_config`  
将`PermitRootLogin no`改为`PermitRootLogin yes`并保存

重启SSH服务  
`systemctl restart sshd.service`

测试是否设置成功

处于安全考虑, 还可以禁用密码登录 (可选), 修改默认的22端口(可选)

### **11\. FAQ**

**Q:** 运行某些命令提示 `*** Operation not permitted`或者`You need to be root to perform this command.`?  
**A:** 一般是权限不够. 管理GCP时, 记得先`sudo -i`切换到root用户, 否则某些命令无法运行.

**Q:** 区域选的是asia-east1-c台湾机房, 为什么ping显示是美国IP?  
**A:** 正常. GCP都是美国IP, 没有地区本地的IP, 也就是Anycast IP. IP地址是谷歌从美国申请的, 但是可用于全球任何一个机房, 因为谷歌是采用网间结算(IP Transit)服务，直接指定部分段与某地区运营商进行连接. 详细机房位置参考: [](https://cloud.google.com/compute/docs/regions-zones/#available%22 "https://cloud.google.com/compute/docs/regions-zones/#available"). 另外可用`tracert`或`mtu`命令测试路由与节点. Ping的话建议 www.ipip.net.

**Q:** 还能更快吗?  
**A:** 可以但是不建议, 关键词: `魔改版BBR`, 自行搜索

**Q:** 装BBR脚本时, 手动更换内核，重启后，整个磁盘变为只读怎么办?  
**A:** 执行命令`mount -o remount rw /`即可恢复.

**Q:** 根据谷歌的Always Free政策, us-east1, us-west1, and us-central1这三个地区的微型机器永久免费, 能用吗?  
**A:** 可以忽略. 流入中国和澳大利亚的流量要收费, 如下图. 更多限制政策请参考:[](https://cloud.google.com/compute/pricing#freeusage%22 "https://cloud.google.com/compute/pricing#freeusage")  
![](https://www.wmsoho.com/wp-content/uploads/0094636f46ffefe97992b09634633aad.png "Google Cloud Platform的Always Free政策使用限制")

**Q:** Google Cloud Platform能用来建站吗?  
**A:** 当然可以, 不过长期建站还是推荐用[Linode](https://www.wmsoho.com/go/linode%22 "Linode"). GCP流量是要扣费的. 另外外贸站建议机房使用目标客户所在国家. 国内站的话, 反正百度是不咋待见国外服务器的.



### 德国机场euserv安装宝塔

#### 连接ssh 到<https://www.heyterm.com/>这个网站注册帐号，并新建资产，将EUSERV的IPV6地址填进去。用户名是root,密码是前面SERVERDATA中的密码。   就可以SSH连接上你的VPS了。

三、安装宝塔
1、连接上你的VPS后，先执行下面命令：

```
echo -e "nameserver 2001:67c:2b0::4\nnameserver 2001:67c:2b0::6" > /etc/resolv.conf
```
这个命令的作用，是设置你的VPS的DNS解析，让你的VPS可以访问IP4网络并下载IP4网络的资源，不设置是无法访问的。也就无法下载宝塔进行安装。

然后编辑`/etc/yum/pluginconf.d/fastestmirror.conf`，改一下

```
vi /etc/yum/pluginconf.d/fastestmirror.conf
```

然后进入相应的编辑界面，修改如下的代码，将1改为0，如下

```
enable=0
```

修改完后先按esc键。再输入:wq 然后回车即可。

2、执行命令：

```
yum update
```

安装升级，如果出现错误，说明前面的代码设置有问题。一般是DNS设置的问题，重新执行DNS修改命令，再次保存。

3、安装带 IPv6 的宝塔，执行命令。

```
curl -sSO http://download.bt.cn/install/new_install.sh && bash new_install.sh
```

4、安装宝塔完成后，运行执行命令

```
bt
``` 

如下图

5、分别执行8修改面板端口为8080，5和6修改面板用户名和密码。

注意：这里只能改为8080端口，如果改为其他端口的话无法访问宝塔面版，因为cloudflare到时转发只支持几个端口而已。

6、登录宝塔。这里就用到刚才说放一放的cloudflare设置那一步了。

宝塔面板的登录地址就是你的cloudflare设置的域名：eu.xxx.com：8080/你的面板目录。

注意：如果套了cloudflare后访问还是提示错误521的话，那么就输入如下命令：

```
echo '::' > /www/server/panel/data/ipv6.pl && /etc/init.d/bt restart
```
如果访问还是520错误之类的，有可能没有开启防火墙端口，输入如下命令分别回车：

```
firewall-cmd --permanent --zone=public --add-port=8080/tcp
firewall-cmd --reload
```
如果出现登录面版提示不是安全的入口等提示的话，那按面版上的要求，输入如下命令后，开启互联网上登录面版：

```
rm -f /www/server/panel/data/admin_path.pl
```
ok,输入你设置的宝塔用户名和密码就可以登录宝塔了。

登录宝塔后，安装LNAP。

完整地址<https://www.shopee6.com/web/web-tutorial/euserv-order-v2ray-bt.html>
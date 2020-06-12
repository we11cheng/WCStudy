### 究极解决国行手机部分app不能联网问题
#### 尝试过很多方法都效果都不佳。
* 无论如何更改设置权限，它都不会更改了。
* 删 App 重装，没有用。
* 开关 “无线局域网助理”，没有用。
* 还原网络设置，没有用。
* 尝试各种开关组合，都不会有用的。
* 还原所有设置。

#### 产生原因，往往是在安装了大量的 App 之后（大约 100 多个），并且经常增删。因为这个设置是单独保存的，并且不会随着 App 的删除而清除，随着使用时间的增加，这个文件越来越大，直到其中某个或者某几个条目乱了。

### 究极办法，亲测有效

- 手机越狱
- iPhone上安装SSH，Cydia安装OpenSSH 工具。
- 使用OpenSSH 远程登录。

```
ssh root@192.168.1.102
输入密码 alpine
```
alpine是默认密码
#### 最好修改下 root账户和mobile账户的登录密码，修改root 用户的密码
(一)使用root账户登录

(二)输入passwd 修改root用户的密码

(三)输入passwd mobile 修改mobile 用户的密码

#### 执行清除网络设置
```
iPhone:~ root# cd /var/preferences/

iPhone:/var/preferences root# rm -rf *.networkextension*

iPhone:/var/preferences root# reboot
```
这样每个 App 的联网权限设置均保存在这个文件里，删除这个文件后，所有权限会被还原成 “无线局域网与蜂窝移动”，可以再次按照自己的需求进行设置更改。

#### 非越狱情况下推荐这个<https://github.com/Soulghost/AppleCellularFixup>
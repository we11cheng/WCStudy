### ShadowsocksR&Shadowsocks安装及多用户管理
### 系统要求 CentOS 6+ / Debian 6+ / Ubuntu 14.04 +
### 操作指南
- 登录服务器

- 切换用户权限

```
sudo su
```

- 运行脚本

```
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/ssrmu.sh && chmod +x ssrmu.sh && bash ssrmu.sh
```

- 输入1，开始安装ShadowsocksR服务端，并且会提示你输入Shadowsocks的 端口/密码/加密方式/ 协议/混淆（混淆和协议是通过输入数字选择的） 等参数来添加第一个用户。

- 运行命令

```
bash ssrmu.sh
```

正确返回示例

```
root@ubuntu:~# bash ssrmu.sh 
  ShadowsocksR MuJSON一键管理脚本 [v1.0.25]
  ---- Toyo | doub.io/ss-jc60 ----

  1. 安装 ShadowsocksR
  2. 更新 ShadowsocksR
  3. 卸载 ShadowsocksR
  4. 安装 libsodium(chacha20)
————————————
  5. 查看 账号信息
  6. 显示 连接信息
  7. 设置 用户配置
  8. 手动 修改配置
  9. 清零 已用流量
————————————
 10. 启动 ShadowsocksR
 11. 停止 ShadowsocksR
 12. 重启 ShadowsocksR
 13. 查看 ShadowsocksR 日志
————————————
 14. 其他功能
 15. 升级脚本
 
 当前状态: 已安装 并 已启动

请输入数字 [1-15]：
```
### 相关文件位置
```
安装目录：/usr/local/shadowsocksr

配置文件：/usr/local/shadowsocksr/user-config.json

数据文件：/usr/local/shadowsocksr/mudb.json
```

### 注意：添加/删除/修改 用户配置后，无需重启ShadowsocksR服务端，ShadowsocksR服务端会定时读取数据库文件内的信息，不过修改 用户配置后，可能要等个十几秒才能应用最新的配置（因为ShadowsocksR不是实时读取数据库的，所以有间隔时间）。
### 其他说明
```
ShadowsocksR 安装后，自动设置为 系统服务，所以支持使用服务来启动/停止等操作，同时支持开机启动。

启动 ShadowsocksR：/etc/init.d/ssrmu start
停止 ShadowsocksR：/etc/init.d/ssrmu stop
重启 ShadowsocksR：/etc/init.d/ssrmu restart
查看 ShadowsocksR状态：/etc/init.d/ssrmu status
ShadowsocksR 默认支持UDP转发，服务端无需任何设置。

本脚本已经集成了 安装/卸载 锐速(ServerSpeeder)/Lotserver，但是是否支持请查看 Linux支持内核列表 。（锐速、LotServer不支持OpenVZ）
```
### 查看内核列表<https://www.91yun.co/wp-content/plugins/91yun-serverspeeder/systemlist.html>
### 本文参考链接<https://github.com/ToyoDAdoubi/doubi>
### 2018-8-24更新 另外一个脚本工具SSR-Bash
#### ShadowsocksR多用户管理脚本（基于官方mujson版本）
一个Shell脚本，集成SSR多用户管理，流量限制，加密更改等基本操作。是一个基于ShadowsocksR官方的mujson的辅助脚本。

系统支持

- Ubuntu 14
- Ubuntu 16
- Debian 7
- Debian 8
- CentOS 6
- CentOS 7

安装

```
wget -N --no-check-certificate https://raw.githubusercontent.com/mucut/SSR-Bash-Python/master/install.sh && bash install.sh
```

卸载

```
wget -N --no-check-certificate https://raw.githubusercontent.com/mucut/SSR-Bash-Python/master/uninstall.sh && bash uninstall.sh
```

自检

```
wget -N --no-check-certificate https://raw.githubusercontent.com/mucut/SSR-Bash-Python/master/self-check.sh && bash self-check.sh
```
### ubuntu防火墙

开启防火墙

```
sudo ufw enable
```

关闭防火墙

```
sudo ufw disable
```

查看防火墙状态

```
sudo ufw status
```
使用范例

```
允许 53 端口
$ sudo ufw allow 53
禁用 53 端口
$ sudo ufw delete allow 53
允许 80 端口
$ sudo ufw allow 80/tcp
禁用 80 端口
$ sudo ufw delete allow 80/tcp
允许 smtp 端口
$ sudo ufw allow smtp
删除 smtp 端口的许可
$ sudo ufw delete allow smtp
允许某特定 IP
$ sudo ufw allow from 192.168.254.254
删除上面的规则
$ sudo ufw delete allow from 192.168.254.254
```
补充：防火墙开了以后ssh连不上，需要到服务器管理面板登陆再关闭防火墙。

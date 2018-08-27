### 配置阿里云ECS支持IPv6
#### 国内大部分服务器是不支持 IPv6 的，不过我们可以使用 HURRICANE ELECTRIC (下面我简称HE)提供的免费 IPv6 通道，自己手动配置 IPv6 。

- 首选去 HE 注册一个账号，地址 <https://www.tunnelbroker.net> 。然后点击 Create Regular Tunnel ，IPv4 Endpoint 项填写你的 ECS 外网 IPv4 地址，Available Tunnel Servers 项选择一个通道服务器并完成通道创建。通道服务器根据自己需求去选择，我选择的是 Tokyo, JP 。如图：

![](https://github.com/we11cheng/WCImageHost/raw/master/WX20180827-135750.png)

- 使用 SSH 登录服务器，修改 `/etc/sysctl.conf` 文件，将下图三行配置改成0。

![](https://github.com/we11cheng/WCImageHost/raw/master/WX20180827-140311.png)

- 修改 `/etc/network/interfaces` 文件，在底部添加下面配置信息。

```
auto he-ipv6
iface he-ipv6 inet6 v4tunnel
address <IPv6>::2
netmask 64
remote <HE Server IPv4>
local <阿里云 内网 IPv4>
endpoint any
ttl 255
gateway <IPv6>::1
up ip -6 route add 2000::/3 via ::<HE Server IPv4> dev he-ipv6
up ip -6 addr add <IPv6>::1:1/128 dev he-ipv6
up ip -6 addr add <IPv6>::2:1/128 dev he-ipv6
down ip -6 route flush dev he-ipv6
```
##### 需要注意的地方：其中 <IPv6> 是 HE 里的 Server IPv6 Address ，但是不带 ::1/64 注意 <> 也一起替换掉哦。强调下 local 是阿里云ECS的内网 IPv4 地址，很多人配置出错就是这里出了问题。
配上我当时的配置文件，可以做个参考
```
auto he-ipv6
iface he-ipv6 inet6 v4tunnel
address 2001:470:23:8e7::2
netmask 64
remote 74.82.46.6
local 172.16.174.227
endpoint any
ttl 255
gateway 2001:470:23:8e7::1
up ip -6 route add 2000::/3 via ::<HE Server IPv4> dev he-ipv6
up ip -6 addr add 2001:470:23:8e7::1:1/128 dev he-ipv6
up ip -6 addr add 2001:470:23:8e7::2:1/128 dev he-ipv6
down ip -6 route flush dev he-ipv6
```

- 配置完成后重启服务器后，重新使用 SSH 登录服务器，执行下面命令确认 IPv6 已经启用。

```
ifup he-ipv6
```

- 如果看到下面文字，则说明启用成功，没有看到请按照流程重新检查你的配置信息。

```
ifup: interface he-ipv6 already configured
```

- 监听IPv6端口。我使用的是 Nginx ，如果你使用的是 Apache ，原理是一样的，具体配置修改请自行查阅。

```
server {
    listen 80 default_server;
    listen [::]:80 default_server;
 
    root /var/www/api/public;
    index index.php index.html index.htm;
 
    server_name cp.6ag.cn;
 
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
 
    location ~ \.php$ {
        try_files $uri /index.php =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass unix:/var/run/php7.1-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
 
}
```

- 修改了配置文件记得重启nginx服务

```
sudo service nginx restart
```

- 添加AAAA解析,注意是添加AAAA解析，而不是直接修改原来的A记录解析，否则可能手机蜂窝网会无法访问。解析记录值填写 HE 里的 Client IPv6 Address 地址，但需要去掉 /64 。完成后如下所示：

![](https://github.com/we11cheng/WCImageHost/raw/master/WX20180827-141240%402x.png)

### 检测是否支持ipv6，参考文章<http://blog.51cto.com/lwm666/1955704>


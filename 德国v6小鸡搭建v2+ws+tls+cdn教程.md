### 德国iPv6小鸡搭建v2+ws+tls+cdn教程 <https://www.euserv.com/>

#### 记录一下核心的东西，旁枝细节不多说。有问题私我微信id:`1396539283`

- 准备一个域名，免费地址<https://www.freenom.com/>
- <https://dash.cloudflare.com/> 添加域名解析
![](https://raw.githubusercontent.com/we11cheng/picBed/master/20200410180118.png)
![](https://raw.githubusercontent.com/we11cheng/picBed/master/20200410180231.png)
- 等待域名生效
- 由于小鸡只能iPv6连接，所以我们可以用手机4G网络共享给电脑，再shh登陆服务器
- `ssh -p 22 root@2a02:0180:0006:0001:0000:0000:0000:47b6`执行类似命令
- ssh成功登陆就成功了一半。

首先修改DNS，输入以下命令即可。这是为了能访问v4网址。

```
echo -e "nameserver 2001:67c:2b0::4 \nnameserver 2001:67c:2b0::6" > /etc/resolv.conf

```


安装unzip，解压v2ray需要。还有psmisc，防止某些系统不自带killall。以下命令是针对debian的。其他系统自行百度安装

```
apt update
apt install unzip psmisc
```


新建文件夹`/root/v2`，并移动过去。后面所有操作都在这里进行。

```
mkdir -p /root/v2
cd /root/v2
```

下载caddy和v2ray。默认下载的是Linux amd64位版本。

```
wget --no-check-certificate "https://caddyserver.com/download/linux/amd64?license=personal&telemetry=off" -O - | tar -xzv caddy
wget --no-check-certificate "https://github.com/v2ray/v2ray-core/releases/download/v4.22.1/v2ray-linux-64.zip" && unzip -o v2ray-linux-64.zip v2ray v2ctl geosite.dat geoip.dat && rm v2ray-linux-64.zip
chmod +x caddy v2ray v2ctl
```

新建文件：Caddyfile。这是caddy的配置文件。
example.com是域名/hello,路径，换成你自己的。
注意保留http://，也就是不申请证书，避免玄学问题。CF的加密模式要选择

```
touch Caddyfile

```
```
http://example.com {
    log stdout
    root /root/share
    browse
    proxy /hello 127.0.0.1:10000 {
      websocket
      header_upstream -Origin
    }
}
```

（可选）新建文件夹：/root/share。这是caddy的根目录。只是创建而已，不要移动过去。
如果该文件夹不存在，访问域名会404，其实也无所谓。如果存在，访问域名会显示该文件夹，注意不要放敏感文件。

```
mkdir -p /root/share
```

新建文件：config.json。这是v2ray的配置文件。`"id":"d80d1881-a00f-7153-1740-0ccff5b916ca"``"path": "/hello"`自行替换
分别是UUID和路径，换成你自己的。这里的路径要和上面的Caddyfile一致。
如果没有UUID，执行`./v2ctl uuid`，会随机输出一个。

```
{
    "log": {
        "loglevel": "info"
    },
    "inbounds": [
        {
            "protocol": "vmess",
            "port": 10000,
            "listen": "127.0.0.1",
            "settings": {
                "clients": [
                    {
                        "alterId": 64,
                        "id": "d80d1881-a00f-7153-1740-0ccff5b916ca"
                    }
                ]
            },
            "streamSettings": {
                "network": "ws",
                "wsSettings": {
                    "path": "/hello"
                }
            }
        }
    ],
    "outbounds": [
        {
            "tag": "direct",
            "protocol": "freedom"
        }
    ]
}
```

- 新建文件：start.sh。这是启动脚本。总之就是在后台运行./v2ray和./caddy。

```
touch start.sh
```

```
#!/bin/bash

killall caddy v2ray

./caddy > caddy.log 2>&1 & 
./v2ray > v2ray.log 2>&1 &

[ -z "$(pidof caddy)" ] || echo "caddy started"

[ -z "$(pidof v2ray)" ] || echo "v2ray started"
```

- 最后运行bash start.sh即可启动。输出caddy started和v2ray started表示成功，否则请看caddy.log和v2ray.log的报错内容。

```
bash start.sh
```
当前目录除了以下文件，其他都可以删除。

```
v2ray v2ctl geosite.dat geoip.dat caddy Caddyfile config.json start.sh
```

#### 回答常见问题

```
1. 客户端怎么配置？
地址：你的域名
端口：443
用户ID：你的UUID
传输协议：ws
path：你的路径
底层传输安全：tls

其他都不要填，保持默认

2. 为什么速度很慢？
套了CF以后，你的域名会解析到2个v4和2个v6地址。如果你的电脑有v6地址，电脑很可能会优先走v6，导致速度很慢。解决方法是在路由器里禁用v6，或使用自选IP。

3. 不套CF怎么搭建？
唯一的区别就是把Caddyfile里的http://去掉，并且手动运行一次./caddy，看到证书申请完再ctrl+c关掉。其他步骤都一样，客户端配置也一样。

4. 为什么v2ray/caddy起不来？
八成是配置文件有问题，看caddy.log和v2ray.log的报错内容。
```
### 连接ssh方法<https://console.heyterm.com/>
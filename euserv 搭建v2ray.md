### euserv 搭建v2ray

其实这个VPS安装V2RAY跟正常安装没什么区别了，详细步骤如下。

一个特别需要注意的地方是，在客户端配置那里，在IP4的情况下，地址是输入VPS的IP地址，但这个VPS要输入你的域名，就是前面设置

eu.xxx.com.

1、SSH下命令行安装unzip

```
yum install -y unzip zip
```
安装完成后又新建文件夹/root/v2

```
mkdir -p /root/v2
cd /root/v2
```
这样我们就进入了V2文件夹。

然后分别下载caddy和v2ray。默认下载的是Linux amd64位版本。每段命令后回车。

```
wget --no-check-certificate "https://caddyserver.com/download/linux/amd64?license=personal&telemetry=off" -O - | tar -xzv caddy
wget --no-check-certificate "https://github.com/v2ray/v2ray-core/releases/download/v4.22.1/v2ray-linux-64.zip" && unzip -o v2ray-linux-64.zip v2ray v2ctl geosite.dat geoip.dat && rm v2ray-linux-64.zip
chmod +x caddy v2ray v2ctl
```
新建文件：Caddyfile。这是caddy的配置文件。

```
touch Caddyfile
```
编辑Caddyfile文件

```
vi Caddyfile
```
复制如下的代码，记得改为自己的网址和文件夹：

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
然后退出并用:wq保存退出。

新建文件：config.json。这是v2ray的配置文件。

```
touch config.json
```
然后编辑这个文件

vi config.json
复制如下的代码，注意路径及端口，如果没有ID则可输入./v2ctl uuid代码生成1个，然后替代下面的ID：

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
然后继续保存退出来

新建文件：start.sh。这是启动脚本。总之就是在后台运行./v2ray和./caddy。

```
touch start.sh
```
继续编辑文件，复制如下代码：

```
#!/bin/bash

killall caddy v2ray 2>/dev/null

./caddy > caddy.log 2>&1 &
./v2ray > v2ray.log 2>&1 &
sleep 5
[ -z "$(pidof caddy)" ] || echo "caddy started"
[ -z "$(pidof v2ray)" ] || echo "v2ray started"
```
保存退出。

2、、宝塔面板-网站-设置-配置文件

打开宝塔的网站配置，即网站-设置-配置文件里面，在最后一个 } 前加入以下代码

注意端口、目录要与前面一致

```
location /hello {     //此处目录第八条设置的目录一致。
        proxy_redirect off;
        proxy_pass http://127.0.0.1:10000;     //此处端口要与第八条设置的端口一致。
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $http_host;
        # Show realip in v2ray access.log
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
```
然后在宝塔里面开放设置的10000端口。注意，要开放10000端口。然后在宝塔里重启NIGNX。
### 基于官方脚本V2ray在宝塔面板下WebSocket+TLS+Nginx的使用

首先，你需要准备一个域名添加A记录解析到你的服务器。
ssh到vps，切换到root，安装BT面板 	<https://www.bt.cn/>
成功安装后，登录宝塔，成功登录后会弹出推荐安装套件，选择一键安装相应的推荐服务，建议编译安装，不容易报错。
安装完成后
接下来开始安装v2ray

安装v2ray

```
  bash <(curl -L -s https://install.direct/go.sh) # 直接使用脚本
  service v2ray start # 启动
  vim /etc/v2ray/config.json # 修改配置文件 
```
服务器配置

uuid生成<https://1024tools.com/uuid>

```
{
"log" : {
  "access": "/var/log/v2ray/access.log",
  "error": "/var/log/v2ray/error.log",
  "loglevel": "warning"
},
"inbound": {
  "port": 233, #自动生成的端口，须与Nignx设置的相一致，可自定义
  "protocol": "vmess",
  "settings": {
    "clients": [
      {
        "id": "4a7eb780-ede5-4973-9ac6-a1f1afa210cc", #自动生成的UUID
        "level": 1,
        "alterId": 64
      }
    ]
  },
  "streamSettings": {
  "network":"ws",
  "wsSettings": {
  "path": "/ws", #path可自定义，这里是/ws，须与Nginx和客户端的path相一致
  "headers": {
  "Host": "www.bilibili.com" #Host可自定于任意域名，此处没有添加
  }
  }
  }
},
"outbound": {
  "protocol": "freedom",
  "settings": {}
},
"outboundDetour": [
  {
    "protocol": "blackhole",
    "settings": {},
    "tag": "blocked"
  }
],
"routing": {
  "strategy": "rules",
  "settings": {
    "rules": [
      {
        "type": "field",
        "ip": [
          "0.0.0.0/8",
          "10.0.0.0/8",
          "100.64.0.0/10",
          "127.0.0.0/8",
          "169.254.0.0/16",
          "172.16.0.0/12",
          "192.0.0.0/24",
          "192.0.2.0/24",
          "192.168.0.0/16",
          "198.18.0.0/15",
          "198.51.100.0/24",
          "203.0.113.0/24",
          "::1/128",
          "fc00::/7",
          "fe80::/10"
        ],
        "outboundTag": "blocked"
      }
    ]
  }
}
}
```
宝塔搭建web页面
新建一个网站，配置ssl，利用宝塔Let's Encrypt免费的ssl。
然后选择配置文件，在最后一个}前一行复制如下代码

```

 location /ws {
 proxy_redirect off;
 proxy_pass http://127.0.0.1:233;
 proxy_http_version 1.1;
 proxy_set_header Upgrade $http_upgrade;
 proxy_set_header Connection "upgrade";
 proxy_set_header Host $http_host;
}
```

重启`v2ray service v2ray restart # 重启`
查看状态 `service v2ray status # 状态`
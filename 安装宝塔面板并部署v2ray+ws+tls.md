### 安装宝塔面板并部署v2ray+ws+tls
##### 问题来源:很多小伙伴可能跟我一样，在VPS上安装了宝塔和v2ray以后，Websocket+TLS方式无法正常使用，原因是宝塔面板会对233脚本的caddy产生影响，导致caddy不能正常工作，所以我们得自己配置TLS才能实现代理分流。OK！废话不多说，直接给它安排起来


- 搭建宝塔并且配置https就不多说了，推荐使用`let’s encrypt`。
 - 用一个二级域名用来v2ray。
- 开始搭建v2ray

```
bash <(curl -s -L https://git.io/v2ray.sh)
```
- 选择4 `WebSocket + TLS`
- 端口号随便取，比如`45678` 端口号记一下。下一步其他步骤有用到。
- 这里的自动配置必须输入n选择不自动配置
- 这里的自动配置必须输入n选择不自动配置
- 这里的自动配置必须输入n选择不自动配置
- 这样已经成功了一半了。
- 修改nginx配置文件

#### 之前
```
server
{
    listen 80;
	listen 443 ssl http2;
    server_name weiguan.ml;
    index index.php index.html index.htm default.php default.htm default.html;
    root /www/wwwroot/weiguan.ml;
    
    #SSL-START SSL相关配置，请勿删除或修改下一行带注释的404规则
    #error_page 404/404.html;
    ssl_certificate    /www/server/panel/vhost/cert/weiguan.ml/fullchain.pem;
    ssl_certificate_key    /www/server/panel/vhost/cert/weiguan.ml/privkey.pem;
    ssl_protocols TLSv1.1 TLSv1.2 TLSv1.3;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
    ssl_prefer_server_ciphers on;
    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 10m;
    error_page 497  https://$host$request_uri;

```

#### server{}新增添加如下代码，端口号需要注意一下

```
 location / {
 		 proxy_redirect off;
        proxy_pass http://localhost:45678; # 34567为刚刚设置的V2Ray端口
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        }
```
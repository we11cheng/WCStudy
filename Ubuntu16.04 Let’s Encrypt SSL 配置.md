### Ubuntu16.04 Let’s Encrypt SSL 配置
### 配置Certbot
- add the repository.

```
sudo add-apt-repository ppa:certbot/certbot
```
#### 找不到命令先安装```apt install software-properties-common```
```
apt install software-properties-common
```
- 更新apt-get

```
sudo apt-get update
```
-  install Certbot's Nginx package

```
sudo apt-get install python-certbot-nginx
```
- 配置nginx域名信息

```
vim /etc/nginx/sites-available/default
```
#### 修改server_name www.example.com;（www.example.com对应自己的域名）
- 检测nginx运行情况

```
sudo nginx -t
```
- reload nginx

```
sudo systemctl reload nginx
```
### 配置防火墙
- 查看防火墙状态

```
sudo ufw status
```
##### 如果是inactive，手动开启
```
sudo ufw enable
```

- 添加配置

```
sudo ufw allow 'Nginx Full'
sudo ufw delete allow 'Nginx HTTP'
```

- 查看状态

```
sudo ufw status
```
##### 返回
```
Status: active

To                         Action      From
--                         ------      ----
Nginx Full                 ALLOW       Anywhere                  
Nginx Full (v6)            ALLOW       Anywhere (v6) 
```

- 配置SSL Certificate

```
sudo certbot --nginx -d example.com -d www.example.com
```

##### 阿里云ipv6配置参考<https://blog.csdn.net/azhonghao/article/details/53726514>

- 自动更新证书

```
sudo certbot renew --dry-run
```

### 参考链接<https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-16-04>





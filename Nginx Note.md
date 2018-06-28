### Nginx Note
### Install Nginx
```
sudo apt-get update
sudo apt-get install nginx
```

#### Check your Web Server

```
systemctl status nginx
```

### eg

```
nginx.service - LSB: starts the nginx web server
   Loaded: loaded (/etc/init.d/nginx; bad; vendor preset: enabled)
   Active: active (running) since Wed 2018-06-27 17:35:47 CST; 34min ago
     Docs: man:systemd-sysv-generator(8)
  Process: 4148 ExecStop=/etc/init.d/nginx stop (code=exited, status=0/SUCCESS)
  Process: 4165 ExecStart=/etc/init.d/nginx start (code=exited, status=0/SUCCESS)
   CGroup: /system.slice/nginx.service
           ├─ 769 nginx: cache manager process                                     
           ├─4167 nginx: master process /www/server/nginx/sbin/nginx -c /www/server/nginx/conf/ngin
           ├─4168 nginx: worker process                                            
           └─4169 nginx: cache manager process                                     

Jun 27 17:35:47 iZbp1ibd5qj8a3dwztxsf8Z systemd[1]: Starting LSB: starts the nginx web server...
Jun 27 17:35:47 iZbp1ibd5qj8a3dwztxsf8Z nginx[4165]: Starting nginx...  done
Jun 27 17:35:47 iZbp1ibd5qj8a3dwztxsf8Z systemd[1]: Started LSB: starts the nginx web server.
Jun 27 18:05:10 iZbp1ibd5qj8a3dwztxsf8Z systemd[1]: Started LSB: starts the nginx web server.
```
### Manage the Nginx Process

```
To stop your web server, you can type:
sudo systemctl stop nginx

To start the web server when it is stopped, type:
sudo systemctl start nginx

To stop and then start the service again, type:
sudo systemctl restart nginx
```
### Check the Nginx port 
```
sudo netstat -anp | grep nginx
```

#### 参考链接 <https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-16-04>
### 2018-6-28 补充
- 版本

```
nginx -v
```
- 检测配置是否正确

```
sudo nginx -t
```
- [emerg]: bind() to 0.0.0.0:80 failed (98: Address already in use) 端口被占用

```
sudo fuser -k 80/tcp
sudo systemctl start nginx
```
- 查看所有端口使用情况

```
sudo netstat -natp
```

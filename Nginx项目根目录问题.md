### Nginx项目根目录问题

- 默认安装位置为`/etc/nginx`
- ls查看

```
conf.d          koi-utf  mime.types  nginx.conf   uwsgi_params
fastcgi_params  koi-win  modules     scgi_params  win-utf
```
- 找到`conf.d `
- `cd conf.d/`
- `vi default.conf `
- 找到

```
location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }
```
- 得到路径`/usr/share/nginx/html`
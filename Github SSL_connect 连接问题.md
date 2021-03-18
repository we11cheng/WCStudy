### Github SSL_connect 连接问题

#### 具体错误

```
fatal: unable to access 'https://github.com/ChinaVolvocars/jd_maotai_seckill.git/': LibreSSL SSL_connect: SSL_ERROR_SYSCALL in connection to github.com:443 
```
#### 解决方法 把https代理改成socks代理
```
vim ~/.gitconfig
```
#### 设置为 `proxy = socks5://127.0.0.1:1080`
```
[http "https://github.com"]
proxy = socks5://127.0.0.1:1080
[https "https://github.com"]
proxy = socks5://127.0.0.1:1080
```
#### 重置git代理，不设置代理
```
git config --global --unset http.proxy
git config --global --unset https.proxy
```
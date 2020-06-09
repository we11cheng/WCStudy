#### git设置和取消代理

##### 本地开启VPN后，GIt也需要设置代理，才能正常略过GFW，访问goole code等网站

设置代理：

```
git config --global http.proxy http://127.0.0.1:1080
git config --global https.proxy https://127.0.0.1:1080
git config --global http.proxy 'socks5://127.0.0.1:1080' 
git config --global https.proxy 'socks5://127.0.0.1:1080'
```

发现clone的时候使用https出现问题

```
fatal: unable to access 'https://github.com/bailicangdu/vue2-elm.git/': Received invalid version in initial SOCKS5 response.
```
取消代理

```
git config --global --unset http.proxy
git config --global --unset https.proxy
```
查看全部的git全局设置

``
git config --global --unset http.https://github.com.proxy
```

只对github.com

```
git config --global http.https://github.com.proxy socks5://127.0.0.1:1080 
git config --global --unset http.https://github.com.proxy
```

举个例子

```
export http_proxy=http://127.0.0.1:1088;export https_proxy=http://127.0.0.1:1088;
```
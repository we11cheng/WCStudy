#### cloudflare 二级域名解析
### 如图

![](https://raw.githubusercontent.com/we11cheng/picBed/master/20200416102922.png)

```
~ » ping weiguan.ml                                                            rby@rbydeMacBook-Pro
PING weiguan.ml (198.181.35.143): 56 data bytes
64 bytes from 198.181.35.143: icmp_seq=0 ttl=49 time=169.003 ms
64 bytes from 198.181.35.143: icmp_seq=1 ttl=49 time=167.311 ms
^C
--- weiguan.ml ping statistics ---
3 packets transmitted, 2 packets received, 33.3% packet loss
round-trip min/avg/max/stddev = 167.311/168.157/169.003/0.846 ms
----------------------------------------------------------------------------------------------------
~ » ping www.weiguan.ml                                                        rby@rbydeMacBook-Pro
PING www.weiguan.ml (198.181.35.143): 56 data bytes
64 bytes from 198.181.35.143: icmp_seq=0 ttl=49 time=165.569 ms
64 bytes from 198.181.35.143: icmp_seq=1 ttl=49 time=167.093 ms
^C
----------------------------------------------------------------------------------------------------
~ » ping www.weiguan.ml                                                        rby@rbydeMacBook-Pro
PING www.weiguan.ml (198.181.35.143): 56 data bytes
64 bytes from 198.181.35.143: icmp_seq=0 ttl=49 time=168.980 ms
64 bytes from 198.181.35.143: icmp_seq=1 ttl=49 time=166.870 ms
^C
--- www.weiguan.ml ping statistics ---
3 packets transmitted, 2 packets received, 33.3% packet loss
round-trip min/avg/max/stddev = 166.870/167.925/168.980/1.055 ms
```
### 补充，三级域名解析
如图
![](https://raw.githubusercontent.com/we11cheng/picBed/master/20200416105137.png)

```
~ » ping gwc.blog.weiguan.ml                                                   rby@rbydeMacBook-Pro
PING gwc.blog.weiguan.ml (198.181.35.143): 56 data bytes
64 bytes from 198.181.35.143: icmp_seq=0 ttl=49 time=182.822 ms
```
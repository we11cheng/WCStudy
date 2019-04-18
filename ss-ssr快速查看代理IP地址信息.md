### ss/ssr快速查看代理IP地址信息

- shadowrocket客户端到`HTTP Proxy Preference`设置，如图

![](https://raw.githubusercontent.com/we11cheng/picBed/master/20190411110317.png)

- 终端输入

```
export http_proxy=http://127.0.0.1:1087
```

- 完成以后，继续输入

```
curl ip.gs

```

- 得到结果

```
Current IP / 当前 IP: 104.199.138.1
ISP / 运营商:  cloud.google.com
City / 城市: Changhua County Taiwan
Country / 国家: China
IP.GS is now IP.SB, please visit https://ip.sb/ for more information. / IP.GS 已更改为 IP.SB ，请访问 https://ip.sb/ 获取更详细 IP 信息！
Please join Telegram group https://t.me/sbfans if you have any issues. / 如有问题，请加入 Telegram 群 https://t.me/sbfans 
```
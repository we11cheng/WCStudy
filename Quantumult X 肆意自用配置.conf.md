### Quantumult X 肆意自用配置.conf

```
# 以 “;” 或 “#” 或 “//“ 开头的行为注释行。
# 此配置是在本人自用配置基础上按照频道用户需求添加修改，给本人频道订阅者使用的，仅供本频道用户使用交流，外来物种勿喷。TG频道：https://t.me/resourcepush_41

# 策略组和task图标：by Orz-3 TG频道：t.me/Orzmini 
[task_local]
# 定时脚本任务

# 京东
# 浏览器登录 https://bean.m.jd.com 点击签到并且出现签到日历
1 0 * * * https://raw.githubusercontent.com/NobyDa/Script/master/JD-DailyBonus/JD_DailyBonus.js, tag=京东, img-url=https://raw.githubusercontent.com/Orz-3/task/master/jd.png,enabled=true
#京东摇钱树
25,56 */2 * * * https://raw.githubusercontent.com/lxk0301/scripts/master/jd_moneyTree.js, tag=京东摇钱树, img-url=https://raw.githubusercontent.com/znz1992/Gallery/master/moneyTree.png, enabled=true
#京东宠汪汪
20,51 */3 * * * https://raw.githubusercontent.com/lxk0301/scripts/master/jd_joy.js, tag=京东宠汪汪, img-url=https://raw.githubusercontent.com/znz1992/Gallery/master/jdww.png, enabled=true
#京东天天加速
15,46 */4 * * * https://raw.githubusercontent.com/lxk0301/scripts/master/jd_speed.js, tag=京东天天加速, img-url=https://raw.githubusercontent.com/znz1992/Gallery/master/jdttjs.png, enabled=true
#东东农场
10,41 7-19/6 * * * https://raw.githubusercontent.com/lxk0301/scripts/master/jd_fruit.js, tag=东东农场, img-url=https://raw.githubusercontent.com/znz1992/Gallery/master/jdsg.png, enabled=true
#京东萌宠
5,36 6-18/6 * * * https://raw.githubusercontent.com/lxk0301/scripts/master/jd_pet.js, tag=京东萌宠, img-url=https://raw.githubusercontent.com/znz1992/Gallery/master/jdmc.png, enabled=true
#种豆得豆
0,31 6-23/2 * * * https://raw.githubusercontent.com/lxk0301/scripts/master/jd_plantBean.js, tag=种豆得豆, img-url=https://raw.githubusercontent.com/znz1992/Gallery/master/jdzd.png, enabled=true
# 喜马拉雅
#打开 APP, 访问下右下角`账号`
1 0 * * * https://raw.githubusercontent.com/chavyleung/scripts/master/ximalaya/ximalaya.js, tag=喜马拉雅, img-url=https://raw.githubusercontent.com/Orz-3/task/master/ximalaya.png, enabled=true

# 快手极速版 (By @Macsuny)
# APP登陆账号后，点击’钱包‘
1 0 * * * https://raw.githubusercontent.com/nzw9314/QuantumultX/master/Task/kuaishou.js, tag=快手极速版, img-url=https://raw.githubusercontent.com/Orz-3/task/master/kuaishou.png, enabled=true

# 中青看点极速版 (By @Macsuny)
# 1.进入app，签到一次,即可获取Cookie. 
# 2.阅读一篇文章，获取阅读请求body，
# 3.在阅读文章最下面有个惊喜红包，点击获取惊喜红包请求
# 4.激励视频获取方法: 关闭vpn，进入任务中心=>抽奖赚点击下面第一个宝箱，出现视频广告页面，然后打开vpn，等待视频播放完毕，点击点我继续领青豆，再重复一次上面操作，获取激励视频请求的body
0 */14 0-20 * * * https://raw.githubusercontent.com/Sunert/Scripts/master/Task/youth.js, tag=中青看点极速版, img-url=https://raw.githubusercontent.com/Orz-3/mini/master/youth.png, enabled=true

# 腾讯视频
# 手机浏览器访问并登录: https://film.qq.com/
1 0 * * * https://raw.githubusercontent.com/chavyleung/scripts/master/videoqq/videoqq.js, tag=腾讯视频, img-url=https://raw.githubusercontent.com/Orz-3/task/master/videoqq.png,enabled=true

# 中国移动签到
# 打开 APP , 进入签到页面, 系统提示: `获取刷新链接: 成功`,然后手动签到 1 次
1 0 * * * https://raw.githubusercontent.com/chavyleung/scripts/master/10086/10086.js, tag=中国移动, img-url=https://raw.githubusercontent.com/Orz-3/task/master/10086.png,enabled=true

# 人人视频
# 打开 APP, 访问下`个人中心`
1 0 * * * https://raw.githubusercontent.com/chavyleung/scripts/master/rrtv/rrtv.js, tag=人人视频, img-url=https://raw.githubusercontent.com/Orz-3/task/master/rrtv.png,enabled=true

[rewrite_local]
# 本地脚本

[server_local]
# 本地节点
# 网易云音乐版权解锁专用节点
# https://raw.githubusercontent.com/nondanee/UnblockNeteaseMusic/master/ca.crt 复制链接到Safari浏览器下载CA证书，然后进入设置>通用>描述文件，安装CA证书，并在设置>通用>关于本机>证书信任设置 开启对CA证书的信任。
http=106.52.127.72:19951, fast-open=false, udp-relay=false, tag=🎧 解锁网易云音乐

[server_remote]
# 机场订阅地址

[general]
server_check_url=http://captive.apple.com/

dns_exclusion_list = *.cmbchina.com, *.cmpassport.com, *.jegotrip.com.cn, *.icitymobile.mobi, *.pingan.com.cn, id6.me

excluded_routes=10.0.0.0/8, 127.0.0.0/8, 169.254.0.0/16, 192.0.2.0/24, 192.168.0.0/16, 198.51.100.0/24, 224.0.0.0/4

geo_location_checker=http://ip-api.com/json/?lang=zh-CN, https://raw.githubusercontent.com/KOP-XIAO/QuantumultX/master/Scripts/IP_API.js

resource_parser_url=https://raw.githubusercontent.com/KOP-XIAO/QuantumultX/master/Scripts/resource-parser.js
profile_img_url=siyi.jpeg

[dns]
server=119.29.29.29
server=223.5.5.5
server=114.114.114.114
server=1.0.0.1
server=8.8.8.8
server=/*.taobao.com/223.5.5.5
server=/*.tmall.com/223.5.5.5
server=/*.alipay.com/223.5.5.5
server=/*.alicdn.com/223.5.5.5
server=/*.aliyun.com/223.5.5.5
server=/*.jd.com/119.28.28.28
server=/*.qq.com/119.28.28.28
server=/*.tencent.com/119.28.28.28
server=/*.weixin.com/119.28.28.28
server=/*.bilibili.com/119.29.29.29
server=/hdslb.com/119.29.29.29
server=/*.163.com/119.29.29.29
server=/*.126.com/119.29.29.29
server=/*.126.net/119.29.29.29
server=/*.127.net/119.29.29.29
server=/*.netease.com/119.29.29.29
server=/*.mi.com/119.29.29.29
server=/*.xiaomi.com/119.29.29.29

[policy]
# 策略组
;ssid=🐳 SSID策略, proxy, proxy, 一般路由器(自己修改WiFi名称): proxy, 翻墙路由器(自己修改WiFi名称): direct, img-url=https://raw.githubusercontent.com/Orz-3/mini/master/ssid.png

static=🐠 漏网之鱼, proxy, direct, img-url=https://raw.githubusercontent.com/Orz-3/mini/master/Final.png

static=🚫 屏蔽系统更新, reject, direct, img-url=https://raw.githubusercontent.com/Orz-3/mini/master/reject.png

static=🍏 苹果服务, direct, proxy, img-url=https://raw.githubusercontent.com/Orz-3/mini/master/Apple.png

static=🌏 国外网站, proxy, direct, img-url=https://raw.githubusercontent.com/Orz-3/mini/master/Global.png

static=💻 国外影视, proxy, direct, img-url=https://raw.githubusercontent.com/Orz-3/mini/master/Streaming.png

static=📱Telegram, proxy, direct, img-url=https://raw.githubusercontent.com/Orz-3/mini/master/Telegram.png

static=🎬 YouTube, proxy, direct, img-url=https://raw.githubusercontent.com/Orz-3/mini/master/YouTube-W.png

static=🖥 Netflix, proxy, direct, img-url=https://raw.githubusercontent.com/Orz-3/mini/master/Netflix-W.png

static=🎵 TikTok, proxy, direct, img-url=https://raw.githubusercontent.com/Orz-3/mini/master/Tiktok.png

static=🎧 解锁网易云音乐, 🎧 解锁网易云音乐, direct, img-url=https://raw.githubusercontent.com/Orz-3/mini/master/neteasemusic.png

# 如何创建机场节点策略
# 1.进入QuantumultX，点击右下角三菱按钮；
# 2.找到节点模块下引用，点击；
# 3.往右滑任意订阅，点击更多即可创建策略；

# 机场节点策略模式解释
# static=静态策略-手动选择： 跟PROXY一样；
# available=健康检查-自动选择： 从第一个节点进行可用性检查，如果可用即选择第一个节点，不可用则继续检查直到节点可用；
# round-robin=负载均衡-轮询调度： 轮流调用节点使用，如果使用该策略则访问谷歌时你的ip是一直变化的；

[filter_remote]
# 分流订阅

https://raw.githubusercontent.com/eHpo1/Rules/master/QuantumultX/Filter/Liby.txt, tag=🚫 广告拦截, force-policy=reject, enabled=true

https://raw.githubusercontent.com/NobyDa/Script/master/QuantumultX/AdRule.list, tag=📵 广告拦截, force-policy=reject, enabled=true

https://raw.githubusercontent.com/eHpo1/Rules/master/QuantumultX/Filter/Sub/Telegram.txt, tag=📱Telegram, force-policy=📱Telegram, enabled=true

https://raw.githubusercontent.com/eHpo1/Rules/master/QuantumultX/Filter/Sub/YouTube.txt, tag=🎬 YouTube, force-policy=🎬 YouTube, enabled=true

https://raw.githubusercontent.com/eHpo1/Rules/master/QuantumultX/Filter/Sub/Netflix.txt, tag=🖥 Netflix, force-policy=🖥 Netflix, enabled=true

https://raw.githubusercontent.com/DivineEngine/Profiles/master/Quantumult/Filter/StreamingMedia/Video/TikTok.list, tag=🎵TikTok, force-policy=🎵 TikTok, enabled=true

https://raw.githubusercontent.com/nzw9314/QuantumultX/master/NeteaseMusic.list, tag=🎧 解锁网易云音乐, force-policy=🎧 解锁网易云音乐, enabled=true

https://raw.githubusercontent.com/eHpo1/Rules/master/QuantumultX/Filter/Apple_CDN.txt, tag=苹果CDN 资源类(建议直连), force-policy=direct, enabled=true

https://raw.githubusercontent.com/eHpo1/Rules/master/QuantumultX/Filter/Apple_API.txt, tag=🍏 苹果API 服务类 (账号所在区), force-policy=🍏 苹果服务, enabled=true

https://raw.githubusercontent.com/eHpo1/Rules/master/QuantumultX/Filter/AsianMedia.txt, tag=📹 国内影视, force-policy=direct, enabled=true

https://raw.githubusercontent.com/eHpo1/Rules/master/QuantumultX/Filter/Domestic.txt, tag=🐼 国内网站, force-policy=direct, enabled=true

https://raw.githubusercontent.com/eHpo1/Rules/master/QuantumultX/Filter/Global.txt, tag=🌏 国外网站, force-policy=proxy, enabled=true

https://raw.githubusercontent.com/eHpo1/Rules/master/QuantumultX/Filter/GlobalMedia.txt, tag=💻 国外影视, force-policy=💻 国外影视, enabled=true

[rewrite_remote]
#远程重写
https://raw.githubusercontent.com/eHpo1/Rules/master/QuantumultX/Rewrite.txt, tag=eHpo1去广告, update-interval=86400, opt-parser=true, enabled=true

https://raw.githubusercontent.com/nzw9314/QuantumultX/master/Js.conf, tag=nzw9314脚本, update-interval=86400, opt-parser=true, enabled=false

https://raw.githubusercontent.com/nzw9314/QuantumultX/master/Get_Cookie_Remote.conf, tag=nzw9314获取Cookie(右滑禁用), update-interval=86400, opt-parser=true, enabled=false


[filter_local]
#本地分流
;user-agent, ?abc*, proxy
;host, www.google.com, proxy
;host-keyword, adsite, reject
;host-suffix, googleapis.com, proxy

#绕过企业证书过期
host, ocsp.apple.com, reject

#迅雷版权问题
host, hub5idx.v6.shub.sandai.net, reject
host, hub5emu.v6.shub.sandai.net, reject
host, hub5btmain.v6.shub.sandai.net, reject


#屏蔽系统更新
host, mesu.apple.com, 🚫 屏蔽系统更新
host, gdmf.apple.com, 🚫 屏蔽系统更新

#去掉YouTube++底部广告
host-suffix, ehg-youtube.hitbox.com, reject

#网易云音乐
host-suffix, music.126.net, direct

ip-cidr, 10.0.0.0/8, direct
ip-cidr, 127.0.0.0/8, direct
ip-cidr, 172.16.0.0/12, direct
ip-cidr, 192.168.0.0/16, direct
ip-cidr, 224.0.0.0/24, direct
geoip, cn, direct
final, 🐠 漏网之鱼

[mitm]
passphrase = UNCLEWANG
p12 = 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

hostname= rdcseason.m.jd.com, *.googlevideo.com, trade-acs.m.taobao.com, 
```
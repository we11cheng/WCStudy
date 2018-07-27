### Clutch砸壳(菜鸡版)

- GitHub下载最新版，<https://github.com/KJCracks/Clutch/releases>

- 2.0.4版本，我存了一份，百度云盘地址链接: <https://pan.baidu.com/s/1OUvaiSKlV2YawG5UG9xxHw> 密码: 2k1c。需要可以自取或者去github下载，上面也给出了下载地址。

- 把下载的Clutch放到越狱的手机的/usr/bin目录下，scp命令或者使用ifunbox传输。

```
admindeMBP-4:~ admin$ scp /Users/admin/Downloads/Clutch-2.0.4 root@192.168.2.116:/usr/bin
```
#### ```/Users/admin/Downloads/Clutch-2.0.4```为电脑端Clutch-2.0.4绝对地址，```root@192.168.2.116```地址为手机ip地址。

- ssh连接手机，切换到/usr/bin目录

```
cd /usr/bin
```

- 赋予权限 +x 可执行权限

```
chmod +x Clutch-2.0.4 
```

- 任意目录下使用

```
Clutch-2.0.4
```

### Clutch-2.0.4使用说明
```
iPersistence:~ root# Clutch-2.0.4
Usage: Clutch-2.0.4 [OPTIONS]
-b --binary-dump <value> Only dump binary files from specified bundleID 
-d --dump <value>        Dump specified bundleID into .ipa file 
-i --print-installed     Print installed applications 
   --clean               Clean /var/tmp/clutch directory 
   --version             Display version and exit 
-? --help                Display this help and exit 
-n --no-color            Print with colors disabled 
```

##### -b 砸特定bundleID二进制文件 
#### -d 生成解密ipa 
#### -i 打印安装的应用

### 查看app主进程及extension/widget进程，正确返回示例

```
iPersistence:~ root# ps -e |grep var
18565 ??         1:48.30 /private/var/containers/Bundle/Application/D88DAA97-33FA-4D27-A172-A320897050E3/SogouInput.app/PlugIns/com.sogou.sogouinput.BaseKeyboard.appex/com.sogou.sogouinput.BaseKeyboard
18568 ??         0:00.57 /var/containers/Bundle/Application/13327F8E-DF9D-4267-BA76-8145A94353E0/News.app/News
18874 ??        19:02.85 /var/containers/Bundle/Application/7995536A-8D35-493A-AFF5-AD4C9972F0EE/WeChat.app/WeChat
18959 ??         0:00.26 /private/var/containers/Bundle/Application/4E4A7584-F6CF-49F0-A0DB-EA989795F5BE/Youkui4Phone.app/PlugIns/UserNotifications.appex/UserNotifications
19091 ??         1:18.71 /var/containers/Bundle/Application/A1372D71-85FF-462A-A7CE-00FF74258C4B/AlipayWallet.app/AlipayWallet
19228 ??         0:00.10 /private/var/containers/Bundle/Application/85533DF6-6883-4B50-8A22-47DC346B18FC/mttlite.app/PlugIns/QQBrowserNotiService.appex/QQBrowserNotiService
19356 ??         0:03.83 /var/containers/Bundle/Application/E50FADAF-C87C-4595-8F4B-75385AB5AC9F/CTRIP_WIRELESS.app/CTRIP_WIRELESS
19427 ??         0:45.66 /var/containers/Bundle/Application/BAF45FB3-6DD1-4247-875E-4AE426BBDCEC/jiongji100.app/jiongji100
19432 ??         0:10.96 /var/containers/Bundle/Application/9C748C68-2C0C-4AF9-A316-27BB6D8A364A/PeanutApp.app/PeanutApp
19434 ??         0:02.10 /var/containers/Bundle/Application/3C59DC21-A4CC-4E5D-8DE7-4436713ABDC3/QQPim.app/QQPim
19438 ??         0:01.60 /var/containers/Bundle/Application/85533DF6-6883-4B50-8A22-47DC346B18FC/mttlite.app/mttlite
19464 ??         1:19.02 /var/containers/Bundle/Application/CAEC68C1-A2B0-420C-973D-13C3004BFC83/QQMusic.app/QQMusic
19505 ??         0:02.56 /private/var/containers/Bundle/Application/7BAF66E3-D357-4E11-967A-3B909BC1A01C/MQQSecure.app/PlugIns/MQQSecureToday.appex/MQQSecureToday
19507 ??         0:13.09 /private/var/containers/Bundle/Application/D5BAA334-38BD-4E19-80C9-7224626963E8/CPU DasherX.app/PlugIns/DasherWidget.appex/DasherWidget
19510 ??         0:02.55 /private/var/containers/Bundle/Application/1A871134-B134-4737-B1AD-A75F5911816B/Shadowrocket.app/PlugIns/Today.appex/Today
19543 ??         0:04.82 /var/containers/Bundle/Application/73C3C0C7-8996-48BD-BF36-5FF488487DB1/PPHub.app/PPHub
19861 ??         0:32.03 /var/containers/Bundle/Application/2B5F8696-10EE-4EFC-864D-D96E6C40C510/kiwi.app/kiwi
19890 ??         0:05.17 /var/containers/Bundle/Application/001E0DF0-1CB3-4A91-A772-4DE60C405675/SecurityCenter.app/SecurityCenter
19892 ??         0:05.50 /private/var/containers/Bundle/Application/BD65D960-30E9-4F5C-89E0-6351D13BDC9A/NetworkSpeed.app/PlugIns/state.appex/state
19922 ??         0:04.87 /var/containers/Bundle/Application/1A871134-B134-4737-B1AD-A75F5911816B/Shadowrocket.app/Shadowrocket
19933 ??         0:07.47 /var/containers/Bundle/Application/69E8FB2D-564E-4B0A-B417-815EFDDE32B0/IHexin.app/IHexin
20010 ??         0:01.22 /var/containers/Bundle/Application/BF6FF444-E39E-4A7F-87CE-CDACAE0A94A9/Wingy.app/Wingy
20018 ??         0:00.06 /private/var/containers/Bundle/Application/79FE9E1B-BAB4-4276-9900-27BC42757695/cmblife.app/PlugIns/CMBLifeNotificationService.appex/CMBLifeNotificationService
20027 ??         0:07.26 /var/containers/Bundle/Application/2352565F-7F91-4532-BABF-C350A6084A86/Weibo.app/Weibo
20029 ??         0:00.25 /private/var/containers/Bundle/Application/D5D3E3E5-58A0-411F-97D7-19FBA3263374/live4iphone.app/PlugIns/NotificationSeviceExtension.appex/NotificationSeviceExtension
20038 ??         0:00.03 /private/var/containers/Bundle/Application/855D5058-822A-4C8C-986E-561F80759DA0/CodoonSport.app/PlugIns/CodoonSport UserNotification Service Extension.appex/CodoonSport UserNotification Service Extension
20126 ??         0:41.84 /var/containers/Bundle/Application/3B4BE59D-A09A-4F7D-970F-8DD7BFFB5347/TIM.app/TIM
20154 ??         0:00.25 /private/var/containers/Bundle/Application/2352565F-7F91-4532-BABF-C350A6084A86/Weibo.app/PlugIns/NotificationServiceExtension.appex/NotificationServiceExtension
20167 ttys000    0:00.01 grep var
iPersistence:~ root# 
```

### Clutch-2.0.4 -i，正确返回示例

```
iPersistence:~ root# Clutch-2.0.4 -i
Installed apps:
1:   Product Hunt <com.producthunt.producthuntbundleid>
2:   豆丁书房-4亿文档图书随意看 <com.docin.DocInBookshelfHDHuZhao>
3:   百词斩-背单词、学英语必备 <com.chaoui.jiongji100CN>
4:   加密相册管家·您的照片保险箱 <com.mihewo.wangzhezs>
5:   赤兔-LinkedIn领英旗下职场分享App <com.linkedin.chitu>
6:   触动力 - 你身边的科技百科 <com.hitnology.hitplayer>
7:   Xender, File Transfer, Sharing <cn.shanchuan.shanchuan>
8:   Steward-玩客云管家 <com.Techsen.StewardStore>
9:   Antidote for Tox <me.dvor.Antidote>
...
```

### Clutch-2.0.4 -d，```com.util.box```为目标app的bundleID，执行命令前确保打开了app并且使用了extension、widget，正确返回示例

```
iPersistence:~ root# Clutch-2.0.4 -d com.util.box
Zipping BNTPoint.app
ASLR slide: 0x100060000
Dumping <PacketTunnelProvider> (arm64)
Patched cryptid (64bit segment)
ASLR slide: 0x10001c000
Dumping <BNTPoint> (arm64)
Patched cryptid (64bit segment)
Writing new checksum
Writing new checksum
Dumping <lwip> arm64
Dumping <Sodium> arm64
Successfully dumped framework lwip!
Dumping <Resolver> arm64
Dumping <Yaml> arm64
Dumping <MMDB> arm64
Dumping <CocoaLumberjack> arm64
Dumping <CocoaAsyncSocket> arm64
Successfully dumped framework Resolver!
Successfully dumped framework MMDB!
Dumping <tun2socks> arm64
Dumping <CocoaLumberjackSwift> arm64
Successfully dumped framework Sodium!
Successfully dumped framework CocoaLumberjackSwift!
Successfully dumped framework CocoaLumberjack!
Successfully dumped framework Yaml!
Successfully dumped framework tun2socks!
Child exited with status 0
Child exited with status 0
Successfully dumped framework CocoaAsyncSocket!
Child exited with status 0
Child exited with status 0
Child exited with status 0
Child exited with status 0
Child exited with status 0
Child exited with status 0
Child exited with status 0
Dumping <NEKit> arm64
Successfully dumped framework NEKit!
Zipping CocoaAsyncSocket.framework
Zipping CocoaLumberjack.framework
Zipping CocoaLumberjackSwift.framework
Zipping MMDB.framework
Child exited with status 0
Zipping NEKit.framework
Zipping Resolver.framework
Zipping Sodium.framework
Zipping Yaml.framework
Zipping lwip.framework
Zipping tun2socks.framework
Zipping PacketTunnelProvider.appex
DONE: /private/var/mobile/Documents/Dumped/com.util.box-iOS9.3-(Clutch-2.0.4)-2.ipa
Finished dumping com.util.box in 8.6 seconds
```
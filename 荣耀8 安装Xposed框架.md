### 荣耀8 安装Xposed框架

####  刷机准备：`打开usb调试`，`关闭查找我的手机`，`电脑上安装好荣耀8的手机驱动（下载刷机精灵成功连接手机一次即可）` 三个步骤缺一不可。
#### 解锁：嫌麻烦的万能的淘宝解决一下(不是很贵)
#### 刷入第三方rec：手机连接电脑，打开K-RecTools_95x_EMUI5_PC，双击运行Run.bat，按提示操作，刷入成功后检验一下，拔掉数据线，关机，长按【电源键】和【音量+】，手机震动后送开电源键，保持音量+直至进入twrp页面，下面的条条右划进入twrp，点击挂载，点击选择存储器，查看内置存储器，SD卡，OTG是否有挂载了内存，如果有会显示多少MB，如果三项均为0，则需解密data分区(刷入第三方rec不会清空数据，开机后仍然是刷入前的样子）百度网盘备份文件，链接:<https://pan.baidu.com/s/1X3SORuqCSyjCXv5Mjp0NRg> 提取码: `wbq4`
#### 卡刷magisk：将magisk v16.0.zip 链接: <https://pan.baidu.com/s/1c9jiPdo6fC45hNhKl10nbg> 提取码: `djb4` 放到根目录下或者任何一个你自己知道怎么找到的文件夹里，进入twrp，点击安装，找到magisk v16.0.zip刷入，刷入成功后重启，安装apk 【Magisk Manager】链接: <https://pan.baidu.com/s/1NjFEMnH1IMucrZ4f2dVrRg> 提取码: `pnww`，（刷入18.0也可以获取root，但是据说18.0不稳定），做到这里你就已经可以授予应用root权限了。
#### 刷入xp：安装apk XposedInstaller_3.1.5，资源见链接: <https://pan.baidu.com/s/10PjJNYaBcUAkjcCgxmmOBg> 提取码: `7vf2`, 卡刷包可以进入<http://dl-xda.xposed.info/framework/>。找到与当前手机机型对应的包，里面还有卸载包。

![](https://raw.githubusercontent.com/we11cheng/picBed/master/20200708162808.png)
#### 选择合适的zip包，进入twip模式，刷入即可。zip链接:<https://pan.baidu.com/s/1QKQO8RWpCCv4k08-flcY1A> 提取码: `srku`
####  资源汇总压缩包链接: <https://pan.baidu.com/s/1nu03TOQQh5BzMbO5F2Yrng> 提取码: `nivv `
##### 参考链接<http://www.laosunit.com/thread-277-1-1.html> <http://www.laosunit.com/thread-279-1-1.html>


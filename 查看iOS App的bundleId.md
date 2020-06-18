### 查看iOS App的bundleId

#### 方法一：用Mac自带工具Console,查看bundle id，已经装在iphone里面的 app:

```
1.用数据线连接手机和Mac.
2.打开Mac 应用Console.
3.在左侧栏Devices中，选择你的设备
4.运行你要查看的APP.
5.在搜索栏里面输入App的名字.
6.你可以看到在process列中找到SpringBoard， Message列中找到 Bootstrapping，点击这一行， bundleId 类似于com.xxxx.xxx.kids 就显示在下图。
```

#### 方法二：在APP下载链接里查看
```
打开Mac的浏览器，搜索app在App Store中iTunes的链接. 比如:
https://itunes.apple.com/us/app/microsoft-outlook/id951937596?mt=8.
拷贝数字在URL中id的后面, 比如951937596.
打开浏览器
https://itunes.apple.com/lookup?id=<Number copied in step 2>.
比如,
https://itunes.apple.com/lookup?id=951937596.
当提示下载text file文本文件, 保存文件. 默认名字是1.txt.
打开1.txt文件，搜索bundleId. 比如:
“bundleId”:“com.microsoft.Office.Outlook”
在这个例子中, bundle ID是： com.microsoft.Office.Outlook.
你还可以检阅其它重要信息比如国际化语言支持, 支持的设备devices, 最低系统版本支持, 发布信息等.
```
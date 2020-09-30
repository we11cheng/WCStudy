### pod install与pod update的区别
#### 终极方案
```
当你需要向向你的项目中安装新的pod库时使用pod install。即使之前你已经有一个Podfile并且执行了pod install，即使你是在向一个已经使用了CocoaPods的项目中添加或移除pod库。
只有当你想要更新pod库的版本时才使用pod update。
```
## 一一详解
### pod install
```
pod install一般是你第一次想要为项目添加pod的时候使用的，它同样也使用在你为Podfile文件添加或移除pod库的时候。

每次pod install命令运行的时候，pod install会为每一个它安装的pod库在Podfile.lock文件中写入其版本号。Podfile.lock文件追踪每一个安装的pod库的版本号，并锁定这些版本号。
当你运行pod install时，它将只解决不在Podfile.lock中的pod库依赖关系
   1.对于在Podfile.lock文件中的pod库，pod install会只下载Podfile.lock文件中指定的版本，而不会去检查这个库是否有更新的版本。
   2.对于不在Podfile.lock文件中的pod库，pod install会搜索这个pod库在Podfile文件中指定的版本
```
### pod outdated
```
每当你运行pod outdated命令时，CocoaPods会列出所有在Podfile.lock中的有新版本的pod库。这意味着当你对这些pod使用pod update PODNAME时，他们会更新（只要新版本仍然遵守你在Podfile中做的类似于pod 'MyPod', '~>x.y'这样的限制）
```
### pod update
```
当你运行了pod update PODNAME命令，CocoaPods会在不考虑Podfile.lock中版本的情况下试着去查找PODNAME的最新版本。pod update PODNAME命令会将相应的pod更新到最新的版本（新版本仍然遵守你在Podfile中做的限制）
```
### 用法
```
通过pod update PODNAME,你可以只更新某个特定的pod库（检查是否存在新版本并更新相应的pod库）。相反，pod install则不会去更新已安装的pod库。

当你向Podfile中添加了pod，你应该使用pod install而不是pod update去在不更新已安装的pod库的版本基础上安装新添加的pod库。

当你想过更新某个特定pod库（或所有的库）的版本时你只需要使用pod update
```
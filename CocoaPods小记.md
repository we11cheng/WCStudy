### CocoaPods小记：
#### CocoaPods的工作主要是通过ProjectName.xcworkspace来组织的，在打开ProjectName.xcworkspace文件后，发现Xcode会多出一个Pods工程。库文件引入及配置：

#### 库文件的引入主要由Pods工程中的Pods-ProjectName-frameworks.sh脚本负责，在每次编译的时候，该脚本会帮你把预引入的所有三方库文件打包的成ProjectName.a静态库文件，放在我们原Xcode工程中Framework文件夹下，供工程使用。
####Resource文件：Resource资源文件主要由Pods工程中的Pods-ProjectName-resources.sh脚本负责，在每次编译的时候，该脚本会帮你将所有三方库的Resource文件copy到目标目录中。
#### 依赖参数设置：在Pods工程中的的每个库文件都有一个相应的SDKName.xcconfig，在编译时，CocoaPods就是通过这些文件来设置所有的依赖参数的，编译后，在主工程的Pods文件夹下会生成两个配置文件，Pods-ProjectName.debug.xcconfig、Pods-ProjectName.release.xcconfig。
#### 使用中遇到的问题：
- install和update命令的配置速度问题：在我们输入pod install或者pod update之后，CocoaPods首先会去匹配本地的spec库，在确认spec版本库不需要更新之后，才会下载相应的库文件，这样比较耗时，有时候，以为是卡死了呢。所以一般使用下面两个命令，跳过spec版本库更新匹配。

```
pod update --verbose --no-repo-update
pod install --verbose --no-repo-update
```
- The dependency **** is not used in any concrete target.

```
The dependency **** is not used in any concrete target.
[!] The dependency UMengAnalytics-NO-IDFA is not used in any concrete target.
```
#### 这个提示是因为，cocoapods升级为1.0以后，Podfile文件书写格式的问题。
```
1.0之前：

   platform :ios
   pod 'UMengAnalytics-NO-IDFA’
   pod 'MBProgressHUD', '~> 0.9.2'
   pod 'FMDB'
   pod 'SDWebImage', '~> 3.7.3'
   pod 'IQKeyboardManager', '~> 3.2.4'
   pod 'MJRefresh', '~> 2.3.2'
   pod 'MJExtension', '~> 0.2.0'
1.0之后：

   platform :ios,’7.0’
   target ‘ProjectName’ do #ProjectName工程名字
   pod 'MBProgressHUD', '~> 0.9.2'
   pod 'FMDB'
   pod 'SDWebImage', '~> 3.7.3'
   pod 'IQKeyboardManager', '~> 3.2.4'
   pod 'MJRefresh', '~> 2.3.2'
   pod 'MJExtension', '~> 0.2.0'
   pod 'UMengAnalytics-NO-IDFA’
   end
```
   
- Unable to satisfy the following requirements: – SDWebImage (~> 3.8) required by Podfile
[!] Unable to satisfy the following requirements: – SDWebImage (~> 3.8) required by Podfile
是因为Podfile文件中写的版本有问题，一般是过大。

- 使用CocoaPods之后，头文件无法自动补齐问题
使用CocoaPods来管理三方库，还是比较方便的，但是突然发现一个美中不足的小问题，在使用import引入文件时，不能自动补齐，需要手工copy文件名，纠结了半天：
解决办法：
Target -> Build Settings ，User Header Search Paths条目中，添加${SRCROOT}或者$(PODS_ROOT)，并且选择Recursive，递归搜索，然后就可以自动补齐了。

#### 在项目中移除CocoaPods三方库配置文件，如果我们在配置CocoaPods的三方库文件后，不在需要了可以移除指定库文件配置，具体步骤如下：

- 删除工程文件夹下的Podfile、Podfile.lock和Pods文件夹；
- 删除xcworkspace文件；
- 打开xcodeproj文件，删除项目中的libpods.a和Pods.xcconfig引用；
- 打开Build Phases选项，删除Check Pods Manifest.lock和Copy Pods Resources；
- 重新pod install


### Cocoapods查看当前项目引入库的版本号&指定版本
####  Cocoapods如何获取项目中引入库的版本号
#### 项目中已经安装过Cocoapods，并生成了Podfile.lock文件。打开终端，cd命令切换到项目中的Podfile.lock文件目录下，执行命令：cat Podfile.lock 即可。也可以用文本方式打开 Podfile.lock 文件

#### Podfile.lock 文件说明如下：
##### Podfile.lock 文件会跟踪每个pod的已安装版本并锁定这些版本(.lock命名因此而来).每次运行pod install命令, 下载并安装新的pod时, 它会为Podfile.lock文件中的每个pod写入已安装第三方库的版本. 运行pod update命令也会写入已安装第三方库的版本。当运行pod install，它只解析Podfile.lock中尚未列在其中的pod的依赖库.对于已经在Podfile.lock中列出的pod, Podfile.lock不会尝试检查是否有更新的版本.对于尚未在Podfile.lock中列出的pod, 会搜索与Podfile匹配的版本或最新的版本.另外执行pod outdated命令，可以查看项目中非最新的第三方库的名字、版本号以及对应的最新的版本号，项目中已经是最新的库不显示。



#### 如何指定版本

```
pod 'AFNetworking'                 //不显式指定依赖库版本，表示每次都获取最新版本
pod 'AFNetworking', '~>0'          //高于0的版本，写这个限制和什么都不写是一个效果，都表示使用最新版本

pod 'AFNetworking', '~> 0.1.2'     //使用大于等于0.1.2但小于0.2的版本
pod 'AFNetworking', '~>0.1'        //使用大于等于0.1但小于1.0的版本

pod 'AFNetworking', '2.0'          //只使用2.0版本
pod 'AFNetworking', '= 2.0'        //只使用2.0版本

pod 'AFNetworking', '> 2.0'        //使用高于2.0的版本
pod 'AFNetworking', '>= 2.0'       //使用大于或等于2.0的版本
pod 'AFNetworking', '< 2.0'        //使用小于2.0的版本
pod 'AFNetworking', '<= 2.0'       //使用小于或等于2.0的版本

pod 'AFNetworking', :git => 'http://gitlab.xxxx.com/AFNetworking.git', :branch => 'R20161010'  //指定分支 

pod 'AFNetworking',  :path => '../AFNetworking'  //指定本地库
```
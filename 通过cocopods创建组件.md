### 通过cocopods创建组件(记录一下)

#### 1、创建远程组件代码库（WCUIKit）github创建一个仓库，比如<https://github.com/we11cheng/WCUIKit>。
#### 2、创建本地仓库.

创建之前，我们想简单介绍一下本地索引库相关东西，如果以前没有创建过本地索引仓库，本地索引仓库应该只有默认的master，所有的本地索引仓库都会存放在下面目录下，你可以去这里查看：

```
~/.cocoapods/repos
```
当然你也可以打开命令行输入一下命令查看：

```
pod repo
```
也可以通过一下命令删除本地索引库

```
pod repo remove 《本地索引库名称》
//举例
pod repo remove WCUIKit
```
创建本地索引仓库
好了，说了那么多，下面进入整体，创建索引库，创建本地索引库也是很简单的，只需要下面一行命令搞定：

```
pod repo add <本地索引库的名字>  <远程索引库的地址> 
//举例
pod repo add WCUIKit https://github.com/we11cheng/WCUIKit.git
```
执行完命令之后可以通过上面介绍的本地索引仓库查看命令查看本地索引仓库是否创建成功。

#### 3、创建WCUIKit组件本地代码库。

首先通过pod lib create命令从cocoapods的模版中初始化自己的本地组件代码库。

```
pod lib create 《组件名》
pod lib create WCUIKit
```
podspec文件参考<https://github.com/we11cheng/WCUIKit>

```
版本号(version)
组件的见到描述(summary)和详细说明(description)
修改主页(homepage)和远程代码库地址(source)
添加组件中的依赖库（比方说你的组件中需要用到AFNetworking）
其他的诸如：development_target,frameworks等根据自己情况修改。
```
接下来就是把组件代码库提交到远程

```
 -  git add .

 - git commit -m "组件初始化"

 - git remote add origin 远程代码仓库地址

 - git push origin master

 - git tag 版本号 （注：这里的版本号必须和podspec里写的版本号一致）

 - git push --tags
```
#### 4、将组件的索引文件提交到远程索引库。

首先我们要验证索引文件的格式是否正确，是否符合cocoapods的要求，验证命令：

```
pod spec lint 《组件索引文件》 --verbose --allow-warnings
//举例
pod spec lint 《ZZFShareKit.podspec》 --verbose --allow-warnings //注：--allow-warnings是为了让组件编译过程中有warning照样能够通过。
```

验证通过后我们就可以将我们组件的索引文件上传到远程索引库里，命令：

```
pod repo push <本地索引库> <索引文件名> --verbose --allow-warnings
//举例
pod repo push ZZFSpecs ZZFShareKit.podspec --verbose --allow-warnings
```
这样就把索引文件上传到远程索引库和本地索引库，自己可以去这两个地方看下是否能够找到。

#### 5、使用自己的组件。
创建一个应用WCUIKitDemo，在项目文件夹创建Podfile，并且输入一下代码

```
source 'https://github.com/we11cheng/WCUIKit'
  
use_frameworks!
target 'WCUIKitDemo' do
pod 'WCUIKit'
end
```

注：

- source：指定索引文件地址，如果不指定，则自动引用cocoapods的索引文件库：https://github.com/CocoaPods/Specs.git，因为我们的组件还没有提交到cocoapods，所以索引库必须指定。下面会讲解怎么将自己的组件上传到cocoapods
- use_frameworks!:这个字段是说组件会被编译成framework使用，否则将会被编译成library（.a）使用

#### 6、将自己的组件上传到cocoapods。

想把自己的组件上传到cocoapods，首先需要有一个cocoapods账号，通过一下命令注册：

```
pod trunk register 《邮箱地址》 《用户名》 --description='描述信息'
```
然后根据命令行输出可以看出自己是否注册成功，如果成功的话会提示让你去邮箱里确认进行激活。
注册成功后，可以通过一下命令查看自己的信息：

```
pod trunk me
```
上传到cocoapods之前还是先需要验证索引文件的是否符合要求，和上面将索引文件提交到远程库一样，使用命令

```
pod spec lint 《WCUIKit.podspec》 --verbose --allow-warnings
```
验证通过之后，就可以真正的将索引文件提交到cocoapods使用命令

```
pod trunk push 《索引文件路径》 --allow-warnings
//举例
pod trunk push WCUIKit.podspec --allow-warnings
```
上传到cocoapods中的组件可以通过以下命令删除

```
pod trunk delete 《组件名》《组件版本号》
```
上传到cocoapods后，我们使用组件的时候就不需要在Podfile中通过source指定索引文件来使用组件了，只需要简单的pod 《组件名》就可以了，如

```
pod 'WCUIKit', '~> 1.0.3'
```








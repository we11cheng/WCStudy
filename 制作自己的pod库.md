### 制作自己的pod库

- 用你自己的GitHub账号创建一个空仓库,创建LICENSE，名字与你本地项目名字相同，记录git地址。

- 将你的本地项目上传到GitHub，并打tag。
 
```
cd 本地工作目录
git add .
git commit -m 'create git'
git remote add origin 远程代码仓库地址
git push origin master
git tag 0.0.1
git push --tags
```
- 创建spec文件并配置。

```pod spec create XX```

#### 参考官方podspec文件<https://guides.cocoapods.org/syntax/podspec.html>
#### 参考他人的podspec文件<https://github.com/jkpang/PPNetworkHelper/blob/master/PPNetworkHelper.podspec>
- 检测podspec语法是否正确 

```
pod spec lint XX.podspec --verbose
```

- 注册Trunk。 参考官方链接<https://guides.cocoapods.org/making/getting-setup-with-trunk.html>

#### eg:
```
pod trunk register gxx@cocoapods.org 'gxx' --description='gxx' macbook pro' --verbose
```

```gxx@cocoapods.org``` - 一个真实存在的邮箱，QQ邮箱即可。

```gxx``` - 用户名

```gxx's macbook pro``` - 描述性文字

- 如果所有的步骤都能成功的话，你会受到一份邮件，需要点击验证下。

#### 补充
```pod trunk me``` 可以查看你已经注册的信息，其中包含你的name、email、since、Pods、sessions，其中Pods为你往CocoaPods提交的所有的Pod！
pod trunk add-owner PPNetworkHelper gwc@cocoapods.org

- 推送到远程CocoaPods列表
```
pod trunk push XX.podspec --verbose
```

- 删除本地CocoaPods索引缓存，执行以下命令

```
pod setup : 初始化,有点慢。
pod repo update : 更新仓库
pod search xxx
```
#### 补充
#### 如果你使用pod search 发现搜索的库是老的库或者search不到，那么你可以这样试一下
```
1、先: pod setup,
2、后: rm ~/Library/Caches/CocoaPods/search_index.json
```

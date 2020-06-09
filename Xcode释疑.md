#### Xcode释疑

- `Copy items if needed`顾名思义就是拷贝文件进项目中。如果不选，则仅仅在项目中引用图片。为了防止忘记图片被引用而不小心删除掉还是勾选上的好。
- `Create groups` 不会在项目中创建真实的文件夹。
-  `Create folder references` 会创建真实的文件夹。

#### 如何更新本地的CocoaPods仓库列表。

1.运行以下命令更新本地的CocoaPods仓库列表： `pod repo update`

2.然后通过以下命令查询 `pod search TLAnimationTabBar`

3.如果仍然查询不到最新版本，可以删除本地仓库重新安装 `sudo rm -rf ~/.cocoapods/repos/master pod setup`
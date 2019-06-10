### Flutter问题小记

#### Q1: 执行`flutter packages get`时一直提示`Running "flutter packages get" in project_name...`
#### A1: 很大可能就是packages获取地址被墙了。具体操作
- Linux 或 Mac

#### 修改当前用户的`~/.bash_profile` vim 打开这个文件，添加

```
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
```
#### `source ~/.bash_profile`让配置生效。
-  Windows

#### 新增两个环境变量

```
PUB_HOSTED_URL ===== https://pub.flutter-io.cn
FLUTTER_STORAGE_BASE_URL ===== https://storage.flutter-io.cn
```
#### Q2: 执行`flutter packages get`时一直提示`Waiting for another flutter command to release the startup lock`
#### A2: 解决方法
1. 打开flutter的安装目录/bin/cache/
2. 删除lockfile文件 
3. 重启AndroidStudio

#### Q3: 正确的添加添加Flutter sdk 路径
#### A3: 解决方法
#### 打开(或创建)` $HOME/.bash_profile.` 文件路径和文件名可能在您的机器上有所不同.
添加以下行并更改`[PATH_TO_FLUTTER_GIT_DIRECTORY]/flutter`为解压后Flutter的路径:

```
export PATH=PATH_TO_FLUTTER_GIT_DIRECTORY/flutter/bin:$PATH
```
运行 `source $HOME/.bash_profile` 刷新当前终端窗口.

#### Q4: AndroidStudio 运行报错
```
Error connecting to the service protocol: HttpException: , uri = http://127.0.0.1:8113/ws
```
#### A4: 解决方法 打开 XCODE - Window - Devices and Simulators，找到用来调试的设备取消勾选 Connect via network。
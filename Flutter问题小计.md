### Flutter问题小计

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

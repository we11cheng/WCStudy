## 配置Frida

### 手机端（越狱情况下）
#### Start Cydia and add Frida’s repository by going to ```Manage``` -> ```Sources``` -> ```Edit``` -> Add and enter ```https://build.frida.re```. You should now be able to find and install the ```Frida``` package which lets Frida inject JavaScript into apps running on your iOS device. This happens over USB, so you will need to have your USB cable handy, though there’s no need to plug it in just yet.
### 简而言之，添加源```https://build.frida.re``` 搜索```Frida``` 安装即可，
#### 如图
![](http://p2bzzkn05.bkt.clouddn.com/18-6-15/26457559.jpg)

### Mac端

```
brew install python3
pip3 install frida
```
### 补充：安装过程终端抛出如下异常
```
Operation not permitted: '/tmp/pip-uW0fNP-uninstall/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/six-1.4
```
### 执行
```
sudo pip3 install frida --ignore-installed six
```
### 测试是否安装成功

```
frida-ps -U
```
### 如果出现

```
Waiting for USB device to appear...
```
表示没有把你的iOS设备插入到电脑里面（或者插到电脑但是没有被正常识别）...

### 如果出现
```
PID NAME
XXX XXXXX
```
表示成功

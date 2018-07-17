
## frida-ios-dump 
### 新手砸壳教程（演示为Python2环境）,前提手机必须越狱&必须通过USB链接SSH。
### iOS端配置参考（越狱情况下）
- 打开cydia 添加源：https://build.frida.re

- 打开刚刚添加的源 安装 Frida

- 安装完成

#### 参考链接  <https://www.frida.re/docs/ios/>

### mac端配置
- 终端执行：

```
sudo pip install frida
```  
- 假如报以下错误：

```
Uninstalling a distutils installed project (six) has been deprecated and will be removed in a future version. This is due to the fact that uninstalling a distutils project will only partially uninstall the project.

```

- 使用以下命令安装：

```
sudo pip install frida –upgrade –ignore-installed six
```

- 查看frida版本

```
frida --version
12.0.3
```

- 检查frida是否正常运行 任意mac终端输入```frida-ps -U```有数据返回表示成功安装，-U表示USB设备，-ps表示进程。

- 如果确定frida --version有正常返回&USBSSH连接也没问题，还是出现```Waiting for USB device to appear...```本人解决方法是，亲测可以，这个坑踩了很久，还是去<https://github.com/frida/frida>才找到的解决方法。

```
sudo pip3 install frida-tools
```


## USB&SSH连接iPhone
- 安装usbmuxd（brew安装最简单暴力推荐使用brew安装）。  

```
brew install usbmuxd
```   
#### 如果你没有安装brew的话，那需要先执行  
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```      
- 手机通过数据线连接电脑。
      
- 终端输入 
     
```
iproxy 2222 22
```     
#### 说明：将当前连接设备的22端口映射到电脑的2222端口上，继而来连接SSH。   
- 另开一个终端

```
ssh -p 2222 root@127.0.0.1
```   
#### 说明：127.0.0.1是默认本机回环地址。

- 输入ssh密码即可（默认密码alpine，建议修改）修改密码参考链接 <https://www.jianshu.com/p/725843850e1d>

#### USB&SSH连接参考地址 <https://www.jianshu.com/p/bf69cefc5f39>

### 开始砸壳之旅

- 从github下载工程：一般情况使用最新的版本，master分支对应Python2，3.x对应Python3版本。

```
git clone https://github.com/AloneMonkey/frida-ios-dump 

```     
- 安装依赖：

```
cd frida-ios-dump
sudo pip install -r requirements.txt --upgrade
```
- 修改dump.py参数：

```
vim frida-ios-dump/dump.py
```
- 找到如下几行

```
User = 'root'   
Password = 'xxxxxxxxxx'   
Host = 'localhost'   
Port = 2222   
```

- 修改Password和端口号。和iproxy命令端口号保持一致。


- 列举出安装的应用的名字和bundle id 

```
cd frida-ios-dump
python dump.py -l
```    

- 执行dump命令 

```
python dump.py bundle id
``` 
- 去壳后的ipa默认在frida-ios-dump目录下。

#### 说明: 执行```dump.py bundle id```命令的时候要确保手机上已经打开你需要砸壳的app。
### 说明: frida-ios-dump有对应Python3分支3.x分支，大家可以多看看作者最新提交情况。地址 <https://github.com/AloneMonkey/frida-ios-dump>


## theos安装和使用

### theos的安装 <https://github.com/theos/theos>
- 设置xcode工具集路径 查看命令：```xcode-select --print-path```
- 选择xcode命令 ``` sudo  xcode-select --switch /Applications/Xcode.app/Contents/Developer ```
- ```sudo mkdir opt```
- ```cd opt```
- ```sudo git clone --recursive https://github.com/theos/theos.git```  
- 修改thoes权限 ```sudo chown -R $(id -u):$(id -g) theos``` 
- 设置环境变量

```
手动设置环境变量（每次启动，都要写一遍来指定路径）
export THEOS=/opt/theos

查看环境变量
echo $THEOS
/opt/theos

写入：~/.bash_profile 避免每次都指定路径
 1、打开文件：命令：vim ~/.bash_profile
2、敲 i 插入
3、将export THEOS=/opt/theos复制进去
4、esc ==> :wq 保存退出
5、执行命令：source ~/.bash_profile  生效 
```
### 创建逆向程序
- 开启我们的theos：```/opt/theos/bin/nic.pl``` 找一个合适的工作目录
 ![](http://p2bzzkn05.bkt.clouddn.com/18-6-5/55869504.jpg)
 
 ```
 //选择tweak工程  
  Choose a Template (required): 11  

  //工程名称
  Project Name (required): MyFirstReProject  

  //deb包的名字（类似于bundle identifier)
  Package Name [com.yourcompany.myfirstreproject]: com.obizsofi.iosre  

  //tweak作者
  Author/Maintainer Name [System Administrator]: gwc 

  //tweak作用对象的bundle identifier
  [iphone/tweak] MobileSubstrate Bundle filter [com.apple.springboard]: com.apple.springboard 

  //tweak安装完成后需要重启的应用,这里填写应用运行时的名称
  [iphone/tweak] List of applications to terminate upon installation (space-separated, '-' for none) [SpringBoard]: SpringBoard
  ```
- 完成以后。查看工作目录文件结构
- cd 到工作目录 对工程进行编译 ```make``` 
- 打包 ```make package``` 可能遇到的问题
```
Can't locate IO/Compress/Lzma.pm in @INC (you may need to install the IO::Compress::Lzma module) (@INC contains: /Library/Perl/5.18/darwin-thread-multi-2level /Library/Perl/5.18 /Network/Library/Perl/5.18/darwin-thread-multi-2level /Network/Library/Perl/5.18 /Library/Perl/Updates/5.18.2 /System/Library/Perl/5.18/darwin-thread-multi-2level /System/Library/Perl/5.18 /System/Library/Perl/Extras/5.18/darwin-thread-multi-2level /System/Library/Perl/Extras/5.18 .) at /opt/theos/bin/dm.pl line 12.
BEGIN failed--compilation aborted at /opt/theos/bin/dm.pl line 12.
```
解决方法 ：

```
1、/opt/theos/vendor/dm.pl/dm.pl
注释掉第12、13行
#use IO::Compress::Lzma;
#use IO::Compress::Xz;

2、/opt/theos/makefiles/package/deb.mk
第6行lzma改为gzip
_THEOS_PLATFORM_DPKG_DEB_COMPRESSION ?= gzip
```
- 安装到越狱手机 ```make install``` 可能遇到的问题
```
==> Error: /Applications/Xcode.app/Contents/Developer/usr/bin/make install requires that you set THEOS_DEVICE_IP in your environment.
==> Notice: It is also recommended that you have public-key authentication set up for root over SSH, or you will be entering your password a lot.
``` 
添加设备THEOS_DEVICE_IP 到系统环境变量中即可。

### 效果图（有错误欢迎指出）
![](http://p2bzzkn05.bkt.clouddn.com/18-6-5/44117725.jpg)



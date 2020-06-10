### Mac系统Pycharm永久激活(不确定啥时候失效)

- 本人下载的是2018.2.7版本，官方有很多版本可供下载，下载地址<http://www.jetbrains.com/pycharm/download/other.html>,安装完成后关闭Pycharm。也有现成的我存了一份网盘，链接: https://pan.baidu.com/s/1CsHxYEhqV83NSgmZCMaLzg 提取码: cfcn 复制这段内容后打开百度网盘手机App，操作更方便哦

-  下载补丁：https://pan.baidu.com/s/1CsHxYEhqV83NSgmZCMaLzg 提取码: cfcn 复制这段内容后打开百度网盘手机App，操作更方便哦。

- 打开Finder-->应用程序-->Pycharm-->右键点击，选择“显示包内容”-->Contents-->bin,将补丁包放到bin目录下

- 打开bin目录下的pycharm.vmoptions进行编辑，在最后一行插入

```
-javaagent:/Applications/PyCharm.app/Contents/bin/JetbrainsCrack-release-enc.jar
```
- 注意jar包文件名,插入后保存。

- 打开Pycharm，会跳出激活的弹窗，点击选择Activation code方式激活，输入

```
-javaagent:/Applications/PyCharm.app/Contents/bin/JetbrainsCrack-release-enc.jar
```
或者

```
javaagent:/Applications/PyCharm.app/Contents/bin/JetbrainsCrack-release-enc.jar
```

- 即激活成功。


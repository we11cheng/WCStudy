## MAC cycript安装
- [cycript下载地址](http://www.cycript.org/) 解压到一个合适的工作目录（配置系统环境变量时会用到）。附上本人目录：
 
![](http://p2bzzkn05.bkt.clouddn.com/18-6-8/43196200.jpg)

- ```cd /opt/cycript_0.9.594```

- ```cycript```

- 如果终端输出 cy# 表示成功。

- control + D 可退出 cycript

### 补充：配置环境变量以便在其他目录下也可以使用cycript命令
- 编辑```~/.bash_profile``` 强烈使用MacVim客户端。

- 添加 cycript绝对路径

```
export cycript_src=/opt/cycript_0.9.594/
export PATH=$PATH:$cycript_src

```
- 使环境变量生效

```source ~/.bash_profile```

- 任意路径下输入```cycript``` 检测是否安装成功。

- 如果终端输出 cy# 表示成功。

### 拓展：配置cycript环境变量遇到的问题（ruby版本过高问题）
#### 问题引入：
```
dyld: Library not loaded: /System/Library/Frameworks/Ruby.framework/Versions/2.0/usr/lib/libruby.2.0.0.dylib Referenced from: /opt/cycript_0.9.594/Cycript.lib/cycript-apl Reason: image not found
```
#### 解决方法
- 关闭系统的SIP。

```
电脑重启按住command+R，进入恢复模式
打开终端，输入csrutil disable，重启
如果想打开SIP，重复上两步，命令改为csrutil enable
```

- 运行命令:

```
sudo mkdir -p /System/Library/Frameworks/Ruby.framework/Versions/2.0/usr/lib/
sudo ln -s /System/Library/Frameworks/Ruby.framework/Versions/2.3/usr/lib/libruby.2.3.0.dylib /System/Library/Frameworks/Ruby.framework/Versions/2.0/usr/lib/libruby.2.0.0.dylib
```
#### ln -s 图解
![](http://p2bzzkn05.bkt.clouddn.com/18-6-8/43427244.jpg)

#### 注：根据每个人ruby版本不同，将上面第二条命令的/System/Library/Frameworks/Ruby.framework/Versions/2.3/usr/lib/libruby.2.3.0.dylib中的2.3改成本机的ruby版本。这里不是降级ruby，只是复制一份2.0的ruby的dylib，让cycript运行起来。

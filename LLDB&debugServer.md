### LLDB&debugServer
#### LLDB 全称为 Low Level Debugger，是由苹果出品，内置于 Xcode 中的动态调试工具。LLDB 是运行在 OSX 中的，要想调试 iOS，还需要另一个工具的配合，它就是 debugserver。debugserver 运行在 iOS 上，顾名思义，它作为服务端，实际执行 LLDB (作为客户端) 传过来的指令，再把执行结果反馈给 LLDB，显示给用户，即所谓的 "远程调试"。在默认情况下，iOS 上并没有安装 debugserver，只有在设备连接过一次 Xcode，并在 Window -> Devices 菜单中添加此设备后，debugserver 才会被 Xcode 安装到 iOS的 /Developer/usr/bin/ 目录下。

#### 配置 debugserver

- 将未经处理的 debugserver 从 iOS 拷贝到 OSX 中的任意目录下。(只是一个临时存放点)

##### eg:
```
admindeMBP-4:~ admin$ scp root@192.168.2.117:/Developer/usr/bin/debugserver ~/Downloads/debugserver
root@192.168.2.117's password: 
debugserver                                                                           100%   13MB  13.9MB/s   00:00    
```

- debugserver瘦身(16M减小到4M多)

```
lipo -thin arm64 ~/Downloads/debugserver -output ~/Downloads/debugserver
```

#### 选择自己真机类型ARM对应的类型。

```
arm64：iPhone6s | iphone6s plus｜iPhone6｜ iPhone6 plus｜iPhone5S | iPad Air｜ iPad mini2(iPad mini with Retina Display)
armv7s：iPhone5｜iPhone5C｜iPad4(iPad with Retina Display)
armv7：iPhone4｜iPhone4S｜iPad｜iPad2｜iPad3(The New iPad)｜iPad mini｜iPod Touch 3G｜iPod Touch4

i386是针对intel通用微处理器32位处理器
x86_64是针对x86架构的64位处理器

模拟器32位处理器测试需要i386架构，
模拟器64位处理器测试需要x86_64架构，
真机32位处理器需要armv7,或者armv7s架构，
真机64位处理器需要arm64架构。
```

- 给 debugserver 添加 task_for_pid 权限

#### 下载 <http://iosre.com/ent.xml> 到 OSX 的 ~/Downloads (也就是和 debugserver 同一个目录)。然后运行如下命令：

```
admindeMBP-4:~ admin$ cd Downloads/
admindeMBP-4:Downloads admin$ ldid -Sent.xml debugserver
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>com.apple.backboardd.debugapplications</key>
	<true/>
	<key>com.apple.backboardd.launchapplications</key>
	<true/>
	<key>com.apple.diagnosticd.diagnostic</key>
	<true/>
	<key>com.apple.frontboard.debugapplications</key>
	<true/>
	<key>com.apple.frontboard.launchapplications</key>
	<true/>
	<key>com.apple.security.network.client</key>
	<true/>
	<key>com.apple.security.network.server</key>
	<true/>
	<key>com.apple.springboard.debugapplications</key>
	<true/>
	<key>run-unsigned-code</key>
	<true/>
	<key>seatbelt-profiles</key>
	<array>
		<string>debugserver</string>
	</array>
</dict>
</plist>
```

- 将经过处理的 debugserver 拷回iOS

```
admindeMBP-4:Downloads admin$ scp ~/Downloads/debugserver root@192.168.2.117:/usr/bin/debugserver
root@192.168.2.117's password: 
debugserver                                                                           100% 4555KB   8.7MB/s   00:00    
```

#### 这里之所以把处理过的 debugserver 存放在 iOS 的 /usr/bin 下，而没有覆盖 /Developer/usr/bin/ 下的原版 debugserver，一是因为原版 debugserver 是不可写的，无法覆盖；二是因为 /usr/bin/ 下的命令无须输入全路径就可以执行，即在任意目录下运行 debugserver 都可启动处理过的 debugserver。

#### LLDB使用说明
- 通过 SSH 连接越狱机，启动 MobileSMS.app ，并开启 1111 端口，等待任意 IP 地址的 LLDB 接入

```
debugserver -x backboard *:1111 /Applications/MobileSMS.app/MobileSMS 
```
- 另开一个终端

```
admindeMBP-4:Downloads admin$ lldb
(lldb) process connect connect://192.168.2.117:1111
Traceback (most recent call last):
  File "<input>", line 1, in <module>
  File "/usr/local/Cellar/python@2/2.7.15_1/Frameworks/Python.framework/Versions/2.7/lib/python2.7/copy.py", line 52, in <module>
    import weakref
  File "/usr/local/Cellar/python@2/2.7.15_1/Frameworks/Python.framework/Versions/2.7/lib/python2.7/weakref.py", line 14, in <module>
    from _weakref import (
ImportError: cannot import name _remove_dead_weakref
Process 23643 stopped
* thread #1, stop reason = signal SIGSTOP
    frame #0: 0x0000000100161000 dyld`_dyld_start
dyld`_dyld_start:
->  0x100161000 <+0>:  mov    x28, sp
    0x100161004 <+4>:  and    sp, x28, #0xfffffffffffffff0
    0x100161008 <+8>:  mov    x0, #0x0
    0x10016100c <+12>: mov    x1, #0x0
Target 0: (MobileSMS) stopped.
(lldb)  
```

#### 参考链接
<https://juejin.im/post/5af0102ef265da0b8a678bd5>

<https://bbs.pediy.com/thread-203592.htm>



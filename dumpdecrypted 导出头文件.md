# dumpdecrypted 导出头文件

### class-dump可以将Mach-O文件中的Objective-C运行时的声明信息导出，即编写OC代码时的.h文件。class-dump只能导出未加密的app头文件，因此我们想要分析头文件就要先对app进行dumpdecrypted（砸壳）操作。

### 配置dumpdecrypted
- 下载dumpdecrypted最新源码。cd 到自己的工作目录 eg:

```
cd SourceCode
git clone https://github.com/stefanesser/dumpdecrypted.git
```

- 切换到umpdecrypted 文件目录

```
cd dumpdecrypted
```
- 执行make命令

```
make
```

##### make命令编译生成dumpdecrypted.dylib动态库文件。

#### 需要注意的地方：生成的dumpdecrypted.dylib 需要对它进行证书签名。参考：
```
  admindeMBP-4:dumpdecrypted admin$  security find-identity -v -p codesigning
  1) 59B89622F39A8F607197ED52988474DD8BD063DD "iPhone Distribution: Obizsoft Co,.Ltd"
  2) DC795A2D887F0EF1043A6F7A2D241EEEE717F23D "iPhone Developer: 2674178351@qq.com (NU8BQ9S8QW)"
  3) CDFFBE18E7F5B1836100BD09A639B81ED7DF74D7 "iPhone Developer: 1396539283@qq.com (GS628688YK)"
  4) 1AFFC1EC9508DE62BCC25205DC3EA038048FB893 "iPhone Developer: Yinan Sun (CA2EFE8B5Z)"
  5) 250841B0087C35B67610CF078F1164131279F61B "iPhone Distribution: Obizsoft Co,.Ltd (296BA8G9JQ)"
  6) E8D86EABEE9111B88F4F74337149DB9AE145E878 "iPhone Developer: 维诚 管 (6MP6GEHAAD)"
  7) E650EAB0A8BC2667F5296247F7B1719CE4E80704 "Mac Developer: 维诚 管 (6MP6GEHAAD)"
  8) CD994F2AFAF8266A212D9F665513EA3506E61466 "iPhone Developer: mao ye (PH3564CJ9J)"
  9) 7467DDB7EBC68407CDA005C7A71B9134C5F46A0A "iPhone Distribution: Shanghai Intermoda Clothing Co., Ltd"
     9 valid identities found
admindeMBP-4:dumpdecrypted admin$ ls
Makefile		README			dumpdecrypted.c		dumpdecrypted.dylib	dumpdecrypted.o
admindeMBP-4:dumpdecrypted admin$ codesign --force --verify --verbose --sign "iPhone Developer: Yinan Sun (CA2EFE8B5Z)" dumpdecrypted.dylib
dumpdecrypted.dylib: signed Mach-O universal (armv7 armv7s arm64) [dumpdecrypted]
```
- 使用ssh登录越狱设备 [SSH连接越狱iPhone](https://www.jianshu.com/p/bf69cefc5f39)

- 连接成功后输入```ps -e``` 找到含bundle的路径，只会存在一个(也就是目标app)，这一步很重要。

- 拷贝拷贝dumpdecrypted.dylib到目标app沙盒Documents目录下。
 
- 我们要把电脑上生成的dumpdecrypted.dylib放在砸壳app的沙盒路径下。
   ##### 这一步可以使用scp命令或者使用[iFunbox](http://www.i-funbox.com/zh-cn_index.html)配合Apple File Conduit "2"插件，很方便的传输文件。
   
 ![](https://github.com/we11cheng/WCImageHost/raw/master/131532338495_.pic.png)

- scp方式传输方式如下(iFunbox和scp二选一)：

```
scp /Users/admin/SourceCode/dumpdecrypted/dumpdecrypted.dylib root@192.168.2.200:/var/containers/Bundle/Application/15BE94E8-4E3F-4FFE-8E89-F3BFEFF66AE0/Documents
root@192.168.2.200's password: 
dumpdecrypted.dylib                                                                   100%  203KB   1.9MB/s   00:00    
```

- dumpdecrypted.dylib添加成功后，切换到iphone ssh，切换到app沙盒文件路径下执行DYLD_INSERT_LIBRARIES命令，如下: 

```
cd /var/containers/Bundle/Application/15BE94E8-4E3F-4FFE-8E89-F3BFEFF66AE0/Documents 
iPhone:/var/containers/Bundle/Application/15BE94E8-4E3F-4FFE-8E89-F3BFEFF66AE0/Documents root# DYLD_INSERT_LIBRARIES=dumpdecrypted.dylib /var/containers/Bundle/Application/15BE94E8-4E3F-4FFE-8E89-F3BFEFF66AE0/Omnistore.app/Omnistore
```
##### 第一次执行可能如下错误：
```
dyld: could not load inserted library 'dumpdecrypted.dylib' because no suitable image found.  Did find:
dumpdecrypted.dylib: required code signature missing for 'dumpdecrypted.dylib'
/private/var/containers/Bundle/Application/15BE94E8-4E3F-4FFE-8E89-F3BFEFF66AE0/Documentsdumpdecrypted.dylib: required code signature missing for '/private/var/containers/Bundle/Application/15BE94E8-4E3F-4FFE-8E89-F3BFEFF66AE0/Documentsdumpdecrypted.dylib'
Abort trap: 6
```
##### 错误提示也很明确。需要对dumpdecrypted.dylib签名。见配置dumpdecrypted需要注意的地方。

- 签名过后重新DYLD_INSERT_LIBRARIES，正常输出如下：
    
```
[+] detected 32bit ARM binary in memory.
[+] offset to cryptid found: @0xb6a90(from 0xb6000) = a90
[+] Found encrypted data at address 00004000 of length 5488640 bytes - type 1.
[+] Opening /private/var/containers/Bundle/Application/15BE94E8-4E3F-4FFE-8E89-F3BFEFF66AE0/Omnistore.app/Omnistore for reading.
[+] Reading header
[+] Detecting header type
[+] Executable is a plain MACH-O image
[+] Opening Omnistore.decrypted for writing.
[+] Copying the not encrypted start of the file
[+] Dumping the decrypted data into the file
[+] Copying the not encrypted remainder of the file
[+] Setting the LC_ENCRYPTION_INFO->cryptid to 0 at offset a90
[+] Closing original file
[+] Closing dump file
```
- 查看.decrypted文件。

```
iPhone:/var/containers/Bundle/Application/15BE94E8-4E3F-4FFE-8E89-F3BFEFF66AE0 root# ls
Documents  Omnistore.app  Omnistore.decrypted  dumpdecrypted.dylib  iTunesArtwork  iTunesMetadata.plist
```
- 然后把Omnistore.decrypted 通过scp命令复制到mac目录下。

```
admindeMBP-4:dumpdecrypted admin$ scp root@192.168.2.200:/var/containers/Bundle/Application/15BE94E8-4E3F-4FFE-8E89-F3BFEFF66AE0/Omnistore.decrypted /Users/admin/Desktop/decrypted
root@192.168.2.200's password: 
Omnistore.decrypted         
```
- otool 检测是否解密

```
admindeMBP-4:decrypted admin$ otool -l Omnistore.decrypted | grep -B 2 crypt
Omnistore.decrypted:
--
          cmd LC_ENCRYPTION_INFO
      cmdsize 20
     cryptoff 16384
    cryptsize 5488640
      cryptid 0
admindeMBP-4:decrypted admin$ 
```
- class-dump导出头文件

```
admindeMBP-4:dumpdecrypted admin$ class-dump -S -s -H /Users/admin/Desktop/decrypted/Omnistore.decrypted -0 /Users/admin/Mygit/dump_header
class-dump: invalid option -- 0
class-dump 3.5 (64 bit) (Debug version compiled Sep 17 2017 16:24:48)
```

### /Users/admin/Mygit/dump_header 导出的头文件路径。

### 进一步完善dumpdecrypted，支持Dumps decrypted mach-o files from encrypted applications、framework or app extensions.
### 步骤如下
- 克隆项目

```
gwcdeMacBook-Pro:SourceCode gwc$ git clone https://github.com/AloneMonkey/dumpdecrypted.git
Cloning into 'dumpdecrypted'...
remote: Counting objects: 73, done.
remote: Total 73 (delta 0), reused 0 (delta 0), pack-reused 73
Unpacking objects: 100% (73/73), done.
```
- Xcode运行项目，进行参数配置

<img width="1280" alt="wx20180726-210143 2x" src="https://user-images.githubusercontent.com/13333981/43264057-9a9fd4e0-9117-11e8-830b-c9730ba8d97e.png">

<img width="1280" alt="wx20180726-210654 2x" src="https://user-images.githubusercontent.com/13333981/43264172-f2926b5e-9117-11e8-95b6-10d4a6f54aef.png">

- 确保电脑安装了sshpass。推荐使用berw安装，命令如下

```
brew install https://raw.githubusercontent.com/kadwanev/bigboybrew/master/Library/Formula/sshpass.rb
```

- xcode运行选择真机运行

- comand+b 。成功大概如下图所示

<img width="1280" alt="wx20180726-211436 2x" src="https://user-images.githubusercontent.com/13333981/43264525-ff2b2e90-9118-11e8-8677-7d5a9b605a7b.png">

- 砸完生成的.decrypted文件默认也是在目标app沙盒ducuments目录下。而且framework、extensions 也是解密的。检测是否解密使用otool工具。

### 参考链接 <https://github.com/AloneMonkey/dumpdecrypted>
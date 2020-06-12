### iOS如何用fastlane重签名app
#### 一.安装重签工具
#### 打开终端，然后输入以下命令：

```
brew install ruby
sudo gem install sigh
```

#### 如何使用fastlane快速重签名app
1.把要重签名的app和新的描述文件放在一起，然后打开终端进入到当前目录（注：新的描述文件哪里来？新建一个项目，然后配置好Bundle Id和证书，编译一下，然后打开app所在的目录，把app的后缀名改为.zip,再解压，就能看到embedded.mobileprovision这个文件了）

2.打开终端，cd到当前app和描述文件所在的目录（最快的方式就输入cd 然后把app所在的文件夹拖到终端），然后再输入`sigh resign`

![](https://raw.githubusercontent.com/we11cheng/picBed/master/20200610172832.png)

```
~/Downloads/Misc/testResign » sigh resign                                      rby@rbydeMacBook-Pro
[WARNING] You are calling sigh directly. Usage of the tool name without the `fastlane` prefix is deprecated in fastlane 2.0
Please update your scripts to use `fastlane sigh resign` instead.
[17:22:59]: Available identities: 
	iPhone Developer: xxx (7XM9RZQC4W)
		616E2DEC45DDC9E565161F1E94A8A020FFC11FC2
	iPhone Developer: 1396539283@qq.com (GS628688YK)
		27CE184FE5121A845ADA6A7034B66B95CD67234F
	Mac Developer: xxx (7XM9RZQC4W)
		29CE908CB3E7E9935288E5233E253E995396B6BD
	iPhone Developer: xxx (488Q9D3TUF)
		7F6B76A73026AAAD5859E11A0242265582C548F3
	iPhone Distribution: Shanghai xxx  Information Technology Co.,Ltd. (4T57E5CWND)
		79E2A7B0763A2CD4CA55E553382EFF500DB5EC19
	Apple Development: xx (488Q9D3TUF)
		B4FCB67DC9D301D7A33687975683BEFD22D414AC
	Apple Distribution: Shanghai xxx  Information Technology Co.,Ltd. (4T57E5CWND)
		0610E9F8C0917DE8CA5106F63DD9BCD90A1BDA53
	Apple Development: 1396539283@qq.com (GS628688YK)
		67FEA2B06B08175AA5404525A9D195E93B7656B1

[17:22:59]: Signing Identity: 79E2A7B0763A2CD4CA55E553382EFF500DB5EC19
/Library/Ruby/Gems/2.6.0/gems/fastlane-2.135.2/sigh/lib/assets/resign.sh /Users/rby/Downloads/Misc/testResign/h3lix-RC6.ipa 79E2A7B0763A2CD4CA55E553382EFF500DB5EC19 -p /Users/rby/Downloads/Misc/testResign/embedded.mobileprovision          /Users/rby/Downloads/Misc/testResign/h3lix-RC6.ipa
cp: _floatsignTemp/Payload/h3lix.app/embedded.mobileprovision: No such file or directory

[17:23:30]: Successfully signed /Users/rby/Downloads/Misc/testResign/h3lix-RC6.ipa!
```
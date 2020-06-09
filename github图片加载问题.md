### 解决github加载不了图片，ping超时的问题

#### 很多人都习惯用github作为图床工具，因为它免费基本稳定。很多写markdown的人，一旦图片不能正常显示，就很难受，于是有了这篇记录。

- 修改hosts文件（以Mac为例）
- 找到`/etc/hosts`文件
- 添加配置文本

```
# GitHub Start 
192.30.253.112 github.com 
192.30.253.119 gist.github.com
151.101.184.133 assets-cdn.github.com
151.101.184.133 raw.githubusercontent.com
151.101.184.133 gist.githubusercontent.com
151.101.184.133 cloud.githubusercontent.com
151.101.184.133 camo.githubusercontent.com
151.101.184.133 avatars0.githubusercontent.com
151.101.184.133 avatars1.githubusercontent.com
151.101.184.133 avatars2.githubusercontent.com
151.101.184.133 avatars3.githubusercontent.com
151.101.184.133 avatars4.githubusercontent.com
151.101.184.133 avatars5.githubusercontent.com
151.101.184.133 avatars6.githubusercontent.com
151.101.184.133 avatars7.githubusercontent.com
151.101.184.133 avatars8.githubusercontent.com
```
- 保存。

#### 可能遇到的问题。如何以普通用户启动的Vim如何保存需要root权限的文件，答案是可以，执行这样一条命令即可：

`:w !sudo tee %`

### 补充`:w !sudo tee %`也可能存在无法保存，解决办法

#### 1.打开Finder，按快捷键组合 Shift+Command+G 查找文件，输入/private,确认前往后可看到 etc 文件夹，邮件选择'显示简介'，在底部打开‘共享和权限’。

![](https://raw.githubusercontent.com/we11cheng/picBed/master/20200420102543.png)

#### 2.将everyone的权限改为‘读与写’，保存后直接修改hosts文件，最后完成后将权限改回来。

![](https://raw.githubusercontent.com/we11cheng/picBed/master/20200420103128.png)

##### 查询ip地址<https://tools.ipip.net/dns.php>,找到响应最快的ip填入host文件。或者这个地址<https://www.ipaddress.com/>
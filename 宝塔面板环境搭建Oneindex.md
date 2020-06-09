### 宝塔面板环境搭建Oneindex(纯记录)

#### OneDrive一直作为一款非常优秀的网盘受到很多人的喜爱，容量大速度快，如果你有Office365或使用微软全家桶都非常方便的同步和存储文件。那么如果我们和一台VPS甚至一个PHP虚拟空间结合呢？你的PHP空间或者VPS就秒变“大盘鸡”了！
#### OneIndex是hostloc论坛大佬@donwa 写出来的PHP程序，利用OneDrive的API接口，程序可以直接列出你的OneDrive目录，和普通的Index列表程序一样简单,简直就是神器~

#### 主要功能

```
不占用服务器空间，不走服务器流量
直接列出 OneDrive 目录，文件直链下载
文件夹加密访问
文档(代码)在线浏览，图片在线浏览，视频可在线播放
支持Markdown语法的头部、底部说明
响应式，支持小屏设备
```

#### 演示站点 <https://weiguan.ml/>

#### 大概说下搭建过程

环境需求
1、PHP空间，PHP 5.6+ 需打开curl支持
2、OneDrive 账号 (个人、企业版或教育版/工作或学校帐户)
3、OneIndex 程序

安装方法

1、用宝塔面板新建网站环境，严格按照上面的环境需求第1条。

2、进入网站根目录，用下载好的Oneindex程序，并将压缩包内所有文件提取至根目录。

3、访问你的域名，进入安装引导页面。(如下图所示)

`https://weiguan.ml/` 宝塔面板环境搭建Oneindex

![](https://weiguan.ml/OneIndex.gif)

4、如果安装失败，请重新按照步骤“3”进行尝试。

进阶教程
计划任务
为了保证实时同步onedrive内的文件列表，需要设置定时任务进行定时同步。

进入宝塔定时任务设置，任务类型选择shell脚本，任务名称：OneIndex-每小时刷新一次Token ，任务周期选择每小时，0分钟，脚本内容为：php /程序具体路径/one.php token:refresh PS:具体路径为你网站根目录路径,PHP后有空格请注意。
www.quchao.net 宝塔面板环境搭建Oneindex

![](https://raw.githubusercontent.com/we11cheng/picBed/master/20200418225341.png)

再添加一条定时任务，任务名称：OneIndex-每十分钟后台刷新一遍缓存,任务周期改为N分钟-10分钟，脚本内容为：php /程序具体路径/one.php cache:refresh
www.quchao.net 宝塔面板环境搭建Oneindex

![](https://raw.githubusercontent.com/we11cheng/picBed/master/20200418225434.png)

伪静态设置
Nginx 伪静态设置

```
if (!-f $request_filename){
set $rule_0 1$rule_0;
}
if (!-d $request_filename){
set $rule_0 2$rule_0;
}
if ($rule_0 = "21"){
rewrite ^/(.*)$ /index.php?/$1 last;
}
```
后台打开去掉`/?/` (需配合伪静态使用!!)，保存设置

主题设置方法
基本设置 -> 网站主题 [演示站点为nexmoe]

特殊文件实现功能
README.md、HEAD.md 、 .password特殊文件使用

可以参考<https://github.com/donwa/oneindex/tree/files>

- 在文件夹底部添加说明:

	在 OneDrive 的文件夹中添加`README.md`文件，使用 Markdown 语法。

- 在文件夹头部添加说明:  

	在 OneDrive 的文件夹中添加`HEAD.md` 文件，使用 Markdown 语法。

- 加密文件夹:  

	在 OneDrive 的文件夹中添加`.password`文件，填入密码，密码不能为空。  

- 直接输出网页:

	在 OneDrive 的文件夹中添加`index.html` 文件，程序会直接输出网页而不列目录。配合 文件展示设置-直接输出 效果更佳。

命令功能
仅能在PHP CLI模式下运行

```
清除缓存:  	php one.php cache:clear
刷新缓存:  	php one.php cache:refresh
刷新令牌:  	php one.php token:refresh
上传文件:  	php one.php upload:file 本地文件 [OneDrive文件]
上传文件夹: php one.php upload:folder 本地文件夹 [OneDrive文件夹]

例如：

//上传demo.zip 到OneDrive 根目录  
php one.php upload:file demo.zip  

//上传demo.zip 到OneDrive /test/目录  
php one.php upload:file demo.zip /test/  

//上传demo.zip 到OneDrive /test/目录并将其命名为 d.zip  
php one.php upload:file demo.zip /test/d.zip  

//上传up/ 到OneDrive /test/ 目录  
php one.php upload:file up/ /test/
```
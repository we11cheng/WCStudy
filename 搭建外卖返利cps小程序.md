### 第一步、申请饿了么官方返利api
- 淘宝联盟登陆账号<https://pub.alimama.com/>
- 点击推广管理-->媒体备案管理-->他方平台-->新增媒体备案
![](https://gitee.com/he11oworld/picBed/raw/master/640.png)
![](https://gitee.com/he11oworld/picBed/raw/master/20201125152224.png)
业务负责人根据自己信息填写
![](https://gitee.com/he11oworld/picBed/raw/master/20201125152322.png)
个人账号会直接审核通过
![](https://gitee.com/he11oworld/picBed/raw/master/20201125152513.png)
点击申请APPKey   -- > 立即申请
![](https://gitee.com/he11oworld/picBed/raw/master/20201125152556.png)
点击创建
![](https://gitee.com/he11oworld/picBed/raw/master/20201125152643.png)
创建应用
![](https://gitee.com/he11oworld/picBed/raw/master/20201125152725.png)
选择刚才创建的推广位提交
![](https://gitee.com/he11oworld/picBed/raw/master/20201125152755.png)
查看appkey
下方所有权限都需要申请
只要随便写100字以上就会通过申请
![](https://gitee.com/he11oworld/picBed/raw/master/20201125152845.png)
选择刚申请的应用 点击推广位进行创建
![](https://gitee.com/he11oworld/picBed/raw/master/20201125152917.png)
保留此pid
前往此链接<https://open.taobao.com/doc.htm?spm=a219a.7386653.0.0.2535669aU8utEf&docId=1&docType=15&apiName=taobao.appstore.subscribe.get>获取饿了么返利api
![](https://gitee.com/he11oworld/picBed/raw/master/20201125153042.png)

activity_material_id有两个选择

直接领取红包：1571715733668

每日生鲜红包：1585018034441

全部填写之后，提交测试，如下图
![](https://gitee.com/he11oworld/picBed/raw/master/20201125153204.png)
因为淘宝联盟有一些延迟需要等候一段时间才能获取推广链接(报错的话耐心等就行了)
![](https://gitee.com/he11oworld/picBed/raw/master/20201125153326.png)
复制此链接删除"/"符号，https://s.click.ele.me/VQSPLuu
保存此链接。
### 第二步、申请小程序并搭建
- 感谢大佬的开源<https://github.com/zwpro/coupons>
- 下载解压HBuilderX<https://www.dcloud.io/hbuilderx.html>
- 下载安装微信开发者工具<https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html>
- 打开HBuilderX  -->文件-->打开目录 刚才解压的代码
- 编辑/pages/index/index.vue
![](https://gitee.com/he11oworld/picBed/raw/master/20201125153808.png)
改为刚才保存的自己的饿了么返利api

不需要的比如爱奇艺之类的 注释掉就好了

然后打包程序，

用微信开发者工具上传小程序并提交审核即可。

体验地址

![](https://gitee.com/he11oworld/picBed/raw/master/0.jpg)
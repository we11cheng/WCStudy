### 使用码云(gitee)解决github下载很慢的问题。
### 方法1
##### 浅拷贝
- git clone慢的主要原因是这条指令默认将所有的git历史记录都克隆下来，也就是把git项目从头演变一次。所以可以使用浅拷贝来clone项目，浅拷贝的好处是不用clone项目的完整历史，而只需clone最近的一次提交，但是项目里面的文件都会完整地被下载下来，只是历史不会完全保留，如果你并不关心项目的git历史，那就完全可以使用浅拷贝来完成clone。

```
git clone --depth=1 https://......
```

### 方法2
##### 利用gitee
- 首先你要登录码云<https://gitee.com/>
- 登录后点击右上角新建仓库+，选择`从github/gitlab导入仓库`导入项目
- 到这里虽然项目已经顺利下载下来，但是本地并没有和GitHub关联，后期修改推送是不通的。所以需要进行一下关联，关联如下：
在下载的项目路径下打开隐藏的`.git`文件夹，找到该路径下的`config`文件，修改`remote "origin"`的地址改为GitHub地址即可。

![](https://raw.githubusercontent.com/we11cheng/picBed/master/20200512195552.png)
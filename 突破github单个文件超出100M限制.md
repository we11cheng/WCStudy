### 突破github单个文件超出100M限制
#### 解决方案-使用Git LFS
#### Git LFS的官方网址在这里： https://git-lfs.github.com/，官网上有很详细的说明，现在来简单说下使用方式：先安装 Git LFS 的客户端，然后在将要push的仓库里重新打开一个bash命令行：

- 只需设置1次 LFS : 

```
git lfs install
```

- 然后在本地仓库跟踪一下你要push的大文件的文件或指定文件类型

```
- git lfs track "*.ipa"  
```
#### 这个时候本地会生成.gitattributes
- 当然还可以直接编辑.gitattributes文件
- 以上已经设置完毕， 其余的工作就是按照正常的 add , commit , push 流程就可以了

```
git add yourLargeFile.pdf
git commit -m "Add Large file"
git push -u origin master
```
- -u 首次提交，不是首次提交直接`git push origin master`
##### 所以可以总结一下：git push -u origin mybranch1 相当于 git push origin mybranch1 + git branch --set-upstream-to=origin/mybranch1 mybranch1 (本地分支与远端分支关联)

- 目前 Git LFS的总存储量为1G左右，超过需要付费。

### ⚠️存在的坑:

```
remote: Resolving deltas: 100% (3/3), completed with 1 local object.
remote: error: GH001: Large files detected. You may want to try Git Large File Storage - https://git-lfs.github.com.
remote: error: Trace: aaec6be289d0e837ea210bced9dbdad3
remote: error: See http://git.io/iEPt8g for more information.
remote: error: File iOS/哔哩哔哩v5.56完美破解版.ipa is 163.60 MB; this exceeds GitHub's file size limit of 100.00 MB
```
#### 解决办法，```.gitattributes```需要单独push。

### 简单概括
```
执行 git lfs install 开启lfs功能
使用 git lfs track 命令进行大文件追踪 例如git lfs track "*.png" 追踪所有后缀为png的文件
使用 git lfs track 查看现有的文件追踪模式
提交代码需要将gitattributes文件提交至仓库. 它保存了文件的追踪记录(这一步很重要,缺少会push失败)
提交后运行git lfs ls-files 可以显示当前跟踪的文件列表
将代码 push 到远程仓库后，LFS 跟踪的文件会以『Git LFS』的形式显示:
clone 时 使用'git clone' 或 git lfs clone均可
```



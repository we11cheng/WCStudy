### git branch 使用
#### 新建分支：

```
git checkout -b dev2  新建一个分支，并且切换到新的分支dev2
git branch dev2 新建一个分支，但是仍停留在原来分支
```
#### 切换分支
```
git checkout master 切换到master分支
```
#### 查看分支：
```
git branch    查看本地所有的分支
git branch -r  查看所有远程的分支
git branch -a  查看所有远程分支和本地分支
```

#### 删除本地分支：
```
git branch -D <branchname>  删除本地branchname分支
```
#### 合并分支

```
git merge master 在当前分支上合并master分支过来
git merge --no-ff origin/dev  在当前分支上合并远程分支dev
git merge --abort 终止本次merge，并回到merge前的状态
```

### git进阶之撤销与回退

![](https://raw.githubusercontent.com/we11cheng/picBed/master/20200630213243.png)

#### git checkout使用，如果文件还在工作区，还没添加到暂存区，可以使用git checkout撤销

```
git checkout [file]  丢弃某个文件file
git checkout .  丢弃所有文件
```

#### git reset的使用，几种模式

```
git reset HEAD --file
回退暂存区里的某个文件，回退到当前版本工作区状态
git reset –-soft 目标版本号 可以把版本库上的提交回退到暂存区，修改记录保留
git reset –-mixed 目标版本号 可以把版本库上的提交回退到工作区，修改记录保留
git reset –-hard  可以把版本库上的提交彻底回退，修改的记录全部revert。
```
![](https://raw.githubusercontent.com/we11cheng/picBed/master/20200630214149.png)

##### 场景1:代码git add到暂存区，并未commit提交,就以下酱紫回退，如下：

```
git reset HEAD file 取消暂存
git checkout file 撤销修改
```
##### 场景2:代码已经git commit了，但是还没有push：
```
git log  获取到想要回退的commit_id
git reset --hard commit_id  想回到过去，回到过去的commit_id
```
##### 场景3:代码已经push到远程仓库了
```
git log
git reset --hard commit_id
git push origin HEAD --force #强行push
```
#### git revert 使用

#### 与git reset不同的是，revert复制了那个想要回退到的历史版本，将它加在当前分支的最前端。
##### 场景1:代码已经推送到远程的话，还可以考虑revert回滚
```
git log  得到你需要回退一次提交的commit id
git revert -n <commit_id>  撤销指定的版本，撤销也会作为一次提交进行保存
```

### git进阶之标签tag
```
git tag  列出所有tag
git tag [tag] 新建一个tag在当前commit
git tag [tag] [commit] 新建一个tag在指定commit
git tag -d [tag] 删除本地tag
git push origin [tag] 推送tag到远程
git show [tag] 查看tag
git checkout -b [branch] [tag] 新建一个分支，指向某个tag
```

### git blame filepath

```
git blame 记录了某个文件的更改历史和更改人，可以查看背锅人，哈哈
```
### git remote
```
git remote   查看关联的远程仓库的名称
git remote add url   添加一个远程仓库
git remote show [remote] 显示某个远程仓库的信息
```

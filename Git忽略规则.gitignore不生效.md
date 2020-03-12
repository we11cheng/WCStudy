### Git忽略规则.gitignore不生效

### 在项目开发过程中个，一般都会添加 .gitignore 文件，规则很简单，但有时会发现，规则不生效。原因是 .gitignore 只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。那么解决方法就是先把本地缓存删除（改变成未track状态），然后再提交。

```
git rm -r --cached .

git add .

git commit -m 'update .gitignore'

```
### Git忽略已经被提交的文件
#### 对于已经提交过的文件，再加入到gitignore中也无济于事，.gitignore文件只能作用于 Untracked Files。正确做法是git update-index --assume-unchanged logs/*.log

#### 方法1：
#### `git update-index --assume-unchanged` 的真正用法是这样的：你正在修改一个巨大的文件，你先对其 `git update-index --assume-unchanged`，这样Git暂时不会理睬你对文件做的修改；当你的工作告一段落决定可以提交的时候，重置改标识：`git update-index --no-assume-unchanged`，于是Git只需要做一次更新，这是完全可以接受的了；

#### 方法2：
#### 把已经提交的文件添加到.gitignore中
```
git rm --cached fileName
git commit -m 'add .gitignore'
git push origin master
```

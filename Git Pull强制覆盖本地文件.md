### Git Pull强制覆盖本地文件

#### 在有些场景下为了避免代码冲突，需要强制使用远程代码覆盖本地代码，比如自动部署，GitHub的webhook,解决方法

```
git fetch --all

git reset --hard origin/master

git pull

```
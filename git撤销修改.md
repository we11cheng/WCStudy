### git撤销修改(本地仓库操作)

1.当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时:

```
命令git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：

一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

git checkout -- readme.txt
```
2.当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作:

```
git reset命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用HEAD时，表示最新的版本。

git reset HEAD readme.txt
```
3.已经提交了不合适的修改到版本库时，想要撤销本次提交:

```
HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset commit_id

--mixed 
意思是：不删除工作空间改动代码，撤销commit，并且撤销git add . 操作
这个为默认参数,git reset --mixed HEAD^ 和 git reset HEAD^ 效果是一样的。

--soft  
不删除工作空间改动代码，撤销commit，不撤销git add . 

--hard
删除工作空间改动代码，撤销commit，撤销git add . 

穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。如果嫌输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数：

要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。
```

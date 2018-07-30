### git问答(不定时更新)
#### Q1:把旧项目提交到Git上，但是会有一些历史记录，这些历史记录中可能会有项目密码等敏感信息。如何删除这些历史记录，形成一个全新的仓库，并且保持代码不变呢？
A1:

```
1.Checkout

   git checkout --orphan latest_branch

2. Add all the files

   git add -A

3. Commit the changes

   git commit -am "commit message"


4. Delete the branch

   git branch -D master

5.Rename the current branch to master

   git branch -m master

6.Finally, force update your repository

   git push -f origin master

```
#### Q2:三种清除Git提交历史的方法
A2:

```
1. 新建另一个远程仓库，命名为B； 
2. 将现有的本地代码提交到远程仓库B； 
3. 删除现有的远程仓库A； 
4. 将远程仓库B命名为A；
```
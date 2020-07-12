### git push refusing to merge unrelated histories
#### 问题描述，pull/push无法进行：我在gitee新建一个仓库，上面有readme/License，然后把本地一个仓库上传。先pull，发现 `refusing to merge unrelated histories`，无法pull/push。
#### 解决办法

```
git pull origin master --allow-unrelated-histories
```


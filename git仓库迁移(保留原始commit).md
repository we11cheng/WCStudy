### git仓库迁移(保留原始commit)

- 从原地址克隆一份裸版本库（原始仓库，包含多个commit）`--bare` 创建的克隆版本库都不包含工作区，直接就是版本库的内容，这样的版本库称为裸版本库。举个例子

```
git clone --bare https://gitee.com/he11oworld/helloworld.git
```

- 目标仓库创建一个仓库（目标仓库，新仓库）

- 以镜像推送的方式上传代码到其他服务器上。`-- mirror` 克隆出来的裸版本对上游版本库进行了注册，这样可以在裸版本库中使用git fetch命令和上游版本库进行持续同步。

```
cd helloworld.git
git push --mirror http://weicheng.guan@git.obizsoft.com:10080/Adidas/adiRetail-Support-iPAD.git
```

- 删除本地的helloworld.git 文件夹。

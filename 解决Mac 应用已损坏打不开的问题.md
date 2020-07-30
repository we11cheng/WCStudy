### 解决Mac 应用已损坏打不开的问题
#### 记一次Navicat打开的时候提示
```
“Navicate Premium” is demaged and cant't be opened.You should move it to the Trash.
```
#### 解决方案

- 首先打开终端，执行：

```
sudo bash
```
这时会提示你输入你的账户密码， 输入完后就切换到了 root 用户，然后执行：

```
xattr -cr /Applications/Navicat\ Premium.app/
```

#### /Applications/Navicat\ Premium.app/ 是应用安装后的路径 ， Mac 应用安装后一般都在 /Applications/ 下有***.app 文件，也就是应用的入口文件。
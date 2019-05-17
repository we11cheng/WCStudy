### brew升级错误：Xcode alone is not sufficient on Sierra.

### 具体错误提示

```
Error: Xcode alone is not sufficient on High Sierra.
Install the Command Line Tools:
  xcode-select --install 
```

### 安装ruby除了安装Xcode外，还需要安装Command Line Tools。

### 打开终端，输入命令：

`
xcode-select --install
`
### 确保安装成功：
`
xcode-select -p
`
### 出现"/Library/Developer/CommandLineTools" 路径即表示安装成功。

### 出现权限问题

### eg

`
Error: Permission denied @ unlink_internal - /usr/local/lib/ruby/gems/2.5.0/gems/ffi-1.10.0/ext/ffi_c/libffi-x86_64-darwin16/include/ffitarget.h
`
### 解决方法

`
sudo chown -R $(whoami) $(brew --prefix)/*
`
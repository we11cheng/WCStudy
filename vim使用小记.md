#### vim使用小记

#### Q1: 如何以普通用户启动的Vim如何保存需要root权限的文件。
#### A2: 终极解决方法`:w !sudo tee %`
```
在Linux上工作的朋友很可能遇到过这样一种情况，当你用Vim编辑完一个文件时，运行:wq保存退出，突然蹦出一个错误：

E45: 'readonly' option is set (add ! to override)
这表明文件是只读的，按照提示，加上!强制保存：:w!，结果又一个错误出现：

"readonly-file-name" E212: Can't open file for writing
文件明明存在，为何提示无法打开？这错误又代表什么呢？查看文档:help E212：

For some reason the file you are writing to cannot be created or overwritten.
The reason could be that you do not have permission to write in the directory
or the file name is not valid.
原来是可能没有权限造成的。此时你才想起，这个文件需要root权限才能编辑，而当前登陆的只是普通用户，在编辑之前你忘了使用sudo来启动Vim，所以才保存失败。于是为了防止修改丢失，你只好先把它保存为另外一个临时文件temp-file-name，然后退出Vim，再运行sudo mv temp-file-name readonly-file-name覆盖原文件。
```
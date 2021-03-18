### Linux（centos）安装vim
当在Linux环境下使用vim提示： vim command not found时，说明系统还没有安装vim。

安装步骤：

## 1.检查是否已安装

查看一下你本机已经存在的包，确认一下你的VIM是否已经安装，输入： 

```
rpm -qa|grep vim
```

如果已安装，会显示：

```
\[root@localhost usr\]# rpm -qa|grep vim
vim\-minimal-7.4.629\-6.el7.x86\_64
vim\-filesystem-7.4.629\-6.el7.x86\_64
vim\-enhanced-7.4.629\-6.el7.x86\_64
vim\-common-7.4.629\-6.el7.x86\_64
vim\-X11-7.4.629\-6.el7.x86\_64
```

## 2.安装

如果缺少了其中某个，比如说： vim-enhanced这个包少了，则执行：

```
yum -y install vim-enhanced
```

它会自动下载安装。如果上面三个包一个都没有显示，则直接输入命令：   

```
yum -y install vim*
```

执行命令后会自动安装，完毕后就可以使用vim编辑器了。

## 3.配置

安装完成后开始配置vim

```
vim /etc/vimrc
```

打开文件后，按 **i** 进入编辑模式，然后找一个位置添加如下代码

```
 set nu          " 设置显示行号
 set showmode    " 设置在命令行界面最下面显示当前模式等
 set ruler       " 在右下角显示光标所在的行数等信息
 set autoindent  " 设置每次单击Enter键后，光标移动到下一行时与上一行的起始字符对齐
 syntax on       " 即设置语法检测，当编辑C或者Shell脚本时，关键字会用特殊颜色显示

```

添加好了之后，按Esc，然后输入

```
:wq
```

退出并保存即可。


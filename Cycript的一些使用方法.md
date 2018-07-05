### Cycript的一些使用方法
#### 首先,要使用Cycript你必须有一台越狱手机,这是必要条件,且能连接SSH。怎么连接SSH参考:[SSH连接越狱iPhone(WIFI和USB)](https://www.jianshu.com/p/bf69cefc5f39)

- 手机安装Cycript：打开Cydia搜索Cycript进行安装

![](https://github.com/we11cheng/WCImageHost/raw/master/91530782040_.pic.jpg)

- 连接ssh

- 开始使用Cycript

- 查看当前手机进程

```
ps ax或者ps -e
```

- 找到自己工程项目名

![](https://github.com/we11cheng/WCImageHost/raw/master/WX20180705-170832.png)

- 当控制台出现

```
cy#
```
##### 表示已经成功hook住我们的程序了。

#### 部分使用方法

```
UIApp.keyWindow.recursiveDescription().toString()
```
获取视图结构图(这里的UIApp相当于[UIApplication sharedApplication])

```
#0x13fd168c0
```
拿到地址所对应的对象,可以查看其相应的属性

```
#0x13fd168c0.text = @"lovepersistence"
```

![](https://github.com/we11cheng/WCImageHost/raw/master/WX20180705-171200.png)

```
?exit
```
退出Crcyipt

程序前后截图

![](https://github.com/we11cheng/WCImageHost/raw/master/81530782040_.pic.jpg)

![](https://github.com/we11cheng/WCImageHost/raw/master/71530782040_.pic.jpg)

可以清楚的看到成功修改了lable的文字。

#### 更多ycript命令参看官网: <http://www.cycript.org/>


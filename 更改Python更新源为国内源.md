### 更改Python更新源为国内源

#### 常见的国内源：

```
清华：https://pypi.tuna.tsinghua.edu.cn/simple
阿里云：http://mirrors.aliyun.com/pypi/simple/
中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/
豆瓣：http://pypi.douban.com/simple/
```

#### 在Linux系统中，修改`~/.pip/pip.conf`文件；在Windows系统中，修改`C:\Users\XXX\pip\pip.ini`文件。如果没有上述文件，需要手动建立。在文件中输入以下内容：

```
[global]
index-url = https://pypi.mirrors.ustc.edu.cn/simple/
[install]
trusted-host = mirrors.ustc.edu.cn
```

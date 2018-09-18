### 常用的pip命令

```
# pip 使用格式：pip <command> [options] package_name

pip install package_name==1.9.2	# 安装指定版本的包
pip install --upgrade package_name	# 更新指定的包
pip uninstall package_name	# 卸载指定的包
pip show package_name	# 查看所安装包的详细信息
pip list	# 查看所有安装的包
pip --help	# 查看帮助

```

### 更改 pip 下载源
```
# 国内常用的镜像
http://pypi.douban.com/simple/            # 豆瓣
http://mirrors.aliyun.com/pypi/simple/    # 阿里
https://pypi.tuna.tsinghua.edu.cn/simple  # 清华
http://pypi.mirrors.ustc.edu.cn/simple/   # 中国科学技术大学
http://pypi.hustunique.com/simple/        # 华中理工大学
```

### 临时使用，添加 -i 或 --index 参数：pip install -i http://pypi.douban.com/simple/ flask


### Linux下永久生效的配置方法

```
cd $HOME  
mkdir .pip  
cd .pip
sudo vim pip.conf  

# 在里面添加，trusted-host 选项为了避免麻烦是必须的，否则使用的时候会提示不受信任  
[global]
index-url=https://pypi.tuna.tsinghua.edu.cn/simple

[install]
trusted-host=pypi.tuna.tsinghua.edu.cn 
disable-pip-version-check=true
timeout = 6000 
```

### Windows 下永久生效的配置方法

```
# a、进入如下目录（没有此目录或文件就自己创建下）
C:\Users\username\AppData\Local\pip
或
C:\Users\username\pip

# b、创建 “pip.ini” 文件（注意：以UTF-8 无BOM格式编码），添加如下内容
[global]
index-url=https://pypi.tuna.tsinghua.edu.cn/simple

[install]
trusted-host=pypi.tuna.tsinghua.edu.cn 
disable-pip-version-check=true
timeout = 6000 
```
### 安装方式:
### 进入 pypi.python.org，搜索你要安装的库的名字，这时候有 3 种可能：

- 第一种是 exe 文件，这种最方便，下载满足你的电脑系统和 Python 环境的对应的 exe，再一路点击 next 就可以安装。

- 第二种是 .whl 类文件，好处在于可以自动安装依赖的包。

```
到命令行输入pip3 install whell 等待执行完成，不能报错（Python 2 中要换成 pip）
从资源管理器中确认你下载的 .whl 类文件的路径，然后在命令行继续输入：cd C:\download，此处需要改为你的路径，路径的含义是文件所在的文件夹，不包含这个文件名字本身，然后再命令行继续输入：pip3 install xxx.whl，xxx.whl 是你下载的文件的完整文件名。
```

- 第三种是源码，大概都是 zip、tar.zip、tar.bz2 格式的压缩包，这个方法要求用户已经安装了这个包所依赖的其他包。例如 pandas 依赖于 numpy，你如果不安装 numpy，这个方法是无法成功安装 pandas 的。

```
- 解压包，进入解压好的文件夹，通常会看见一个 setup.py 的文件，从资源管理器中确认你下载的文件的路径，打开命令行，输入：cd C:\download 此处需要改为你的路径，路径的含义是文件所在的文件夹，不包含这个文件名字本身
- 然后在命令行中继续输入：python3 setup.py install 这个命令，就能把这个第三方库安装到系统里，也就是你的 Python路径，windows 大概是在 C:\Python3.5\Lib\site-packages。 
```

### 想要卸库的时候，找到 Python 路径，进入 site-packages 文件夹，在里面删掉库文件就可以了。

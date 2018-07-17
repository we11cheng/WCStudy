### Python虚拟环境

- 安装virtualenv

```
pip3 install virtualenv
```

- 创建目录

```
mkdir python3_env
```

- 创建一个独立的Python运行环境，命名为gwc

```
virtualenv --no-site-packages gwc
```

##### 命令virtualenv就可以创建一个独立的Python运行环境，我们还加上了参数--no-site-packages，这样，已经安装到系统Python环境中的所有第三方包都不会复制过来，这样，我们就得到了一个不带任何第三方包的“干净”的Python运行环境。

- source进入该环境

```
cd gwc/
source ./bin/activate
```

- 退出环境

```
deactivate
```


实际操作

```
admindeMBP-4:Mygit admin$ mkdir python3_env
admindeMBP-4:Mygit admin$ cd python3_env/
admindeMBP-4:python3_env admin$ virtualenv --no-site-packages gwc
Using base prefix '/usr/local/Cellar/python/3.6.4_4/Frameworks/Python.framework/Versions/3.6'
New python executable in /Users/admin/Mygit/python3_env/gwc/bin/python3.6
Also creating executable in /Users/admin/Mygit/python3_env/gwc/bin/python
Installing setuptools, pip, wheel...done.
admindeMBP-4:python3_env admin$ ls
gwc
admindeMBP-4:python3_env admin$ cd gwc/
admindeMBP-4:gwc admin$ ls
bin			include			lib			pip-selfcheck.json
admindeMBP-4:gwc admin$ source ./bin/activate
(gwc) admindeMBP-4:gwc admin$ deactivate
admindeMBP-4:gwc admin$ 
```

原理:把系统Python复制一份到virtualenv的环境，用命令source gwc/bin/activate进入一个virtualenv环境时，virtualenv会修改相关环境变量，让命令python和pip均指向当前的virtualenv环境。

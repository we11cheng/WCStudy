    
### 安装 Docker 和 Docker Compose (Centos 7)
![](https://gitee.com/he11oworld/picBed/raw/master/20210224141250.png)

`Docker`是一个开放源码的产品，分为 **社区版**（Community Edition，缩写为 CE）和 **企业版**（Enterprise Edition，缩写为 EE）。社区版是免费的，而企业版包含一些收费服务，一般来说个人开发者用社区版就足够了，本篇博文的教程也只是针对社区版。

安装环境及版本：

+   系统：Centos 7.6 服务器系统
+   Docker 版本：19.03.3

Ubuntu 系统中的安装教程请阅读[安装 Docker 和 Docker Compose (Ubuntu)](http://jemgeek.com/archives/2019/docker-base-install.html)

英文好的小伙伴也可以直接阅读官方文档，本文只详细介绍 `Centos 7` 系统下的 `Docker` 及 `Docker Compose` 安装，其他系统的安装请自行参考官方文档。

+   [Mac](https://docs.docker.com/docker-for-mac/install/)
+   [Windows](https://docs.docker.com/docker-for-windows/install/)
+   [CentOS](https://docs.docker.com/install/linux/docker-ce/centos/)
+   [Debian](https://docs.docker.com/install/linux/docker-ce/debian/)
+   [Fedora](https://docs.docker.com/install/linux/docker-ce/fedora/)
+   [Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
+   [其他Linux版本](https://docs.docker.com/install/linux/docker-ce/binaries/)

### 卸载老版本

一般来说`Centos 7`系统中默认是不会安装`Docker`的，但是如果安装了老版本的话可以使用下面的命令进行卸载。

```auto
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

### 安装 Docker CE

1.  更新`yum`包索引：

```auto
$ sudo yum update
```

2.  安装一些必要的依赖包：

```auto
$ sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

3.  配置 docker-ce 仓库：

```auto
$ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

4.  安装 docker-ce:

```auto
$ sudo yum install docker-ce # 安装过程中跳出确认则输入 y
```

至此，Docker 已经安装完成了，Docker 服务是没有启动的，操作系统里的 docker 组被创建，但是没有用户在这个组里。

使用下面的命令将你的用户添加到 docker 的用户组：

```auto
$ sudo usermod -aG docker $(yourname)
```

添加完成后重启系统生效。

设置 Docker 开机自启：

```auto
$ sudo systemctl enable docker
```

启动 Docker 服务：

```auto
$ sudo systemctl start docker
```

5.  验证`Docker`

使用下面的命令查看`Docker`的版本

```auto
$ docker -v
Docker version 19.03.3, build a872fc2f86
```

然后使用下面的命令可以运行`hello-world`程序，因为`Docker`中还没有`hello-world`程序的镜像，所以会先`pull`（下载）下来然后运行。

```auto
$ sudo docker run hello-world
```

如果看到打印 `Hello for Docker!`说明程序已经运行成功了。

![](https://gitee.com/he11oworld/picBed/raw/master/20210224141314.png)

### 更新及卸载

1.  更新 Docker CE

```auto
$ sudo yum update docker-ce
```

2.  卸载 Docker CE

```auto
$ sudo yum remove docker-ce
```

3.  主机上的镜像、容器、卷或者自定义配置文件是不会自动删除的，需要使用下面的命令手动删除这些文件：

```auto
$ sudo rm -rf /var/lib/docker
```

### Docker Compose 安装

1.  安装额外依赖包：

```auto
$ sudo yum install epel-release
```

2.  安装 python-pip:

```auto
$ sudo yum install -y python-pip
```

3.  安装 Docker Compose:

```auto
$ sudo pip install docker-compose
```

4.  升级 python 包：

```auto
$ sudo yum upgrade python*
```

5.  验证安装：

```auto
$ docker-compose version
docker-compose version 1.24.1, build 4667896
docker-py version: 3.7.3
CPython version: 2.7.5
OpenSSL version: OpenSSL 1.0.2k-fips  26 Jan 2017
```

6.  卸载 Docker Compose:

如果你是使用`curl`的方式安装的，则运行下面的命令删除docker-conpose的文件（本文使用此种方式安装）：

```auto
$ sudo rm /usr/local/bin/docker-compose
```

如果你是使用 `pip` 的方式安装的，则运行下面的命令删除docker-conpose的文件：

```auto
$ sudo pip uninstall docker-compose
```

## Docker 的使用

### Docker 的启动、关闭等

可以使用下面的命令对`Docker`进行启动、关闭、重启等操作。

```auto
# 开启 Docker
$ sudo service docker start

# 关闭 Docker
$ sudo service docker stop

# 重启 Docker
$ sudo service docker restart
```

也可以使用`systemctl`命令进行操作

```auto
# 开启 Docker
$ sudo systemctl start docker

# 关闭 Docker
$ sudo systemctl stop docker

# 重启 Docker
$ sudo systemctl restart docker
```

### Docker 及镜像

请阅读[安装 Docker 和 Docker Compose (Ubuntu)](http://jemgeek.com/archives/2019/docker-base-install.html)后面的部分，在此不再赘述。

## 其它

关于`Docker`的知识还有很多，我会在后续的文章继续介绍，欢迎您持续关注本博客。


[原文地址](https://juejin.cn/post/6844903965381885966)
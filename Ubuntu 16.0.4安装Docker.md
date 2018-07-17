### Ubuntu 16.0.4安装Docker
1.首先打开终端窗口输入命令，更新包信息：

```
sudo apt-get update
```

2.安装CA证书，支持Https:

```
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
```

3.添加一个官方的GPG密钥：

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

4.验证密钥指纹：

```
sudo apt-key fingerprint 0EBFCD88
```

#### 正确返回示例

```
9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88
```

5.下载amd64的官方最新稳定版Docker：

```
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

6.再次更新包信息：

```
sudo apt-get update
```

7.安装Docker CE版本：

```
sudo apt-get install docker-ce
```

8.创建一个docker组，防止每次都要用sudo命令执行docker命令：

```
sudo groupadd docker
```

9.将该用户加入到组内：

```
sudo gpasswd -a ${USER} docker
```

10.重启docker：

```
sudo service docker restart
```

11.切换当前会话到新的组：

```
newgrp - docker
```

12.去网易蜂巢镜像库下载docker测试镜像hello-word，看能否正常运行：

```
docker pull hub.c.163.com/library/hello-world:latest
```

13.下载镜像后，运行：

```
docker run hub.c.163.com/library/hello-world:latest
```

#### 实践操作记录如下：

```
root@iZbp1ibd5qj8a3dwztxsf8Z:~# sudo apt-get update
Hit:1 http://mirrors.cloud.aliyuncs.com/ubuntu xenial InRelease
Get:2 http://mirrors.cloud.aliyuncs.com/ubuntu xenial-updates InRelease [109 kB]    
Get:3 http://mirrors.cloud.aliyuncs.com/ubuntu xenial-security InRelease [107 kB]              
Get:4 http://mirrors.cloud.aliyuncs.com/ubuntu xenial-updates/main Sources [313 kB]            
Get:5 http://mirrors.cloud.aliyuncs.com/ubuntu xenial-updates/universe Sources [207 kB]         
Get:6 http://mirrors.cloud.aliyuncs.com/ubuntu xenial-updates/main amd64 Packages [806 kB]      
Get:7 http://mirrors.cloud.aliyuncs.com/ubuntu xenial-updates/main i386 Packages [736 kB]       
Get:8 http://mirrors.cloud.aliyuncs.com/ubuntu xenial-updates/main Translation-en [333 kB]      
Get:9 http://mirrors.cloud.aliyuncs.com/ubuntu xenial-updates/universe amd64 Packages [642 kB]  
Get:10 http://mirrors.cloud.aliyuncs.com/ubuntu xenial-updates/universe i386 Packages [586 kB]  
Get:11 http://mirrors.cloud.aliyuncs.com/ubuntu xenial-updates/universe Translation-en [258 kB] 
Get:12 http://mirrors.cloud.aliyuncs.com/ubuntu xenial-security/main Sources [128 kB]           
Get:13 http://mirrors.cloud.aliyuncs.com/ubuntu xenial-security/universe Sources [67.3 kB]      
Get:14 http://mirrors.cloud.aliyuncs.com/ubuntu xenial-security/main amd64 Packages [521 kB]    
Get:15 http://mirrors.cloud.aliyuncs.com/ubuntu xenial-security/main i386 Packages [459 kB]     
Get:16 http://mirrors.cloud.aliyuncs.com/ubuntu xenial-security/main Translation-en [223 kB]    
Get:17 http://mirrors.cloud.aliyuncs.com/ubuntu xenial-security/universe amd64 Packages [358 kB]
Get:18 http://mirrors.cloud.aliyuncs.com/ubuntu xenial-security/universe i386 Packages [305 kB] 
Get:19 http://mirrors.cloud.aliyuncs.com/ubuntu xenial-security/universe Translation-en [134 kB]
Hit:20 http://ppa.launchpad.net/certbot/certbot/ubuntu xenial InRelease                         
Ign:21 http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 InRelease  
Hit:22 http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 Release    
Fetched 6,293 kB in 1s (4,058 kB/s)                   
Reading package lists... Done
root@iZbp1ibd5qj8a3dwztxsf8Z:~# sudo apt-get install \
>     apt-transport-https \
>     ca-certificates \
>     curl \
>     software-properties-common
Reading package lists... Done
Building dependency tree       
Reading state information... Done
ca-certificates is already the newest version (20170717~16.04.1).
software-properties-common is already the newest version (0.96.20.7).
The following packages were automatically installed and are no longer required:
  linux-headers-4.4.0-87 linux-headers-4.4.0-87-generic linux-image-4.4.0-87-generic
  linux-image-extra-4.4.0-87-generic
Use 'sudo apt autoremove' to remove them.
The following additional packages will be installed:
  libcurl3-gnutls
The following packages will be upgraded:
  apt-transport-https curl libcurl3-gnutls
3 upgraded, 0 newly installed, 0 to remove and 82 not upgraded.
Need to get 349 kB of archives.
After this operation, 0 B of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://mirrors.cloud.aliyuncs.com/ubuntu xenial-updates/main amd64 curl amd64 7.47.0-1ubuntu2.8 [139 kB]
Get:2 http://mirrors.cloud.aliyuncs.com/ubuntu xenial-updates/main amd64 libcurl3-gnutls amd64 7.47.0-1ubuntu2.8 [185 kB]
Get:3 http://mirrors.cloud.aliyuncs.com/ubuntu xenial-updates/main amd64 apt-transport-https amd64 1.2.27 [26.1 kB]
Fetched 349 kB in 0s (8,027 kB/s)         
(Reading database ... 135120 files and directories currently installed.)
Preparing to unpack .../curl_7.47.0-1ubuntu2.8_amd64.deb ...
Unpacking curl (7.47.0-1ubuntu2.8) over (7.47.0-1ubuntu2.7) ...
Preparing to unpack .../libcurl3-gnutls_7.47.0-1ubuntu2.8_amd64.deb ...
Unpacking libcurl3-gnutls:amd64 (7.47.0-1ubuntu2.8) over (7.47.0-1ubuntu2.7) ...
Preparing to unpack .../apt-transport-https_1.2.27_amd64.deb ...
Unpacking apt-transport-https (1.2.27) over (1.2.26) ...
Processing triggers for man-db (2.7.5-1) ...
Processing triggers for libc-bin (2.23-0ubuntu10) ...
Setting up libcurl3-gnutls:amd64 (7.47.0-1ubuntu2.8) ...
Setting up curl (7.47.0-1ubuntu2.8) ...
Setting up apt-transport-https (1.2.27) ...
Processing triggers for libc-bin (2.23-0ubuntu10) ...
root@iZbp1ibd5qj8a3dwztxsf8Z:~# curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
OK
root@iZbp1ibd5qj8a3dwztxsf8Z:~# sudo apt-key fingerprint 0EBFCD88
pub   4096R/0EBFCD88 2017-02-22
      Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid                  Docker Release (CE deb) <docker@docker.com>
sub   4096R/F273FCD8 2017-02-22

root@iZbp1ibd5qj8a3dwztxsf8Z:~# sudo add-apt-repository \
>    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
>    $(lsb_release -cs) \
>    stable"
root@iZbp1ibd5qj8a3dwztxsf8Z:~# sudo apt-get update
Hit:1 http://mirrors.cloud.aliyuncs.com/ubuntu xenial InRelease
Hit:2 http://mirrors.cloud.aliyuncs.com/ubuntu xenial-updates InRelease                         
Hit:3 http://mirrors.cloud.aliyuncs.com/ubuntu xenial-security InRelease                        
Hit:4 http://ppa.launchpad.net/certbot/certbot/ubuntu xenial InRelease                          
Ign:5 http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 InRelease           
Hit:6 http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 Release
Get:8 https://download.docker.com/linux/ubuntu xenial InRelease [65.8 kB]
Get:9 https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages [3,653 B]
Fetched 69.5 kB in 1s (36.8 kB/s)
Reading package lists... Done
root@iZbp1ibd5qj8a3dwztxsf8Z:~# sudo apt-get install docker-ce
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following packages were automatically installed and are no longer required:
  linux-headers-4.4.0-87 linux-headers-4.4.0-87-generic linux-image-4.4.0-87-generic
  linux-image-extra-4.4.0-87-generic
Use 'sudo apt autoremove' to remove them.
The following additional packages will be installed:
  aufs-tools cgroupfs-mount libltdl7 pigz
Suggested packages:
  mountall
The following NEW packages will be installed:
  aufs-tools cgroupfs-mount docker-ce libltdl7 pigz
0 upgraded, 5 newly installed, 0 to remove and 82 not upgraded.
Need to get 34.2 MB of archives.
After this operation, 182 MB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://mirrors.cloud.aliyuncs.com/ubuntu xenial/universe amd64 pigz amd64 2.3.1-2 [61.1 kB]
Get:2 http://mirrors.cloud.aliyuncs.com/ubuntu xenial/universe amd64 aufs-tools amd64 1:3.2+20130722-1.1ubuntu1 [92.9 kB]
Get:3 http://mirrors.cloud.aliyuncs.com/ubuntu xenial/universe amd64 cgroupfs-mount all 1.2 [4,970 B]
Get:4 http://mirrors.cloud.aliyuncs.com/ubuntu xenial/main amd64 libltdl7 amd64 2.4.6-0.1 [38.3 kB]
Get:5 https://download.docker.com/linux/ubuntu xenial/stable amd64 docker-ce amd64 18.03.1~ce-0~ubuntu [34.0 MB]
Fetched 34.2 MB in 1min 32s (371 kB/s)                                                          
Selecting previously unselected package pigz.
(Reading database ... 135120 files and directories currently installed.)
Preparing to unpack .../pigz_2.3.1-2_amd64.deb ...
Unpacking pigz (2.3.1-2) ...
Selecting previously unselected package aufs-tools.
Preparing to unpack .../aufs-tools_1%3a3.2+20130722-1.1ubuntu1_amd64.deb ...
Unpacking aufs-tools (1:3.2+20130722-1.1ubuntu1) ...
Selecting previously unselected package cgroupfs-mount.
Preparing to unpack .../cgroupfs-mount_1.2_all.deb ...
Unpacking cgroupfs-mount (1.2) ...
Selecting previously unselected package libltdl7:amd64.
Preparing to unpack .../libltdl7_2.4.6-0.1_amd64.deb ...
Unpacking libltdl7:amd64 (2.4.6-0.1) ...
Selecting previously unselected package docker-ce.
Preparing to unpack .../docker-ce_18.03.1~ce-0~ubuntu_amd64.deb ...
Unpacking docker-ce (18.03.1~ce-0~ubuntu) ...
Processing triggers for man-db (2.7.5-1) ...
Processing triggers for libc-bin (2.23-0ubuntu10) ...
Processing triggers for ureadahead (0.100.0-19) ...
Processing triggers for systemd (229-4ubuntu21.2) ...
Setting up pigz (2.3.1-2) ...
Setting up aufs-tools (1:3.2+20130722-1.1ubuntu1) ...
Setting up cgroupfs-mount (1.2) ...
insserv: can not symlink(../init.d/aegis, ../rc2.d/S02aegis): File exists
insserv: can not symlink(../init.d/aegis, ../rc3.d/S02aegis): File exists
insserv: can not symlink(../init.d/aegis, ../rc4.d/S02aegis): File exists
insserv: can not symlink(../init.d/aegis, ../rc5.d/S02aegis): File exists
Setting up libltdl7:amd64 (2.4.6-0.1) ...
Setting up docker-ce (18.03.1~ce-0~ubuntu) ...
Processing triggers for libc-bin (2.23-0ubuntu10) ...
Processing triggers for systemd (229-4ubuntu21.2) ...
Processing triggers for ureadahead (0.100.0-19) ...
root@iZbp1ibd5qj8a3dwztxsf8Z:~# sudo groupadd docker
groupadd: group 'docker' already exists
root@iZbp1ibd5qj8a3dwztxsf8Z:~# sudo gpasswd -a ${USER} docker
Adding user root to group docker
root@iZbp1ibd5qj8a3dwztxsf8Z:~# sudo service docker restart
root@iZbp1ibd5qj8a3dwztxsf8Z:~# newgrp - docker
root@iZbp1ibd5qj8a3dwztxsf8Z:~# docker pull hub.c.163.com/library/hello-world:latest
latest: Pulling from library/hello-world
7a9d05de7670: Pull complete 
Digest: sha256:7391d42f476e10480a3da94f15233703f6c6abcd9b5165e390121f867039a6df
Status: Downloaded newer image for hub.c.163.com/library/hello-world:latest
root@iZbp1ibd5qj8a3dwztxsf8Z:~# ls
bark  exampleApi.js
root@iZbp1ibd5qj8a3dwztxsf8Z:~# docker run hub.c.163.com/library/hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://cloud.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/engine/userguide/

root@iZbp1ibd5qj8a3dwztxsf8Z:~# docker --version
Docker version 18.03.1-ce, build 9ee9f40
root@iZbp1ibd5qj8a3dwztxsf8Z:~# 
```
#### 参考 <https://blog.csdn.net/jay100500/article/details/76472939>


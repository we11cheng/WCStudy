### 阿里云服务器ubuntu 16.04安装mysql
- 更新系统

```
sudo apt-get update
```
- 安装mysql相关服务

```
sudo apt-get install mysql-server
apt install mysql-client
apt install libmysqlclient-dev
```
#### 期间会输入两次数据库密码

- 使用如下命令查询是否安装成功
 
```
sudo netstat -tap | grep mysql
```

- 重启mysql

```
sudo /etc/init.d/mysql restart
```


- 登录mysql

```
mysql -u root -p
```

- 建议输入如下命令

```
CREATE DATABASE IF NOT EXISTS RUNOOB DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
```
 1. 如果数据库不存在则创建，存在则不创建。
 2. 创建RUNOOB数据库，并设定编码集为utf8

- 添加权限

```
mysql -uroot -p -P3306 -h127.0.0.1
```
#### 表示超级用户名root,密码稍后输入，端口号3306（不输入P默认为3306），主机地址127.0.0.1（若使用本机作为主机，h默认127.0.0.1）
#### 默认是不能用客户端远程连接的，阿里云提供的help.docx里面做了设置说明，mysql密码默认存放在/alidata/account.log，继续操作：
 

```
mysql -u root -h localhost -p
use mysql                #打开mysql数据库
```

```
update user set host='%' where user='root' and host='localhost';

flush privileges;        #刷新权限表，使配置生效
   
```

#### 然后我们就能远程连接我们的mysql了。
     

- 如果您想关闭远程连接，恢复mysql的默认设置（只能本地连接），您可以通过以下步骤操作：

```

use mysql                #打开mysql数据库

#将host设置为localhost表示只能本地连接mysql

update user set host='localhost' where user='root';

flush privileges;        #刷新权限表，使配置生效
     
```

#### 备注：您也可以添加一个用户名为yuancheng，密码为123456，权限为%（表示任意ip都能连接）的远程连接用户。命令参考如下：

```
grant all on *.* to 'yuancheng'@'%' identified by '123456';

flush privileges;

```

#### 启动
sudo service mysql start
#### 停止
sudo service mysql stop
### 服务状态
sudo service mysql status
### 查看数据库
show DATABASES;
### 退出mysql
quit;
exit;

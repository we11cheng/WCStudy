## Ubuntu服务器安装MongoDB
#### MongoDB仅提供64位LTS（长期支持）Ubuntu版本的软件包。
#### 步骤如下
- ssh连接服务器：

- 导入包管理系统使用的公钥：

```
//Ubuntu软件包管理工具（即dpkg和apt）通过要求分销商使用GPG密钥对软件包进行签名来确保软件包的一致性和真实性。用以下命令导入MongoDB公共GPG密钥：
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
```
- 为MongoDB创建一个列表文件：其他ubuntu版本系统请查看MongoDB官网。

```
Ubuntu 12.04
echo "deb [ arch=amd64 ] http://repo.mongodb.org/apt/ubuntu precise/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list

Ubuntu 14.04
echo "deb [ arch=amd64 ] http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list

Ubuntu 16.04
echo "deb [ arch=amd64,arm64 ] http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
```

- 更新本地依赖包索引：

```
sudo apt-get update
```

- 安装mongodb：

```
sudo apt-get install -y mongodb-org
```
- 安装完成后，查看MongoDB版本

```
mongo -version
```
- 启动与重启命令：

```
sudo service mongod start
sudo service mongod restart
sudo service mongod stop
```

- sudo service mongod start可能出现

``` 
Unit mongod.service not found
```
#### 执行
```
sudo systemctl enable mongod
sudo service mongod restart
```
#### 参考链接 <https://askubuntu.com/questions/921753/failed-to-start-mongod-service-unit-mongod-service-not-found>

### 补充:
- 查看是否启动成功

```
sudo cat /var/log/mongodb/mongod.log
```

- 在 mongod.log 日志中若出现如下信息，说明启动成功

```
[initandlisten] waiting for connections on port 27017
```

- MongoDB配置文件路径(vim查看&修改)

```
vim /etc/mongod.conf
```

### mongod.conf参考配置，配置如下：
```
# mongod.conf

# for documentation of all options, see:
#   http://docs.mongodb.org/manual/reference/configuration-options/

# Where and how to store data.
storage:
  dbPath: /var/lib/mongodb
  journal:
    enabled: true
#  engine:
#  mmapv1:
#  wiredTiger:

# where to write logging data.
systemLog:
  destination: file
  logAppend: true
  path: /var/log/mongodb/mongod.log

# network interfaces
net:
  port: 27017
  bindIp: 0.0.0.0
```
#### 补充：bindIp: 0.0.0.0 表示允许所有ip访问。

### MongoDB 数据、日志及配置文件默认存放路径
- 数据默认存放路径：/var/lib/mongodb
- 日志默认存放路径：/var/log/mongodb
- 配置文件默认存放路径: /etc/mongod.conf
- 用户权限设置
- 添加管理员账号

```
mongo
MongoDB shell version v3.6.2
connecting to: mongodb://127.0.0.1:27017
MongoDB server version: 3.6.2
> use admin 
> db.createUser(
   {
     user: "admin",
     pwd: "mongodb123456",
     roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
   }
)
Successfully added user: {
    "user" : "admin",
    "roles" : [
        {
            "role" : "userAdminAnyDatabase",
            "db" : "admin"
        }
    ]
}
```

- 在配置文件中开启权限验证

```
sudo vim /etc/mongod.conf
```

修改security

```
security:  
     authorization: enabled
```
 
- 重启 MongoDB 服务

```
sudo service mongod restart
```

- 验证权限是否生效

```
mongo
MongoDB shell version v3.6.2
connecting to: mongodb://127.0.0.1:27017
MongoDB server version: 3.6.2
> show dbs
2018-02-01T14:39:46.976+0800 E QUERY    [thread1] Error: listDatabases failed:{
	"ok" : 0,
	"errmsg" : "not authorized on admin to execute command { listDatabases: 1.0, $db: \"admin\" }",
	"code" : 13,
	"codeName" : "Unauthorized"
} :
_getErrorWithCode@src/mongo/shell/utils.js:25:13
Mongo.prototype.getDBs@src/mongo/shell/mongo.js:65:1
shellHelper.show@src/mongo/shell/utils.js:813:19
shellHelper@src/mongo/shell/utils.js:703:15
@(shellhelp2):1:1
> use admin
switched to db admin
> db.auth('admin', 'mongodb123456')
1
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
```

- 添加普通用户

```
> use spiders
switched to db spiders
> db.createUser(
... {
...    user: "spiders",
...    pwd: "spiders@2018",
...    roles: [{ role: "readWrite", db: "spiders" }]
... }
... )
Successfully added user: {
	"user" : "spiders",
	"roles" : [
		{
			"role" : "readWrite",
			"db" : "spiders"
		}
	]
}
```
成功添加一个普通用户：

用户名：spiders

密码：spiders@2018

权限：读写 spiders 数据库

#### 角色介绍
- 数据库用户角色：read、readWrite
- 数据库管理角色：dbAdmin、dbOwner、userAdmin
- 集群管理角色：clusterAdmin、clusterManager、clusterMonitor、hostManager
- 备份恢复角色：backup、restore
- 所有数据库角色：readAnyDatabase、readWriteAnyDatabase、userAdminAnyDatabase、dbAdminAnyDatabase
- 超级用户角色：root // 这里还有几个角色间接或直接提供了系统超级用户的访问（dbOwner 、userAdmin、userAdminAnyDatabase）
- 内部角色：__system

#### 角色说明  
- Read：允许用户读取指定数据库
- readWrite：允许用户读写指定数据库
- dbAdmin：允许用户在指定数据库中执行管理函数，如索引创建、删除，查看统计或访问 system.profile
- userAdmin：允许用户向 system.users 集合写入，可以找指定数据库里创建、删除和管理用户
- clusterAdmin：只在 admin 数据库中可用，赋予用户所有分片和复制集相关函数的管理权限
- readAnyDatabase：只在 admin 数据库中可用，赋予用户所有数据库的读权限
- readWriteAnyDatabase：只在 admin 数据库中可用，赋予用户所有数据库的读写权限
- userAdminAnyDatabase：只在 admin 数据库中可用，赋予用户所有数据库的 userAdmin 权限
- dbAdminAnyDatabase：只在 admin 数据库中可用，赋予用户所有数据库的 dbAdmin 权限
- root：只在 admin 数据库中可用。超级账号，超级权限

### MongoDB 卸载

```
sudo apt-get purge mongodb-org*
sudo rm -r /var/log/mongodb
sudo rm -r /var/lib/mongodb
```

#### 官方文档 <https://docs.mongodb.com/master/tutorial/install-mongodb-on-ubuntu/?_ga=2.157663545.222669535.1493745656-724007558.1488558955>
### 踩坑：切记开启服务器端口权限(宝塔面板放行端口竟然不管用),参考阿里云安全组规则。
![](http://p2bzzkn05.bkt.clouddn.com/18-6-20/43353613.jpg)



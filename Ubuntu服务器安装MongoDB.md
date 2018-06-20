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

- MongoDB 卸载

```
sudo apt-get purge mongodb-org*
sudo rm -r /var/log/mongodb
sudo rm -r /var/lib/mongodb
```

#### 官方文档 <https://docs.mongodb.com/master/tutorial/install-mongodb-on-ubuntu/?_ga=2.157663545.222669535.1493745656-724007558.1488558955>
#### 切记开启服务器27017端(宝塔面板放行端口不管用,采坑了) 参考阿里云安全组设置
![](http://p2bzzkn05.bkt.clouddn.com/18-6-20/43353613.jpg)


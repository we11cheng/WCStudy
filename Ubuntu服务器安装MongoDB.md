## Ubuntu服务器安装MongoDB
#### MongoDB仅提供64位LTS（长期支持）Ubuntu版本的软件包。
#### 步骤如下
- ssh连接服务器：

- 导入包管理系统使用的公钥：

```
//Ubuntu软件包管理工具（即dpkg和apt）通过要求分销商使用GPG密钥对软件包进行签名来确保软件包的一致性和真实性。用以下命令导入MongoDB公共GPG密钥：
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
```
- 为MongoDB创建一个列表文件：

```
//使用适合的Ubuntu版本的命令创建/etc/apt/sources.list.d/mongodb-org-3.4.list列表文件：
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
sudo apt-get install -y mongodb
```

- 配置启动文件，需要手动新建/lib/systemd/system/mongod.service文件，并写入下面内容：

```
[Unit]
Description=High-performance, schema-free document-oriented database
After=network.target
Documentation=https://docs.mongodb.org/manual

[Service]
User=mongodb
Group=mongodb
ExecStart=/usr/bin/mongod --quiet --config /etc/mongod.conf

[Install]
WantedBy=multi-user.target
```
- 启动与重启命令：

```
sudo service mongod start
sudo service mongod restart
sudo service mongod stop
```

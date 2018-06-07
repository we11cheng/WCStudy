### 部署Node.js项目(Ubuntu16.04)
#### Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境，用来方便地搭建快速的易于扩展的网络应用。Node.js 使用了一个事件驱动、非阻塞式 I/O 的模型，使其轻量又高效，非常适合运行在分布式设备的数据密集型的实时应用。Node.js 的包管理器 npm，是全球最大的开源库生态系统。

### 基本流程
- ssh连接到云服务器 
 
- 安装NVM，NVM是Node.js的版本管理软件。安装好git的前提下

```
git clone https://github.com/cnpm/nvm.git ~/.nvm && cd ~/.nvm && git checkout `git describe --abbrev=0 --tags`
```
- 激活NVM 

```echo ". ~/.nvm/nvm.sh" >> /etc/profile ```

```source /etc/profile```

- 列出Node.js的所有版本。

```nvm list-remote```

- 安装指定版本

```nvm install v7.4.0```

- 部署测试项目

```cd ~```

```touch exampleApi.js```

- 编辑exampleApi.js ```vim exampleApi.js```

```const http = require('http');
const hostname = '0.0.0.0';
const port = 30001;
var data = {
    'code':'000',
    'message':'message',
    'lists':[
        {
          'naem':'小明',
            'age': '12',
            'sex':'男'
        },{
          'naem':'小红',
            'age': '112',
            'sex':'女'
        }
    ]

}
const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/json');
  res.end(JSON.stringify(data));
});
server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```
- 运行项目

```node ~/example.js```

- 后台运行 

```node ~/example.js &```

- 查看效果

![](http://p2bzzkn05.bkt.clouddn.com/18-6-7/35979962.jpg)

### 补充：
#### 查看端口使用情况
```ps -ef | grep node```
#### 查看某一个端口
```
netstat -ap | grep 30001
```
eg:

```
root@ubuntu:~# netstat -ap | grep 30001
tcp        0      0 *:30001                 *:*                     LISTEN      30134/node     
```

#### Linux关闭nodejs服务
```
1、ps -ef | grep node

查看node对应的pid，然后kill pid，再进入对应项目npm start

2、如果以上方法不行可以这样：

kill node 或者kilall node
```
eg:

```root@ubuntu:~# ps -ef | grep node
root     30080 28754  1 15:42 pts/4    00:00:00 node exampleApi.js
root     30087 28754  0 15:42 pts/4    00:00:00 grep --color=auto node
root@ubuntu:~# kill 30080
[1]+  Terminated              node exampleApi.js
root@ubuntu:~# ps -ef | grep node
root     30098 28754  0 15:44 pts/4    00:00:00 grep --color=auto node
root@ubuntu:~# 

```


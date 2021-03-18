## picgo配置七牛云
### 配置项说明
``` 
{
  "accessKey": "",
  "secretKey": "",
  "bucket": "", // 存储空间名
  "url": "", // 自定义域名
  "area": "z0" | "z1" | "z2" | "na0" | "as0", // 存储区域编号
  "options": "", // 网址后缀，比如?imgslim
  "path": "" // 自定义存储路径，比如img/
}
```
对应的密钥信息需要到七牛自己的控制台里找到<https://portal.qiniu.com/user/key>。其中需要注意的是，自己的存储空间的区域需要确定：
![](http://img.ipersistence.top/20201224233207.png)
一目了然
![](http://img.ipersistence.top/20201224233316.png)
![](http://img.ipersistence.top/20201224233420.png)
### 参考链接<https://picgo.github.io/PicGo-Doc/zh/guide/config.html#%E4%B8%83%E7%89%9B%E5%9B%BE%E5%BA%8A>
### 关于GitHub Pages自定义域名

### 相关概念

```
用户 Pages	username.github.io	自动重定向至已经设定好的自定义域名	user.example.com
```

```
组织 Pages	orgname.github.io	自动重定向至已经设定好的自定义域名	org.example.com
```
```
用户账号拥有的项目 Pages	username.github.io/projectname	自动重定向到一个由用户指定的，用户网站自定义域名的子目录（ user.example.com/projectname），以及所有自定义域名	project.example.com
```
```
组织拥有的项目 Pages	orgname.github.io/projectname	自动重定向到一个由组织指定的，项目 Pages 自定义域名的子目录(org.example.com/projectname)，以及所有自定义域名	project.example.com
```

### 设置 GitHub Pages 的自定义域名

- 为你的库添加一个 CNAME 文件。

![](https://raw.githubusercontent.com/we11cheng/picBed/master/20190418181554.png)

- 填写Github Pages 设置部分。（配置Custome domain 会和手动创建CNAME冲突，选择一种方式即可）

![](https://raw.githubusercontent.com/we11cheng/picBed/master/20190418181834.png)

- 域名解析配置部分（域名使用 cname 指向 xxx.github.io）

![](https://raw.githubusercontent.com/we11cheng/picBed/master/20190418182217.png)

### 配套repo <https://github.com/we11cheng/gwcBlog> 
### 自定义域名访问 <https://208917.xyz/>
### 如何搭建 <https://github.com/getgridea/gridea>
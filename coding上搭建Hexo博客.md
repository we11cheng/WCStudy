### coding上搭建Hexo博客
#### coding官网地址<https://coding.net/>

#### 环境安装,需要预先好git和node.js

#### 全局安装hexo和hexo的运行工具
```
  npm install hexo
  npm install hexo-cli
```
#### 初始化项目,这里使用blog作为项目名
```
  hexo init blog
```
#### 运行hexo	
```
  cd blog
  hexo server
```
#### 查看 http://localhost:4000/ 可以浏览到hexo初始化的默认主体博客。
#### 关联coding项目

- 在coding的上面创建一个新项目，创建项目-选择模版-选择DevOps 项目包含项目管理、代码托管、CI/CD 和制品库等功能，提供完整的研发流程。⚠️选择模版跟重要，涉及到发布静态网站。

- 绑定ssh证书(可以选择性配置)，本地生成ssh证书，把证书的公钥(~/.ssh/id_rsa.pub)的内容拷贝到coding个人账户下的SSH公钥里面。

- 关联本地仓库与coding远程仓库

```
cd blog
git init
git remote add origin https://e.coding.net/gwc-01/xxx.git
git add -A
git commit -m "first commit"   
git push -u origin master 
```
#### 如果push遇到```refusing to merge unrelated histories```,执行`git pull origin master --allow-unrelated-histories`即可同步。

- 配置hexo的deploy方式，在一开始hexo生成的blog目录下面找到_config.yml，在文件最低端加上deploy到coding的配置。具体细节可以参考<https://gist.github.com/we11cheng/0ec6084c3443f4c1d7edd688cf30b300>或者问我也行。

```
deploy:
  type: git
  repo:
    coding: git@git.coding.net:gwc-01/xxx.git,master
```
#### 注意coding的配置是 coding:{项目git地址},{branch}
- 最后执行生成静态文件

```
hexo g
```
#### ⚠️由于coding的Pages服务是默认执行jekyll项目, 静态项目需要一个.nojekyll文件标识.所以第一次执行, 需要在生成的public文件夹中加入一个 .nojekyll 空白文件
- 本地预览命令

```
hexo s
```
- 部署命令

```
hexo d
```
或者

```
hexo d -g
```

#### 就可以发布成功了,发布的方式可以在coding 仓库自行配置，手动发布or自定发布自动。


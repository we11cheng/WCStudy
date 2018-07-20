### Ubuntu添加User用户

- root登录

```
ssh root@your_server_ip
```
- 创建新用户

```
adduser newusername
```
##### ```newusername```为添加的用户名

- 赋予权限

```
usermod -aG sudo newusername
```

- 测试

```
ssh newusername@your_server_ip
```

- 切换root

```
sudo su
```

- 切换newusername

```
su newusername
```

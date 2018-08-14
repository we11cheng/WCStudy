### 删除Docker镜像

- 查看镜像

```
docker images
```
#### 正确返回示例
```
root@iZbp1ibd5qj8a3dwztxsf8Z:~# docker images
REPOSITORY                          TAG                 IMAGE ID            CREATED             SIZE
88250/solo                          latest              6d1c7d248b8a        2 days ago          748MB
local_pluosi/app_host               0.1.1               811c2bbb6e5a        3 weeks ago         1.04GB
<none>                              <none>              db23b227f83a        3 weeks ago         1.04GB
<none>                              <none>              47825a3eb37c        3 weeks ago         1.04GB
<none>                              <none>              d5bbed4c9e5f        3 weeks ago         1.04GB
<none>                              <none>              4398e1d55208        3 weeks ago         1.04GB
ruby                                2.5.0               213a086149f6        5 months ago        869MB
hub.c.163.com/library/hello-world   latest              1815c82652c0        14 months ago       1.84kB
```

- 尝试使用 ```docker rmi```删除镜像

```
docker rmi <image-id>
```
#### 尝试举例删除6d1c7d248b8a这个镜像
```
root@iZbp1ibd5qj8a3dwztxsf8Z:~# docker rmi 6d1c7d248b8a

```
#### 如无意外，这个镜像嘟嘟嘟就会被干掉。然而很不幸大多数时候都会提示下来错误，表示镜像正在被某个容器 container 使用...
```
Error response from daemon: conflict: unable to delete 6d1c7d248b8a (cannot be forced) - image is being used by running container 32046a6bbb39
```
- 解决方案


```
docker ps -a | grep <container-id>
docker stop <container-id>
docker rm <container-id>
docker rmi <image-id>
```

#### 正确返回示例

```
root@iZbp1ibd5qj8a3dwztxsf8Z:~# docker ps -a | 32046a6bbb39
root@iZbp1ibd5qj8a3dwztxsf8Z:~# docker stop 32046a6bbb39
32046a6bbb39
root@iZbp1ibd5qj8a3dwztxsf8Z:~# docker rm 32046a6bbb39
32046a6bbb39
root@iZbp1ibd5qj8a3dwztxsf8Z:~# docker rmi 6d1c7d248b8a
Untagged: 88250/solo:latest
Untagged: 88250/solo@sha256:f27bb35ed5162f9d051d89b0daeede5230200ad59ca043d4b2a21af70180bb37
Deleted: sha256:6d1c7d248b8a088ab192aaaac3268d749664e5e7c6eeb146fb7abd847ef784b3
Deleted: sha256:db87b486e3f8270ea8675c47a39b96613a9c23c5ac57d7404624aacd13bfcb8a
Deleted: sha256:817a66efbca6a38b8ae3e4d349dfe4e8f5be2ec59b8df0b9697cdd3be84981c1
Deleted: sha256:65afde8212865c8cdb0e1cb99633658903f1e788bafd99803b1112f0544670d1
Deleted: sha256:95c25c3ec7387f18c4506fa52949027040fcfe0f1c3b51fd9cc2d10635ba598f
Deleted: sha256:4336a314fa2eeadb67a64289b7b68fc3615bb87b5ac4087929235d09700e3dac
Deleted: sha256:8f5bb3bab2459c19860271e58a19bcac30b5e76af36ded12b70293b0a07e4519
Deleted: sha256:1cbc61bc6b9f20eba253047a205910c3379bf2924c4441e4a74aea8541d76da3
Deleted: sha256:62e9f47848051ad0fe098d0532f47ee76a80d805e627d4b72fb0d272b15892a2
Deleted: sha256:0f2a1e31925280c37d08ebf5f6c1298674a396e8c1b6247b019fd90991f8839e
Deleted: sha256:6aad81f3879e5e943b98f5f13e723b16830930e82cb943988960b074ed3d9d0a
Deleted: sha256:12a3a53c8177c08e285cd92e99e72ee67d9b0d82ab1fde59937cf31df3802ee7
Deleted: sha256:16f7de7f9429c8fc39fc9e8207a9f6b63513a7f30b7dc8af0faf8bf213315520
Deleted: sha256:d3898f75e7b8a2a7e45bfdd351a00c4ad95b743861860635d702378fd073771d
Deleted: sha256:46ddeaf1e1efd81fd6cad11c44af4e4ba71cbab32b75f60f8647f025a8874315
Deleted: sha256:4e9ac8670c1ea60c504c1dc22e38a177afd782a48e17e81e06ecf60a1c8f4ef0
Deleted: sha256:76dc20911db5ba40907269c70aa4ef7caf207ea4aa23818b8db2ff83ba74e1e4
Deleted: sha256:b4ff564f2a75c2bc85c8eda2928ec73b13809416658f949d2b55fa24448c08b1
Deleted: sha256:2d9c829ae3f7ff3e148e5c7c3a1cf378b0f90b79035e2fe9a8d78c63ccde4c89
Deleted: sha256:b1ae7168c6f3e061aa3943740ec3ceaf8e582dc65feab31d2b56d464a5062d59
Deleted: sha256:4a495dbc04bd205c728297a08cf203988e91caeafe4b21fcad94c893a53d96dc
Deleted: sha256:3b10514a95bec77489a57d6e2fbfddb7ddfdb643907470ce5de0f1b05c603706
```
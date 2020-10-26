---
title: 搭建Harbor
linktitle:  搭建Harbor
toc: true
type: docs
date: "2020-06-28T00:00:00+01:00"
draft: false
menu:
 docker-study:
    parent: docker 私有库
    weight: 2
---

## 下载

下载链接：https://storage.googleapis.com/harbor-releases/harbor-offline-installer-v1.6.1.tgz

将文件移动到/opt目录下

解压会在当前目录生成harbor目录

```bash
tar xvf harbor-offline-installer-v1.6.1.tgz
```
## 修改harbor.cfg

```bash
vim  harbor/harbor.cfg
```

 hostname设置访问地址，可以使用ip、域名，不可以设置为127.0.0.1或localhost，此处我设置为本地ip
```
hostname = 172.16.50.37
```
Harbor启动后，管理员UI登录的密码，默认是Harbor12345
```
harbor_admin_password = Harbor12345
```

认证方式，这里支持多种认证方式，如LADP、本次存储、数据库认证。默认是db_auth，mysql数据库认证
```
auth_mode = db_auth
```
是否开启自注册
```
self_registration = on
```

Token有效时间，默认30分钟
```
token_expiration = 30
```



## 启动 Harbor

在harbor目录下执行
```bash
./install.sh
```
![](/img/docker/47.jpg)

查看compose状态

```bash
docker-compose ps
```
![](/img/docker/48.jpg)

## 修改端口
在公网上，一般情况下都不暴露默认端口，避免被攻击！
以下修改harbor的默认80端口为其他端口！

修改docker-compose.yml
![](/img/docker/53.jpg)

修改common/templates/registry/config.yml文件
![](/img/docker/54.jpg)

重新启动并生成配置文件
```bash
docker-compose stop
```
```bash
./install.sh
```
## 访问

输入http://你的IP:18080/harbor/sign-in
![](/img/docker/49.jpg)


输入用户名admin，默认密码（或已修改密码）
![](/img/docker/50.jpg)

原文链接：https://blog.csdn.net/funtaster/java/article/details/83268974





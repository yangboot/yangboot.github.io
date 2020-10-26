---
title: 安装MySQL
linktitle: 安装MySQL
toc: true
type: docs
date: "2020-06-24T00:00:00+01:00"
draft: false
menu:
 docker-study:
    parent: docker
    weight: 7
---

### 拉取镜像
```shell
docker pull mysql
```
### 运行
```
docker run  -p 3306:3306 --name mysql 
-v /mysql/conf:/etc/mysql/conf.d 
-v /mysql/logs:/logs -v /mysql/data:/var/lib/mysql 
-e MYSQL_ROOT_PASSWORD=你的密码 -d   mysql:5.7
```

### 开启运程登录权限

登录数据库
```shell
mysql -uroot -p
```

分配权限
```
GRANT ALL ON *.* TO 'root'@'%' IDENTIFIED BY "密码" ;
```
刷新权限
```
FLUSH PRIVILEGES ;
```

![](/img/docker/38.jpg)



---
title:  Nacos安装.
linktitle: Nacos安装.
toc: true
type: docs
date: "2020-07-28T00:00:00+01:00"
draft: false
menu:
  cloud-albaba:
    parent: Spring-Cloud-Alibaba-Necos
    weight: 1
---

## 下载镜像
```shell
docker pull nacos/nacos-server
```
## 启动
```shell
docker run --env MODE=standalone --name nacos -d -p 8848:8848 -v /opt/nacos/conf:/home/nacos/conf nacos/nacos-server:latest
```


## 使用docker-compose启动



- 配置文件

docker-compose文件 `standalone-derby.yaml`



```ruby
version: "2"
services:
  nacos:
    image: nacos/nacos-server:latest
    container_name: nacos
    environment:
    - MODE=standalone
    volumes:
    - /opt/nacos/logs:/home/nacos/logs
    -  /opt/nacos/init.d/custom.properties:/home/nacos/init.d/custom.properties
    ports:
    - "8848:8848"
```

- 启动



```css
docker-compose -f standalone-derby.yaml up
```

- 关闭



```css
docker-compose -f standalone-derby.yaml stop
```

- 移除



```css
docker-compose -f standalone-derby.yaml rm
```

- 关闭且移除



```css
docker-compose -f standalone-derby.yaml down
```




## 测试访问

直接访问 [http://127.0.0.1:8848/nacos](https://links.jianshu.com/go?to=http%3A%2F%2F127.0.0.1%3A8848%2Fnacos)， 使用账号：nacos，密码：nacos 直接登录。nacos是默认账号和密码，登录成功后可以修改密码。



/home/nacos/conf


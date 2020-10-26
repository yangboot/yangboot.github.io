---
title:  docker Compose使用
linktitle:  docker Compose使用
toc: true
type: docs
date: "2020-06-28T00:00:00+01:00"
draft: false
menu:
 docker-study:
    parent: docker 三剑客 Compose
    weight: 2
---

## 术语

- 服务 (`service`)：一个应用容器，实际上可以运行多个相同镜像的实例。

- 项目 (`project`)：由一组关联的应用容器组成的一个完整业务单元。

一个项目可以由多个服务（容器）关联而成，Compose 面向项目进行管理。

## docker Compose启动tomcat
在 `/usr/local/docker/tomcat`下创建一个` docker-compose.yml `
```
version: '3'
services:
  tomcat:
    restart: always
    image: tomcat
    container_name: tomcat
    ports:
     - 18080:8080
```
## 运行 compose 项目
在` docker-compose.yml `所在的目录下执行
```shell
docker-compose up
```
后台运行
```shell
docker-compose up -d
```
![](/img/docker/44.jpg)


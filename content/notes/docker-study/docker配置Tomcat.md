---
title: 配置Tomcat
linktitle: 配置Tomcat
toc: true
type: docs
date: "2020-06-15T00:00:00+01:00"
draft: false
menu:
 docker-study:
    parent: docker
    weight: 4
---

### 拉取Tomcat镜像

```shell
docker pull tomcat
```
### 运行tomcat

将宿主机的8081端口指向到容器的8080端口

```shell
docker run -d --name tomcat -p 8081:8080 tomcat
```



发现Tomcat已经启动了但是访问不了

![](/img/docker/7.jpg)

进入交互模式发现webapps是空的，资源都在webapps.dist下

```shell
docker exec -it 926d769607f4 /bin/bash
```
![](/img/docker/8.jpg)

将webapps.dist下的所有文件复制到webapps
```shell
cp -R webapps.dist/*  webapps
```

再次访问成功！！！！！
![](/img/docker/9.jpg)




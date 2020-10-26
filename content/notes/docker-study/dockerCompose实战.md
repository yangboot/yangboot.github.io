---
title:  dockerCompose实战
linktitle:  dockerCompose实战
toc: true
type: docs
date: "2020-06-28T00:00:00+01:00"
draft: false
menu:
 docker-study:
    parent: docker 三剑客 Compose
    weight: 3
---

## docker Compose 部署RuoYi项目

`RuoYi` ， `MYSQL` ， `docker Compose`，  `dockerfile`

 [项目gitee地址](https://gitee.com/MrPen/docker-ruoyi-demo).

根据 [ruoyi官方](http://doc.ruoyi.vip/ruoyi/document/hjbs.html#%E5%BF%85%E8%A6%81%E9%85%8D%E7%BD%AE).文档将RuoYi项目打包成jar

### 打jar包

`bin/package.bat` 在项目的目录下执行
 然后会在项目下生成 `target`文件夹包含 `war` 或`jar` （多模块生成在ruoyi-admin）

1、jar部署方式
 使用命令行执行：`java –jar ruoyi.jar` 或者执行脚本：`bin/run.bat`

2、war部署方式
 pom.xml packaging修改为`war` 放入tomcat服务器webapps

### 修改配置文件
application-druid.yml
```yaml
spring:
    datasource:
        type: com.alibaba.druid.pool.DruidDataSource
        driverClassName: com.mysql.cj.jdbc.Driver
        druid:
            # 主库数据源 mysqldbserver必须与 container_name一致
            master:
                url: jdbc:mysql://mysqldbserver:3306/ry?useUnicode=true&characterEncoding=utf8&zeroDateTimeBehavior=convertToNull&useSSL=true&serverTimezone=GMT%2B8
                username: root
                password: root
```

### 编写app-dockerfile
```dockerfile
FROM java:8
MAINTAINER mrpen
ADD  ./ruoyi/ruoyi-admin.jar  app.jar
EXPOSE 8081
ENTRYPOINT ["java","-jar","app.jar"]
```



- FROM: 基础镜像,通过jdk8镜像开始
- MAINTAINER: 维护者
- ADD: 复制jar包到镜像内,名字为app.jar
- EXPOSE: 声明端口
- ENTRYPOINT: docker启动时,运行的命令.这里就是容器运行就启动jar服务

### 编写mysql-dockerfile

```dockerfile
FROM mysql:5.7
MAINTAINER mrpen
```

### 编写docker-compose.yml
```yml

version : '3'
services:
  mysqldbserver:
    container_name: mysqldbserver
    image: mysql:5.7
    build:
      context: .
      dockerfile: mysql-dockerfile
    ports:
      - "3306:3306"
    volumes:
      - ./mysql/conf:/etc/mysql/conf.d
      - ./mysql/logs:/logs
      - ./mysql/data:/var/lib/mysql
    command: [
          'mysqld',
          '--innodb-buffer-pool-size=80M',
          '--character-set-server=utf8',
          '--collation-server=utf8_general_ci',
          '--default-time-zone=+8:00',
          '--lower-case-table-names=1'
        ]
    environment:
      MYSQL_DATABASE: ry
      MYSQL_ROOT_PASSWORD: root
  ryappserver:
    container_name: ryappserver
    build:
      context: .
      dockerfile: app-dockerfile
    ports:
      - "8081:8081"
    volumes:
      - ./uploadPath:/home/ruoyi/uploadPath
    depends_on:
      - mysqldbserver
    links:
      - mysqldbserver

```

### 启动

```bash
docker-compose ps -d
```

这个时候还是访问不了的，因为我们的数据库还没导入

![](/img/docker/61.jpg)

将`ry.sql`导入后

重新启动`ryappserver`服务

```bash
docker-compose restart ryappserver
```
### 查看`ryappserver`的容器ID
```bash
docker ps
```
![](/img/docker/62.jpg)

### 查看容器日志

```bash
docker logs -f 2559d6cdadf7
```
![](/img/docker/63.jpg)

### 运行成功
![](/img/docker/64.jpg)
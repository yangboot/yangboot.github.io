---
title:  config总控中心
linktitle: config总控中心
toc: true
type: docs
date: "2020-07-28T00:00:00+01:00"
draft: false
menu:
  cloud-study:
    parent: config
    weight: 1
---

![](/img/springCloud/43.jpg)   

## git仓库

`cloud2020_config`

https://gitee.com/MrPen/cloud2020_config.git

## 新建工程

`cloud-config-center-3344`

## pom

```xml
<dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-config-server</artifactId>
        </dependency>
```

## yml

```java
server:
  port: 3344

spring:
  application:
    name: cloud-gateway
  cloud:
    config:
      server:
        git:
          #GitHub上面的git仓库名字
          uri: https://gitee.com/MrPen/cloud2020_config.git
          #搜索目录
          search-paths:
            - cloud2020_config
      #读取分支
      label: master
     
eureka:
  instance:
    hostname: cloud-config-center
  client:
    fetch-registry: true
    register-with-eureka: true
    service-url:
      defaultZone: http://localhost:7001/eureka/

```



## 启动类

```java


import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.config.server.EnableConfigServer;

/**
 * @author yzg
 */
@SpringBootApplication
@EnableConfigServer
public class ConfigCenterMain3344 {
    public static void main(String[] args) {
        SpringApplication.run(ConfigCenterMain3344.class, args);
    }
}

```
## 测试

访问http://localhost:3344/master/config-dev.yml
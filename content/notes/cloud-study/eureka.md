---
title: eureka
date: "2020-06-29T00:00:00Z"
lastmod: "2020-07-20T00:00:00Z"
draft: false  # Is this a draft? true/false
toc: true  # Show table of contents? true/false
type: docs  # Do not modify.

# Add menu entry to sidebar.
# - name: Declare this menu item as a parent with ID `name`.
# - weight: Position of link in menu.
menu:
 cloud-study:
    name: eureka
    weight: 2
---



## POM

```xml
 <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>
```

## yml

```
server:
  port: 7001
eureka:
  instance:
    hostname: localhost
  client:
    #false表示不向注册中心注册自己
    register-with-eureka: false
    #false表示自己端就是注册中心
    fetch-registry: false
    #设置与eureka server交互的地址，查询服务和注册服务都是这个地址
    service-url:
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/

```

## 启动类

```java
package com.yzg.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

/**
 * eureka服务端启动类
 *
 * @author yangzhangguan
 * @create 2020/7/20 23:32
 **/
@SpringBootApplication
@EnableEurekaServer
public class EurekaMain7001 {
    public static void main(String[] args) {
        SpringApplication.run(EurekaMain7001.class);
    }
}

```



![](/img/springCloud/14.jpg)
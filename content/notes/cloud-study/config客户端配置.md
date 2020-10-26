---
title:  config客户端配置
linktitle: config客户端配置
toc: true
type: docs
date: "2020-08-4T00:00:00+01:00"
draft: false
menu:
  cloud-study:
    parent: config
    weight: 2
---



## POM

```xml
<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-config</artifactId>
		</dependency>
```

## YML

新建`bootstrap.yml`

```
server:
  port: 3355

spring:
  application:
    name: config-client
  cloud:
    config:
      label: master 
      name: config 
      profile: dev 
      uri: http://localhost:3344 
eureka:
  client:
    service-url:
      defaultZone: http://localhost:7001/eureka

```

命名规则

![](/img/springCloud/45.jpg)   

## 启动类

```java
package com.cloud.config;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

@SpringBootApplication
@EnableEurekaClient
public class ConfigClientMain3355 {
    public static void main(String[] args) {
        SpringApplication.run(ConfigClientMain3355.class, args);
    }
}

```



## 测试

controller

```java
package com.cloud.config.controller;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.cloud.context.config.annotation.RefreshScope;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

/**
 *
 * @author yzg
 * @version 1.0
 * @create 2020/08/04
 */
@RestController
@RefreshScope
public class ConfigClientController {

    @Value("${config.info}")
    private String configInfo;

    @GetMapping("/configInfo")
    public String getConfigInfo(){
        return configInfo;
    }
}

```
## 测试
运行cloud-eureka-server7001  
运行cloud-eureka-server7002  
运行cloud-config-center-3344  
运行cloud-config-client-3355   
访问http://localhost:3355/configInfo    

![](/img/springCloud/46.jpg)   
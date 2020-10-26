---
title:  bus动态刷新全局广播
linktitle:  bus动态刷新全局广播
toc: true
type: docs
date: "2020-07-28T00:00:00+01:00"
draft: false
menu:
  cloud-study:
    parent: bus
    weight: 3
---
## 架构图

![](/img/springCloud/49.jpg)   

## 控制总线3344配置

`cloud-config-center-3344`

### POM

```xml
<dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-bus-amqp</artifactId>
        </dependency>
```

## yml

添加  rabbitmq相关配置

```
  rabbitmq:
    host: 203.176.95.155
    port: 8672
    username: guest
    password: guest

# 暴露bus刷新配置的端点
management:
  endpoints:
    web:
      exposure:
        include: "bus-refresh"

```

## 客户端3355，3366配置
### POM

```xml
<dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-bus-amqp</artifactId>
        </dependency>
```

## yml

添加  rabbitmq相关配置
```
  rabbitmq:
    host: 203.176.95.155
    port: 8672
    username: guest
    password: guest

```



## 测试

运行cloud-eureka-server7001  
运行cloud-eureka-server7002  
运行cloud-config-center-3344  
运行cloud-config-client-3355   
运行cloud-config-client-3366   
修改gitee上的config-dev.yml文件

```
config: 
  info: "master Branches cloud2020_config/config-dev-yml v=3"
```
给控制总线发送POST请求   curl -X POST "http://localhost:3344/actuator/bus-refresh"  
![](/img/springCloud/50.jpg) 
访问http://localhost:3344/master/config-dev.yml
![](/img/springCloud/51.jpg) 
访问http://localhost:3355/configInfo    
![](/img/springCloud/52.jpg) 
访问http://localhost:3366/configInfo    
![](/img/springCloud/53.jpg)   
查看rabbitMQ
![](/img/springCloud/54.jpg)   
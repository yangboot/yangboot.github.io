---
title:  eureka-server集群
linktitle:  eureka-server集群
toc: true
type: docs
date: "2020-06-28T00:00:00+01:00"
draft: false
menu:
  cloud-study:
    parent: eureka
    weight: 1
---





## 修改主机映射文件(模拟两台主机）

文件地址`C:\Windows\System32\drivers\etc\hosts`
```
127.0.0.1 eureka7001.com
127.0.0.1 eureka7002.com
```

## 修改配置文件

cloud-eureka-server7001项目
```
server:
  port: 7001
eureka:
  instance:
    hostname: eureka7001.com
  client:
    #false表示不向注册中心注册自己
    register-with-eureka: false
    #false表示自己端就是注册中心
    fetch-registry: false
    #设置与eureka server交互的地址，查询服务和注册服务都是这个地址
    service-url:
      defaultZone: http://eureka7002.com:7002/eureka/
```
cloud-eureka-server7002项目
```
server:
  port: 7002
eureka:
  instance:
    hostname: eureka7002.com
  client:
    #false表示不向注册中心注册自己
    register-with-eureka: false
    #false表示自己端就是注册中心
    fetch-registry: false
    #设置与eureka server交互的地址，查询服务和注册服务都是这个地址
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/
      
```
![](/img/springCloud/16.jpg)

![](/img/springCloud/17.jpg)
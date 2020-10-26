---
title:  config手动刷新
linktitle: config手动刷新
toc: true
type: docs
date: "2020-08-4T00:00:00+01:00"
draft: false
menu:
  cloud-study:
    parent: config
    weight: 3
---



## 编辑项目

`cloud-config-client-3355`

## POM

```xml
<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>
```

## YML

新建以下配置

```
management:
  endpoints:
    web:
      exposure:
        include: "*"
```
## controller
添加`@RefreshScope`注解


## 测试
运行cloud-eureka-server7001  
运行cloud-eureka-server7002  
运行cloud-config-center-3344  
运行cloud-config-client-3355   
访问http://localhost:3355/configInfo    

![](/img/springCloud/46.jpg)   

修改gitee上的config-dev.yml文件

```
config: 
  info: "master Branches cloud2020_config/config-dev-yml v=2"
```

再次访问http://localhost:3355/configInfo    

![](/img/springCloud/46.jpg)   

客户端发送POST请求curl -X POST "http://localhost:3355/actuator/refresh"

![](/img/springCloud/47.jpg)   

再次访问http://localhost:3355/configInfo    

![](/img/springCloud/48.jpg)   
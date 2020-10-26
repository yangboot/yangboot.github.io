---
title:  Gateway配置动态路由
linktitle: Gateway配置动态路由
toc: true
type: docs
date: "2020-07-28T00:00:00+01:00"
draft: false
menu:
  cloud-study:
    parent: Gateway
    weight: 1
---
修改项目`cloud-gateway-gateway9527`
## POM
```xml
<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
		</dependency>
```

## YML
 开启从注册中心动态创建路由的功能，利用微服务名称j进行路由
```
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true 
```

修改` uri`
```
  - id: payment_route2
          uri: lb://CLOUD-PAYMENT-SERVICE
          predicates:
            Path=/payment/lb/** #断言,路径相匹配的进行路由
```





## 测试

运行cloud-eureka-server7001  
运行cloud-eureka-server7002  
运行cloud.provider.payment8001  
运行cloud.provider.payment8002  
运行cloud-gateway-gateway9527  
不断访问http://localhost:9527/payment/lb  
![](/img/springCloud/5.gif)



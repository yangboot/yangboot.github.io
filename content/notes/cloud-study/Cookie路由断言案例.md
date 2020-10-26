---
title:   Cookie路由断言案例
linktitle:  Cookie路由断言案例
toc: true
type: docs
date: "2020-07-28T00:00:00+01:00"
draft: false
menu:
  cloud-study:
    parent: Gateway
    weight: 4
---
### yml
```java
predicates:
            - Path=/payment/lb/** 
            - Cookie=username,yzg
            
```

## 测试

运行cloud-eureka-server7001  
运行cloud-eureka-server7002  
运行cloud.provider.payment8001  
运行cloud.provider.payment8002  
运行cloud-gateway-gateway9527  
访问curl http://localhost:9527/payment/lb  
![](/img/springCloud/39.jpg)
访问curl http://localhost:9527/payment/lb --cookie "username=yzg"   
![](/img/springCloud/40.jpg)
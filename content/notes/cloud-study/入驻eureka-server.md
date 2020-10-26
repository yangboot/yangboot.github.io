---

title:  服务入驻eureka-server
linktitle:  服务入驻eureka-server
toc: true
type: docs
date: "2020-06-28T00:00:00+01:00"
draft: false
menu:
  cloud-study:
    parent: eureka
    weight: 2
---

## POM

```xml
       <!--eureka client-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>
```

## yml

```
eureka:
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      defaultZone: http://localhost:7001/eureka
     
```

集群版

```
 # 集群版 
 defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
```



## 启动类

```java
@SpringBootApplication
@EnableEurekaClient
public class PaymentMain8001 {
    public static void main(String[] args) {
        SpringApplication.run(PaymentMain8001.class, args);
    }
}
```

## 效果
先运行注册中心，再运行提供者

![](/img/springCloud/15.jpg)
---
title:  Gateway搭建
linktitle: Gateway搭建
toc: true
type: docs
date: "2020-07-28T00:00:00+01:00"
draft: false
menu:
  cloud-study:
    parent: Gateway
    weight: 1
---
新建项目`cloud-gateway-gateway9527`
## POM
```XML
 <?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<artifactId>com-yzg-springcloud</artifactId>
		<groupId>com.yzg.springcloud</groupId>
		<version>1.0-SNAPSHOT</version>
	</parent>

	<artifactId>cloud-gateway-gateway9527</artifactId>
	<description>网失模块</description>
	<dependencies>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-gateway</artifactId>
		</dependency>
		<!--gateway无需web和actuator -->
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
		</dependency>
		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
			<optional>true</optional>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>com.yzg.springcloud</groupId>
			<artifactId>cloud-api-common</artifactId>
			<version>${project.version}</version>
		</dependency>
	</dependencies>

</project>
```

## YML

```
server:
  port: 9527

spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true # 开启从注册中心动态创建路由的功能，利用微服务名称j进行路由
      routes:
        - id: payment_route # 路由的id,没有规定规则但要求唯一,建议配合服务名
          #匹配后提供服务的路由地址
          uri: http://localhost:8001
          predicates:
            - Path=/payment/get/** # 断言，路径相匹配的进行路由
        - id: payment_route2
          uri: http://localhost:8001
          predicates:
            Path=/payment/lb/** #断言,路径相匹配的进行路由

eureka:
  instance:
    hostname: cloud-gateway-service
  client:
    fetch-registry: true
    register-with-eureka: true
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/

```



## 启动类


```java
package com.cloud.gateway;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

/**
 * @author yzg
 */
@SpringBootApplication
@EnableEurekaClient
public class GatewayMain9527 {
    public static void main(String[] args) {
        SpringApplication.run(GatewayMain9527.class, args);
    }
}

```

## 测试

运行cloud-eureka-server7001  
运行cloud-eureka-server7002  
运行cloud.provider.payment8001  
运行cloud-gateway-gateway9527  
访问http://localhost:8001/payment/get/1  
![](/img/springCloud/33.jpg)
访问http://localhost:9527/payment/get/1  
![](/img/springCloud/34.jpg)  


## config方式配置

新建GatewayConfig.java

```java
package com.cloud.gateway.config;

import org.springframework.cloud.gateway.route.RouteLocator;
import org.springframework.cloud.gateway.route.builder.RouteLocatorBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * 网关配置
 *
 * @author yzg
 * @version 1.0
 * @create 2020/08/03
 */
@Configuration
public class GatewayConfig {

	/**
	 * 配置了一个id为route-name的路由规则
	 * 当访问localhost:9527/guonei的时候，将会转发至https://news.baidu.com/guonei
	 *
	 * @param routeLocatorBuilder
	 * @return
	 */
	@Bean
	public RouteLocator customRouteLocator(RouteLocatorBuilder routeLocatorBuilder) {
		RouteLocatorBuilder.Builder routes = routeLocatorBuilder.routes();
		return routes.route("path_route_yzg", r -> r.path("/guonei").uri("https://news.baidu.com/guonei")).build();
	}

	@Bean
	public RouteLocator customRouteLocator2(RouteLocatorBuilder routeLocatorBuilder) {
		RouteLocatorBuilder.Builder routes = routeLocatorBuilder.routes();
		return routes.route("path_route_yzg", r -> r.path("/guoji").uri("https://news.baidu.com/guoji")).build();
	}
}

```

## 测试
运行cloud-eureka-server7001  
运行cloud-eureka-server7002  
运行cloud-gateway-gateway9527  
访问https://news.baidu.com/guonei
![](/img/springCloud/35.jpg)  
访问http://localhost:9527/guonei
![](/img/springCloud/36.jpg)  
点击国际  
![](/img/springCloud/37.jpg)   
点击军事
![](/img/springCloud/38.jpg)   
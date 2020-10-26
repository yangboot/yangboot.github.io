---
title:  服务注册consul
linktitle:  服务注册consul
toc: true
type: docs
date: "2020-06-28T00:00:00+01:00"
draft: false
menu:
  cloud-study:
    parent: consul
    weight: 2
---
## 提供端

新建项目`cloud-consul-provider-payment8006`

### POM

添加以下依赖

```xml
      <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-consul-discovery</artifactId>
        </dependency>
```

pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
   <parent>
		<artifactId>com-yzg-springcloud</artifactId>
		<groupId>com.yzg.springcloud</groupId>
		<version>1.0-SNAPSHOT</version>
	</parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-consul-provider-payment8006</artifactId>
    <description>Zookeeper服务提供者</description>

    
    <dependencies>
        <!--SpringCloud consul-server-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-consul-discovery</artifactId>
        </dependency>
          <dependency>
			<groupId>com.yzg.springcloud</groupId>
			<artifactId>cloud-api-common</artifactId>
			<version>${project.version}</version>
		</dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
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
    </dependencies>

</project>
```
### controller
```java
package com.cloud.provider.payment.contrller;

import java.util.UUID;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import lombok.extern.slf4j.Slf4j;


/**
 * @author yzg
 * @create 2020-07-27 12:00
 **/
@RestController
@Slf4j
public class PaymentController {
    @Value("${server.port}")
    private String serverPort;
    /**
     * http://localhost:8006/payment/consul
     *
     * @return
     */
    @RequestMapping(value = "payment/consul")
    public String paymentZk() {
        return "SpringCloud with consul:" + serverPort + "\t" + UUID.randomUUID().toString();
    }
}

```
### yml

```
server:
  # consul服务端口
  port: 8006
spring:
  application:
    name: cloud-provider-payment
  cloud:
    consul:
      # consul注册中心地址
      host: 203.176.95.155
      port: 8500
      discovery:
        hostname: 203.176.95.155
        service-name: ${spring.application.name}
```
### 启动类

```java
package com.cloud.provider.payment.contrller;

import java.util.UUID;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import lombok.extern.slf4j.Slf4j;


/**
 * @author yzg
 * @create 2020-07-27 12:00
 **/
@RestController
@Slf4j
public class PaymentController {
    @Value("${server.port}")
    private String serverPort;
    /**
     * http://localhost:8006/payment/consul
     *
     * @return
     */
    @RequestMapping(value = "payment/consul")
    public String paymentZk() {
        return "SpringCloud with consul:" + serverPort + "\t" + UUID.randomUUID().toString();
    }
}

```





## 消费端

新建项目`cloud-consul-consumer-order8012`

### POM
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
   <parent>
		<artifactId>com-yzg-springcloud</artifactId>
		<groupId>com.yzg.springcloud</groupId>
		<version>1.0-SNAPSHOT</version>
	</parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>cloud-consul-consumer-order8012</artifactId>
    <description>consul服务消费者</description>

    
    <dependencies>
        <!--SpringCloud consul-server-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-consul-discovery</artifactId>
        </dependency>
          <dependency>
			<groupId>com.yzg.springcloud</groupId>
			<artifactId>cloud-api-common</artifactId>
			<version>${project.version}</version>
		</dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
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
    </dependencies>

</project>
```

### controller

```java
package com.cloud.consumer.order.comtroller;

import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import javax.annotation.Resource;

/**
 *
 * @author yzg
 * @create 2020/07/26
 */
@RestController
@Slf4j
public class OrderConsulController {

    public static final String INVOKE_URL = "http://cloud-provider-payment";

    @Resource
    private RestTemplate restTemplate;
    /**
     * http://localhost:8012/consumer/payment/consul
     * @return
     */
    @GetMapping("/consumer/payment/consul")
    public String payment(){
        return restTemplate.getForObject(INVOKE_URL + "/payment/consul",String.class );
    }
}

```
### Config

```java
package com.cloud.consumer.order.config;

import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

/**
 * @author yzg
 * @create 2020/07/27
 */
@Configuration
public class ApplicationContextConfig {

    @Bean
    @LoadBalanced
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }
}

```



### yml

```
server:
  port: 8012
spring:
  application:
    name: cloud-consumer-order
  cloud:
    consul:
      host: 203.176.95.155
      port: 8500
      discovery:
        service-name: ${spring.application.name}

```
### 启动类

```java
package com.cloud.consumer.order;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@SpringBootApplication
@EnableDiscoveryClient
public class OrderConsulMain8012 {
    public static void main(String[] args) {
        SpringApplication.run(OrderConsulMain8012.class, args);
    }
}

```

## 测试

启动这两个项目

![](/img/springCloud/20.jpg)

访问：http://localhost:8012/consumer/payment/consul

![](/img/springCloud/21.jpg)

## 注意事项

服务必需是健康的才能访问得到

所以需要在pom添加健康监控的依赖

```xml
 <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
```

配置文件修改

```
server:
  port: 8006
spring:
  application:
    name: cloud-provider-pay
  cloud:
    consul:
      host: 203.176.95.155
      port: 8500
      discovery:
        service-name: ${spring.application.name}
        heartbeat:
          enabled: true
```


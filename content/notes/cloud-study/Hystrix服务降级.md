---
title:  Hystrix服务降级
linktitle:  Hystrix服务降级
toc: true
type: docs
date: "2020-07-28T00:00:00+01:00"
draft: false
menu:
  cloud-study:
    parent: Hystrix
    weight: 2
---
## 提供端降级

项目：cloud-hystrix-provider-payment8001

### pom

```xml
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
        </dependency>
```

###  service

编写f`allbackMethod`方法`payment_TimeOutHandler()`  
`@HystrixCommand`进行调用  
` @HystrixProperty`服务超时限制


```java
import com.netflix.hystrix.contrib.javanica.annotation.HystrixCommand;
import com.netflix.hystrix.contrib.javanica.annotation.HystrixProperty;  

    @HystrixCommand(fallbackMethod = "payment_TimeOutHandler", commandProperties = {
            @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds", value = "3000")
    })
    public String paymentInfo_TimeOut(Integer id) {
        int timeNumber = 5;
        try {
            // 暂停5秒钟
            TimeUnit.SECONDS.sleep(timeNumber);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return "线程池:" + Thread.currentThread().getName() + " paymentInfo_TimeOut,id:" + id + "\t" +
                "O(∩_∩)O哈哈~  耗时(秒)" + timeNumber;
    }

    public String payment_TimeOutHandler(Integer id) {
        return "线程池:" + Thread.currentThread().getName() + " 系统繁忙或运行错误,请稍后重试,id:" + id + "\t" + "o(╥﹏╥)o";
    }
```

###  启动类
添加`@EnableCircuitBreaker`注解

###  测试
运行cloud-eureka-server7001  
运行cloud-eureka-server7002  
运行cloud-hystrix-provider-payment8001  
访问http://localhost:8001/payment/hystrix/ok/1  （正常请求）  
![](/img/springCloud/29.jpg)  
访问http://localhost:8001/payment/hystrix/timeout/1   （超时请求）  
![](/img/springCloud/30.jpg) 

## 消费端降级

项目：cloud-openfeign-hystrix-comsumer-order8011

### pom

```xml
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
        </dependency>
```

### 配置全局服务降级

OrderController添加全局服务降级

```java
@DefaultProperties(defaultFallback = "payment_Global_FallbackMethod")

    public String payment_Global_FallbackMethod() {
        return "Global异常处理信息,请稍后重试.o(╥﹏╥)o";
    }
```

### 配置通配服务降级

添加`PaymentFallbackService`实现`PaymentFeignService`接口

```java
package com.cloud.comsumer.order.service.falkback;

import org.springframework.stereotype.Component;

import com.cloud.comsumer.order.service.PaymentFeignService;

/**
 * @author yzg
 * @create 2020/7/28 16:01
 **/
@Component
public class PaymentFallbackService implements PaymentFeignService {
    @Override
    public String paymentInfo_OK(Integer id) {
        return "----PaymentFallbackService fall back-paymentInfo_OK,o(╥﹏╥)o";
    }

    @Override
    public String paymentInfo_TimeOut(Integer id) {
        return "----PaymentFallbackService fall back-paymentInfo_TimeOut,o(╥﹏╥)o";
    }
}

```



### yml

```
feign:
  hystrix:
    # 在feign中开启Hystrix
    enabled: true
```



### 启动类

添加`@EnableHystrix`注解
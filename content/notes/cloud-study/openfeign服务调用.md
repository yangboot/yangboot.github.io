---
title:  openfeign服务调用
linktitle:  openfeign服务调用
toc: true
type: docs
date: "2020-06-28T00:00:00+01:00"
draft: false
menu:
  cloud-study:
    parent: openfeign
    weight: 1
---
## 新建项目

复制`cloud-comsumer-order8011`   
重命名为 `cloud-openfeign-comsumer-order8011`

## pom

```xml
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
```



## service

新建服务接口  
添加`@Component`注解  
添加`@FeignClient(value = "CLOUD-PAYMENT-SERVICE")`注解   
并且接口方法需要与提供方的controller方法保持一致

```java
package com.cloud.comsumer.order.service;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.stereotype.Component;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

import cn.hutool.json.JSONObject;
@Component
@FeignClient(value = "CLOUD-PAYMENT-SERVICE")
public interface PaymentFeignService {
	
	@GetMapping(value = "/payment/get/{id}")
	public JSONObject findById(@PathVariable("id")Long id);
}

```

## controller
```java
package com.cloud.comsumer.order.contrller;

import javax.annotation.Resource;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

import com.cloud.comsumer.order.service.PaymentFeignService;

import cn.hutool.json.JSONObject;
import lombok.extern.slf4j.Slf4j;

/**
 * @author yzg
 * @create 2020/7/28 11:24
 **/
@RestController
@Slf4j
public class OrderController {
    @Resource
    private PaymentFeignService paymentFeignService;
    /**
     * http://localhost:8011/consumer/payment/get/1
     * @param id
     * @return
     */
    @GetMapping(value = "/consumer/payment/get/{id}")
    public JSONObject getPaymentById(@PathVariable("id") Long id) {
    	return paymentFeignService.findById(id);
    }

}

```
## 启动类
给启动昂类添加`@EnableFeignClients`注解

## 测试

启动cloud-eureka-server7001  
启动cloud-eureka-server7002  
启动cloud.provider.payment8001  
启动cloud.provider.payment8002  
启动cloud-openfeign-comsumer-order8011  
访问http://localhost:8011/consumer/payment/get/1

![](/img/springCloud/22.jpg)
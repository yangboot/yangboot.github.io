---
title: SentinelResource注解代码解耦
linktitle: SentinelResource注解代码解耦
toc: true
type: docs
date: "2020-08-17T00:00:00+01:00"
draft: false
menu:
  cloud-albaba:
    parent: sentinel
    weight: 10

---

## 优化代码
blockHandlerClass  ：兜底类  
blockHandler：兜底方法  

```java
    @GetMapping("/rateLimit/customerBlockHandler")
    @SentinelResource(value = "customerBlockHandler",
                    blockHandlerClass = CustomerBlockHandler.class, blockHandler = "handlerException2")
    public CommomResult customerBlockHandler(){
        return new CommomResult(200, "客户自定义 限流测试OK", new Payment(2020L, "serial001"));
    }
```
CustomerBlockHandler.java
```java
package com.sentinel.myhandler;

import com.alibaba.csp.sentinel.slots.block.BlockException;

import cloud.api.common.entities.CommomResult;

/**
 *
 * @author yzg
 * @version 1.0
 * @create 2020/08/20
 */
public class CustomerBlockHandler {

    public static CommomResult handlerException(BlockException exception) {
        return new CommomResult(444, "客户自定义，global handlerException---1");
    }

    public static CommomResult handlerException2(BlockException exception) {
        return new CommomResult(444, "客户自定义，global handlerException---2");
    }
}

```
## 配置
![](/img/cloudAlibaba/28.jpg)  
## 测试
![](/img/cloudAlibaba/29.jpg)  

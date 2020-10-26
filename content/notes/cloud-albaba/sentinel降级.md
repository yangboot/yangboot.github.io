---
title: sentinel降级
linktitle: sentinel降级
toc: true
type: docs
date: "2020-08-17T00:00:00+01:00"
draft: false
menu:
  cloud-albaba:
    parent: sentinel
    weight: 6
---
官网 :[https://github.com/alibaba/Sentinel/wiki/%E7%86%94%E6%96%AD%E9%99%8D%E7%BA%A7](https://github.com/alibaba/Sentinel/wiki/%E7%86%94%E6%96%AD%E9%99%8D%E7%BA%A7)

## RT模式

### 配置

![](/img/cloudAlibaba/13.jpg)  

一秒钟10个进程（大于5个） 调用testB,我们希望200毫秒处理完本次任务，如果20毫秒没处理完，在未来的3秒时间窗口内，断路器打开  


让服务器响应1秒

```java
@GetMapping("/testB")
    public String testB(){
      try {
      TimeUnit.SECONDS.sleep(1);
  } catch (InterruptedException e) {
      e.printStackTrace();
  }
        return "testB   -----";
    }
```

![](/img/cloudAlibaba/14.jpg)  

### 测试
![](/img/cloudAlibaba/2.gif)  

## 异常比例
![](/img/cloudAlibaba/15.jpg)  



触发条件  
 - 每秒请求数大于5  
 - .异常比例大于0.2

## 异常数

![](/img/cloudAlibaba/16.jpg)
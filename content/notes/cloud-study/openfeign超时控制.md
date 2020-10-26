---
title:  openfeign超时控制
linktitle:  openfeign超时控制
toc: true
type: docs
date: "2020-07-28T00:00:00+01:00"
draft: false
menu:
  cloud-study:
    parent: openfeign
    weight: 2
---
## 提供端

故意在提供方`cloud.provider.payment8001`写个方法，相应时间为3秒

```java
    @GetMapping(value = "/payment/feign/timeout")
    public String paymentFeignTimeout() {
        try {
            // 暂停3秒钟
            TimeUnit.SECONDS.sleep(3);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return serverPort;
    }
}
```

## 消费端

在消费端内调用该方法  

PaymentFeignService.java

```java
@GetMapping(value = "/payment/feign/timeout")
public String paymentFeignTimeout();
```
OrderController.java
```java
 @GetMapping("/consumer/fegin/timeout")
    public String paymentTimeOut() {
        return paymentFeignService.paymentFeignTimeout();
    }
```
## 测试
运行cloud-eureka-server7001  
运行cloud-eureka-server7002  
运行cloud.provider.payment8001  
运行cloud-openfeign-comsumer-order8011  
访问提供端：http://localhost:8001/payment/feign/timeout
![](/img/springCloud/23.jpg)
访问消费端：http://localhost:8011/consumer/fegin/timeout  
![](/img/springCloud/24.jpg)  

## 修改配置
修改消费端的yml配置文件，添加以下配置  
5000代表5秒  
application.yml
```
ribbon:
  # 指的是建立连接所用的时间,适用于网络状态正常的情况下,两端连接所用的时间
  ReadTimeout: 5000
  # 指的是建立连接后从服务器读取到可用资源所用的时间
  ConnectTimeout: 5000
```
## 测试
重启cloud-openfeign-comsumer-order8011  
访问消费端：http://localhost:8011/consumer/fegin/timeout  
![](/img/springCloud/25.jpg)  

## 总结
1.openfeign客户端默认等待的时间是1秒  
2.可以通过修改application.yml配置文件来进行超时控制
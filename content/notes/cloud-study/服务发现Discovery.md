---

title:  服务发现Discovery
linktitle:  服务发现Discovery
toc: true
type: docs
date: "2020-06-28T00:00:00+01:00"
draft: false
menu:
  cloud-study:
    parent: eureka
    weight: 4
---
## 示例

```java
import org.springframework.cloud.client.discovery.DiscoveryClient;

@Resource
private DiscoveryClient discoveryClient;

@GetMapping(value = "/payment/discovery")
    public Object discovery(){
        List<String> services = discoveryClient.getServices();
        for (String element : services) {
            log.info("****element: " +element);
        }
        List<ServiceInstance> instances = discoveryClient.getInstances("CLOUD-PAYMENT-SERVICE");
        for (ServiceInstance instance:instances) {
            log.info(instance.getServiceId()+"\t"+instance.getHost()+"\t"+instance.getPort()+"\t"+instance.getUri());
        }
        return this.discoveryClient;
    }

```

启动类添加`@EnableDiscoveryClient`注解
运行`cloud-eureka-server7001`

运行`cloud-eureka-server7002`

运行`cloud.provider.payment8001`
控制台输出

```
2020-07-21 23:03:11.050  INFO 13360 --- [nio-8001-exec-1] c.c.p.p.contrller.PaymentController      : ****element: cloud-payment-service
2020-07-21 23:03:11.050  INFO 13360 --- [nio-8001-exec-1] c.c.p.p.contrller.PaymentController      : CLOUD-PAYMENT-SERVICE	PC-20190228DBGH	8001	http://PC-20190228DBGH:8001

```
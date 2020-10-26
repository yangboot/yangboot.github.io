---
title:  Ribbon负载规则
linktitle:  Ribbon负载规则
toc: true
type: docs
date: "2020-06-28T00:00:00+01:00"
draft: false
menu:
  cloud-study:
    parent: Ribbon
    weight: 1
---
### Ribbon负载均衡的八种算法，其中~~`ResponseTimeWeightedRule`~~已废除

| 规则名称                       | 特点                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| `AvailabilityFilteringRule`    | 过滤掉一直连接失败的被标记为circuit tripped（电路跳闸）的后端Service，并过滤掉那些高并发的后端Server或者使用一个AvailabilityPredicate来包含过滤Server的逻辑，其实就是检查status的记录的各个Server的运行状态 |
| `BestAvailableRule`            | 选择一个最小的并发请求的Server，逐个考察Server，如果Server被tripped了，则跳过 |
| `RandomRule`                   | 随机选择一个Server                                           |
| ~~`ResponseTimeWeightedRule`~~ | 已废弃，作用同WeightedResponseTimeRule                       |
| `RetryRule`                    | 对选定的负责均衡策略机上充值机制，在一个配置时间段内当选择Server不成功，则一直尝试使用subRule的方式选择一个可用的Server |
| `RoundRobinRule`               | 轮询选择，轮询index，选择index对应位置Server                 |
| `WeightedResponseTimeRule`     | 根据相应时间加权，相应时间越长，权重越小，被选中的可能性越低 |
| `ZoneAvoidanceRule`            | （默认是这个）负责判断Server所Zone的性能Server的可用性选择Server，在没有Zone的环境下，类似于轮询（`RoundRobinRule`） |


### Ribbon负载规则替换  


复制项目`cloud-comsumer-order8011`    


重命名为`cloud-ribbon-comsumer-order8011`   


新建`com.cloud.comsumer.myrule`包  

新建Myrule规则类

```java
package com.cloud.comsumer.myrule;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import com.netflix.loadbalancer.IRule;
import com.netflix.loadbalancer.RandomRule;

/**
 * 自定义负载均衡路由规则类
 *
 * @author yzg
 * @date 2020/7/27 15:15
 **/
@Configuration
public class CloudRole  {

    @Bean
    public IRule myRule() {
        // 定义为随机
        return new RandomRule();
    }
}


```

启动类添加注解

`@RibbonClient(name = "CLOUD-PAYMENT-SERVICE", configuration = Myrule.class)`  

测试 
运行cloud-eureka-server7001   
运行cloud-eureka-server7002  
运行cloud-provider-payment8001  
运行cloud-provider-payment8002  
运行cloud-ribbon-comsumer-order8011  
不断的访问http://localhost:8011/consumer/payment/get/1   

![](/img/springCloud/1.gif)

### 原理
**负载均衡算法**：第N次请求数    %   服务器集群总数量   =  实际调用服务器位置下标
---
title:  openfeign日志增强
linktitle:  openfeign日志增强
toc: true
type: docs
date: "2020-07-28T00:00:00+01:00"
draft: false
menu:
  cloud-study:
    parent: openfeign
    weight: 4
---

## config
```java
package com.cloud.comsumer.order.config;

import feign.Logger;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * OpenFeignClient配置
 *
 * @author yzg
 * @create 2020/7/28 11:02
 **/
@Configuration
public class FeignConfig {
    /**
     * feignClient配置日志级别
     *
     * @return
     */
    @Bean
    public Logger.Level feignLoggerLevel() {
        // 请求和响应的头信息,请求和响应的正文及元数据
        return Logger.Level.FULL;
    }
}

```
## yml
```
logging:
  level:
    com.cloud.comsumer.order.service.PaymentFeignService: debug
```

## test

启动cloud-eureka-server7001  
启动cloud-eureka-server7002  
启动cloud.provider.payment8001  
启动cloud-openfeign-comsumer-order8011  

访问http://localhost:8011/consumer/payment/get/1

日志输出

```
2020-07-28 11:07:26.273  INFO 14276 --- [nio-8011-exec-1] o.apache.tomcat.util.http.parser.Cookie  : A cookie header was received [1586915103; CNZZDATA1277660940=1307569842-1585038106-%7C1586919560; rememberMe=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] that contained an invalid cookie. That cookie will be ignored.
 Note: further occurrences of this error will be logged at DEBUG level.
2020-07-28 11:07:26.288  INFO 14276 --- [nio-8011-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring DispatcherServlet 'dispatcherServlet'
2020-07-28 11:07:26.288  INFO 14276 --- [nio-8011-exec-1] o.s.web.servlet.DispatcherServlet        : Initializing Servlet 'dispatcherServlet'
2020-07-28 11:07:26.299  INFO 14276 --- [nio-8011-exec-1] o.s.web.servlet.DispatcherServlet        : Completed initialization in 11 ms
2020-07-28 11:07:26.592  INFO 14276 --- [nio-8011-exec-1] c.netflix.config.ChainedDynamicProperty  : Flipping property: CLOUD-PAYMENT-SERVICE.ribbon.ActiveConnectionsLimit to use NEXT property: niws.loadbalancer.availabilityFilteringRule.activeConnectionsLimit = 2147483647
2020-07-28 11:07:26.635  INFO 14276 --- [nio-8011-exec-1] c.n.u.concurrent.ShutdownEnabledTimer    : Shutdown hook installed for: NFLoadBalancer-PingTimer-CLOUD-PAYMENT-SERVICE
2020-07-28 11:07:26.635  INFO 14276 --- [nio-8011-exec-1] c.netflix.loadbalancer.BaseLoadBalancer  : Client: CLOUD-PAYMENT-SERVICE instantiated a LoadBalancer: DynamicServerListLoadBalancer:{NFLoadBalancer:name=CLOUD-PAYMENT-SERVICE,current list of Servers=[],Load balancer stats=Zone stats: {},Server stats: []}ServerList:null
2020-07-28 11:07:26.650  INFO 14276 --- [nio-8011-exec-1] c.n.l.DynamicServerListLoadBalancer      : Using serverListUpdater PollingServerListUpdater
2020-07-28 11:07:26.674  INFO 14276 --- [nio-8011-exec-1] c.netflix.config.ChainedDynamicProperty  : Flipping property: CLOUD-PAYMENT-SERVICE.ribbon.ActiveConnectionsLimit to use NEXT property: niws.loadbalancer.availabilityFilteringRule.activeConnectionsLimit = 2147483647
2020-07-28 11:07:26.676  INFO 14276 --- [nio-8011-exec-1] c.n.l.DynamicServerListLoadBalancer      : DynamicServerListLoadBalancer for client CLOUD-PAYMENT-SERVICE initialized: DynamicServerListLoadBalancer:{NFLoadBalancer:name=CLOUD-PAYMENT-SERVICE,current list of Servers=[USER-20200308JZ:8002, USER-20200308JZ:8001],Load balancer stats=Zone stats: {defaultzone=[Zone:defaultzone;	Instance count:2;	Active connections count: 0;	Circuit breaker tripped count: 0;	Active connections per server: 0.0;]
},Server stats: [[Server:USER-20200308JZ:8002;	Zone:defaultZone;	Total Requests:0;	Successive connection failure:0;	Total blackout seconds:0;	Last connection made:Thu Jan 01 08:00:00 CST 1970;	First connection made: Thu Jan 01 08:00:00 CST 1970;	Active Connections:0;	total failure count in last (1000) msecs:0;	average resp time:0.0;	90 percentile resp time:0.0;	95 percentile resp time:0.0;	min resp time:0.0;	max resp time:0.0;	stddev resp time:0.0]
, [Server:USER-20200308JZ:8001;	Zone:defaultZone;	Total Requests:0;	Successive connection failure:0;	Total blackout seconds:0;	Last connection made:Thu Jan 01 08:00:00 CST 1970;	First connection made: Thu Jan 01 08:00:00 CST 1970;	Active Connections:0;	total failure count in last (1000) msecs:0;	average resp time:0.0;	90 percentile resp time:0.0;	95 percentile resp time:0.0;	min resp time:0.0;	max resp time:0.0;	stddev resp time:0.0]
]}ServerList:org.springframework.cloud.netflix.ribbon.eureka.DomainExtractingServerList@27f28632
2020-07-28 11:07:27.656  INFO 14276 --- [erListUpdater-0] c.netflix.config.ChainedDynamicProperty  : Flipping property: CLOUD-PAYMENT-SERVICE.ribbon.ActiveConnectionsLimit to use NEXT property: niws.loadbalancer.availabilityFilteringRule.activeConnectionsLimit = 2147483647
```




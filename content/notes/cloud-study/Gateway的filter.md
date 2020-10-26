---
title:   Gateway的filter
linktitle:  Gateway的filter
toc: true
type: docs
date: "2020-07-28T00:00:00+01:00"
draft: false
menu:
  cloud-study:
    parent: Gateway
    weight: 6
---
## 新建配置类
```java
package com.cloud.gateway.filter;

import lombok.extern.slf4j.Slf4j;
import org.springframework.cloud.gateway.filter.GatewayFilterChain;
import org.springframework.cloud.gateway.filter.GlobalFilter;
import org.springframework.core.Ordered;
import org.springframework.http.HttpStatus;
import org.springframework.http.server.reactive.ServerHttpRequest;
import org.springframework.stereotype.Component;
import org.springframework.web.server.ServerWebExchange;
import reactor.core.publisher.Mono;

import java.util.Date;

/**
 * 全局自定义过滤器
 *
 * @author yzg
 * @version 1.0
 * @create 2020/08/03
 */
@Component
@Slf4j
public class MyLogGatewayFilter implements GlobalFilter, Ordered {

    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        System.out.println("come in global filter: {}"+ new Date());

        ServerHttpRequest request = exchange.getRequest();
        String uname = request.getQueryParams().getFirst("uname");
        if (uname == null) {
        	System.out.println("用户名为null，非法用户");
            exchange.getResponse().setStatusCode(HttpStatus.NOT_ACCEPTABLE);
            return exchange.getResponse().setComplete();
        }
        // 放行
        return chain.filter(exchange);
    }

    /**
     * 过滤器加载的顺序 越小,优先级别越高
     *
     * @return
     */
    @Override
    public int getOrder() {
        return 0;
    }
}
```

## 测试

运行cloud-eureka-server7001  
运行cloud-eureka-server7002  
运行cloud.provider.payment8001  
运行cloud.provider.payment8002  
运行cloud-gateway-gateway9527  
访问http://localhost:9527/payment/get/1  
![](/img/springCloud/41.jpg)
访问http://localhost:9527/payment/get/1?uname=zs  
![](/img/springCloud/42.jpg)
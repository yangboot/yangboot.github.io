---
title: sentinel持久化配置
linktitle: sentinel持久化配置
toc: true
type: docs
date: "2020-08-17T00:00:00+01:00"
draft: false
menu:
  cloud-albaba:
    parent: sentinel
    weight: 12

---

官网教程地址：https://github.com/alibaba/spring-cloud-alibaba/wiki/Sentinel

![](/img/cloudAlibaba/30.jpg)

## 改造项目

`cloudalibaba-sentinel-service8401`

## pom

```xml
<dependency>
			<groupId>com.alibaba.csp</groupId>
			<artifactId>sentinel-datasource-nacos</artifactId>
		</dependency>
```

## yml

```
    sentinel:
      transport:
        # sentinel dashboard 地址
        dashboard: 192.168.126.128:8858
        # 默认为8719，如果被占用会自动+1，直到找到为止
        port: 8719
      # 流控规则持久化到nacos
      datasource:
        dsl:
          nacos:
            server-addr: 192.168.126.128:88488
            data-id: ${spring.application.name}
            group-id: DEFAULT_GROUP
            data-type: json
            rule-type: flow
```
## 配置
![](/img/cloudAlibaba/29.jpg)
```json
[
    {
        "resource": "/testA",
        "count": 1,
        "grade": 1,
        "limitApp": "default",
        "strategy": 0,
        "controlBehavior": 0
    }
]

```

resource : 资源名称  
limitApp：来源应用  
grade：阈值类型，0表示线程数，1表示QPS   
count :  单机阈值
strategy：流控模式，0直接，1关联，2链路
controlBehavior：流控效果，0快速失败，1warm up .2排队等待
clusterMode:是否集群
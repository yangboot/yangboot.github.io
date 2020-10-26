---
title:  bus动态定点刷新广播
linktitle:  bus动态定点刷新广播
toc: true
type: docs
date: "2020-07-28T00:00:00+01:00"
draft: false
menu:
  cloud-study:
    parent: bus
    weight: 4
---



## 公式

`curl   配置中心地址(3344)/actuator/bus-refresh/{destination}`

## 案例

修改gitee上的config-dev.yml文件，只让3355刷新，3366不刷新  



## 测试

运行cloud-eureka-server7001  
运行cloud-eureka-server7002  
运行cloud-config-center-3344  
运行cloud-config-client-3355   
运行cloud-config-client-3366   
修改gitee上的config-dev.yml文件

```
config: 
  info: "master Branches cloud2020_config/config-dev-yml v=4"
```
发送请求  curl -X POST "http://localhost:3344/actuator/bus-refresh/config-client:3355"  
![](/img/springCloud/55.jpg)   
测试结果：3355刷新，3366不刷新  


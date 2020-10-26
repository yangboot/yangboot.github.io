---
title:  sentinel安装
linktitle: sentinel安装
toc: true
type: docs
date: "2020-08-17T00:00:00+01:00"
draft: false
menu:
  cloud-albaba:
    parent: sentinel
    weight: 1
---
## 下载jar
https://github.com/alibaba/Sentinel/releases/tag/1.7.2  
  
    
      
## 运行
```
java -Dserver.port=:8858 -Dcsp.sentinel.dashboard.server=localhost:8080 -Dproject.name=sentinel-dashboard -jar sentinel-dashboard.jar
```
  
    
      
## 测试访问

直接访问 [http://127.0.0.1:8858](http://127.0.0.1:8858)   
使用账号：sentinel，密码：sentinel  登录  
![](/img/cloudAlibaba/5.jpg)





---
title: sentine热点参数限流
linktitle: sentine热点参数限流
toc: true
type: docs
date: "2020-08-17T00:00:00+01:00"
draft: false
menu:
  cloud-albaba:
    parent: sentinel
    weight: 7
---
官网 :[https://github.com/alibaba/Sentinel/wiki/%E7%83%AD%E7%82%B9%E5%8F%82%E6%95%B0%E9%99%90%E6%B5%81](https://github.com/alibaba/Sentinel/wiki/%E7%83%AD%E7%82%B9%E5%8F%82%E6%95%B0%E9%99%90%E6%B5%81)





## 配置

```java
//热点限流
    @GetMapping("/testHotKey")
    @SentinelResource(value = "testHotKey", blockHandler = "dealTestHotKey")
    public String testHotKey(@RequestParam(value = "p1", required = false) String p1,
                             @RequestParam(value = "p2", required = false) String p2){
        return "testHotKey -----";
    }

    public String dealTestHotKey(String p1, String p2, BlockException blockException){
        return "deal TestHotKey------QAQ-";
    }
```

![](/img/cloudAlibaba/17.jpg)  

## 测试

访问http://localhost:8401/testHotKey  
![](/img/cloudAlibaba/18.jpg)   
访问http://localhost:8401/testHotKey?p1=a  
![](/img/cloudAlibaba/19.jpg)   
访问http://localhost:8401/testHotKey?p2=a  
![](/img/cloudAlibaba/20.jpg)   
访问http://localhost:8401/testHotKey?p2=a&p1=a  
![](/img/cloudAlibaba/21.jpg)   

## 参数例外项
![](/img/cloudAlibaba/22.jpg)  
访问http://localhost:8401/testHotKey?p1=5
![](/img/cloudAlibaba/23.jpg)  
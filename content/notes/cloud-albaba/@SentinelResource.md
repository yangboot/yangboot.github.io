---
title: SentinelResource注解
linktitle: SentinelResource注解
toc: true
type: docs
date: "2020-08-17T00:00:00+01:00"
draft: false
menu:
  cloud-albaba:
    parent: sentinel
    weight: 9
---


## 按资源名称限流

```java
	@GetMapping("/byResource")
    @SentinelResource(value = "byResource", blockHandler = "handleException")
    public CommomResult byResource(){
        return new CommomResult(200, "按资源名称限流测试OK", new Payment(2020L, "serial001"));
    }

    public CommomResult handleException(BlockException blockException){
        return new CommomResult<>(444, blockException.getClass().getCanonicalName()+"\t服务不可用" );
    }
```



## 测试

 **配置1**

![](/img/cloudAlibaba/24.jpg)     

![](/img/cloudAlibaba/25.jpg)     

 **配置2**

![](/img/cloudAlibaba/26.jpg)     

![](/img/cloudAlibaba/27.jpg)  

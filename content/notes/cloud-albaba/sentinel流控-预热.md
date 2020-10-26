---
title: sentinel流控-预热
linktitle: sentinel流控-预热
toc: true
type: docs
date: "2020-08-17T00:00:00+01:00"
draft: false
menu:
  cloud-albaba:
    parent: sentinel
    weight: 5
---
官网 [https://github.com/alibaba/Sentinel/wiki/%E9%99%90%E6%B5%81---%E5%86%B7%E5%90%AF%E5%8A%A8](https://github.com/alibaba/Sentinel/wiki/%E9%99%90%E6%B5%81---%E5%86%B7%E5%90%AF%E5%8A%A8
## 预热
预热（RuleConstant.CONTROL_BEHAVIOR_WARM_UP）
如果系统的使用率一段时间以来一直很低，但是突然有大量请求进入，则系统可能无法立即处理  所  有这些请求。但是，如果我们稳步增加传入的请求并允许系统预热，则它最终可能能够处理所  有请求  

**默认 `coldFactor` 为 3，即请求 QPS 从 `threshold / 3` 开始，经预热时长逐渐升至设定的 QPS 阈值**

## 配置

![](/img/cloudAlibaba/12.jpg)  

## 测试

初始化阈值为`10 / 3 = 3`  , QPS 阈值 进过`5``秒后逐渐升至10

![](/img/cloudAlibaba/1.gif)  
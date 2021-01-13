---
title: 【工作日志_02】SpringBoot+Thymeleaf外部部署静态文件
categories: 工作日志
tags:
  - SpringBoot
  - Thymeleaf
excerpt: SpringBoot+Thymeleaf外部调用静态文件
date: 2020-01-12 20:33:36
img:  /images/workLog/workLog.jpg
---

## 需求
- 项目用Java  -jar部署
- 静态文件在外部调用，并与Java程序分离
- 再不重启jar的情况下修改静态文件实时更新

## 实操
### 静态文件
将静态文件抽取出来，放在与jar包相同的目录下。
### 修改yml配置文件



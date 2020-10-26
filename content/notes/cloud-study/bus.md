---
title: bus
date: "2020-08-04T00:00:00Z"
lastmod: "2020-08-04T00:00:00Z"
draft: false  # Is this a draft? true/false
toc: true  # Show table of contents? true/false
type: docs  # Do not modify.

# Add menu entry to sidebar.
# - name: Declare this menu item as a parent with ID `name`.
# - weight: Position of link in menu.
menu:
 cloud-study:
    name: bus
    weight: 15
---

### 什么是Bus 消息总线

Spring Cloud Bus 将分布式的节点用轻量的消息代理连接起来。它可以用于**广播配置文件的更改**或者服务之间的通讯，也可以用于监控。

Spring cloud bus被国内很多都翻译为**消息总线**。可以将它理解为管理和传播所有分布式项目中的消息既可，**其实本质是利用了MQ的广播机制在分布式的系统中传播消息，目前常用的有Kafka和RabbitMQ**

一张图来描述bus在配置中心使用的机制
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200208164624333.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MDk5MTQwOA==,size_16,color_FFFFFF,t_70)
这时Spring Cloud Bus做配置更新步骤如下:

1. 提交代码触发post请求给bus/refresh
2. server端接收到请求并发送给Spring Cloud Bus
3. Spring Cloud bus接到消息并通知给其它客户端
4. 其它客户端接收到通知，请求Server端获取最新配置
5. 全部客户端均获取到最新的配置
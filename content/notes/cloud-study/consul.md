---
title: consul
date: "2020-07-27T00:00:00Z"
lastmod: "2020-07-20T00:00:00Z"
draft: false  # Is this a draft? true/false
toc: true  # Show table of contents? true/false
type: docs  # Do not modify.

# Add menu entry to sidebar.
# - name: Declare this menu item as a parent with ID `name`.
# - weight: Position of link in menu.
menu:
 cloud-study:
    name: consul
    weight: 4
---

官网：https://www.consul.io/
中文文档：https://www.springcloud.cc/spring-cloud-consul.html
## 什么是consul

Consul是一个基于CP的轻量级分布式高可用的系统，提供服务发现、健康检查、K-V存储、多数据中心等功能，不需要再依赖其他组件(Zk、Eureka、Etcd等)。

服务发现：Consul可以提供一个服务，比如api或者MySQL之类的，其他客户端可以使用Consul发现一个指定的服务提供者，并通过DNS和HTTP应用程序可以很容易的找到所依赖的服务。
健康检查：Consul客户端提供相应的健康检查接口，Consul服务端通过调用健康检查接口检测客户端是否正常
K-V存储：客户端可以使用Consul层级的Key/Value存储，比如动态配置,功能标记,协调,领袖选举等等
多数据中心：Consul支持开箱即用的多数据中心

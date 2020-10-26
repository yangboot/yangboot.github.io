---
title:   内置Predicates断言
linktitle:  内置Predicates断言
toc: true
type: docs
date: "2020-07-28T00:00:00+01:00"
draft: false
menu:
  cloud-study:
    parent: Gateway
    weight: 3
---
###  内置Predicates断言

原文地址：https://blog.csdn.net/weixin_43839681/article/details/106160813

#### 1 After 路由断言

`After` 路由断言只有一个ZonedDateTime生成的datetime参数，只有在这个事件之后的请求才能匹配上。

> Example 1. application.yml

```java
spring:
  cloud:
    gateway:
      routes:
      - id: after_route
        uri: https://example.org
        predicates:
        - After=2017-01-20T17:42:47.789-07:00[America/Denver]
12345678
```

https://example.org这个地址在美国Denver时间2017-01-20 17:42：00之后才能

```java
// 生成时间的代码
// demo 2020-05-06T19:16:43.338+08:00[Asia/Shanghai]
System.out.println(ZonedDateTime.now().toString());
123
```

#### 2 Before 路由断言

`Before` 同样是一个时间断言，只不过是在配置的时间之前有效，过时之后失效。

> Example 2. application.yml

```java
spring:
  cloud:
    gateway:
      routes:
      - id: before_route
        uri: https://example.org
        predicates:
        - Before=2017-01-20T17:42:47.789-07:00[America/Denver]
12345678
```

上面的链接地址在America/Denver时间2017-01-20 17:42::00之前有效

#### 3  Between  路由断言

Between   路由断言，时间类断言，在声明的时间内有效。并且第二个时间必须大于第一个时间。

> Example 3. application.yml

```java
spring:
  cloud:
    gateway:
      routes:
      - id: between_route
        uri: https://example.org
        predicates:
        - Between=2017-01-20T17:42:47.789-07:00[America/Denver], 2017-01-21T17:42:47.789-07:00[America/Denver]
12345678
```

上面的链接在America/Denver时间2017-01 20日17时42-21日17时47分之间有效。

#### 4 Cookie 路由断言

`Cookie` 断言有两个参数，一个cookie 名称和一个java 正则表达式，这个断言匹配给定的cookie 名和值正则匹配的请求。

> Example 4. application.yml

```java
spring:
  cloud:
    gateway:
      routes:
      - id: cookie_route
        uri: https://example.org
        predicates:
        - Cookie=chocolate, ch.p
12345678
```

上面的匹配那些请求中包含参数名为`chocolate`并且值匹配`ch.p`的请求。

#### 5 Header  路由断言

`Header`断言有两个参数，一个参数名，一个正则。只有当有这个参数并且值匹配正则的时候才能执行下去。

> Example 5. application.yml

```java
spring:
  cloud:
    gateway:
      routes:
      - id: header_route
        uri: https://example.org
        predicates:
        - Header=X-Request-Id, \d+
12345678
```

这个路由规则匹配`Header`中包含`X-Request-Id`并且值为纯数字的请求。

#### 6 Host 路由断言

Host 路由断言接受一个正则域名列表，正则是用英文句号分割的`Ant-Style`(ant 风格？)正则表达式。

> Example 6. application.yml

```java
spring:
  cloud:
    gateway:
      routes:
      - id: host_route
        uri: https://example.org
        predicates:
        - Host=**.somehost.org,**.anotherhost.org
12345678
```

这个路由匹配`somehost.org` `anotherhost.org`这两个域名下的所有请求。

#### 7 Method 路由断言

`Method`路由断言匹配一个或多个`Http`请求方式（`GET` `POST` `PUT` `DELETE` `HEAD`）.

> Example 7. application.yml

```java
spring:
  cloud:
    gateway:
      routes:
      - id: method_route
        uri: https://example.org
        predicates:
        - Method=GET,POST
12345678
```

这个路由匹配所有的`GET` `POST`请求。

#### 8 Path 路径路由断言

`Path`路由断言接受两个参数：Spring `PathMatcher` 正则列表和一个名为 `matchOptionalTrailingSeparator` 的可选标志。

> Example 8. application.yml

```java
spring:
  cloud:
    gateway:
      routes:
      - id: path_route
        uri: https://example.org
        predicates:
        - Path=/red/{segment},/blue/{segment}
12345678
```

如果请求路径为(例如: / red / 1或 / red / blue 或 / blue / green) ，则此路由匹配。
 这个断言提取了URL中模板变量（像前面例子中声明的`segment`）组为key/value键值对放置在`ServerWebExchange.getAttributes()`中，并用已经声明的`ServerWebExchangeUtils.URI_TEMPLATE_VARIABLES_ATTRIBUTE`来声明。这些内容可被`GatewayFilter` factories的一个名为`get`的方法很简单的去访问。

```java
Map<String, String> uriVariables = ServerWebExchangeUtils.getPathPredicateVariables(exchange);

String segment = uriVariables.get("segment");
123
```

#### 9 Query 查询路由断言

`Query`路由断言有两个参数: 一个必传参数 and 和一个可选的正则表达式。

> Example 9. application.yml

```java
spring:
  cloud:
    gateway:
      routes:
      - id: query_route
        uri: https://example.org
        predicates:
        - Query=green
12345678
```

如果一个请求中包含green的参数，则匹配成功。

> application.yml

```java
spring:
  cloud:
    gateway:
      routes:
      - id: query_route
        uri: https://example.org
        predicates:
        - Query=red, gree.
12345678
```

如果一个请求中包含参数red并且值匹配·gree.·这个正则，那么路由匹配。比如：green和greet。

#### 10 RemoteAddr 远程地址路由断言

`RemoteAddr`路由断言接受一个来源地址的list，由IPv4或者IPv6组成，比如：`192.168.0.1/16`（其中`192.168.0.1`是IP地址，`16`是子网掩码）

> Example 10. application.yml

```java
spring:
  cloud:
    gateway:
      routes:
      - id: remoteaddr_route
        uri: https://example.org
        predicates:
        - RemoteAddr=192.168.1.1/24
12345678
```

如果请求的远程地址是192.168.1.10，则此路由匹配。

#### 11. Weight 权重路由断言

权重路由断言接受两个参数：`group`分组和`Weight`权重。权重是按组计算的。

> Example 11. application.yml

```java
spring:
  cloud:
    gateway:
      routes:
      - id: weight_high
        uri: https://weighthigh.org
        predicates:
        - Weight=group1, 8
      - id: weight_low
        uri: https://weightlow.org
        predicates:
        - Weight=group1, 2
123456789101112
```

这条路线将80% 的流量转发到 weighthigh. org，20% 的流量转发到 weighlow. org。




---
title: idea热部署
linktitle: idea热部署
toc: true
type: docs
date: "2020-06-28T00:00:00+01:00"
draft: false
menu:
 cloud-study:
    parent: SpringCloud微服务学习
    weight: 3
---

## 给工程添加devtools

```xml
  <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
```

## 给父类工程添加插件

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <fork>true</fork>
                <addResources>true</addResources>
            </configuration>
        </plugin>
    </plugins>
</build>
```

## 开启自动编译

![](/img/springCloud/11.jpg)



 ## 更新配置

输入快捷键`ctrl+shift+Alt+/`

![](/img/springCloud/12.jpg)

![](/img/springCloud/13.jpg)

## 重启idea


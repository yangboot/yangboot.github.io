---
title:  sonarQube安装
linktitle:  sonarQube安装
toc: true
type: docs
date: "2020-06-28T00:00:00+01:00"
draft: false
menu:
 jenkins-study:
    parent: sonarQube代码审查
    weight: 1
---

## 前提条件
安装MYSQL数据库
新建sonnar数据库

## 下载安装

下载链接： https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-6.7.7.zip

解压sonnar并配置权限

```
yum install unzip

unzip sonarqube-6.7.7.zip

mv /root/sonarqube-6.7.7./* /opt/sonar 

useradd  sonar 

chown -R sonar. /opt/sonar
```

## 配置

```shell
vim /opt/sonar/conf/sonar.properties
```
```
# 配置数据库
sonar.jdbc.url=jdbc:mysql://localhost:3306/sonar?useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true&useConfigs=maxPerformance
sonar.jdbc.username=root
sonar.jdbc.password=root
sonar.sorceEncoding=UTF-8
sonar.login=admin
sonar.password=admin
# 配置端口
sonar.web.port=9000
```



## 启动

不能用root账号启动

```shell
su sonar /opt/sonar/bin/linux-x86-64/sonar.sh start
```

查看日志

```shell
tail -f /opt/sonar/logs/sonar.log
```

![](/img/sonar/2.jpg)

![](/img/sonar/1.jpg)





## 安装中文插件

[下载地址](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2FSonarQubeCommunity%2Fsonar-l10n-zh%2Freleases%2Ftag%2Fsonar-l10n-zh-plugin-1.16)

下载好的jar包放到sonar安装目录下`extensions/plugins`重启即可
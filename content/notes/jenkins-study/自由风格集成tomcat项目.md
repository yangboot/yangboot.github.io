---
title:  自由风格集成tomcat项目
linktitle:  自由风格集成tomcat项目
toc: true
type: docs
date: "2020-06-28T00:00:00+01:00"
draft: false
menu:
 jenkins-study:
    parent: jenkins持续集成
    weight: 4
---
## 配置tomcat远程访问权限

修改tomcat安装目录下的tomcat-users.xml

```shell
vim /opt/apache-tomcat-8.5.57/conf/tomcat-users.xml
```

```
  <role rolename="admin-gui"/>
  <role rolename="admin-script"/>
  <role rolename="manager-gui"/>
  <role rolename="manager-script"/>
  <role rolename="manager-jmx"/>
  <role rolename="manager-status"/>
  <user username="admin" password="admin" roles="manager-gui,manager-script,manager-jmx,manager-status,admin-script,admin-gui"/>
</tomcat-users>

```

重启
```
./shutdown.sh
```

```
./startup.sh
```



修改`webapps/manager/META-INF/context.xml`配置文件

```shell
vim /opt/apache-tomcat-8.5.57/webapps/manager/META-INF/context.xml
```

给`value`标签加上注释

```
<!-- <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />-->
```

点击 manager webapp
![](/img/jenkins/10.jpg)



## 安装Deploy to container插件

## 创建自由风格项目
配置Git地址
构建----->>> 执行shell ----->>> 输入`mvn clean package`
![](/img/jenkins/11.jpg)
构建后操作----->>>  Deploy war to container
![](/img/jenkins/12.jpg)
立即构建

![](/img/jenkins/13.jpg)
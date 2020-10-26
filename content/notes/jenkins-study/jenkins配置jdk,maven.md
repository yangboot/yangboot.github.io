

---
title:  jenkins配置jdk,maven
linktitle: jenkins配置jdk,maven
toc: true
type: docs
date: "2020-07-07T00:00:00+01:00"
draft: false
menu:
 jenkins-study:
    parent: jenkins持续集成
    weight: 3
---


### 安装git
```shell
yum install git -y
```

### 安装maven

下载maven https://maven.apache.org/download.cgi
将tar包移动到服务器
解压
```shell
tar -xzf apache-maven-3.6.3-bin.tar.gz
```
配置环境变量
```
vim /etc/profile
```
追加以下内容
```
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk
export MAVEN_HOME=/opt/maven
export PATH=${JAVA_HOME}/bin:${MAVEN_HOME}/bin
```

重载环境变量

```shell
source /etc/profile
```

测试

```
mvn -v
```

出现以下内容安装成功

```
Apache Maven 3.6.3 (cecedd343002696d0abb50b32b541b8a6ba2883f)
Maven home: /opt/maven
Java version: 1.8.0_252, vendor: Oracle Corporation, runtime: /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.252.b09-2.el7_8.x86_64/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "3.10.0-327.el7.x86_64", arch: "amd64", family: "unix"
```

### 配置settings.xml

```
mkdir /root/repo
```


```shell
vim /opt/maven/conf/settings.xml
```
配置本地仓库
```
 <localRepository>/root/repo</localRepository>
```



配置阿里云镜像

```
      <mirror>
         <id>nexus-aliyun</id>
         <mirrorOf>*</mirrorOf>
         <name>Nexus aliyun</name>
         <url>http://maven.aliyun.com/nexus/content/groups/public</url>
      </mirror> 
```





### jenkins配置jdk和maven

![](/img/jenkins/8.jpg)

系统设置-->>系统配置-->>全局属性环境变量

分别配置`JAVA_HOME`，`M2_HOME`，`PATH+EXTRA`


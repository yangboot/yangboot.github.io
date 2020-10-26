---
title:  jenkins集成sonarQube
linktitle:  jenkins集成sonarQube
toc: true
type: docs
date: "2020-06-28T00:00:00+01:00"
draft: false
menu:
 jenkins-study:
    parent: sonarQube代码审查
    weight: 2
---


## 安装插件
安装`SonarQube Scanner`插件


## 添加凭证
![](/img/sonar/3.jpg)

## 安装SonarQube Scanner
![](/img/sonar/5.jpg)
## 配置SonarQube servers

系统配置-->>系统配置-->>SonarQube servers
![](/img/sonar/4.jpg)

## 自由风格项目配置
![](/img/sonar/6.jpg)

![](/img/sonar/7.jpg)

## 流水线项目配置
在项目根目录下创建sonar-project.properties文件
sonar-project.properties
```shell
sonar.projectKey=com-yzg-jfinaloa-pipeline
sonar.projectName=com-yzg-jfinaloa-pipeline
sonar.projectVersion=1.0  
sonar.sources=src/main/java/com/pointlion/
sonar.exclusions=**/test/**,**/target/**
sonar.java.sources=1.8
sonar.java.target=1.8
sonar.sourceEncoding=UTF-8
```
jenkinsfile加入以下配置

```shell
stage('code checking') {
            steps {
              script {
                 //引入sonarqubeScanner工具
                 scannerHome= tool 'SonarQube'
                 }
              
              withSonarQubeEnv('SonarQube') {
                 //引入SonarQube的服务器环境
                sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
```

![](/img/sonar/8.jpg)
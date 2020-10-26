---
title:  Pipeline风格集成项目
linktitle:  Pipeline风格集成项目
toc: true
type: docs
date: "2020-07-08T00:00:00+01:00"
draft: false
menu:
 jenkins-study:
    parent: jenkins持续集成
    weight: 5
---
## 1.配置pipeline插件

![](/img/jenkins/19.jpg)

## 2.编写**Jenkinsfile**

在项目的根目录下编写Jenkinsfile

```shell
pipeline {
    agent any

    stages {
        stage('pull code') {
            steps {
                 checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '95f5cb07-e0fd-4186-9504-4d7e1d211d3b', url: 'https://gitee.com/MrPen/jfinaloa.git']]])
            }
        }
        stage('package war') {
            steps {
                  sh "mvn clean package"
            }
        }
         stage('publish') {
            steps {
                 deploy adapters: [tomcat8(credentialsId: 'afc2e433-a99c-4070-b9d9-7125f4139bec', path: '', url: 'http://203.176.95.155:8081/')], contextPath: null, war: 'target/*.war'
            }
        }
    }
}

```

## 3.新建流水线项目

![](/img/jenkins/17.jpg)

![](/img/jenkins/18.jpg)



## 4.立即构建

![](/img/jenkins/13.jpg)




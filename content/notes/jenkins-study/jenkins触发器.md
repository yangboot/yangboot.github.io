---
title:  jenkins集成邮件
linktitle:  jenkins集成邮件
toc: true
type: docs
date: "2020-07-09T00:00:00+01:00"
draft: false
menu:
 jenkins-study:
    parent: jenkins持续集成
    weight: 7
---
## 触发远程构建

![](/img/jenkins/14.jpg)

输入JENKINS_URL`/view/all/job/cn-yzg-jfinaloa/build?token=`TOKEN_NAME

` 或者 /buildWithParameters?token=`TOKEN_NAME

![](/img/jenkins/15.jpg)

## 其他工程构建后触发 

当frist-pro构建时触发

![](/img/jenkins/16.jpg)

## 定时触发

```
每30分钟构建一次
H/30* * * *

每2小时构建
H H/2* * *

每天8、12、22 一天构建3次
0.8，12，22* * *

每天中午12点定时构建一次
H 12* * *

每天下午18点定时构建一次
H 18* * *

在每小时的前半小时内的10分钟
H(0-29)/10* * * *

每两小时一次，每个工作日上午9点到下午5点
H H(9-16)/2* *1-5
```



## 轮询 SCM触发（不建议）

代码仓库发生变更时触发定时构建




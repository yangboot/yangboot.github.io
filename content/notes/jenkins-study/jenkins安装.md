---
title:  jenkins安装
linktitle:  jenkins安装
toc: true
type: docs
date: "2020-06-28T00:00:00+01:00"
draft: false
menu:
 jenkins-study:
    parent: jenkins持续集成
    weight: 1
---

# jenkins安装

`jenkins`  

官方网址：https://pkg.jenkins.io/redhat-stable/

上官网下载rpm包

###  安装
```bash
rpm -ivh jenkins-2.235.1-1.1.noarch.rpm
```

### 修改配置

```shell
vim /etc/sysconfig/jenkins
```

修改端口号为8888

JENKINS_PORT="8888"

修改用户名"root"

JENKINS_USER="root"

### 启动jenkins
```shell
systemctl start jenkins
```
开放8888端口
```shell
firewall-cmd --zone=public --add-port=8888/tcp --permanent
```
```shell
firewall-cmd --reload 
```
输入http://你的主机IP:8888/访问

![](/img/jenkins/2.jpg)

查看密码

```shell
cat /var/lib/jenkins/secrets/initialAdminPassword-
```

跳过插件安装

![](/img/jenkins/3.jpg)

![](/img/jenkins/4.jpg)



### 使用清华源加速插件安装

修改 Update Site

Manage Jenkins  ->>Manager plugin->>Advanced

![](/img/jenkins/6.jpg)

修改为

```shell
http://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json
```

替换default.json的下载路径

```shell
 cd /var/lib/jenkins/updates
```

```
sed -i 's/http:\/\/updates.jenkins-ci.org\/download/https:\/\/mirrors.tuna.tsinghua.edu.cn\/jenkins/g' default.json && sed -i 's/http:\/\/www.google.com/https:\/\/www.baidu.com/g' default.json
```

浏览器输入 http://你的主机名:8888/restart  重启

## docker

```
docker run -u root --rm  -d  -p 8888:8080  -p 50000:50000 -v jenkins-data:/var/jenkins_home    jenkins/jenkins

```



## 卸载

```
find / -iname jenkins | xargs -n 1000 rm -rf
```


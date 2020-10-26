---
title:  consul安装
linktitle:  consul安装
toc: true
type: docs
date: "2020-06-28T00:00:00+01:00"
draft: false
menu:
  cloud-study:
    parent: consul
    weight: 1
---
## 下载
下载地址
https://releases.hashicorp.com/consul/1.8.0/consul_1.8.0_linux_amd64.zip    
## 安装
```shell
unzip consul_1.8.0_linux_amd64.zip
```

将文件迁移到/usr/bin/

```shell
mv consul /usr/bin
```

## 运行

```shell
consul agent -dev -client=0.0.0.0 -ui&
```

开发8500端口

```shell
firewall-cmd --zone=public --add-port=8500/tcp --permanent
```

```shell
firewall-cmd --reload 
```





![](/img/springCloud/19.jpg)


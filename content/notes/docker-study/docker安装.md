---
title: docker安装
linktitle: docker安装
toc: true
type: docs
date: "2020-06-16T00:00:00+01:00"
draft: false
menu:
  docker-study:
    parent: docker
    weight: 5

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 2
---



### centos 6.10   安装 docker



官方文档 ：https://docs.docker.com/engine/install/centos/



查看centos系统版本,6和7有不同的安装步骤

```shell
cat /etc/redhat-release
```



### 卸载旧版本
较旧的Docker版本称为docker或docker-engine。如果已安装这些程序，请卸载它们以及相关的依赖项。

```shell
    yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine

```
### 安装
网上试试很多方法都失败了，下面这个亲测可用

```shell
 wget https://get.docker.com/rpm/1.7.1/centos-6/RPMS/x86_64/docker-engine-1.7.1-1.el6.x86_64.rpm 
```
```shell
sudo rpm -ivh docker-engine-1.7.1-1.el6.x86_64.rpm 
```
```shell
### 安装配置cgroup
```
```shell
yum install -y libcgroup
```
```shell
sudo vim /etc/fstab
```

 增加一行
```shell
none      /sys/fs/cgroup          cgroup  defaults        0  0
```
### 重启
```shell
reboot
```
### 启动服务
```shell
service docker start
```

### 测试
```shell
 docker -v
```

![](/img/linux/17.jpg)


###  CentOS Linux release 7.2.1511   安装 docker
/docker要求内核版本必须高于3.10，因此先查看自己的内核版本
查看内核


```shell
uname -r
```

```shell
yum -y install docker-io
```
```shell
yum list installed |grep docker
```
启动docker服务
```shell
systemctl start docker.service 
```
```shell
systemctl status docker.service 
```

### 开机自启动：
检查服务是否开机启动
```shell
systemctl is-enabled docker.service 
```
将服务配置成开机启动
```shell
systemctl enable docker.service 
```
启动服务
```shell
systemctl start docker.service 
```
 

systemctl 相关其他命令：
禁止开机启动
```shell
systemctl disable docker.service 
```
 停止
```shell
systemctl stop docker.service
```
重启
```shell
systemctl restart docker.service 重启
```
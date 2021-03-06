---
title: 【docker笔记_01】docker安装
categories: docker笔记
tags:
  - docker
  - centos7
excerpt: centos7安装docker
date: 2020-11-03 20:33:36
img: '/images/docker/docker.png'
---
## docker安装
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
### 查看内核版本

```shell
uname -a
```
### yum包更新到最新

```shell
yum update
```

### 安装需要的软件驱动

```shell
yum install -y yum-utils device-mapper-persistent-data lvm2
```

### 设置docker阿里云下载源

```shell
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

删除  docker-ce.repo文件
```shell
/etc/yum.repos.d/docker-ce.repo
```
### 查看所有仓库中所有docker版本

```shell
 yum list docker-ce --showduplicates | sort -r
```

### 安装

```shell
yum install docker-ce-<VERSION_STRING>
```





### 启动docker服务

```shell
systemctl start docker
```
### 加入开机启动
```shell
 systemctl enable docker
```


### 禁止开机启动

```shell
systemctl disable docker.service 
```
### 停止

```shell
systemctl stop docker.service
```
### 重启

```shell
systemctl restart docker.service 重启
```
## 配置阿里镜像加速
```shell
vim /etc/docker/daemon.json
```
```json
{
  "registry-mirrors": ["https://mu2ralus.mirror.aliyuncs.com"]
}
```
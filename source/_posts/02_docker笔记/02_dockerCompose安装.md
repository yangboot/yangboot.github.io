---
title: 【docker学习笔记_02】dockerCompose 安装
categories: docker学习笔记
tags:
  - dockerCompose 
  - centos7
excerpt: centos7安装dockerCompose 
date: 2020-12-17 10:33:36
img: '/images/docker/docker.png'
---
# Compose 安装

在 Linux 上的也安装十分简单，从 [官方 GitHub Release](https://github.com/docker/compose/releases) 处直接下载编译好的二进制文件即可。

运行以下命令以下载 Docker Compose 的当前稳定版本：

```shell
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

要安装其他版本的 Compose，请替换 1.24.1。

## 高速安装

Docker Compose 存放在Git Hub，不太稳定。
你可以也通过执行下面的命令，高速安装Docker Compose。

```shell
curl -L https://get.daocloud.io/docker/compose/releases/download/1.26.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
```

## 授权

```shell
chmod +x /usr/local/bin/docker-compose
```

## 验证

```shell
docker-compose --version
```

![](/img/docker/43.jpg)
安装成功！！！

## 卸载

```shell
sudo rm /usr/local/bin/docker-compose
```
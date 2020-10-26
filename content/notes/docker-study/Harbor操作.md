---
title: Harbor（上传、搜索、下载镜像）
linktitle:  Harbor（上传、搜索、下载镜像）
toc: true
type: docs
date: "2020-06-28T00:00:00+01:00"
draft: false
menu:
 docker-study:
    parent: docker 私有库
    weight: 3
---
## 前提条件
开启两台机器
机器A: (docker私有库)
机器B: (外部访问)





## 注意事项

如果你不想使用 `127.0.0.1:5000` 作为仓库地址，比如想让本网段的其他主机也能把镜像推送到私有仓库。你就得把例如 `192.168.199.100:5000` 这样的内网地址作为私有仓库地址，这时你会发现无法成功推送镜像。

这是因为 Docker 默认不允许非 `HTTPS` 方式推送镜像。我们可以通过 Docker 的配置选项来取消这个限制，或者查看下一节配置能够通过 `HTTPS` 访问的私有仓库。

### 机器B
添加http信任

编辑 `/etc/docker/daemon.json` 文件

```
vim /etc/docker/daemon.json
```
内容如下
```json
{
  "registry-mirrors": [
    "https://registry.docker-cn.com"
  ],
   "insecure-registries": [
    "机器A的IP:18080"
  ]
}
```

重启docker

```bash
systemctl daemon-reload
```

```bash
systemctl restart docker.service
```
## 登录
```
docker login [机器A的ip]:[docker私有库端口号]
```

![](/img/docker/54.jpg)

## 镜像推送


### 机器A
新建一个私有项目
![](/img/docker/51.jpg)
![](/img/docker/52.jpg)


### 机器B
在项目中标记镜像
```bash
 docker tag tomcat:latest [机器A的ip]:18080/repo-test/mytomcat
```
推送镜像到当前项目
```bash
docker push [机器A的ip]:18080/repo-test/mytomcat
```
![](/img/docker/57.jpg)
![](/img/docker/58.jpg)

## 镜像拉取

为了避免混淆先把本地的镜像全部删了
```bash
docker rmi $(docker images -a)
```

先确认本地没有镜像

![](/img/docker/59.jpg)


```bash
docker pull [机器A的ip]:18080/repo-test/mytomcat
```
![](/img/docker/60.jpg)
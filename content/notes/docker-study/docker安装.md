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
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repoyum-config-manager --add-repo
```

### 查看所有仓库中所有docker版本

```shell
docker-ce --showduplicates | sort -rshe
```

### 安装

```shell
yum install docker-ce-<VERSION_STRING>
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
---
title:  安装docker-registry
linktitle: 安装docker-registry
toc: true
type: docs
date: "2020-06-28T00:00:00+01:00"
draft: false
menu:
 docker-study:
    parent: docker 私有库
    weight: 1
---

## 安装运行 docker-registry

参考：https://www.funtl.com/zh/docs-docker/Docker-%E7%A7%81%E6%9C%89%E4%BB%93%E5%BA%93.html#%E5%AE%89%E8%A3%85%E8%BF%90%E8%A1%8C-docker-registry

### [#](https://www.funtl.com/zh/docs-docker/Docker-私有仓库.html#容器运行)容器运行

你可以通过获取官方 `registry` 镜像来运行。

```bash
 docker run -d -p 5000:5000 --restart=always --name registry registry
```

这将使用官方的 `registry` 镜像来启动私有仓库。默认情况下，仓库会被创建在容器的 `/var/lib/registry` 目录下。你可以通过 `-v` 参数来将镜像文件存放在本地的指定路径。例如下面的例子将上传的镜像放到本地的 `/opt/data/registry` 目录。

```bash
 docker run -d \
    -p 5000:5000 \
    -v /opt/data/registry:/var/lib/registry \
    registry
```

输入 http://你的IP:5000/v2/_catalog
![](/img/docker/45.jpg)
表示私有仓库搭建成功并且内容为空。
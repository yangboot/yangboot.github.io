---
title: 普罗米修斯监控docker
linktitle: 普罗米修斯监控docker
toc: true
type: docs
date: "2020-06-16T00:00:00+01:00"
draft: false
menu:
  prometheus:
    parent: prometheus监控学习
    weight: 3


---

## 安装cadvisor

```shell
docker run   --volume=/:/rootfs:ro   --volume=/var/run:/var/run:rw   --volume=/sys:/sys:ro   --volume=/apps/docker/:/var/lib/docker:ro   --volume=/dev/disk/:/dev/disk:ro   --publish=9101:8080   --detach=true   --name=cadvisor   google/cadvisor:latest
```

## 修改prometheus配置

```
  - job_name: docker 
    static_configs: 
      - targets: ['203.176.95.155:9101']

```

## 可视化

docker 监控仪表盘193
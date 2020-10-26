---
title:  GitLab安装
linktitle:  GitLab安装
toc: true
type: docs
date: "2020-06-28T00:00:00+01:00"
draft: false
menu:
 paas-study:
    parent: GitLab仓库管理系统
    weight: 1
---

# GitLab中文版安装

`GitLab`  ,`docker-compose`



 [dockerHub 中文版镜像地址](https://hub.docker.com/r/twang2218/gitlab-ce-zh) 

###  拉取镜像
```bash
docker pull twang2218/gitlab-ce-zh
```

### 编写docker-compose.yml

```shell
version: '3'
services:
    gitlab:
      image: 'docker.io/twang2218/gitlab-ce-zh:latest'
      restart: unless-stopped
      hostname: '203.176.95.155:8081'
      environment:
        TZ: 'mrpen/Nanning'
        GITLAB_OMNIBUS_CONFIG: |
          external_url 'http://203.176.95.155:8088'
          gitlab_rails['gitlab_shell_ssh_port'] = 2222
          gitlab_rails['port'] = 8888
          gitlab_rails['listen_port'] = 8088 
      ports:
        - '8088:8088'
        - '8443:443'
        - '2222:22'
      volumes:
        - ./config:/etc/gitlab
        - ./config:/var/opt/gitlab
        - ./log:/var/log/gitlab


```

输入http://你的主机IP:8088/访问

![](/img/gitlab/1.jpg)

修改密码后登陆，账号是root

![](/img/gitlab/2.jpg)



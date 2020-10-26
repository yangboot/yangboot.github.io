---
title:  GitLab基本设置
linktitle:  GitLab基本设置
toc: true
type: docs
date: "2020-06-28T00:00:00+01:00"
draft: false
menu:
 paas-study:
    parent: GitLab仓库管理系统
    weight: 2
---



`GitLab`

##  新建用户
![](/img/gitlab/3.jpg)

##  新建项目
![](/img/gitlab/4.jpg)

## SSH拉取和推送项目
进入Gti的安装目录下的的`usr\bin`下，默认在`C:\Program Files\Git\usr\bin`
进入cmd，输入以下命令（1785868089@qq.com可以改成你的邮箱）

```shell
ssh-keygen -t rsa -C "1785868089@qq.com"
```
 ![](/img/gitlab/5.jpg)
设置SSH密钥
![](/img/gitlab/7.jpg)
 复制`id_rsa.pub`文件下的内容
![](/img/gitlab/6.jpg)
![](/img/gitlab/8.jpg)

配置TortoiseGit

![](/img/gitlab/9.jpg)

采用SSH克隆

不需要密码拉取成功！！！！

![](/img/gitlab/10.jpg)
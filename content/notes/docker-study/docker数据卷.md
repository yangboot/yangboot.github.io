---
title: docker数据卷
linktitle: docker数据卷
toc: true
type: docs
date: "2020-06-18T00:00:00+01:00"
draft: false
menu:
 docker-study:
    parent: docker
    weight: 5
---

### docker数据卷（数据持久化）

1.命令       docke run  -it  -v   /宿主机绝对目录 : /容貌内目录   镜像名
2.查看数据是否挂载成功
3.容器与数据之间数据共享
4.容器停止后。主机修改后数据是否同步


### 数据之间数据共享

查看容器镜像
![](/img/docker/11.jpg)


查看宿主机根目录
![](/img/docker/10.jpg)


```shell
docker run -d -it -v /tomcatvolume:/usr/local/tomcat/logs tomcat:7
```

此时对比宿主机和容器

宿主机

![](/img/docker/12.jpg)

容器

![](/img/docker/13.jpg)


### 查看数据是否挂载成功

```shell
docker inspect 90b560cb23f5
```
![](/img/docker/17.jpg)




### 容器停止后。主机修改后数据是否同步

#### step1
这时我们可以在宿主机上新建一个test.txt 文件

宿主机
![](/img/docker/14.jpg)

容器

![](/img/docker/15.jpg)

#### step2

重启容器

```shell
docker restart 90b560cb23f5
```
查看容器/tomcat/logs下的文件发现文件依然存在

![](/img/docker/15.jpg)


### docker file实现

编写dockerFile
```shell
vim dockerfile
```
输入以下内容
```shell
FROM tomcat
VOLUME ["/dataVolumContainer1","/dataVolumContainer2"]
CMD echo "success!!!!!"
CMD /bin/bash
```
构建容器（官方文档：https://docs.docker.com/get-started/part2/）

```shell
docker build --tag dockerfile . 
```
![](/img/docker/18.jpg)

![](/img/docker/19.jpg)

运行镜像

```shell
docker run -it cb368229b4ab
```
![](/img/docker/20.jpg)

![](/img/docker/21.jpg)


### 数据卷容器

命名的数据挂载数据卷，其他容器通过挂载这个（父容器）实现数据共享，挂载数据卷的容器，称之为数据卷容器。
实验步骤：
1.创建一个父容器d01
2.d02/d03j继承d01
3.删除d01后查看d02和d03能否访问


#### 创建父类
```shell
docker run -it -d  --name d01 cb368229b4ab
```
![](/img/docker/22.jpg)

#### d02/d03j继承d01
d02继承d01容器
```shell
docker run -it -d --name  d02  --volumes-from d01 cb368229b4ab
```
![](/img/docker/23.jpg)
d03继承d01
![](/img/docker/24.jpg)

#### d01(父容器)新增文件

d01:
![](/img/docker/25.jpg)

d02:
![](/img/docker/26.jpg)

d03: 
![](/img/docker/27.jpg)
结论：父容器可以跟子容器数据共享

#### d02新增文件

d01
![](/img/docker/29.jpg)

d02
![](/img/docker/28.jpg)

d03:
![](/img/docker/30.jpg)
结论：子容器可以跟父容器数据共享

#### 删除d01容器

![](/img/docker/31.jpg)

d02:
![](/img/docker/32.jpg)

d03:
![](/img/docker/32.jpg)

结论：父容器删除后子容器依然存在









---
title: docker命令
linktitle: docker命令
toc: true
type: docs
date: "2019-05-05T00:00:00+01:00"
draft: false
menu:
  docker-study:
    parent: docker
    weight: 1

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 2
---

docker hub地址： https://hub.docker.com/



### 镜像命令





#### 运行hello-world

```shell
docker run hello-world
```



#### 查询镜像

```shell
docker     images                                     查看主机上可运行的镜像

docker     images   -a             			          列出本地所有镜像

docker     images   -q              			      只显示镜像ID

docker     images   --digests           	          显示镜像摘要信息
```



#### 搜索镜像

（系统去docker hub搜索，但是是从阿里云拉）
```shell
docker search  XXX
```
列出点赞数不少于20的tomcat镜像
```shell
docker search  -s  20  tomcat
```
![](/img/docker/5.jpg)



#### 下载（拉取）镜像

docker pull  XXX

下载tomcat 
```shell
docker pull  tomcat      后面接版本号，不写就是最新版本
```




#### 删除镜像

```shell
docker rmi image id   (有容器的时候删不掉)
```
删除全部镜像

```shell
docker rmi $(docker images -q)
```




### 容器命令

运行容器
```shell
docker run -it  XXX
```

后台运行
```shell
docker run  -d -it  XXX
```

运行hellow-world
```shell
docker run -it fd74c3615f76
```


#### 查看


l列出所有正在运行的容器
```shell
docker ps
```
查看两条运行记录
```shell
docker ps -n 2
```

#### 启动容器
启动容器
```shell
docker start  XXX（容器ID或者容器名）
```
#### 进入交互模式

```shell
docker exec -it XXX /bin/bash
```

### 安全退出交互式界面：
```shell
Ctrl+P+Q
```
重启容器
```shell
docker restart  XXX（容器ID或者容器名）
```
#### 关闭容器

温柔停止
```shell
docker stop XXX（容器ID或者容器名）
```
强制停止
```shell
docker stop XXX（容器ID或者容器名）
```
```shell
docker kill  XXX（容器ID或者容器名）

```

#### 删除容器


```shell
docker rm  -f XXX（容器ID或者容器名）
```
停止所有容器
```shell
docker stop $(docker ps -aq)
```
删除所有容器

```shell
docker rm $(docker ps -aq)
```
### 从容器创建一个新的镜像

将容器a404c6c174a2 保存为新的镜像,并添加提交人信息和说明信息。
```shell
docker commit -a "runoob.com" -m "my apache" a404c6c174a2  mytoncat:v1
```

- **-a :**提交的镜像作者；
- **-c :**使用Dockerfile指令来创建镜像；
- **-m :**提交时的说明文字；
- **-p :**在commit时，将容器暂停。

###  查看容器占用

ps -ef|grep 容器Id


```
[root@wentao-2 order]# ps -ef|grep 3a61cb3fd4f6
root      7358 12956  0 09:14 ?        00:00:00 containerd-shim -namespace moby -workdir /var/lib/containerd/io.containerd.runtime.v1.linux/moby/3a61cb3fd4f64f6fed464ca6c7185c5138c9256ac8ceb049b9527272573e994d -address /run/containerd/containerd.sock -containerd-binary /usr/bin/containerd -runtime-root /var/run/docker/runtime-runc
root      7893  5652  0 09:19 pts/0    00:00:00 grep --color=auto 3a61cb3fd4f6
123
```

top -p 7358(pid)

```
[root@wentao-2 order]# top -p 7358
top - 09:22:14 up 103 days, 18:13,  2 users,  load average: 0.04, 0.09, 0.12
Tasks:   1 total,   0 running,   1 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.8 us,  0.3 sy,  0.0 ni, 98.9 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  8010424 total,   170620 free,  7303264 used,   536540 buff/cache
KiB Swap:        0 total,        0 free,        0 used.   369860 avail Mem 

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND                                                                                                                    
 7358 root      20   0   10724   3744   2656 S   0.0  0.0   0:00.05 containerd-shim      
123456789
```

参数说明：
PID：进程的ID
USER：进程所有者
PR：进程的优先级别，越小越优先被执行
NInice：值
VIRT：进程占用的虚拟内存
RES：进程占用的物理内存
SHR：进程使用的共享内存
S：进程的状态。S表示休眠，R表示正在运行，Z表示僵死状态，N表示该进程优先值为负数
%CPU：进程占用CPU的使用率
%MEM：进程使用的物理内存和总内存的百分比
TIME+：该进程启动后占用的总的CPU时间，即占用CPU使用时间的累加值。
COMMAND：进程启动命令名称
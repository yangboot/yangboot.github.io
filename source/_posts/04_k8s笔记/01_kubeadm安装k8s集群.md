---
title: 【k8s笔记_01】kubeadm安装k8s集群
categories: k8s笔记
tags:
  - kubeadm
  - k8s
  - calico
excerpt: k8s笔记
date: 2021-03-08 01:36:36
img:  /images/k8s/logo.png
---


### 软件介绍
Kubernetes 集群（以下简称 K8S）是一个开源的容器集群管理平台，可以实现容器集群的自动化部署、自动扩缩容、维护等功能。Kubernetes的目标是促进完善组件和工具的生态系统，以减轻应用程序在云上运行的负担。

Kubernetes 集群中存在两种节点，Master 节点和 Worker 节点。Master 节点是集群的控制节点，负责整个集群的管理和控制。针对集群执行的控制命令都是发送给 Master 节点的。Worker 节点是 Kubernetes 集群中的工作负载节点，Worker 上的工作负载由 Master 分配，当某个 Worker 宕机时，Master 会将上面的工作负载转移到其他节点上去。

本文参照 [openEuler迁移k8s文章](https://openeuler.org/zh/docs/20.03_LTS_SP1/docs/thirdparty_migration/k8sinstall.html)  

### 环境配置
#### 环境介绍

系统环境为 **centos 7.8 minimal**

软件名称|版本号
:--:|:--:
docker-engine|19.03.12
kubelet|1.18.0
kubeadm|1.18.0
kubectl|1.18.0
kubernetes-cni|0.75

### 系统配置

#### 修改主机名 
- 通过命令设置你的主机名
``` bash
sudo hostnamectl set-hostname xxx
```
#### 修改主机配置
分别编辑 Master 和 Worker 节点的 **/etc/hosts** 文件，在文件末尾添加 Master 和 Worker 节点的IP。
``` bash
cat >> /etc/hosts <<EOF
192.168.75.10   k8s-master
192.168.75.20   k8s-node01
192.168.75.21   k8s-node02
EOF
```

### 安装 docker

略

### 关闭防火墙和selinux
分别在 Master 和 Worker 节点上执行如下命令，关闭防火墙和 selinux。（不同操作系统操作不一样）
``` bash
systemctl stop firewalld
systemctl disable firewalld
setenforce 0
sed -i '/^SELINUX=/s/enforcing/disabled/' /etc/selinux/config
```

### 配置 kubernetes yum 源
1. 分别在 Master 和 Worker 节点上执行如下命令，配置 kubernetes 的 yum 源。（apt 源需要自行解决）
- x86架构：
``` bash
cat <<EOF > /etc/yum.repos.d/kubernetes.repo

[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enable=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=http://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
	  http://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF

```
- aarch64架构
``` bash
cat <<EOF > /etc/yum.repos.d/kubernetes.repo

[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-aarch64
enable=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=http://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
      http://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF

```

2. 配置完成后，执行如下命令，清除缓存中的软件包及旧的 headers，重新建立缓存。
``` bash
yum clean all
yum makecache -y
```

### 关闭交换分区
在安装 K8S 集群时，Linux 的 Swap 内存交换机制需要关闭，否则会因为内存交换影响系统的性能和稳定性。

1. 分别在 Master 和 Worker 节点上执行如下命令，关闭交换分区。
``` bash
swapoff -a
cp -p /etc/fstab /etc/fstab.bak$(date '+%Y%m%d%H%M%S')
sed -i "s/\/dev\/mapper\/openeuler-swap/\#\/dev\/mapper\/openeuler-swap/g" /etc/fstab
```

2. 执行如下命令查看是否修改成功。
``` bash
cat /etc/fstab
```

3.执行如下命令重启系统。
``` bash
reboot
```

### 软件安装

#### 安装k8s组件
分别在 Master 和 Worker 节点上执行如下命令，安装 k8s 组件。
``` bash
yum install -y kubelet-1.18.0 kubeadm-1.18.0 kubectl-1.18.0 kubernetes-cni-0.7.5
```

#### 配置开机启动项
1. 分别在 Master 和 Worker 节点上执行如下命令，配置开机启动 **kubelet。**
``` bash
systemctl enable kubelet
```

2. 分别在 Master 和 Worker 节点上创建 **/etc/sysctl.d/k8s.conf** 文件，并添加如下内容。
``` bash
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
vm.swappiness=0
```

3.分别在 Master 和 Worker 节点上执行如下命令，使修改生效。

``` bash
modprobe br_netfilter
sysctl -p /etc/sysctl.d/k8s.conf
```

#### 通过Docker下载组件
Master 和 Worker 节点通过 Docker 下载其他组件，下载镜像时需要根据架构选择相应的版本,以下命令分别两台节点上执行，操作步骤如下。

1. 查看初始化所需镜像，执行如下命令

   ```bash
   kubeadm config images list
   ```


- x86架构
``` bash
docker pull registry.aliyuncs.com/google_containers/kube-apiserver-amd64:v1.18.0
docker pull registry.aliyuncs.com/google_containers/kube-controller-manager-amd64:v1.18.0
docker pull registry.aliyuncs.com/google_containers/kube-scheduler-amd64:v1.18.0
docker pull registry.aliyuncs.com/google_containers/kube-proxy-amd64:v1.18.0
docker pull registry.aliyuncs.com/google_containers/pause-amd64:3.2
docker pull registry.aliyuncs.com/google_containers/etcd-amd64:3.4.3-0
docker pull coredns/coredns:1.6.7
```

- aarch64架构
``` bash
docker pull registry.aliyuncs.com/google_containers/kube-apiserver-arm64:v1.18.0
docker pull registry.aliyuncs.com/google_containers/kube-controller-manager-arm64:v1.18.0
docker pull registry.aliyuncs.com/google_containers/kube-scheduler-arm64:v1.18.0
docker pull registry.aliyuncs.com/google_containers/kube-proxy-arm64:v1.18.0
docker pull registry.aliyuncs.com/google_containers/pause-arm64:3.2
docker pull registry.aliyuncs.com/google_containers/etcd-arm64:3.4.3-0
docker pull coredns/coredns:1.6.7
```

2. 执行如下命令，给已下载的镜像打标签。
- x86架构
``` bash
docker tag registry.aliyuncs.com/google_containers/kube-apiserver-amd64:v1.18.0 k8s.gcr.io/kube-apiserver:v1.18.0
docker tag registry.aliyuncs.com/google_containers/kube-controller-manager-amd64:v1.18.0 k8s.gcr.io/kube-controller-manager:v1.18.0
docker tag registry.aliyuncs.com/google_containers/kube-scheduler-amd64:v1.18.0 k8s.gcr.io/kube-scheduler:v1.18.0
docker tag registry.aliyuncs.com/google_containers/kube-proxy-amd64:v1.18.0 k8s.gcr.io/kube-proxy:v1.18.0
docker tag registry.aliyuncs.com/google_containers/pause-amd64:3.2 k8s.gcr.io/pause:3.2
docker tag registry.aliyuncs.com/google_containers/etcd-amd64:3.4.3-0 k8s.gcr.io/etcd:3.4.3-0
docker tag coredns/coredns:1.6.7 k8s.gcr.io/coredns:1.6.7
```

- aarch64架构
``` bash
docker tag registry.aliyuncs.com/google_containers/kube-apiserver-arm64:v1.18.0 k8s.gcr.io/kube-apiserver:v1.18.0
docker tag registry.aliyuncs.com/google_containers/kube-controller-manager-arm64:v1.18.0 k8s.gcr.io/kube-controller-manager:v1.18.0
docker tag registry.aliyuncs.com/google_containers/kube-scheduler-arm64:v1.18.0 k8s.gcr.io/kube-scheduler:v1.18.0
docker tag registry.aliyuncs.com/google_containers/kube-proxy-arm64:v1.18.0 k8s.gcr.io/kube-proxy:v1.18.0
docker tag registry.aliyuncs.com/google_containers/pause-arm64:3.2 k8s.gcr.io/pause:3.2
docker tag registry.aliyuncs.com/google_containers/etcd-arm64:3.4.3-0 k8s.gcr.io/etcd:3.4.3-0
docker tag coredns/coredns:1.6.7 k8s.gcr.io/coredns:1.6.7
```

3. 执行如下命令，查看上步中的镜像是否成功打上 k8s 标签
``` bash
docker images | grep k8s
```

4. 标签打好后，执行如下命令，删除当前环境上的旧镜像(可选步骤)
- x86架构
``` bash
docker rmi registry.aliyuncs.com/google_containers/kube-apiserver-amd64:v1.18.0
docker rmi registry.aliyuncs.com/google_containers/kube-controller-manager-amd64:v1.18.0
docker rmi registry.aliyuncs.com/google_containers/kube-scheduler-amd64:v1.18.0
docker rmi registry.aliyuncs.com/google_containers/kube-proxy-amd64:v1.18.0
docker rmi registry.aliyuncs.com/google_containers/pause-amd64:3.2
docker rmi registry.aliyuncs.com/google_containers/etcd-amd64:3.4.3-0
docker rmi coredns/coredns:1.6.7
```

- aarch64架构
``` bash
docker rmi registry.aliyuncs.com/google_containers/kube-apiserver-arm64:v1.18.0
docker rmi registry.aliyuncs.com/google_containers/kube-controller-manager-arm64:v1.18.0
docker rmi registry.aliyuncs.com/google_containers/kube-scheduler-arm64:v1.18.0
docker rmi registry.aliyuncs.com/google_containers/kube-proxy-arm64:v1.18.0
docker rmi registry.aliyuncs.com/google_containers/pause-arm64:3.2
docker rmi registry.aliyuncs.com/google_containers/etcd-arm64:3.4.3-0
docker rmi coredns/coredns:1.6.7
```

#### 配置 Master 节点
1. 在 Master 节点上执行如下命令，进行集群初始化。
``` bash
systemctl daemon-reload
systemctl restart kubelet
kubeadm init --kubernetes-version v1.18.0 --pod-network-cidr=10.244.0.0/16  
```

2. 按照初始化成功的控制台显示信息配置集群
``` bash
mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config  
```

3. 将子节点加入 Master 节点
集群初始化成功后，界面显示信息如下。
![](/images/k8s/1.png)
在 Worker 节点执行配置 Master 节点中保存的命令，将 Worker 节点加入集群。

```bash 
kubeadm join 192.168.75.10:6443 --token 4fhy0b.f3emt5y22pgq2rd7 \
    --discovery-token-ca-cert-hash sha256:16ac8798732f44ef9431ad4c42e5e14d5199e09e4882b02ba97612f46a0544c6
```
> 说明： token默认有效期为24小时，若token超时，可在Master节点上执行命令kubeadm token create --print-join-command重新生成。

4. 在Master节点执行如下命令，查看集群节点信息。
``` bash
kubectl get node
```
![](/images/k8s/2.png)

由于还没有配置**网络插件**，当前node状态为未就绪。

#### 安装calico网络插件
1. 分别在 Master 和 Worker 节点上执行如下命令，下载 calico 容器镜像

- x86架构
``` bash
docker pull calico/cni:v3.14.2-amd64
docker pull calico/node:v3.14.2-amd64
docker pull calico/kube-controllers:v3.14.2-amd64
docker pull calico/pod2daemon-flexvol:v3.14.2-amd64
```

- aarch64架构
``` bash
docker pull calico/cni:v3.14.2-arm64
docker pull calico/node:v3.14.2-arm64
docker pull calico/kube-controllers:v3.14.2-arm64
docker pull calico/pod2daemon-flexvol:v3.14.2-arm64
```

2. 分别在 Master 和 Worker 节点上执行如下命令，修改已下载的镜像标签

- x86架构
``` bash
docker tag calico/cni:v3.14.2-amd64 calico/cni:v3.14.2
docker tag calico/node:v3.14.2-amd64 calico/node:v3.14.2
docker tag calico/kube-controllers:v3.14.2-amd64 calico/kube-controllers:v3.14.2
docker tag calico/pod2daemon-flexvol:v3.14.2-amd64 calico/pod2daemon-flexvol:v3.14.2
```

- aarch64架构
``` bash
docker tag calico/cni:v3.14.2-arm64 calico/cni:v3.14.2
docker tag calico/node:v3.14.2-arm64 calico/node:v3.14.2
docker tag calico/kube-controllers:v3.14.2-arm64 calico/kube-controllers:v3.14.2
docker tag calico/pod2daemon-flexvol:v3.14.2-arm64 calico/pod2daemon-flexvol:v3.14.2
```

3. 执行如下命令，查看是否成功打上 calico 标签
``` bash
docker images | grep calico
```

4. 分别在 Master 和 Worker 节点上执行如下命令，删除旧镜像

- x86架构
``` bash
docker rmi calico/cni:v3.14.2-amd64
docker rmi calico/node:v3.14.2-amd64
docker rmi calico/kube-controllers:v3.14.2-amd64
docker rmi calico/pod2daemon-flexvol:v3.14.2-amd64
```

- aarch64架构
``` bash
docker rmi calico/cni:v3.14.2-arm64
docker rmi calico/node:v3.14.2-arm64
docker rmi calico/kube-controllers:v3.14.2-arm64
docker rmi calico/pod2daemon-flexvol:v3.14.2-arm64
```

5. 在 Master 节点上执行如下命令，下载 calico 的 yaml 文件
``` bash
wget https://docs.projectcalico.org/v3.14/getting-started/kubernetes/installation/hosted/kubernetes-datastore/calico-networking/1.7/calico.yaml --no-check-certificate
```

6. 在 Master 节点上执行如下命令，部署 calico
``` bash
kubectl apply -f calico.yaml
```
7. 在 Master 节点上执行如下命令，查看节点状态,状态为 Ready 即表明安装成功。
``` bash
kubectl get nodes
```
![](/images/k8s/3.png)

8. 将子节点标记为 worker 节点
``` bash
kubectl label node xxxxx node-role.kubernetes.io/worker=worker
```

#### 查看状态信息相关命令
- 查看所有 pods
``` bash
kubectl get pods -A
```

- 查看当前节点上运行在某一命名空间的所有 pod。
``` bash
kubectl get pods -n $namespace
```

- 查看某一命名空间下 pod 的详细信息。
``` bash
kubectl get pods -n $namespace -o wide
```

- 查看单个 pod 信息，可用于定位 pod 状态异常问题。
``` bash
kubectl describe pod $podname -n $namespace
```

- 删除pod，删除正在运行的pod,控制器会马上再创建一个新的
``` bash
kubectl delete pods $podname
```

#### 软件卸载
如果不需要使用 k8s 集群时，可以按本章节操作，删除 k8s 集群，以下命令需要分别在 Master 和 Worker 节点上执行

1. 执行如下命令，清空 k8s 集群设置
``` bash
kubeadm reset
rm –rf $HOME/.kube/config
```

2. 执行如下命令，删除基础组件镜像
``` bash
docker rmi k8s.gcr.io/kube-apiserver:v1.18.0
docker rmi k8s.gcr.io/kube-controller-manager:v1.18.0
docker rmi k8s.gcr.io/kube-scheduler:v1.18.0
docker rmi k8s.gcr.io/kube-proxy:v1.18.0
docker rmi k8s.gcr.io/pause:3.2
docker rmi k8s.gcr.io/etcd:3.4.3-0
docker rmi k8s.gcr.io/coredns:1.6.7
```

3. 执行如下命令，卸载管理软件
``` bash
yum erase –y kubelet kubectl kubeadm kubernetes-cni 
```
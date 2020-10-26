---
linktitle: 安装jdk
toc: true
type: docs
date: "2020-05-05T00:00:00+01:00"
draft: false
menu:
  linux-study:
   parent: Linux操作系统学习笔记
   weight: 4
# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---



### yum安装jdk



```shell
yum install java-1.8.0-openjdk* -y
```



### 手动安装

下载好[jdk-8u112-linux-x64.tar.gz ](https://pan.baidu.com/s/1JREejFlPOmr2_uSbDkHf2w?_blank)提取码：liak

下载好[Xshell 6和Xftp 6](https://pan.baidu.com/s/1EC_TeGLMJmXdJxdEHHXMEw?_blank)提取码：9b13rvice


### 上传安装包
使用Xftp连接云服务器，连接可能会出现"无法与XXX连接"，别慌

![](/img/linux/4.png)

执行以下命令，观察22号端口有没有被监听

```shell
netstat  -anp  |  more
```

没有启动的话，重启一下
```shell
service  sshd  restart
```

### 安装
打开Xshell连接到服务器跳转到/opt目录下
```shell
cd  /opt  
```

解压opt文件夹下的jdk-8u112-linux-x64.tar.gz文件
```shell
tar  -zxvf jdk-8u112-linux-x64.tar.gz
```
![](/img/linux/5.jpg)

![](/img/linux/6.jpg)

###  配置jdk环境变量
编辑/etc/目录下的profile文件

```shell
vim    /etc/profile
```

按下` Esc键` 进入命令行模式 输入` G ` 进入最后一行
按下 ` i ` 键进入编辑模式 ，输入以下配置


```shell
JAVA_HOME=/opt/jdk1.8.0_112
PATH=/opt/jdk1.8.0_112/bin:$PATH
export  JAVA_HOME PATH
```

按下` Esc键` 进入命令行模式，输入` wq!` 强制退出保存，再logout注销命令

重新登录后
输入java
![](/img/linux/8.jpg)
输入javac
![](/img/linux/9.jpg)
说明安装成功

### 编写Hello
```shell
vim  /opt/Hello.java
```
按下 i 键进入编辑模式 ，输入以下配置
```java
public class Hello{
	public static void main (String[] args){
		System.out.println("Hellow");
}
```
按下` Esc键` 进入命令行模式，输入` wq!` 强制退出保存输入
```shell
javac Hello.java
```
![](/img/linux/10.jpg)













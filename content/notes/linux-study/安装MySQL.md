---

linktitle: 手动安装MySQL
toc: true
type: docs
date: "2019-05-05T00:00:00+01:00"
draft: false
menu:
  linux-study:
    parent: Linux操作系统学习笔记
    weight: 3

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 2
---



### 安装MySQL

### 卸载老版本MySQL

检查是否已经安装过mysql

```shell
rpm -qa | grep mysql
```
![](/img/linux/11.jpg)

强制卸载
```shell
rpm -e --nodeps  mysql-libs-5.1.73-8.el6_8.x86_64
```


### 安装编译代码需要的包
复制mysql-5.6.14.tar.gz到Linux服务器上
![](/img/linux/13.jpg)
安装gcc环境 
```shell
yum -y install make gcc-c++ cmake bison-devel ncurses-devel
```
```shell
tar -xzvf   mysql-5.6.14.tar.gz
```


源码编译
```shell
cd   mysql-5.6.14
```
```shell
cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql -DMYSQL_DATADIR=/usr/local/mysql/data -DSYSCONFDIR=/etc -DWITH_MYISAM_STORAGE_ENGINE=1 -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_MEMORY_STORAGE_ENGINE=1 -DWITH_READLINE=1 -DMYSQL_UNIX_ADDR=/var/lib/mysql/mysql.sock -DMYSQL_TCP_PORT=3306 -DENABLED_LOCAL_INFILE=1 -DWITH_PARTITION_STORAGE_ENGINE=1 -DEXTRA_CHARSETS=all -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci
```

编译并安装
```shell
make && make install
```

### 添加mysql用户和分组
```shell
cat /etc/passwd 查看用户列表
```
```shell
cat /etc/group  查看用户组列表
```
```shell
groupadd mysql
```
```shell
useradd -g mysql mysql
```

修改/usr/local/mysql权限
```shell
chown -R mysql:mysql /usr/local/mysql
```

### 初始化配置，
进入安装路径（在执行下面的指令），执行初始化配置脚本，创建系统自带的数据库和表
```shell
cd /usr/local/mysql
```
```shell
scripts/mysql_install_db --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data --user=mysql  
```

>  注：在启动MySQL服务时，会按照一定次序搜索my.cnf，先在/etc目录下找，找不到则会搜索"$basedir/my.cnf"，在本例中就是 /usr/local/mysql/my.cnf，这是新版MySQL的配置文件的默认位置！
>
>
>  注意：在CentOS 6.8版操作系统的最小安装完成后，在/etc目录下会存在一个my.cnf，需要将此文件更名为其他的名字，如：/etc/my.cnf.bak，否则，该文件会干扰源码安装的MySQL的正确配置，造成无法启动。
>

修改名称，防止干扰：


```shell
mv /etc/my.cnf /etc/my.cnf.bak
```


### 启动MySQL

添加服务，拷贝服务脚本到init.d目录，并设置开机启动 

[注意在 /usr/local/mysql 下执行]


```shell
cp support-files/mysql.server /etc/init.d/mysql
```

设置默认自启 

```shell
chkconfig mysql on
```
```shell
service mysql start  --启动MySQL
```
![](/img/linux/14.jpg)


### 修改密码
```shell
cd /usr/local/mysql/bin
```
```shell
./mysql -u  账号  -p  密码  
```
```shell
SET PASSWORD = PASSWORD('你的新密码');
```
```shell
quit  退出Mysql
```

### 给linux下的mysql用户开启远程登录权限

配好之后发现本地用Navicat连不上,原因是没有开启远程登录权限


mysql>     GRANT ALL PRIVILEGES ON *`①`* TO '`②`'@'`③`' IDENTIFIED BY '`④`' ; 

① ：允许访问的数据库,可以是某个数据库，也可以是` *.* `所有

②：分配的账号

③：允许访问的主机IP，也可以是`%`所有IP

④：分配的密码







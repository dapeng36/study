# Centos 7.2 安装和卸载 MongoDB

[![](http://images.extlight.com/mongodb-logo-black.png)](http://images.extlight.com/mongodb-logo-black.png)

## 一、前言

MongoDB 是由 C++ 语言编写的，是一个基于分布式文件存储的开源数据库系统。

MongoDB 旨在为 WEB 应用提供可扩展的高性能数据存储解决方案。

MongoDB 将数据存储为一个文档，数据结构由键值（key-value）对组成，其文档类似于 JSON 对象，字段值可以包含其他文档，数组及文档数组。在高负载的情况下，添加更多的节点，可以保证服务器性能。

## 二、安装

### 2.1 添加源

```
vim /etc/yum.repos.d/mongodb-org-3.4.repo

[mongodb-org-3.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc

```

### 2.2 安装

```
yum install -y mongodb-org

```

### 2.3 启动服务

```
service mongod start

```

结果：

```
[root@localhost ~]# service mongod start
Redirecting to /bin/systemctl start  mongod.service
[root@localhost ~]# ps -ef | grep mongod
mongod    24960      1  1 23:43 ?        00:00:00 /usr/bin/mongod -f /etc/mongod.conf
root      24989  24056  0 23:43 pts/0    00:00:00 grep --color=auto mongod

```

### 2.4 开机自启

```
chkconfig mongod on

```

## 三、目录介绍

配置文件：

```
/etc/mongod.conf

```

数据目录：

```
/var/lib/mongo

```

日志目录：

```
/var/log/mongodb

```

如果需要修改数据目录和日志目录，只需修改 /etc/mongod.conf 中的 storage.dbPath 和 systemLog.path 即可。

## 四、卸载

### 4.1 关闭服务

```
service mongod stop

```

### 4.2 删除相关的包

```
yum erase $(rpm -qa | grep mongodb-org)

```

### 4.3 删除目录和文件

```
rm -r /var/log/mongodb
rm -r /var/lib/mongo

```

## 五、偶遇问题

1. Failed to unlink socket file /tmp/mongodb-27017.sock Operation not permitted

解决方案：删除该文件

1. Unable to lock file: /var/lib/mongo/mongod.lock

解决方案：清空该文件内容


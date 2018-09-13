# CentOS 7.2 安装 RabbitMQ



## 一、前言

RabbitMQ 是轻量级且易于部署的消息中间件。它支持多种消息传递协议，可以在多个操作系统环境中运行，为大多数流行的语言提供了广泛的开发工具。

## 二、安装 Erlang

安装 RabbitMQ 之前需要安装 Erlang。

### 2.1 添加源

vim /etc/yum.repos.d/rabbitmq-erlang.repo

```
[rabbitmq-erlang]
name=rabbitmq-erlang
baseurl=https://dl.bintray.com/rabbitmq/rpm/erlang/20/el/7
gpgcheck=1
gpgkey=https://www.rabbitmq.com/rabbitmq-release-signing-key.asc
repo_gpgcheck=0
enabled=1

```

### 2.2 安装

```
yum install erlang

```

## 三、安装 RabbitMQ

### 3.1 下载安装包

```
wget https://github.com/rabbitmq/rabbitmq-server/releases/download/v3.7.0/rabbitmq-server-3.7.0-1.el7.noarch.rpm

```

### 3.2 安装

```
yum install rabbitmq-server-3.7.0-1.el7.noarch.rpm

```

### 3.3 启动 RabbitMQ：

```
# 启动服务
service rabbitmq-server start

# 开机自启动
chkconfig rabbitmq-server on

```

## 四、目录结构

**命令相关：**

```
/usr/lib/rabbitmq

```

**配置相关：**

```
/etc/rabbitmq

```

该目录下主要存放 rabbitmq.conf 和 rabbitmq-env.conf 两个配置文件。但是，默认情况下，它们并没有被创建，需要我们手动创建。

至于配置文件中需要配置什么内容，请参考文章末尾提供的链接。

**日志相关：**

```
/var/log/rabbitmq

```

## 五、管理界面

RabbitMQ 提供一个管理插件用于管理和监视 RabbitMQ 服务器。

### 5.1 访问

需要开启管理插件。

```
rabbitmq-plugins enable rabbitmq_management 

```

使用浏览器访问 [http://192.168.2.30:15672/](http://192.168.2.30:15672/) ，如下图：

[![](http://images.extlight.com/rabbitmq-02.jpg)](http://images.extlight.com/rabbitmq-02.jpg)

### 5.2 登陆

默认情况下，RabbitMQ 提供用户名和密码为 guest 的账号给用户进行登陆，但是这个账号只能针对 localhost 链接访问，因此现在 [http://192.168.2.30:15672](http://192.168.2.30:15672/) 无法通过此账号密码登陆。

我们需要新建账户：

```
# 添加新用户
rabbitmqctl add_user light light

# 设置角色
rabbitmqctl set_user_tags light administrator

```

使用该账户登录，结果如下：

[![](http://images.extlight.com/rabbitmq-01.jpg)](http://images.extlight.com/rabbitmq-01.jpg)

### 5.3 Http API

管理插件还提供了 HTTP API，该 API 旨在用于监视和警报。它提供了关于节点、连接、通道、队列、消费者等详细信息。

访问 [http://192.168.2.30:15672/api](http://192.168.2.30:15672/api) 后可以看到 api 相关的接口信息。

## 六、参考资料

- [https://github.com/rabbitmq/erlang-rpm](https://github.com/rabbitmq/erlang-rpm) 安装 erlang 相关
- [http://www.rabbitmq.com/install-rpm.html](http://www.rabbitmq.com/install-rpm.html) 安装 RabbitMQ 相关
- [http://www.rabbitmq.com/configure.html#config-file](http://www.rabbitmq.com/configure.html#config-file) RabbitMQ 配置文件相关
- [http://www.rabbitmq.com/management.html](http://www.rabbitmq.com/management.html) 管理界面相关


# Jenkins 基础入门



## 一、前言

Jenkins是一个开源软件项目，是基于Java开发的一种持续集成工具，用于监控持续重复的工作，旨在提供一个开放易用的软件平台，使软件的持续集成变成可能。

## 二、安装工作

> 测试环境：CentOS 7.4, IP：192.168.10.100

### 2.1 下载

```
wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo

rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key

yum install jenkins

```

### 2.2 启动/停止/重启命令

```
service jenkins start/stop/restart

chkconfig jenkins on

```

如果是首次安装 jenkins 启动失败，应该是 jenkins 没有找到 jdk 命令的缘故。我们有两种方式解决：

方式一： `yum install java`

方式二：解压包的 jdk ：

```
vim /etc/sysconfig/jenkins

修改

JENKINS_JAVA_CMD="/usr/jdk1.8/bin/java"

```

精确到 java 命令。

启动成功后，打开浏览器访问：[http://192.168.10.100:8080](http://192.168.10.100:8080/) 来到 Jenkins 初始化首页，按照提示设置即可，最终会看到如下界面：

[![](http://images.extlight.com/jenkins-02.jpg)](http://images.extlight.com/jenkins-02.jpg)

**如果还出现问题，请查看 jenkins 日志查找原因。**

### 2.3 文件目录

```
# 配置文件相关
/etc/sysconfig/jenkins

# 工作空间相关
/var/lib/jenkins/

# 日志相关
/var/log/jenkins/jenkins.log
```

### 2.4 插件安装

Jenkins 提供了很多插件，我们可以根据自己的需求进行下载，下载方式如下：

主界面-> 插件管理，右上角搜索框，效果图如下：

[![](http://images.extlight.com/jenkins-03.jpg)](http://images.extlight.com/jenkins-03.jpg)

## 三、安全授权

主界面 -> 系统管理 -> 全局安全管理

在授权策略中选择 “安全矩阵”，添加用户，输入我们安装 Jenkins 时设置的用户名。

[![](http://images.extlight.com/jenkins-04.jpg)](http://images.extlight.com/jenkins-04.jpg)

授权：

[![](http://images.extlight.com/jenkins-05.jpg)](http://images.extlight.com/jenkins-05.jpg)

## 四、全局工具

主界面 -> 系统管理 -> 全局工具配置。

我们需要配置 JDK、Git 和 Maven。如下图：

[![](http://images.extlight.com/jenkins-06.jpg)](http://images.extlight.com/jenkins-06.jpg)

**注意：要去掉自动安装的勾选**

## 五、实战演练

场景：通过 Jenkins 从 Github 上拉取 Maven 项目到本地进行打包，并自动部署到 Tomcat 中。

**测试的 maven 项目涉及到连接 mysql 数据库，在构建之前请设置数据库的数据。具体步骤请浏览 

### 5.1 新建任务

主界面 -> 新建任务，选择自由风格的软件项目

[![](http://images.extlight.com/jenkins-07.jpg)](http://images.extlight.com/jenkins-07.jpg)

### 5.2 源码管理

**这一步骤作用是获取源码。**

选中 Git，设置需要拉取的项目地址。

[![](http://images.extlight.com/jenkins-08.jpg)](http://images.extlight.com/jenkins-08.jpg)

### 5.3 构建

**这一步骤作用是将源码进行打包和部署。**

点击 “增加构建步骤”，选中 “调用顶层 Maven 目标”，设置 maven 执行的命令：`clean package -Dmaven.test.skip=true`，如下图：

[![](http://images.extlight.com/jenkins-09.jpg)](http://images.extlight.com/jenkins-09.jpg)

设置好 Maven 命令后，再点击 “增加构建步骤”，选中 “执行 Shell”，输入如下脚本（请根据自己的情况修改）：

```
BUILD_ID=DONTKILLME
TOMCAT_HOME="/usr/tomcat8"
JENKINS_HOME="/var/lib/jenkins" 

kill -9 `ps -ef | grep tomcat | awk 'NR==1 {print $2}'`

rm -rf $TOMCAT_HOME"/webapps/ROOT"
rm -rf $TOMCAT_HOME"/webapps/ROOT.war"

cp $JENKINS_HOME"/workspace/ml-blog/target/ml-blog-0.0.1-SNAPSHOT.war"  $TOMCAT_HOME"/webapps/ROOT.war"

sh $TOMCAT_HOME"/bin/startup.sh"
```

[![](http://images.extlight.com/jenkins-10.jpg)](http://images.extlight.com/jenkins-10.jpg)

保存，最后点击左侧 “立即构建” 即可。

构建完成后，我们打开浏览器访问：[http://192.168.10.100:8090](http://192.168.10.100:8090/)（jenkins 使用 8080 端口，将 tomcat 改成 8090），效果图如下：

[![](http://images.extlight.com/jenkins-11.jpg)](http://images.extlight.com/jenkins-11.jpg)

## 六、参考资料

- [Jenkins wiki](https://wiki.jenkins.io/display/JENKINS/Use+Jenkins)
- [Jenkins教程](https://www.yiibai.com/jenkins/)


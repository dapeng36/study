# ntelliJ IDEA 基础教程



## 一、前言

IDEA 全称 IntelliJ IDEA，是java语言开发的集成环境，IntelliJ在业界被公认为最好的java开发工具之一，尤其在智能代码助手、代码自动提示、重构、J2EE支持、各类版本工具(git、svn、github等)、JUnit、CVS整合、代码分析、 创新的GUI设计等方面的功能可以说是超常的。

> 演示版本：Version 2017.1.4

## 二、基础设置

首次打开 IntelliJ IDEA 会出现导航界面。

导航界面 -> Configure -> Settings，如下图：

[![](http://images.extlight.com/idea-01.jpg)](http://images.extlight.com/idea-01.jpg)

之后出现默认设置界面：

[![](http://images.extlight.com/idea-02.jpg)](http://images.extlight.com/idea-02.jpg)

**注意：通过此方式修改设置都是全局设置，会影响今后所有的项目。**

**另一种方式设置：在创建工程后，File -> Settings ...，该方式只对当前项目起作用。**

### 2.1 修改主题

Default Settings 界面 -> Appearance & Behavior -> Appearance

### 2.2 修改字体

Default Settings 界面 -> Editor -> Colors & Fonts -> Font

### 2.3 修改字符集

Default Settings 界面 -> Editor -> File Encodings

### 2.4 代码风格

Default Settings 界面 -> Editor -> Code Style -> Java

### 2.5 代码提示忽略大小写

Default Settings 界面 -> Editor -> General -> Code Completion

将右侧 Case sensitive completion 设置成 None。

### 2.6 自动导包

Default Settings 界面 -> Editor -> General -> Auto Import

将 Insert imports on paste 设置成 All。

同时勾选 Add unamiguous imports on the fly 和 Optimize imports on the fly。

### 2.7 取消最后编辑工程

默认情况下，再次启动 IDEA 会打开最后编辑工程，如果现取消该设置，可以如下操作：

Default Settings 界面 -> Appearance & Behavior -> System Settings

取消 Reopen last project on startup 的勾选。

### 2.8 生成 serialVersionUID

Default Settings 界面 -> Editor -> Inspections ，在右侧搜索框中搜索 Serialization class without 'serialVersionUID' ，在选项框打勾。

当类实现 Serializable 接口，alt + Enter 就有提示生成 serialVersionUID。

## 三、插件安装

Default Settings 界面 -> Plugins，右侧出现插件列表，共有三种安装插件方式：

[![](http://images.extlight.com/idea-03.jpg)](http://images.extlight.com/idea-03.jpg)

根据实际情况点击底部提供的 3 个按钮安装插件即可。

## 四、配置 JDK

导航界面 -> Configure -> Project Defaults -> Project Structure，

弹出新窗口，修改 Project SDK 即可：

[![](http://images.extlight.com/idea-04.jpg)](http://images.extlight.com/idea-04.jpg)

## 五、创建 JavaSE 工程

如图示：

[![](http://images.extlight.com/idea-05.jpg)](http://images.extlight.com/idea-05.jpg)

## 六、设置 JVM 参数

编辑界面 -> Help -> Editor Custom VM options ...

根据机器实际情况设置参数，笔者笔记本内存 12G ,设置参数如下：

```
# custom IntelliJ IDEA VM options

-Xms1024m
-Xmx2048m
-XX:ReservedCodeCacheSize=500m
-XX:+UseConcMarkSweepGC
-XX:SoftRefLRUPolicyMSPerMB=50
-ea
-Dsun.io.useCanonCaches=false
-Djava.net.preferIPv4Stack=true
-XX:+HeapDumpOnOutOfMemoryError
-XX:-OmitStackTraceInFastThrow
```

## 七、创建 Java Web 工程

如图示：

[![](http://images.extlight.com/idea-07.jpg)](http://images.extlight.com/idea-07.jpg)

## 八、配置 Tomcat

编辑界面 -> 倒三角按钮 -> Edit Configurations

[![](http://images.extlight.com/idea-08-1.jpg)](http://images.extlight.com/idea-08-1.jpg)

弹出 Configurations 界面，根据下图所示，选择 Tomcat Server 设置：

[![](http://images.extlight.com/idea-08-2.jpg)](http://images.extlight.com/idea-08-2.jpg)

之后会弹出新窗口，设置 Tomcat 目录即可。

## 九、添加第三方 jar 包

先将 jar 拷贝到项目中，具体操作如下图所示：

[![](http://images.extlight.com/idea-09-1.jpg)](http://images.extlight.com/idea-09-1.jpg)

保存后，我们还要操作一个步骤：

[![](http://images.extlight.com/idea-09-2.jpg)](http://images.extlight.com/idea-09-2.jpg)

## 十、配置 Maven

Default Settings 界面 -> Build,Execution,Deployment -> Build Tools -> Maven

## 十一、创建 Maven 工程

[![](http://images.extlight.com/idea-10.jpg)](http://images.extlight.com/idea-10.jpg)

## 十二、配置 SVN

### 12.1 设置 svn.exe

Default Settings 界面 -> Version Control -> Subversion

右侧设置 svn.exe 路径并勾选前边的选框。

**为了方便起见，读者可以直接安装 TortoiseSVN ,里边包含 svn.exe。**

### 12.2 上传项目

编辑界面 -> VCS -> Import into Version Control -> Share Project(Subversion)

弹出窗口填写 svn 服务器地址即可上传项目至 SVN 服务器。

**忽略上传文件/文件夹：编辑界面 -> File -> Settings -> Version Control -> ignored Files，右侧添加文件路径即可。**

### 12.3 下载项目

编辑界面 -> VCS -> Check out from Version Control -> Subversion

选择/添加 svn 地址即可下载 SVN 中的项目。

### 12.4 更新/提交文件

编辑界面的菜单栏，有两个 vcs 按钮，其中向下箭头表示更新文件，向上箭头表示提交文件。

## 十三、配置 GIT

### 13.1 设置 git.exe

Default Settings 界面 -> Version Control -> Git

右侧设置 git.exe 路径。

### 13.2 启动 GIT

编辑界面 -> VCS -> Enable Version Control Intergration

弹出窗口选择 git 即可。

## 十四、参考资料

- [IDEA 快捷键汇总](https://blog.csdn.net/wei83523408/article/details/60472168)
- [在IDEA中实战Git](https://blog.csdn.net/autfish/article/details/52513465)


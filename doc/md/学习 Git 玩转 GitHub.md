## 一、基本了解

### 1.1 什么是版本控制系统

版本控制系统是一种记录一个或若干个文件内容变化，以便将来查阅特定版本修订情况的系统。

### 1.2 什么是 Git

Git是一个免费、开源的版本控制软件

### 1.3 什么是 Github

Github是全球最大的社交编程及代码托管网站

### 1.4 Git 和 GitHub 的关系

- Git是版本控制软件
- Github是项目代码托管的平台，借助git来管理项目代码

### 1.5 什么要学习 GitHub

- 学习优秀的开源项目
- 关注行业前辈了解最新的行业动态

## 二、使用 GitHub

### 2.1 目的

借助 GitHub 托管项目代码

### 2.2 GitHub 中的基本概念

#### 2.2.1 Repository（仓库）

仓库用来存放项目代码，每个项目对应一个仓库，多个开源项目则有多个仓库

#### 2.2.2 Watch（关注）

关注项目，当项目更新可以接收到通知

#### 2.2.3 Star（收藏）

收藏项目，方便下次查看

#### 2.2.4 Fork（复制克隆项目）

复制克隆别人的项目到自己的 Github 仓库中，独立存在，任何操作都不会对原先的仓库造成影响。

#### 2.2.5 Issue（问题讨论）

发现代码 bug 时，发起消息与作者讨论

#### 2.2.6 Pull Request（发起请求）

当 fork 仓库，修改仓库的 bug 或 添加新功能后，想同步到原先的仓库时，发起请求给原仓库作者，让其对代码审核与合并。

### 2.3 注册 GitHub 账号

第一步：登陆官网 [GitHub官网](https://github.com/)，如下图：

[!\](http://ow97db1io.bkt.clouddn.com/github-1.jpg)](http://ow97db1io.bkt.clouddn.com/github-1.jpg)

第二步：点击首页右上角的 "Sign up"，进入如下页面：

[!\](http://ow97db1io.bkt.clouddn.com/github-2.jpg)](http://ow97db1io.bkt.clouddn.com/github-2.jpg)

填写好信息，点击 "Create an account"，来到如下页面：

[!\](http://ow97db1io.bkt.clouddn.com/github-3.jpg)](http://ow97db1io.bkt.clouddn.com/github-3.jpg)

GitHub已经帮我们设置好默认选项，我们只管点击左下角绿色按钮即可。

当看到下边的界面时，我们就注册好账号了：

[!\](http://ow97db1io.bkt.clouddn.com/github-4.jpg)](http://ow97db1io.bkt.clouddn.com/github-4.jpg)

### 2.4 创建仓库/创建新项目

点击上图 "Start a project" 按钮创建项目（仓库）。但是，由于我们刚注册好账号，并没有对邮件地址进行合法校验。所以会出现如下界面：

[!\](http://ow97db1io.bkt.clouddn.com/github-5.jpg)](http://ow97db1io.bkt.clouddn.com/github-5.jpg)

点击 红色框住的链接，GitHub 会跳转页面且重新发送一封邮件让我们进行对邮箱地址的校验。如图：

[!\](http://ow97db1io.bkt.clouddn.com/github-6.jpg)](http://ow97db1io.bkt.clouddn.com/github-6.jpg)

笔者使用的是 qq 邮箱，点击右下角提示进入到 qq 邮箱管理界面，如图：

[!\](http://ow97db1io.bkt.clouddn.com/github-7.jpg)](http://ow97db1io.bkt.clouddn.com/github-7.jpg)

点击 "Verify email address" 连接进行校验，浏览器会打开新标签提示邮箱地址校验成功。

[!\](http://ow97db1io.bkt.clouddn.com/github-8.jpg)](http://ow97db1io.bkt.clouddn.com/github-8.jpg)

此时，我们点击 "Start a project" 就能真正创建项目了。此时，我们会来到如下界面：

[!\](http://ow97db1io.bkt.clouddn.com/github-9.jpg)](http://ow97db1io.bkt.clouddn.com/github-9.jpg)

填写好基本信息，点击 "Create a repository"，来到仓库管理界面：

[!\](http://ow97db1io.bkt.clouddn.com/github-10.jpg)](http://ow97db1io.bkt.clouddn.com/github-10.jpg)

根据图中的标注，我们可以根据自己的需求管理仓库。

### 2.5 开源项目贡献流程

#### 2.5.1 新建Issue

提交使用问题、建议或者想法

#### 2.5.2 Pull Request

1. fork 项目
2. 修改自己仓库的项目代码
3. 新建 pull request
4. 等待作者操作审核

## 三、使用 Git

### 3.1 目的

通过 Git 管理 GitHub 托管项目代码

### 3.2 下载安装

登陆 [https://www.git-scm.com/download/win](https://www.git-scm.com/download/win) 下载

安装过程傻瓜化方式（在选择安装时，选择 Git Bash 和 Git GUI ，其余默认选项即可），步骤省略。

### 3.3 Git 工作原理

对于任何一个文件，在 Git 内都只有三种状态：已提交（committed），已修改（modified）和已暂存（staged）。已提交表示该文件已经被安全地保存在本地数据库中了；已修改表示修改了某个文件，但还没有提交保存；已暂存表示把已修改的文件放在下次提交时要保存的清单中。

由此我们看到 Git 管理项目时，文件流转的三个工作区域：Git 的工作目录，暂存区域，以及本地仓库。

[!\](http://ow97db1io.bkt.clouddn.com/git-1.png)](http://ow97db1io.bkt.clouddn.com/git-1.png)

每个项目都有一个 Git 目录（如果 git clone 出来的话，就是其中 .git 的目录）。它是 Git 用来保存元数据和对象数据库的地方。该目录非常重要，每次克隆镜像仓库的时候，实际拷贝的就是这个目录里面的数据。

从项目中取出某个版本的所有文件和目录，用以开始后续工作的叫做工作目录。这些文件实际上都是从 Git 目录中的压缩对象数据库中提取出来的，接下来就可以在工作目录中对这些文件进行编辑。

所谓的暂存区域只不过是个简单的文件，一般都放在 Git 目录中。有时候人们会把这个文件叫做索引文件，不过标准说法还是叫暂存区域。

基本的 Git 工作流程如下：

- 在工作目录中修改某些文件
- 对修改后的文件进行快照，然后保存到暂存区域
- 提交更新，将保存在暂存区域的文件快照永久转储到 Git 目录中。

### 3.4 Git 基本命令

#### 3.4.1 git init

初始化工作目录，在当前目录下会创建一个名为 .git 的隐藏文件夹。

#### 3.4.2 git add [文件名全路径]

创建或编辑文件后，执行该命令，将文件添加到暂存区

#### 3.4.3 git commit -m "提交信息"

将暂存区的文件推送到仓库中

#### 3.4.4 git status

列出当前目录所有还没有被 Git 管理的文件和被 Git 管理且被修改但还未提交(git commit)的文件

#### 3.4.5 git fetch

检测远程仓库所有分支

#### 3.4.6 git checkout [分支名称]

切换分支

#### 3.4.7 git branch

查看当前所在分支

#### 3.4.8 git rm

删除仓库中的文件

#### 3.4.9 git clone [远程仓库地址]

克隆远程仓库

#### 3.4.10 git push

同步本地仓库到远程仓库

### 3.5 Git 管理远程仓库

Git 连接和管理 GitHub 上的文件，为了安全性，需要设置公钥。

之前的步骤我们安装了 Git，打开 Git 安装目录下的 Git Bash ，键入

```
ssh-keygen -t rsa -C "GitHub邮箱地址" 

```

命令执行后会提示输入信息，我们不用填写，直接回车。最终会在系统 "C:/Users/用户名/.ssh/" 目录中生成 rsa 和 rsa.pub 文件。

打开 rsa.pub 文件复制里边的所有内容。

回到 GitHub 中，按照如下图操作：

[!\](http://ow97db1io.bkt.clouddn.com/github-11.jpg)](http://ow97db1io.bkt.clouddn.com/github-11.jpg)

最后我们进行测试，在 Git Bash 中键入：

```
ssh -T git@github.com

```

如果出现 "Hi xxx! You've successfully authenticated" 字样，说明我们配置成功了。

[!\](http://ow97db1io.bkt.clouddn.com/git-2.jpg)](http://ow97db1io.bkt.clouddn.com/git-2.jpg)

下边我们模拟场景运用 Git 命令将文件提交到 GitHub 的仓库中。

**场景一：非 clone 方式** 该场景下，我们没有对远程仓库进行关联，需要手动初始化 .git 文件夹

在 "E:\demo\git\test1"（自定义） 中，使用 Git Bash 或 CMD 键入：

```
git init

echo "hello world" > test.txt

git add .

git status

git commit -m "first commit"

#添加需要连接的远程仓库地址（关联远程仓库）
git remote add origin git@github.com:bluesky960/test.git 

git fetch 

# 切换到 master 分支
git checkout master 

git branch 

git push origin master

```

此时，在 Github 的 test 仓库中会多出 test.txt 文件。

**场景二：clone 方式**

在 "E:\demo\git\test2"（自定义） 中，使用 Git Bash 或 CMD 键入：

```
# 执行 clone 操作会在当前目录中创建 .git 文件夹，关联远程仓库
git clone git@github.com:bluesky960/test.git

cd test

echo "clone hello world" > test2.txt

git add .

git commit -m "add file"

git push

```

此时，在 Github 的 test 仓库中会多出 test2.txt 文件。

**为了防止每次同步文件都需要输入账号和密码，我们需要进特殊配置**

打开 Git Bash 窗口，键入：

```
git config --global user.name "GitHub用户名"
    
git config --global user.email "GitHub邮箱地址"

```

**踩坑提醒**

在执行 "git remote add origin" 命令时，后边跟着是 SSH 类型的仓库地址，否则，在每次 git push 时都要输入账号和密码！


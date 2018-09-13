# Lombok 简单入门



## 一、前言

Lombok 是一个 Java 库，它作为插件安装至编辑器中，其作用是通过简单注解来精简代码，以此达到消除冗长代码的目的。

## 二、简单介绍

### 2.1 优点

1. 通过注解自动生成成员变量的 getter、setter 等方法，使代码简洁
2. 修改类变量名时，无需关注其 getter、setter 等方法

### 2.2 缺点

降低源码文件的可读性。

### 2.3 原理

从 Java 6 开始，javac 就支持 JSR 269 API 规范，而 Lombok 实现 JSR 269 Pluggable Annation Processing API 规范。

当我们编写代码并保存后，编辑器会自动编译源码文件，在这个过程中，源码先被转化为 AST。

然后，Lombok 插件解析 AST 是否存在 Lombok 的注解。如果存在则修改 AST ，使其生成注解对应的代码。

最终将修改的 AST 解析并生成字节码文件。

## 三、安装插件

为编辑器安装 Lombok 插件。

### 3.1 IDEA 安装

在 IDEA 界面点击 “File”->"Settings" 弹出设置框，选择左侧 “Plugins”，通过 “Browse repositories” 搜索 **lombok**关键字安装即可。

### 3.2 Eclipse 安装

点击 [Lombok.jar](https://projectlombok.org/download)，下载该 jar 包。

双击 jar 包会弹出一个安装界面，点击界面的“Specify location...” 安装选择 Eclipse 的安装路径（精确到 eclipse.exe）。

## 四、使用

使用 Lombok 的方法非常简单，就是在类上或者成员变量上添加注解即可。

为了能使用注解，我们还需要在项目中引入 lombok 的 jar 包。

```
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>0.9.2</version>
</dependency>

```

### 4.1 注解介绍

Lombok 常用的注解如下：

| 注解名                 | 作用描述                                     |
| ------------------- | ---------------------------------------- |
| @Getter             | 作用在类上或成员变量上，生成对应的 getter 方法              |
| @Setter             | 作用在类上或成员变量上，生成对应的 setter 方法              |
| @NoArgsConstructor  | 作用在类上，生成对应的无参构造方法                        |
| @AllArgsConstructor | 作用在类上，生成对应的有参构造方法                        |
| @ToString           | 作用在类上，生成对应的 toString 方法                  |
| @EqualsAndHashCode  | 作用在类上，生成对应的 equals 和 hashCode 方法         |
| @Data               | 作用在类上，效果等同于上述 5 个注解，排除 @AllArgsConstructor 功能 |
| @Log4j/@Slf4j       | 作用在类上，生成对应的 Logger 对象，变量名为 log           |

### 4.2 案例演示

本次测试使用 Ecplise 编辑器。

```
@Data
public class User {

    private int id;
    
    private String name;
    
    private String password;
    
    private Date birthday;
    
}
```

当添加注解保存文件后，Ecplise 编辑器的 Outline 视图结果如下：

[![](http://images.extlight.com/lombok-02.jpg)](http://images.extlight.com/lombok-02.jpg)

我们还可以使用 jd-gui 等反编译工具查看源码，结果如下：

[![](http://images.extlight.com/lombok-01.jpg)](http://images.extlight.com/lombok-01.jpg)

## 五、参考资料

- [http://jnb.ociweb.com/jnb/jnbJan2010.html](http://jnb.ociweb.com/jnb/jnbJan2010.html) 官方文档


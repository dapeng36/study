# Shiro 基础教程



[![](http://images.extlight.com/apache-shiro-logo-02.png)](http://images.extlight.com/apache-shiro-logo-02.png)

## 一、前言

Apache Shiro 是 Java 的一个安全框架。功能强大，使用简单的Java安全框架，它为开发人员提供一个直观而全面的认证，授权，加密及会话管理的解决方案。

## 二、介绍

### 2.1 功能特点

Shiro 包含 10 个内容，如下图：

[![](http://images.extlight.com/shiro-01.png)](http://images.extlight.com/shiro-01.png)

1. Authentication：身份认证/登录，验证用户是不是拥有相应的身份。
2. Authorization：授权，即权限验证，验证某个已认证的用户是否拥有某个权限；即判断用户是否能做事情，常见的如：验证某个用户是否拥有某个角色。或者细粒度的验证某个用户对某个资源是否具有某个权限。
3. Session Manager：会话管理，即用户登录后就是一次会话，在没有退出之前，它的所有信息都在会话中；会话可以是普通 JavaSE 环境的，也可以是如 Web 环境的。
4. Cryptography：加密，保护数据的安全性，如密码加密存储到数据库，而不是明文存储。
5. Web Support：Web支持，可以非常容易的集成到 web 环境。
6. Caching：缓存，比如用户登录后，其用户信息、拥有的角色/权限不必每次去查，这样可以提高效率。
7. Concurrency：shiro 支持多线程应用的并发验证，即如在一个线程中开启另一个线程，能把权限自动传播过去。
8. Testing：提供测试支持。
9. Run As：允许一个用户假装为另一个用户（如果他们允许）的身份进行访问。
10. Remember Me：记住我，这个是非常常见的功能，即一次登录后，下次再来的话不用登录了。

### 2.2 运行原理

Shiro 运行原理图1（应用程序角度）如下：

[![](http://images.extlight.com/shiro-02.png)](http://images.extlight.com/shiro-02.png)

1. Subject：主体，代表了当前“用户”。这个用户不一定是一个具体的人，与当前应用交互的任何东西都是 Subject，如网络爬虫，机器人等。所有 Subject 都绑定到 SecurityManager，与 Subject 的所有交互都会委托给 SecurityManager。我们可以把 Subject 认为是一个门面，SecurityManager 才是实际的执行者。
2. SecurityManager：安全管理器。即所有与安全有关的操作都会与 SecurityManager 交互，且它管理着所有 Subject。可以看出它是 Shiro 的核心，它负责与后边介绍的其他组件进行交互，如果学习过 SpringMVC，我们可以把它看成 DispatcherServlet 前端控制器。
3. Realm：域。Shiro 从 Realm 获取安全数据（如用户、角色、权限），就是说 SecurityManager 要验证用户身份，那么它需要从 Realm 获取相应的用户进行比较以确定用户身份是否合法，也需要从 Realm 得到用户相应的角色/权限进行验证用户是否能进行操作。我们可以把 Realm 看成 DataSource，即安全数据源。

Shiro 运行原理图2（Shiro 内部架构角度）如下：

[![](http://images.extlight.com/shiro-03.png)](http://images.extlight.com/shiro-03.png)

1. Subject：主体，可以看到主体可以是任何与应用交互的“用户”。
2. SecurityManager：相当于 SpringMVC 中的 DispatcherServlet 或者 Struts2 中的 FilterDispatcher。它是 Shiro 的核心，所有具体的交互都通过 SecurityManager 进行控制。它管理着所有 Subject、且负责进行认证和授权、及会话、缓存的管理。
3. Authenticator：认证器，负责主体认证的，这是一个扩展点，如果用户觉得 Shiro 默认的不好，我们可以自定义实现。其需要认证策略（Authentication Strategy），即什么情况下算用户认证通过了。
4. Authrizer：授权器，或者访问控制器。它用来决定主体是否有权限进行相应的操作，即控制着用户能访问应用中的哪些功能。
5. Realm：可以有1个或多个 Realm，可以认为是安全实体数据源，即用于获取安全实体的。它可以是 JDBC 实现，也可以是 LDAP 实现，或者内存实现等。
6. SessionManager：如果写过 Servlet 就应该知道 Session 的概念，Session 需要有人去管理它的生命周期，这个组件就是 SessionManager。而 Shiro 并不仅仅可以用在 Web 环境，也可以用在如普通的 JavaSE 环境。
7. SessionDAO：DAO 大家都用过，数据访问对象，用于会话的 CRUD。我们可以自定义 SessionDAO 的实现，控制 session 存储的位置。如通过 JDBC 写到数据库或通过 jedis 写入 redis 中。另外 SessionDAO 中可以使用 Cache 进行缓存，以提高性能。
8. CacheManager：缓存管理器。它来管理如用户、角色、权限等的缓存的。因为这些数据基本上很少去改变，放到缓存中后可以提高访问的性能。
9. Cryptography：密码模块，Shiro 提高了一些常见的加密组件用于如密码加密/解密的。

### 2.3 过滤器

当 Shiro 被运用到 web 项目时，Shiro 会自动创建一些默认的过滤器对客户端请求进行过滤。以下是 Shiro 提供的过滤器：

| 过滤器简称             | 对应的 JAVA 类                               |
| ----------------- | ---------------------------------------- |
| anon              | org.apache.shiro.web.filter.authc.AnonymousFilter |
| authc             | org.apache.shiro.web.filter.authc.FormAuthenticationFilter |
| authcBasic        | org.apache.shiro.web.filter.authc.BasicHttpAuthenticationFilter |
| perms             | org.apache.shiro.web.filter.authz.PermissionsAuthorizationFilter |
| port              | org.apache.shiro.web.filter.authz.PortFilter |
| rest              | org.apache.shiro.web.filter.authz.HttpMethodPermissionFilter |
| roles             | org.apache.shiro.web.filter.authz.RolesAuthorizationFilter |
| ssl               | org.apache.shiro.web.filter.authz.SslFilter |
| user              | org.apache.shiro.web.filter.authc.UserFilter |
| logout            | org.apache.shiro.web.filter.authc.LogoutFilter |
| noSessionCreation | org.apache.shiro.web.filter.session.NoSessionCreationFilter |

解释：

```
/admins/**=anon               # 表示该 uri 可以匿名访问
/admins/**=auth               # 表示该 uri 需要认证才能访问
/admins/**=authcBasic         # 表示该 uri 需要 httpBasic 认证
/admins/**=perms[user:add:*]  # 表示该 uri 需要认证用户拥有 user:add:* 权限才能访问
/admins/**=port[8081]         # 表示该 uri 需要使用 8081 端口
/admins/**=rest[user]         # 相当于 /admins/**=perms[user:method]，其中，method 表示  get、post、delete 等
/admins/**=roles[admin]       # 表示该 uri 需要认证用户拥有 admin 角色才能访问
/admins/**=ssl                # 表示该 uri 需要使用 https 协议
/admins/**=user               # 表示该 uri 需要认证或通过记住我认证才能访问
/logout=logout                # 表示注销,可以当作固定配置

```

**注意：**

anon，authcBasic，auchc，user 是认证过滤器。

perms，roles，ssl，rest，port 是授权过滤器。

## 三、基础入门

### 3.1 添加依赖

```
<dependency>
    <groupId>commons-logging</groupId>
    <artifactId>commons-logging</artifactId>
    <version>1.1.3</version>
</dependency>

<dependency>
    <groupId>org.apache.shiro</groupId>
    <artifactId>shiro-core</artifactId>
    <version>1.4.0</version>
</dependency>

```

### 3.2 配置文件

在 src/main/resources 目录下创建名为 shiro.ini 的配置文件，内容如下：

```
[users]
# admin=admin 分别表示账号和密码，administrator 表示逗号前边的账号拥有 administrator 这个角色。
admin=admin,administrator
zhangsan=zhangsan,manager
lisi=lisi,guest

[roles]
# administrator 表示角色名称，* 表示这个角色拥有所有权限
administrator=*
manager=user:*,department:*
guest=user:query,department:query

```

其中，每个用户可以拥有多个角色，通过逗号分隔。每个角色可以拥有多个权限，同样通过逗号分隔。

### 3.3 编码

```
public class ShiroTest {

    @Test
    public void test() {

        // 读取 shiro.ini 文件内容
        Factory<SecurityManager> factory = new IniSecurityManagerFactory("classpath:shiro.ini");
        SecurityManager securityManager = factory.getInstance();
        SecurityUtils.setSecurityManager(securityManager);

        Subject currentUser = SecurityUtils.getSubject();

        Session session = currentUser.getSession();
        session.setAttribute("someKey", "aValue");
        String value = (String) session.getAttribute("someKey");
        if (value.equals("aValue")) {
            System.out.println("someKey 的值：" + value);
        }

        // 登陆
        UsernamePasswordToken token = new UsernamePasswordToken("zhangsan", "zhangsan");
        token.setRememberMe(true);
        try {
            currentUser.login(token);
        } catch (UnknownAccountException uae) {
            System.out.println("用户名不存在:" + token.getPrincipal());
        } catch (IncorrectCredentialsException ice) {
            System.out.println("账户密码 " + token.getPrincipal()  + " 不正确!");
        } catch (LockedAccountException lae) {
            System.out.println("用户名 " + token.getPrincipal() + " 被锁定 !");
        }
        
        // 认证成功后
        if (currentUser.isAuthenticated()) {
            
            System.out.println("用户 " + currentUser.getPrincipal() + " 登陆成功！");
            
            //测试角色
            System.out.println("是否拥有 manager 角色：" + currentUser.hasRole("manager"));
            
            //测试权限
            System.out.println("是否拥有 user:create 权限" + currentUser.isPermitted("user:create"));
            
            //退出
            currentUser.logout();
        }

    }
}

```

打印结果：

```
someKey 的值：aValue
用户 zhangsan 登陆成功！
是否拥有 manager 角色：true
是否拥有 user:create 权限true

```

在项目的**后端代码**中，我们可以通过 Subject 对象检测出当前登录用户的认证状态和授权信息，以下是 Subject 对象认证和授权相关的方法列表：

[![](http://images.extlight.com/shiro-05.jpg)](http://images.extlight.com/shiro-05.jpg)

如果开发者不想使用代码进行用户进行授权校验等操作，那么可以使用注解代替。

在使用 Shiro 的注解之前，请确保项目中已经添加支持 AOP 功能的相关 jar 包。常用注解如下：

```
@RequiresRoles( "manager" )      # 角色校验
public String save() {
    //...
}
@RequiresPermissions("user:manage")  # 权限检验
public String delete() {
    //...
}

```

当客户端发送请求后，系统会使用 AOP 生成请求目标的代理类来解析注解，然后判断当前请求的用户是否拥有权限访问。

在项目的**前端代码**中，如果使用的是 JSP 模板，我们就可以使用 Shiro 提供的标签对页面元素的展示进行控制。

例如：

```
<%@ taglib prefix="shiro" uri=http://shiro.apache.org/tags %>
<html>
<body>
    <shiro:hasPermission name="user:manage">
        <a href="manageUsers.jsp">
            点击进入管理界面
        </a>
    </shiro:hasPermission>
    <shiro:lacksPermission name="user:manage">
        没有管理权限
    </shiro:lacksPermission>
</body>
</html>

```

其中，user:manage 对应 shiro.ini 文件中[roles]下边的设置。

## 四、自定义 Realm

上边的程序使用的是 Shiro 自带的 IniRealm，而 IniRealm 从 ini 配置文件中读取用户的信息，**大部分情况下需要从系统的数据库中读取用户信息，所以需要自定义 realm**。

Shiro 为我们提供了如下 Realm:

[![](http://images.extlight.com/shiro-04.jpg)](http://images.extlight.com/shiro-04.jpg)

其中，最基础的是 Realm 接口，CachingRealm 负责缓存处理，AuthenticationRealm 负责认证，AuthorizingRealm负责授权，通常自定义的 realm 继承 **AuthorizingRealm**。

自定义 Realm：

```
public class CustomRealm extends AuthorizingRealm {

    /**
     * 认证
     */
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        // 从 token 中获取用户身份信息
        String username = (String) token.getPrincipal();
        // 通过 username 从数据库中查询
    
        // 如果查询不到则返回 null
        if(!username.equals("zhangsan")){//这里模拟查询不到
            return null;
        }
        
        //获取从数据库查询出来的用户密码 
        String dbPassword = "zhangsan";//这里使用静态数据模拟
        
        //返回认证信息由父类 AuthenticatingRealm 进行认证
        SimpleAuthenticationInfo simpleAuthenticationInfo = new SimpleAuthenticationInfo(username, dbPassword, getName());

        return simpleAuthenticationInfo;
    }
    
    /**
     * 授权
     */
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        // 获取身份信息
        String username = (String) principals.getPrimaryPrincipal();
        // 根据身份信息从数据库中查询权限数据
        // 这里使用静态数据模拟
        List<String> permissions = new ArrayList<String>();
        permissions.add("user:*");
        permissions.add("department:*");
        
        // 将权限信息封闭为AuthorizationInfo
        SimpleAuthorizationInfo simpleAuthorizationInfo = new SimpleAuthorizationInfo();
        // 模拟数据，添加 manager 角色
        simpleAuthorizationInfo.addRole("manager");
        
        for(String permission:permissions){
            simpleAuthorizationInfo.addStringPermission(permission);
        }
        
        return simpleAuthorizationInfo;
    }

}

```

在 src/main/resources 目录下创建 shiro-realm.ini 文件，内容如下：

```
[main]
#自定义 realm
customRealm=com.light.shiroTest.realm.CustomRealm
#将realm设置到securityManager
securityManager.realms=$customRealm

```

将测试类中，shiro.ini 改成 shiro-realm.ini 后执行，结果如下：

```
someKey 的值：aValue
用户 zhangsan 登陆成功！
是否拥有 manager 角色：true
是否拥有 user:create 权限true

```

## 五、与 Spring 整合

由于项目设计思路不同，整合 Shiro 框架时的设置也会有所不同，因此下边只列出部分通用的代码。

### 5.1 添加依赖

```
<dependency>
    <groupId>org.apache.shiro</groupId>
    <artifactId>shiro-core</artifactId>
    <version>1.4.0</version>
</dependency>
<dependency>
    <groupId>org.apache.shiro</groupId>
    <artifactId>shiro-spring</artifactId>
    <version>1.4.0</version>
</dependency>

```

### 5.2 配置文件

web.xml ：

```
<filter>
    <filter-name>shiroFilter</filter-name>
    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
    <init-param>
        <param-name>targetFilterLifecycle</param-name>
        <param-value>true</param-value>
    </init-param>
</filter>

<filter-mapping>
    <filter-name>shiroFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>

```

application-shiro.xml：

```
<bean id="shiroFilter" class="org.apache.shiro.spring.web.ShiroFilterFactoryBean">
    <!-- 必须设置 -->
    <property name="securityManager" ref="securityManager"/>
    <!-- 3 个 url 属性为可选设置 -->
    <property name="loginUrl" value="/login.jsp"/>
    <property name="successUrl" value="/home.jsp"/>
    <property name="unauthorizedUrl" value="/unauthorized.jsp"/>
    <property name="filterChainDefinitions">
        <value>
            <!-- 对静态资源设置匿名访问 -->
            /resources/** = anon
            /login = anon
            <!-- /** = authc 所有url都必须认证通过才可以访问-->
            /** = authc
        </value>
    </property>
</bean>

<!-- 安全管理器 -->
<bean id="securityManager" class="org.apache.shiro.web.mgt.DefaultWebSecurityManager">
    <property name="realm" ref="customRealm" />
</bean>

<!-- 自定义 realm -->
<bean id="customRealm" class="com.light.ac.web.realm.CustomRealm"></bean>

<!-- 保证实现了Shiro内部lifecycle函数的bean执行 -->
<bean id="lifecycleBeanPostProcessor" class="org.apache.shiro.spring.LifecycleBeanPostProcessor" />

```

其中，application-shiro.xml 中的 shiroFilter 名字和 web.xml 文件中的 shiroFilter是对应的，必须一致。

anon 和 authc 对应上文提到的过滤器。

CustomRealm 类：

```
public class CustomRealm extends AuthorizingRealm {

    @Autowired
    private UserService userService;
    
    @Autowired
    private PermissionService permissionService;
    
    /**
     * 认证
     */
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        // 获取用户名
        String userName = (String) token.getPrincipal();
        // 通过用户名获取用户对象
        User user = this.userService.findUserByUserName(userName);
        
        if (user == null) {
            return null;
        }
        
        // 通过 userId 获取该用户拥有的所有权限，返回值根据自己需求编写，并非固定值。
        Map<String,List<Permission>> permissionMap = this.permissionService.getPermissionMapByUserId(user.getId());
        
        // （目录+菜单，分层级，用于前端 jsp 遍历）
        user.setMenuList(permissionMap.get("menuList"));
        // （目录+菜单+按钮，用于后端权限判断）
        user.setPermissionList(permissionMap.get("permissionList"));
        
        SimpleAuthenticationInfo info = new SimpleAuthenticationInfo(user,user.getPassword(),this.getName());
        
        return info;
    }
    
    /**
     * 授权
     */
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        User user = (User) principals.getPrimaryPrincipal();
        
        SimpleAuthorizationInfo info = new SimpleAuthorizationInfo();
        
        // （目录+菜单+按钮，用于后端权限判断）
        List<Permission> permissionList = user.getPermissionList();
        
        for (Permission permission : permissionList) {
            if (StringUtil.isNotEmpty(permission.getCode())) {
                info.addStringPermission(permission.getCode());
            }
        }
        
        return info;
    }
}
```

具体代码可以根据下文提供的源码地址进行下载查看。

## 六、源码下载

[authority-control-shiro]

## 七、参考资料

- [http://shiro.apache.org/documentation.html](http://shiro.apache.org/documentation.html) 官方文档


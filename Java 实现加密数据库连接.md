# Java 实现加密数据库连接



## 一、前言

在很多项目中，数据库相关的配置文件内容都是以明文的形式展示的，这存在一定的安全隐患。

在开发和维护项目时，不仅要关注项目的性能，同时也要注重其安全性。

## 二、实现思路

我们都知道项目启动时，Spring 容器会加载配置文件并读取文件中的内容，那么我们可以下边步骤操作：

1. 通过 DES 算法加密连接数据库的账号和密码并将加密后的密文写到 db 配置文件中。
2. 在 Spring 读取 db 配置文件时将密文解密回明文。

## 三、实现编码

### 3.1 加密工具类

DESUtil 类：

```
public class DESUtil {

    private static Key key;
    private static String KEY_STR = "myKey";
    private static String CHARSETNAME = "UTF-8";
    private static String ALGORITHM = "DES";

    static {
        try {
            KeyGenerator generator = KeyGenerator.getInstance(ALGORITHM);
            SecureRandom secureRandom = SecureRandom.getInstance("SHA1PRNG");
            secureRandom.setSeed(KEY_STR.getBytes());
            generator.init(secureRandom);
            key = generator.generateKey();
            generator = null;
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    /**
     * 加密
     * @param str
     * @return
     */
    public static String getEncryptString(String str) {
        BASE64Encoder base64encoder = new BASE64Encoder();
        try {
            byte[] bytes = str.getBytes(CHARSETNAME);
            Cipher cipher = Cipher.getInstance(ALGORITHM);
            cipher.init(Cipher.ENCRYPT_MODE, key);
            byte[] doFinal = cipher.doFinal(bytes);
            return base64encoder.encode(doFinal);
        } catch (Exception e) {
            // TODO: handle exception
            throw new RuntimeException(e);
        }
    }

    /**
     * 解密
     * @param str
     * @return
     */
    public static String getDecryptString(String str) {
        BASE64Decoder base64decoder = new BASE64Decoder();
        try {
            byte[] bytes = base64decoder.decodeBuffer(str);
            Cipher cipher = Cipher.getInstance(ALGORITHM);
            cipher.init(Cipher.DECRYPT_MODE, key);
            byte[] doFinal = cipher.doFinal(bytes);
            return new String(doFinal, CHARSETNAME);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}

```

通过上边的工具类对连接数据库的账号密码进行加密。笔者主机上连接数据库的账号和密码分别是 “root” 和 “tiger”。

经过加密后得到 “WnplV/ietfQ=” 和 “xyHEykQVHqA=” 。

db.properties 配置文件完整内容如下：

```
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://127.0.0.1:3306/test?characterEncoding=utf-8&allowMultiQueries=true&serverTimezone=UTC
jdbc.username=WnplV/ietfQ=
jdbc.password=xyHEykQVHqA=

```

### 3.2 配置文件解析类

EncryptPropertyPlaceholderConfigurer 类：

```
public class EncryptPropertyPlaceholderConfigurer extends PropertyPlaceholderConfigurer {
    // 需要解密的字段
    private String[] encryptPropNames = { "jdbc.username", "jdbc.password" };

    @Override
    protected String convertProperty(String propertyName, String propertyValue) {
        if (isEncryptProp(propertyName)) {
            // 解密
            String decryptValue = DESUtil.getDecryptString(propertyValue);
            return decryptValue;
        } else {
            return propertyValue;
        }
    }

    private boolean isEncryptProp(String propertyName) {
        for (String encryptpropertyName : encryptPropNames) {
            if (encryptpropertyName.equals(propertyName))
                return true;
        }
        return false;
    }
}

```

### 3.3 Spring 配置文件

applicationContext-mybatis.xml 部分内容：

```
<!-- <context:property-placeholder location="classpath:*.properties"/> -->
    
<bean class="com.light.ac.common.configuration.EncryptPropertyPlaceholderConfigurer">
    <property name="locations">
        <list>
            <value>classpath:db.properties</value>
        </list>
    </property>
    <property name="fileEncoding" value="UTF-8"/>
</bean>

```

未加密明文前，使用的是 <context:property-placeholder /> 加载 db 配置文件。

加密明文后，使用配置文件解析类加载 db 配置文件。

完成上述 3 个步骤后按照往常操作，直接运行项目即可。

## 四、总结

起初，在不了解实现思路前觉得这功能很神秘和高大尚。但是，理清思路后功能实现起来就非常简单了。

作为程序员不能被神秘的表象惊叹而“望而却步”，需要学会思考和理清思路，这样才能不断提升自身能力。


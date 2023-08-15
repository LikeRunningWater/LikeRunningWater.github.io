### IOC控制反转-----把对象交给spring容器去管理



![image-20230814113137081](https://github.com/LikeRunningWater/LikeRunningWater.github.io/tree/main/%24%7Bimg%7D\image-20230814113137081.png)



<font color="red">注意：</font>

快捷键：ctrl + shift +t  快速查找类--eclipse   

​				ctrl+n ---idea



### spring容器 常用方法

applicationContext .getBean(""); 获取object对象

applicationContext .getBean(<T>.class)  //获取指定类型对象

<font color="blue" size="4">默认对象是单例的,只要获取同一个对象,创建的对象的地址值一致，可以在配置文件使用scope=</font>

getDefintionNames()  获取对象名返回String数组<font color="blue" size="4">返回String数组

getDefintionint() 获取定义的对象数量返回int



### spring简单配置

1.导入需要的jar包  

结构如图：

![image-20230814210858947](C:\Users\Administrator\Desktop\笔记\学习笔记\${img}\image-20230814210858947.png)

2.编写spring 核心配置文件

### spring 核心配置文件

applicationContext.xml   位置->放在src下

:factory:配置的一些标签：  bean

```tex
<bean> 定义要管理的对象  属性=name 命名 不能重复  属性 =calss 对象全限定类名  属性= id 不能重复
id和name属性区别不大，都是唯一标识
scope属性 作用域  定义对象模式---单例或者多例模式
factory-method 属性  --指定工厂方法去创建对象
```



主配置文件引入其他xml配置文件

标签：import

```java
<import resource="路径1">
<import resource="路径2">
```





## spring 注入（DI）--方式





#### 1.set方式注入

setter方法注入

```java
配置文件使用<properties>属性   ---注入属性
<properties>内 value->ref 属性---注入对象
```

#### 2.构造器注入

```java
 <!--构造注入-->
<bean id="u3" class="com.mylifes1110.bean.Student">
    <!-- 除标签名称有变化,其他均和Set注入一致 -->
    <constructor-arg name="id" value="1234" /> 
    <constructor-arg name="name" value="tom" />
    <constructor-arg name="age" value="20" />
    <constructor-arg name="sex" value="male" />
    
</bean>
```



注入指的是注入到ioc容器里，因为需要它来管理对象

注入什么？需要的对象、资源等等。。

## 注入类型

![image-20230814161138935](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230814161138935.png)





<font color="red" >set方式注入：</font>

```java
<!--前提实现ioc 创建对象-->
    <bean  name="user" class="com.gec.pojo.User">
        <!--DI  set方式依赖注入-->
        <property name="id" value="0001"></property>
        <property name="userName" value="张三"></property>
        <property name="password" value="12346"></property>
    </bean>
```



<font color="orange">各种类型数据的注入</font>

```java
<bean id="User" class="com.mylifes1110.bean.User">
    <!--注入基本数据类型-->
    <property name="id" value="1"/>
    <property name="password" value="123456"/>
    <property name="sex" value="male"/>
    <property name="age" value="18"/>
    <!--注入日期类型-->
    <property name="bornDate" value="1999/09/09"/>
    <!--注入数组类型-->
    <property name="hobbys">
        <array>
            <value>Run</value>
            <value>Jump</value>
            <value>Climb</value>
        </array>
    </property>
    <!--注入List集合类型-->
    <property name="names">
        <list>
            <value>Ziph</value>
            <value>Join</value>
            <value>Marry</value>
        </list>
    </property>
    <!--注入Set集合类型-->
    <property name="phones">
        <set>
            <value>110</value>
            <value>119</value>
            <value>120</value>
        </set>
    </property>
    <!--注入Properties类型-->
    <property name="files">
        <props>
            <prop key="first">One</prop>
            <prop key="second">Two</prop>
            <prop key="third">Three</prop>
        </props>
    </property>
    <!--注入Map集合类型-->
    <property name="countries">
        <map>
            <entry key="CHINA" value="中国"/>
            <entry key="USA" value="美国"/>
            <entry key="UK" value="英国"/>
        </map>
    </property>
</bean>
```

<font color ="red">注意：</font>

数组类型注入标准如上，也可以这么写：

```java
 <property name="names" value="aaa,bbb,ccc">
```

但是不标准,不规范---不建议使用



<font color="blue">练习：</font>

使用spring的ioc和DI完成三层架构的创建注入

​         将Dao层实现类对象注入到Service层并调用方法得以测试





<font color ="orange">配置文件：</font>

```java

    <!--三层架构案例-->
    <bean name="userService" class="com.gec.service.UserServiceImpl">
    <property name="userDao" ref="userdao" />
    </bean>

    <bean id="userdao" class="com.gec.dao.UserDaoImpl"/>
```



<font color ="orange">UserServiceImpl:</font>

 ```java
     private UserDao userDao ;
 
     public UserDao getUserDao() {
         return userDao;
     }
     public void setUserDao(UserDao userDao) {
         this.userDao=userDao;
     }
 ```

<font color ="orange">测试</font>

```java
 
 ApplicationContext applicationContext =
                new ClassPathXmlApplicationContext("applicationContext.xml");

 UserService userService = (UserService) applicationContext.getBean("userService");

 UserService userService1=applicationContext.getBean(UserServiceImpl.class);

 System.out.println(userService);
```



#### 8.14.2023  

#### <font color = "red">Spring内容总结</font> 



spring解决了什么问题？

解决的是业务逻辑层和其他各层的松耦合问题

 

#### Spring模拟   使用反射创建对象

1. 编写property文件---里面是类名的键值对

2. 创建BeanFatory类，通过classloader	将源文件转化为inputStream流

3. 通过property.load加载流,创建一个动态生成对象的方法,参数为配置源文件的key返回string字符串

4. 通过反射newInstance()创建对应的对象  最后返回一个object对象

   

property文件

```java
userDao=com.gec.dao.UserDaoImpl
userService=com.gec.service.UserServiceImpl
```



BeanFatory代码

```java
package com.gec.bean;


// bean   -> spring 称呼   一个 bean 其实 就是javabean  其实就是之前你们创建的 一个个java对象 User  product

import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.util.Properties;

// 专门去造 对象的
public class BeanFactory {

    private static  Properties properties =null;
    static {

        // 绝对路径
     //   InputStream inputStream = new FileInputStream("D:\\gz2316\\day_0814(spring)\\code\\01-反射模拟spring创建对象\\src\\applicationContext.properties");

        //1.  加载  applicationContext.properties

        // 反射方式加载
        ClassLoader classLoader = BeanFactory.class.getClassLoader();

        //2. 从配置文件 得到 inputstream 流
        InputStream inputStream = classLoader.getResourceAsStream("applicationContext.properties");

         properties = new Properties();
        try {

            //3.讲 inputstream 流 转为 properties 对象
            properties.load(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    // 创建对象
    public static Object createObj(String beanName){

        // com.gec.dao.UserDaoImpl

        //4. 根据 properties 对象 中 的key  获取到 值
        String str = properties.getProperty(beanName);
        Object obj = null;
        try {
            //5. 通过反射 去根据 类的全路径字符串 创建对象
             obj = Class.forName(str).newInstance();

        } catch (Exception e) {
            e.printStackTrace();
        }
        //6. 返回反射创建的对象
        return obj;

    }


}
```

 

#### Spring基本配置



1.导入四个基本包+logging  准备bean对象

2.配置xml文件  -----配置bean类到容器

通过以下代码加载配置

测试代码：

```java
public class DemoTest1 {

    public static void main(String[] args) {

        //1. 加载  applicationContext.xml 得到 一个spring 容器

        ApplicationContext applicationContext =
        new ClassPathXmlApplicationContext("applicationContext.xml");

        //2.  从容器中要对象   根据名称

        User user = (User) applicationContext.getBean("user");
        System.out.println(user);

    }

}
```



1.List集合
2.Map集合
3.JVM调优






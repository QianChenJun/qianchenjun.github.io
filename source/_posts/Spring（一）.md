---
title: Spring（一）
author: 小小千辰
tags:
  - Java
  - Spring
  - 框架
categories:
  - 千辰的小小笔记
abbrlink: 1d2ccb18
date: 2021-11-01 17:49:26
updated: 2021-11-03 17:49:26
---

## **Spring的IoC和DI**

### **Spring的简介**

> Spring是分层的 Java SE/EE应用 full-stack 轻量级开源框架，以 **IoC**（Inverse Of Control：反转控制）和**AOP**（Aspect Oriented Programming：面向切面编程）为内核。

<!-- more -->

#### **Spring的体系结构**

![image-20211101175724885](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241811148.png)



<br>

### **Spring快速入门**

#### **Spring程序开发步骤**

1. 导入 Spring 开发的基本包坐标
2. 编写 Dao 接口和实现类
3. 创建 Spring 核心配置文件
4. 在 Spring 配置文件中配置 UserDaoImpl
5. 使用 Spring 的 API 获得 Bean 实例

<br>

####  **导入Spring开发的基本包坐标**

```xml
<dependency>
	<version>5.0.5.RELEASE</version>
</dependency>

<dependencies>
	<!--导入spring的context坐标，context依赖core、beans、expression--> 
    <dependency> 
        <groupId>org.springframework</groupId> 
    	<artifactId>spring-context</artifactId> 
        <version>${spring.version}</version>
    </dependency>
</dependencies>
```

<br>

#### **编写Dao接口和实现类**

```java
public interface UserDao {
    void save();
}

public class UserDaoImpl implements UserDao {
    @Override
    public void save() {
        System.out.println("save running...");
    }
}
```

<br>

#### **创建Spring核心配置文件**

在类路径下（resources）创建applicationContext.xml配置文件

![image-20211101180539103](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241811855.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 			
       http://www.springframework.org/schema/beans/spring-beans.xsd">
</beans>
```

<br>

#### **在Spring配置文件中配置UserDaoImpl**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 			
       http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <bean id="userDao" class="com.hifi.dao.impl.UserDaoImpl"></bean>
</beans>
```

<br>

#### **使用Spring的API获得Bean实例**

```java
public class SpringTest {

    @Test
    public void test1() {
        // 获取客户端对象（参数放入文件名）
        ClassPathXmlApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
        // 参数传入id值
        UserDao userDao = (UserDao) app.getBean("userDao");

        System.out.println(userDao);
    }
}
```

![image-20211101181107412](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241811745.png)

#### **知识要点**

**Spring开发步骤**

1. 导入坐标
2. 创建Bean
3. 创建applicationContext.xml
4. 在配置文件中进行配置
5. 创建ApplicationContext对象getBean



<br>

### **Spring配置文件**

#### **Bean标签的基本配置**

用于配置对象交由 **Spring** 来创建。

默认情况下它调用的是类中的**无参构造函数**，如果没有无参构造函数则不能创建成功

基本属性

* **id**：Bean实例在Spring容器中的唯一标识
* **class**：Bean全限定名称

<br>

#### **Bean标签范围配置**

scope：指对象的作用范围，取值如下：

|    取值范围    |                             说明                             |
| :------------: | :----------------------------------------------------------: |
| **singleton**  |                      **默认值，单例的**                      |
| **prototype**  |                          **多例的**                          |
|    request     | WEB 项目中，Spring 创建一个 Bean 的对象，将对象存入到 request 域中 |
|    session     | WEB 项目中，Spring 创建一个 Bean 的对象，将对象存入到 session 域中 |
| global session | WEB 项目中，应用在 Portlet 环境，如果没有 Portlet 环境那么globalSession 相当 |

<br>

#### **Bean声明周期配置**

* **init-method**：指定类中的初始化方法名称
* **destroy-method**：指定类中的销毁方法



写在指定类的标签里面

![image-20211101182950121](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241811428.png)

<br>

#### **Bean实例化的三种方式**

1. **使用无参构造方法实现（用的多）**

   它会根据默认无参构造方法来创建类对象，如果bean中没有默认无参构造函数，将会创建失败

   ```xml
   <bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl"/>
   ```

2. **工厂静态方法实例化**

   工厂的静态方法返回Bean实例

   ```java
   public class StaticFactoryBean {
   	public static UserDao createUserDao(){
   		return new UserDaoImpl();
   	} 
   }
   ```

   ```xml
   <bean id="userDao" class="com.itheima.factory.StaticFactoryBean" factory-method="createUserDao" />
   ```

3. **工厂实例方法实例化**

   工厂的非静态方法返回Bean实例

   ```java
   public class DynamicFactoryBean {
   	public UserDao createUserDao(){
   		return new UserDaoImpl();
   	} 
   }
   ```

   ```xml
   <bean id="factoryBean" class="com.itheima.factory.DynamicFactoryBean"/> 
   <bean id="userDao" factory-bean="factoryBean" factory-method="createUserDao"/>
   ```

<br>

#### **Bean的依赖注入**

1. 创建 UserService，UserService 内部在调用 UserDao的save() 方法

   ```java
   public class UserServiceImpl implements UserService {
       @Override
       public void save() {
       	ApplicationContext applicationContext = new 
       		ClassPathXmlApplicationContext("applicationContext.xml");
           UserDao userDao = (UserDao) applicationContext.getBean("userDao");
           userDao.save();
   	} 
   }
   ```

2. 将 UserServiceImpl 的创建权交给 Spring

   ```xml
   <bean id="userService" class="com.itheima.service.impl.UserServiceImpl"/>
   ```

3. 从 Spring 容器中获得 UserService 进行操作

   ```java
   ApplicationContext applicationContext = new 
   ClassPathXmlApplicationContext("applicationContext.xml");
   UserService userService = (UserService)applicationContext.getBean("userService");
   userService.save();
   ```

<br>

#### **Bean的依赖注入分析**

目前UserService实例和UserDao实例都存在与Spring容器中，当前的做法是在容器外部获得UserService实例和UserDao实例，然后在程序中进行结合。

![image-20211101183752724](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241811415.png)

因为UserService和UserDao都在Spring容器中，而最终程序直接使用的是UserService，所以可以在Spring容器中，将UserDao设置到UserService内部。

![image-20211101183821017](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241811288.png)

<br>

#### **Bean的依赖注入概念**

依赖注入（**Dependency Injection**）：它是 Spring 框架核心 IOC 的具体实现。

在编写程序时，通过控制反转，把对象的创建交给了 Spring，但是代码中不可能出现没有依赖的情况。

IOC 解耦只是降低他们的依赖关系，但不会消除。例如：业务层仍会调用持久层的方法。

那这种业务层和持久层的依赖关系，在使用 Spring 之后，就让 Spring 来维护了。

简单的说，就是坐等框架把持久层对象传入业务层，而不用我们自己去获取。

<br>

#### **Bean的依赖注入方式**

1. set方法注入

   在UserServiceImpl中添加setUserDao方法

   ```java
   public class UserServiceImpl implements UserService {
       private UserDao userDao;
       public void setUserDao(UserDao userDao) {
       	this.userDao = userDao;
       }
       @Override
       public void save() {
       	userDao.save();
   	} 
   }
   ```

   配置Spring容器调用set方法进行注入

   ```xml
   <bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl"/>
   
   <bean id="userService" class="com.itheima.service.impl.UserServiceImpl">
   	<property name="userDao" ref="userDao"/>
   </bean>
   ```

2. P命名空间注入

   P命名空间注入本质也是set方法注入，但比起上述的set方法注入更加方便，主要体现在配置文件中，如下：

   首先，需要引入P命名空间：

   ```xml
   xmlns:p="http://www.springframework.org/schema/p"
   ```

   其次，需要修改注入方式:

   ```xml
   <bean id="userService" class="com.itheima.service.impl.UserServiceImpl" p:userDaoref="userDao"/>
   ```

3. 构造方法注入

   创建有参构造

   ```java
   public class UserServiceImpl implements UserService {
       @Override
       public void save() {
           ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
           UserDao userDao = (UserDao) applicationContext.getBean("userDao");
           userDao.save();
   	} 
   }
   ```

   配置Spring容器调用有参构造时进行注入

   ```xml
   <bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl"/>
   <bean id="userService" class="com.itheima.service.impl.UserServiceImpl"> 
   	<constructor-arg name="userDao" ref="userDao"></constructor-arg>
   </bean>
   ```

<br>

#### **Bean的依赖注入的数据类型**

上面的操作，都是注入的引用Bean，除了对象的引用可以注入，普通数据类型，集合等都可以在容器中进行注入。

注入数据的三种数据类型

* **普通数据类型**
* **引用数据类型**
* **集合数据类型**

##### **普通数据类型的注入**

```java
public class UserDaoImpl implements UserDao {
    private String company;
    private int age;
    public void setCompany(String company) {
    	this.company = company;
    }
    public void setAge(int age) {
    	this.age = age;
    }
    public void save() {
        System.out.println(company+"==="+age);
        System.out.println("UserDao save method running....");
	} 
}
```

```xml
<bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl"> 
    <property name="company" value="腾讯公司"></property> 
    <property name="age" value="15"></property>
</bean>
```

##### **集合数据类型（List<String>）的注入**

```java
public class UserDaoImpl implements UserDao {
    private List<String> strList;
    public void setStrList(List<String> strList) {
    	this.strList = strList;
    }
    public void save() {
        System.out.println(strList);
        System.out.println("UserDao save method running....");
	}
}
```

```xml
<bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl"> 
	<property name="strList"> 
		<list>
			<value>aaa</value> 
			<value>bbb</value> 
			<value>ccc</value>
        </list>
    </property>
</bean>
```

##### **集合数据类型（ Map<String,User> ）的注入**

```java
public class UserDaoImpl implements UserDao {
    private Map<String,User> userMap;
    public void setUserMap(Map<String, User> userMap) {
    	this.userMap = userMap;
    }
    public void save() {
        System.out.println(userMap);
        System.out.println("UserDao save method running....");
	} 
}
```

```xml
<bean id="u1" class="com.itheima.domain.User"/>
<bean id="u2" class="com.itheima.domain.User"/>
<bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl"> 
	<property name="userMap"> 
		<map>
			<entry key="user1" value-ref="u1"/>
			<entry key="user2" value-ref="u2"/>
        </map>
    </property>
</bean>
```

##### **集合数据类型（Properties）的注入**

```java
public class UserDaoImpl implements UserDao {
    private Properties properties;
    public void setProperties(Properties properties) {
    	this.properties = properties;
    }
    public void save() {
        System.out.println(properties);
        System.out.println("UserDao save method running....");
	} 
}
```

```xml
<bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl"> 
	<property name="properties"> 
		<props> 
			<prop key="p1">aaa</prop> 
			<prop key="p2">bbb</prop> 
			<prop key="p3">ccc</prop>
    	</props>
    </property>
</bean>
```

<br>

#### **引入其他配置文件（分模块开发）**

实际开发中，Spring的配置内容非常多，这就导致Spring配置很繁杂且体积很大，所以，可以将部分配置拆解到其他配置文件中，而在Spring主配置文件通过import标签进行加载

```xml
<import resource="applicationContext-xxx.xml"/>
```

**Spring的重点配置**

```
<bean>标签
    id属性:在容器中Bean实例的唯一标识，不允许重复
    class属性:要实例化的Bean的全限定名
    scope属性:Bean的作用范围，常用是Singleton(默认)和prototype
    <property>标签：属性注入
        name属性：属性名称
        value属性：注入的普通属性值
        ref属性：注入的对象引用值
        <list>标签
        <map>标签
    <properties>标签
    <constructor-arg>标签
<import>标签:导入其他的Spring的分文件
```

`<constructor-arg>`标签中同样包括`<property>`标签包括的所有内容

<br>

### **Spring相关API**

#### **ApplicationContext的继承体系**

**applicationContext：**接口类型，代表应用上下文，可以通过其实例获得 Spring 容器中的 Bean 对象

![image-20211101190145471](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241811525.png)

<br>

#### **ApplicationContext的实现类**

1. **ClassPathXmlApplicationContext**

   它是从类的根路径下加载配置文件 推荐使用这种

2. **FileSystemXmlApplicationContext**

   它是从磁盘路径上加载配置文件，配置文件可以在磁盘的任意位置。

3. **AnnotationConfigApplicationContext**

   当使用注解配置容器对象时，需要使用此类来创建 spring 容器。它用来读取注解。

<br>

#### **getBean()方法使用**

```java
public Object getBean(String name) throws BeansException {
    assertBeanFactoryActive();
    return getBeanFactory().getBean(name);
}

public <T> T getBean(Class<T> requiredType) throws BeansException {
    assertBeanFactoryActive();
    return getBeanFactory().getBean(requiredType);
}
```

其中，当参数的数据类型是字符串时，表示根据Bean的id从容器中获得Bean实例，返回是Object，需要强转。当参数的数据类型是Class类型时，表示根据类型从容器中匹配Bean实例，当容器中相同类型的Bean有多个时，则此方法会报错。

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
UserService userService1 = (UserService)applicationContext.getBean("userService");
UserService userService2 = applicationContext.getBean(UserService.class);
```

<br>

#### **知识要点**

**Spring的重点API**

```java
ApplicationContext app = new ClasspathXmlApplicationContext("xml文件")
app.getBean("id")
app.getBean(Class)
```



<br>

<br>

> 写在最后，呼，终于敲完了**1**，手酸的慌（虽然很多都是复制的），加油！


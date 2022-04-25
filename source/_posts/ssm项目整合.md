---
title: ssm整合
author: 小小千辰
abbrlink: 91794a99
date: 2022-01-13 00:00:00
updated: 2022-01-15 00:00:00
tags:
  - Spring
  - SpringMVC
  - Mybatis
  - 项目
categories:
  - 千辰的小小笔记
---

#  SSM整合

## 一、整体分析

### 1. 功能点

1. 分页
2. 数据校验
   * `jquery`前端校验 + `JSR303`后端校验
3. `ajax`
4. `Rest`风格的`URI`

<br>

### 2. 技术点

* 基础框架-`ssm`
* 数据库-`MySQL`
* 前端框架-`bootstrap`快速搭建简洁美观的界面
* 项目的依赖管理-`Maven`
* 分页-`pagehelper`
* 逆向工程-`MyBatis Generator`

## 二、基础环境搭建

### 1. 引入所需要的基本jar包

> * `spring`
> * `springmvc`
> * `mybatis`
> * `spring-test`
> * `数据库连接池，驱动包`（这里数据库连接池选用了c3p0，我觉得druid可能会更好用）
> * 其他（`jstl，servlet-api, junit`）

```xml    <!--引入项目依赖的jar包 -->
    <!--引入项目依赖的jar包 -->
    <dependencies>

        <!--引入pageHelper分页插件 -->
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper</artifactId>
            <version>5.0.0</version>
        </dependency>

        <!-- MBG（代码生成器） -->
        <!-- https://mvnrepository.com/artifact/org.mybatis.generator/mybatis-generator-core -->
        <dependency>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-core</artifactId>
            <version>1.3.5</version>
        </dependency>

        <!-- 返回json字符串的支持 -->
        <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.8.8</version>
        </dependency>

        <!--JSR303数据校验支持；tomcat7及以上的服务器，
        tomcat7以下的服务器：el表达式。额外给服务器的lib包中替换新的标准的el
        -->
        <!-- https://mvnrepository.com/artifact/org.hibernate/hibernate-validator -->
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-validator</artifactId>
            <version>5.4.1.Final</version>
        </dependency>

        <!-- SpringMVC、Spring -->
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>4.3.7.RELEASE</version>
        </dependency>

        <!-- Spring-Jdbc -->
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>4.3.7.RELEASE</version>
        </dependency>

        <!-- Spring面向切面编程 -->
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-aspects -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
            <version>4.3.7.RELEASE</version>
        </dependency>

        <!--Spring-test -->
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-test -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>4.3.7.RELEASE</version>
        </dependency>


        <!--MyBatis -->
        <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.2</version>
        </dependency>
        <!-- MyBatis整合Spring的适配包 -->
        <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>1.3.1</version>
        </dependency>


        <!-- 数据库连接池、驱动 -->
        <!-- https://mvnrepository.com/artifact/c3p0/c3p0 -->
        <dependency>
            <groupId>c3p0</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.1</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.15</version>
        </dependency>


        <!-- （jstl，servlet-api，junit） -->
        <!-- https://mvnrepository.com/artifact/jstl/jstl -->
        <dependency>
            <groupId>jstl</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.0.1</version>
            <scope>provided</scope>
        </dependency>

        <!-- junit -->
        <!-- https://mvnrepository.com/artifact/junit/junit -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
    </dependencies>
```

> **对于其中出现的部分包，比如spring的相关包可以选择更版本的代替。**

<br>

### 2. 编写ssm整合的关键配置文件

#### （1）web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!-- 1、启动Spring的容器 -->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext.xml</param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

    <!-- 2、springmvc 的前端控制器，拦截所有的请求 -->
    <servlet>
        <servlet-name>DispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springMVC.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>DispatcherServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <!-- 3、字符编码过滤器 -->
    <filter>
        <filter-name>CharacterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
        <init-param>
            <param-name>forceRequestEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>CharacterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

    <!-- 4、使用Rest风格的URI，将页面普通的post请求转为指定的delete或者put请求 -->
    <filter>
        <filter-name>HiddenHttpMethodFilter</filter-name>
        <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>HiddenHttpMethodFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

</web-app>
```

<br>

#### （2）applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!--配置扫描，不扫描controller-->
    <context:component-scan base-package="com.teng">
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>

    <!-- Spring的配置文件，这里主要配置和业务逻辑有关的 -->
    <!--============================数据源============================-->
    <context:property-placeholder location="classpath:jdbc.properties"></context:property-placeholder>
    <bean id="pooledDataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="jdbcUrl" value="${jdbc.jdbcUrl}"></property>
        <property name="driverClass" value="${jdbc.driverClass}"></property>
        <property name="user" value="${jdbc.user}"></property>
        <property name="password" value="${jdbc.password}"></property>
    </bean>

    <!--============================配置Mybatis的整合============================-->
    <bean id="SqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!--指定mybatis的全局配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"></property>
        <property name="dataSource" ref="pooledDataSource"></property>
        <!--指定mybatis，mapper文件的位置-->
        <property name="mapperLocations" value="classpath:mapper/*.xml"></property>
    </bean>

    <!--配置扫描器，将mybatis接口的实现加入到ioc容器中-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!--扫描所有的dao接口的实现，加入到ioc容器中-->
        <property name="basePackage" value="com.teng.dao"></property>
    </bean>

    <!--配置一个可以执行批量的sqlSession-->
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
        <constructor-arg name="sqlSessionFactory" ref="SqlSessionFactory"></constructor-arg>
        <!--batch 批量处理-->
        <constructor-arg name="executorType" value="BATCH"></constructor-arg>
    </bean>

    <!--============================事务控制的配置============================-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!--控制数据源-->
        <property name="dataSource" ref="pooledDataSource"></property>
    </bean>

    <!--开启基于注解的事务，使用xml配置形式的事务（主要用xml）-->
    <aop:config>
        <!--切入点表达式-->
        <aop:pointcut id="txPoint" expression="execution(* com.teng.service..*(..))" />
        <!--配置事务增强-->
        <aop:advisor advice-ref="txAdvice" pointcut-ref="txPoint"></aop:advisor>
    </aop:config>

    <!--配置事务增强，事务如何切入-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <tx:attributes>
            <!--所有的方法都是事务方法-->
            <tx:method name="*"/>
            <!--以get开始的所有方法（查询）-->
            <tx:method name="get*" read-only="true"/>
        </tx:attributes>
    </tx:advice>

    <!--Spring配置文件的核心点（数据源，与mybatis的整合，事务控制）-->

</beans>
```

> 关于事务的控制有一个小点：**切入点表达式**和**事务增强**联系在于`<aop:advisor advice-ref="txAdvice" pointcut-ref="txPoint"></aop:advisor>`，但是它们跟**事务管理器**的联系在于事务管理的`id`设置为了`transactionManager`， `<tx:advice id="txAdvice" transaction-manager="transactionManager">`这个默认的事务管理器的引用`id`就是`transactionManager`。
>
> 如果事务管理器的`id`为其他名字，需要在`tx:advice`中指出其`id`

<br>

#### （3）spring-mvc.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!-- SpringMVC的配置文件，包含网站跳转逻辑的控制，配置 -->
    <!-- 禁用默认规则 -->
    <context:component-scan base-package="com.teng" use-default-filters="false">
        <!-- 只扫描控制器 -->
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>

    <!--配置视图解析器，方便页面返回-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/views"></property>
        <property name="suffix" value=".jsp"></property>
    </bean>

    <!--两个标准配置-->
    <!--将springmvc不能处理的请求交给tomcat-->
    <mvc:default-servlet-handler/>
    <!--能支持springmv更高级的一些功能，JSR303校验，快捷ajax... 映射动态请求-->
    <mvc:annotation-driven/>


</beans>
```

<br>

#### （4）mybatis-config.xml

> [Mybatis](https://mybatis.org/mybatis-3/zh/index.html)

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <settings>
        <!--开启驼峰命名规则-->
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>
    <typeAliases>
        <!--别名配置-->
        <package name="com.teng.bean"/>
    </typeAliases>

</configuration>
```

<br>

### 3. 逆向工程配置文件

> [Spring-generator](http://mybatis.org/generator) 

> 该文件放在与`pom.xml`文件同目录下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>

    <context id="DB2Tables" targetRuntime="MyBatis3">

        <!--不生成注释-->
        <commentGenerator>
            <property name="suppressAllComments" value="true" />
        </commentGenerator>

        <!--配置数据库连接信息-->
        <jdbcConnection
                driverClass="com.mysql.cj.jdbc.Driver"
                connectionURL="jdbc:mysql://localhost:3306/ssm_crud?serverTimezone=UTC"
                userId="root"
                password="333">
        </jdbcConnection>

        <javaTypeResolver>
            <property name="forceBigDecimals" value="false"/>
        </javaTypeResolver>

        <!--指定JavaBean生成的位置-->
        <javaModelGenerator
                targetPackage="com.teng.bean"
                targetProject=".\src\main\java">
            <property name="enableSubPackages" value="true"/>
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>

        <!--指定sql映射文件生成的位置-->
        <sqlMapGenerator
                targetPackage="mapper"
                targetProject=".\src\main\resources">
            <property name="enableSubPackages" value="true"/>
        </sqlMapGenerator>

        <!--指定dao接口生成的位置，mapper接口-->
        <javaClientGenerator type="XMLMAPPER"
                 targetPackage="com.teng.dao"
                 targetProject=".\src\main\java">
            <property name="enableSubPackages" value="true"/>
        </javaClientGenerator>

        <!--table指定每个表的生成策略-->
        <table tableName="tbl_stu" domainObjectName="Student"></table>
        <table tableName="tbl_dept" domainObjectName="Department"></table>

    </context>
</generatorConfiguration>
```

> 测试文件

```java
import org.mybatis.generator.api.MyBatisGenerator;
import org.mybatis.generator.config.Configuration;
import org.mybatis.generator.config.xml.ConfigurationParser;
import org.mybatis.generator.internal.DefaultShellCallback;

import java.io.File;
import java.util.ArrayList;
import java.util.List;

public class MBGTest {
    public static void main(String[] args) throws Exception {
        List<String> warnings = new ArrayList<String>();
        boolean overwrite = true;
        File configFile = new File("mbg.xml");
        ConfigurationParser cp = new ConfigurationParser(warnings);
        Configuration config = cp.parseConfiguration(configFile);
        DefaultShellCallback callback = new DefaultShellCallback(overwrite);
        MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, callback, warnings);
        myBatisGenerator.generate(null);
    }
}
```

<br>

### 4. 数据库的相关文件

> 学生表

```sql
create table `tbl_stu` (
	`stu_id` int (11),
	`stu_name` varchar (255),
	`gender` char (1),
	`email` varchar (255),
	`d_id` int (11)
); 
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('1','白曜溥','M','yaopu@163.com','3');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('2','柴高岩','M','gaoyan@163.com','1');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('3','陈露','M','chenlu@163.com','2');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('4','褚宸皓','M','chenhao@163.com','1');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('5','冯金平','M','jinpin@163.com','2');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('6','高奇泽','M','qize@163.com','3');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('7','宫敏','M','gongmin@163.com','4');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('8','郭旭','M','guoxu@163.com','1');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('9','郝思远','M','siyuan@163.com','2');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('10','呼晓辉','M','xiaohui@163.com','3');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('11','家彦明','M','yanming@163.com','4');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('12','焦晨帆','F','chenfan@163.com','1');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('13','李兴栋','M','xindong@163.com','2');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('14','李英','F','liying@163.com','3');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('15','李源','M','liyuan@163.com','4');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('16','刘丹','F','liudan@163.com','1');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('17','牛旭东','M','xudong@163.com','2');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('18','牛泽鹏','M','zepeng@163.com','3');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('19','裴怡博','M','yibo@163.com','4');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('20','秦新宇','M','xinyu@163.com','1');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('21','师伟','M','shiwei@163.com','2');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('22','石宇飞','M','yufei@163.com','3');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('23','孙亚龙','M','yalong@163.com','4');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('24','王攀榕','F','panrong@163.com','1');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('25','王洋','M','wangyang@163.com','2');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('26','王振龙','M','zhenlong@163.com','3');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('27','魏超群','M','chaoqun@163.com','4');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('28','闫青','F','yanqing@163.com','1');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('29','杨华','F','yanghua@163.com','2');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('30','杨欢','F','yanghuan@163.com','3');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('31','杨凯','F','yangkai@163.com','4');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('32','姚丹娜','F','danna@163.com','1');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('33','由国婧','F','guojing@163.com','2');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('34','张强文','M','qiangwen@163.com','3');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('35','张勇','M','zhangyong@163.com','4');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('41','冯阳阳','M','yangyang@163.com','1');
insert into `tbl_stu` (`stu_id`, `stu_name`, `gender`, `email`, `d_id`) values('42','王静玲','F','jingling@163.com','2');
```

> 部门表

```sql
create table `tbl_dept` (
	`dept_id` int (11),
	`dept_name` varchar (255)
); 
insert into `tbl_dept` (`dept_id`, `dept_name`) values('1','学生部');
insert into `tbl_dept` (`dept_id`, `dept_name`) values('2','信息部');
insert into `tbl_dept` (`dept_id`, `dept_name`) values('3','记者部');
insert into `tbl_dept` (`dept_id`, `dept_name`) values('4','实创部');
```



## 三、测试mapper

> 这个地方就贴一个批量插入的设置叭~

> 上述applicationContext.xml文件中已经配置了批量插入。

```xml
    <!--配置一个可以执行批量的sqlSession-->
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
        <constructor-arg name="sqlSessionFactory" ref="SqlSessionFactory"></constructor-arg>
        <!--batch 批量处理-->
        <constructor-arg name="executorType" value="BATCH"></constructor-arg>
    </bean
```

> 测试文件

```java
    @Autowired
    SqlSession sqlSession;
    
    @Test
    public void testCRUD() {
        StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
        for (int i = 0; i < 1000; i++) {
            String uid = UUID.randomUUID().toString().substring(0, 5) + i;
            // 这个insertSelective方法是通过执行Mybatis自动生成的方法
            mapper.insertSelective(new Student(null, uid, "M", uid + "@163.com", 1));
        }
        System.out.println("批量完成");
    }
```





## 四、ajax发送put请求小知识点

> 如果直接发送`ajax=PUT`形式的请求
> 封装的数据
> Student
> [empId=25, stuName=null, gender=null, email=null, dId=null]

### 1.问题

> **请求体中有数据：**
> 但是`Employee`对象封装不上；
>
> 在实现更新的时候`sql`语句拼成结果如下，没有`set`条件，会出现`sql`语句异常
>
> `update tbl_emp  where emp_id = 1014;`

### 2.原因

> **Tomcat**：
>
> 1. 将请求体中的数据，封装一个`map`。
> 2. `request.getParameter("empName")`就会从这个map中取值。
> 3. `SpringMVC`封装`POJO`对象的时候。会把`POJO`中每个属性的值，`request.getParamter("email");`
>
> **AJAX发送PUT请求引发的血案：**
> 		`PUT`请求，请求体中的数据，`request.getParameter("stuName")`拿不到
> 		`Tomcat`一看是PUT不会封装请求体中的数据为`map`，只有`POST`形式的请求才封装请求体为`map`
>
> ```java
> org.apache.catalina.connector.Request--parseParameters() (**源码3111行**);
> 
> protected String parseBodyMethods = "POST";
> if( !getConnector().isParseBodyMethod(getMethod()) ) {
>          success = true;
>          return;
> }
> ```

### 3.解决方案

> 我们要能支持直接发送PUT之类的请求还要封装请求体中的数据
>
> 1. 配置上`HttpPutFormContentFilter`；
> 2. 他的作用；将请求体中的数据解析包装成一个`map`。
> 3. `request`被重新包装，`request.getParameter()`被重写，就会从自己封装的`map`中取数据



#### （1）使用post请求

```jsp
$.ajax({
  url: "${APP_PATH}/stu/" + id,
  type: "POST",
  data: $("#stuUpdateModel form").serialize()+"&_method=PUT",
  success: function (result) {
      alert(result.msg)
  }
})
```

#### （2）使用put请求

```xml
<filter>
    <filter-name>HttpPutFormContentFilter</filter-name>
    <filter-class>org.springframework.web.filter.HttpPutFormContentFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>HttpPutFormContentFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

```jsp
$.ajax({
  url: "${APP_PATH}/stu/" + id,
  type: "PUT",
  data: $("#stuUpdateModel form").serialize(),
  success: function (result) {
      alert(result.msg)
  }
})
```


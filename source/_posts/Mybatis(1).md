---
title: Mybatis的简单使用
author: 小小千辰
tags:
  - Mybatis
categories:
  - 千辰的小小笔记
abbrlink: 803e529f
date: 2022-03-23 14:59:31
---





# Mybatis简单学习

## 1. 两个模板

![image-20220319113209874](https://gitee.com/Thousand_Star/cloudimage/raw/master/Thousand_Star/img/202203191132001.png)

### mybatis-config

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <properties resource="jdbc.properties"></properties>
    
    <typeAliases>
        <package name=""/>
    </typeAliases>

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    
    <mappers>
        <package name=""/>
    </mappers>
    
</configuration>
```

### mybatis-mapper

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="">

</mapper>
```



## 2. 核心配置文件详解

核心配置文件中的标签必须按照固定的顺序：

```xml
properties?,
settings?,
typeAliases?,
typeHandlers?,
objectFactory?,
objectWrapperFactory?,
reflectorFactory?,
plugins?,
environments?,
databaseIdProvider?,
mappers?
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//MyBatis.org//DTD Config 3.0//EN"
        "http://MyBatis.org/dtd/MyBatis-3-config.dtd">
<configuration>
    <!--引入properties文件，此时就可以${属性名}的方式访问属性值-->
    <properties resource="jdbc.properties"></properties>
    <settings>
        <!--将表中字段的下划线自动转换为驼峰-->
        <setting name="mapUnderscoreToCamelCase" value="true"/>
        <!--开启延迟加载-->
        <setting name="lazyLoadingEnabled" value="true"/>
    </settings>
    <typeAliases>
        <!--
        typeAlias：设置某个具体的类型的别名
        属性：
        type：需要设置别名的类型的全类名
        alias：设置此类型的别名，若不设置此属性，该类型拥有默认的别名，即类名且不区分大小
        写
        若设置此属性，此时该类型的别名只能使用alias所设置的值
        -->
        <!--<typeAlias type="com.atguigu.mybatis.bean.User"></typeAlias>-->
        <!--<typeAlias type="com.atguigu.mybatis.bean.User" alias="abc">
        </typeAlias>-->
        <!--以包为单位，设置改包下所有的类型都拥有默认的别名，即类名且不区分大小写-->
        <package name="com.atguigu.mybatis.bean"/>
    </typeAliases>
    <!--
    environments：设置多个连接数据库的环境
    属性：
    default：设置默认使用的环境的id
    -->
    <environments default="mysql_test">
        <!--
        environment：设置具体的连接数据库的环境信息
        属性：
        id：设置环境的唯一标识，可通过environments标签中的default设置某一个环境的id，
        表示默认使用的环境
        -->
        <environment id="mysql_test">
            <!--
            transactionManager：设置事务管理方式
            属性：
            type：设置事务管理方式，type="JDBC|MANAGED"
            type="JDBC"：设置当前环境的事务管理都必须手动处理
            type="MANAGED"：设置事务被管理，例如spring中的AOP
            -->
            <transactionManager type="JDBC"/>
            <!--
            dataSource：设置数据源
            属性：
            type：设置数据源的类型，type="POOLED|UNPOOLED|JNDI"
            type="POOLED"：使用数据库连接池，即会将创建的连接进行缓存，下次使用可以从
            缓存中直接获取，不需要重新创建
            type="UNPOOLED"：不使用数据库连接池，即每次使用连接都需要重新创建
            type="JNDI"：调用上下文中的数据源
            -->
            <dataSource type="POOLED">
                <!--设置驱动类的全类名-->
                <property name="driver" value="${jdbc.driver}"/>
                <!--设置连接数据库的连接地址-->
                <property name="url" value="${jdbc.url}"/>
                <!--设置连接数据库的用户名-->
                <property name="username" value="${jdbc.username}"/>
                <!--设置连接数据库的密码-->
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    <!--引入映射文件-->
    <mappers>
        <mapper resource="UserMapper.xml"/>
        <!--
        以包为单位，将包下所有的映射文件引入核心配置文件
        注意：此方式必须保证mapper接口和mapper映射文件必须在相同的包下
        -->
        <package name="com.atguigu.mybatis.mapper"/>
    </mappers>
</configuration>
```



## 3. MyBatis获取参数值的两种方式（重点）

MyBatis获取参数值的两种方式：`${}和#{}`

<b style="color: #FF0000">${}的本质就是字符串拼接，#{}的本质就是占位符赋值</b>

1. ${}使用字符串拼接的方式拼接sql，若为字符串类型或日期类型的字段进行赋值时，需要手动加单引
   号；
2. 但是#{}使用占位符赋值的方式拼接sql，此时为字符串类型或日期类型的字段进行赋值时，可以自
   动添加单引号



## 4. 特殊SQL的执行

### 4.1 模糊查询

```java
/**
* 测试模糊查询
* @param mohu
* @return
*/
List<User> testMohu(@Param("mohu") String mohu);
```

```xml
<!--List<User> testMohu(@Param("mohu") String mohu);-->
<select id="testMohu" resultType="User">
    <!--select * from t_user where username like '%${mohu}%'-->
    <!--select * from t_user where username like concat('%',#{mohu},'%')-->
    select * from t_user where username like "%"#{mohu}"%"
</select>
```

### 4.2 批量删除

```java
/**
* 批量删除
* @param ids
* @return
*/
int deleteMore(@Param("ids") String ids);
```

```xml
<!--int deleteMore(@Param("ids") String ids);-->
<delete id="deleteMore">
    delete from t_user where id in (${ids})
</delete>
```

### 4.3 动态设置表名

```java
/**
* 动态设置表名，查询所有的用户信息
* @param tableName
* @return
*/
List<User> getAllUser(@Param("tableName") String tableName);
```

```xml
<!--List<User> getAllUser(@Param("tableName") String tableName);-->
<select id="getAllUser" resultType="User">
    select * from ${tableName}
</select>
```

### 4.4 添加功能获取自增的主键

t_clazz(clazz_id,clazz_name)
t_student(student_id,student_name,clazz_id)

1. 添加班级信息
2. 获取新添加的班级的id
3. 为班级分配学生，即将某学的班级id修改为新添加的班级的id

```java
/**
* 添加用户信息
* @param user
* @return
* useGeneratedKeys：设置使用自增的主键
* keyProperty：因为增删改有统一的返回值是受影响的行数，因此只能将获取的自增的主键放在传输的参
数user对象的某个属性中
*/
int insertUser(User user);
```

```xml
<!--int insertUser(User user);-->
<insert id="insertUser" useGeneratedKeys="true" keyProperty="id">
    insert into t_user values(null,#{username},#{password},#{age},#{sex})
</insert>
```



## 5. 自定义映射resultMap

### 5.1 resultMap处理字段和属性的映射关系

> 若字段名和实体类中的属性名不一致，则可以通过resultMap设置自定义映射

```xml
<!--
    resultMap：设置自定义映射
    属性：
    id：表示自定义映射的唯一标识
    type：查询的数据要映射的实体类的类型
    子标签：
    id：设置主键的映射关系
    result：设置普通字段的映射关系
    association：设置多对一的映射关系
    collection：设置一对多的映射关系
    属性：
    property：设置映射关系中实体类中的属性名
    column：设置映射关系中表中的字段名
-->
<resultMap id="userMap" type="User">
    <id property="id" column="id"></id>
    <result property="userName" column="user_name"></result>
    <result property="password" column="password"></result>
    <result property="age" column="age"></result>
    <result property="sex" column="sex"></result>
</resultMap>
<!--List<User> testMohu(@Param("mohu") String mohu);-->
<select id="testMohu" resultMap="userMap">
    <!--select * from t_user where username like '%${mohu}%'-->
    select id,user_name,password,age,sex from t_user where user_name like
concat('%',#{mohu},'%')
</select>
```

> 若字段名和实体类中的属性名不一致，但是字段名符合数据库的规则（使用_），实体类中的属性
> 名符合Java的规则（使用驼峰）
> 此时也可通过以下两种方式处理字段名和实体类中的属性的映射关系
> a>可以通过为字段起别名的方式，保证和实体类中的属性名保持一致
> b>可以在MyBatis的核心配置文件中设置一个全局配置信息mapUnderscoreToCamelCase，可
> 以在查询表中数据时，自动将_类型的字段名转换为驼峰
> 例如：字段名user_name，设置了mapUnderscoreToCamelCase，此时字段名就会转换为
> userName

### 5.2 多对一映射处理

> 查询员工信息以及员工所对应的部门信息

#### 5.2.1 级联方式处理映射关系

```xml
<resultMap id="empDeptMap" type="Emp">
    <id column="eid" property="eid"></id>
    <result column="ename" property="ename"></result>
    <result column="age" property="age"></result>
    <result column="sex" property="sex"></result>
    <result column="did" property="dept.did"></result>
    <result column="dname" property="dept.dname"></result>
</resultMap>
<!--Emp getEmpAndDeptByEid(@Param("eid") int eid);-->
<select id="getEmpAndDeptByEid" resultMap="empDeptMap">
    select emp.*,dept.* from t_emp emp left join t_dept dept on emp.did = dept.did where emp.eid = #{eid}
</select>
```

#### 5.2.2 使用association处理映射关系

```xml
<resultMap id="empDeptMap" type="Emp">
    <id column="eid" property="eid"></id>
    <result column="ename" property="ename"></result>
    <result column="age" property="age"></result>
    <result column="sex" property="sex"></result>
    <association property="dept" javaType="Dept">
        <id column="did" property="did"></id>
        <result column="dname" property="dname"></result>
    </association>
</resultMap>
<!--Emp getEmpAndDeptByEid(@Param("eid") int eid);-->
<select id="getEmpAndDeptByEid" resultMap="empDeptMap">
    select emp.*,dept.* from t_emp emp left join t_dept dept on emp.did = dept.did where emp.eid = #{eid}
</select>
```

#### 5.2.3 分步查询

1. 查询员工信息

   ```java
   /**
   * 通过分步查询查询员工信息
   * @param eid
   * @return
   */
   Emp getEmpByStep(@Param("eid") int eid);
   ```

   ```xml
   <resultMap id="empDeptStepMap" type="Emp">
       <id column="eid" property="eid"></id>
       <result column="ename" property="ename"></result>
       <result column="age" property="age"></result>
       <result column="sex" property="sex"></result>
       <!--
           select：设置分步查询，查询某个属性的值的sql的标识（namespace.sqlId）
           column：将sql以及查询结果中的某个字段设置为分步查询的条件
       -->
       <association property="dept"
       	select="com.atguigu.MyBatis.mapper.DeptMapper.getEmpDeptByStep" 
           column="did">
       </association>
   </resultMap>
   <!--Emp getEmpByStep(@Param("eid") int eid);-->
   <select id="getEmpByStep" resultMap="empDeptStepMap">
       select * from t_emp where eid = #{eid}
   </select>
   ```

2. 根据员工所对应的部门id查询部门信息

   ```java
   /**
   * 分步查询的第二步：根据员工所对应的did查询部门信息
   * @param did
   * @return
   */
   Dept getEmpDeptByStep(@Param("did") int did);
   ```

   ```xml
   <!--Dept getEmpDeptByStep(@Param("did") int did);-->
   <select id="getEmpDeptByStep" resultType="Dept">
       select * from t_dept where did = #{did}
   </select>
   ```



### 5.3 一对多映射处理

#### 5.3.1 collection

```java
/**
* 根据部门id查新部门以及部门中的员工信息
* @param did
* @return
*/
Dept getDeptEmpByDid(@Param("did") int did);
```

```xml
<resultMap id="deptEmpMap" type="Dept">
    <id property="did" column="did"></id>
    <result property="dname" column="dname"></result>
    <!--
    	ofType：设置collection标签所处理的集合属性中存储数据的类型
    -->
    <collection property="emps" ofType="Emp">
        <id property="eid" column="eid"></id>
        <result property="ename" column="ename"></result>
        <result property="age" column="age"></result>
        <result property="sex" column="sex"></result>
    </collection>
</resultMap>
<!--Dept getDeptEmpByDid(@Param("did") int did);-->
<select id="getDeptEmpByDid" resultMap="deptEmpMap">
    select dept.*,emp.* from t_dept dept left join t_emp emp on dept.did = emp.did where dept.did = #{did}
</select>
```

#### 5.3.2 分步查询

1. 查询部门信息

   ```java
   /**
   * 分步查询部门和部门中的员工
   * @param did
   * @return
   */
   Dept getDeptByStep(@Param("did") int did);
   ```

   ```xml
   <resultMap id="deptEmpStep" type="Dept">
       <id property="did" column="did"></id>
       <result property="dname" column="dname"></result>
       <collection property="emps" fetchType="eager"
   		select="com.atguigu.MyBatis.mapper.EmpMapper.getEmpListByDid" 
           column="did">
       </collection>
   </resultMap>
   <!--Dept getDeptByStep(@Param("did") int did);-->
   <select id="getDeptByStep" resultMap="deptEmpStep">
       select * from t_dept where did = #{did}
   </select>
   ```

2. 根据部门id查询部门中的所有员工

   ```java
   /**
   * 根据部门id查询员工信息
   * @param did
   * @return
   */
   List<Emp> getEmpListByDid(@Param("did") int did);
   ```

   ```xml
   <!--List<Emp> getEmpListByDid(@Param("did") int did);-->
   <select id="getEmpListByDid" resultType="Emp">
       select * from t_emp where did = #{did}
   </select>
   ```



> 分步查询的优点：可以实现延迟加载，但是必须在核心配置文件中设置全局配置信息：
> `lazyLoadingEnabled`：延迟加载的全局开关。当开启时，所有关联对象都会延迟加载
> `aggressiveLazyLoading`：当开启时，任何方法的调用都会加载该对象的所有属性。 否则，每个属性会按需加载
> 此时就可以实现按需加载，获取的数据是什么，就只会执行相应的sql。此时可通过association和
> collection中的fetchType属性设置当前的分步查询是否使用延迟加载，fetchType="lazy(延迟加
> 载)|eager(立即加载)"



## 6. 动态sql

### 6.1 if

> if标签可通过test属性的表达式进行判断，若表达式的结果为true，则标签中的内容会执行；
>
> 反之标签中的内容不会执行

```xml
<!--List<Emp> getEmpListByMoreTJ(Emp emp);-->
<select id="getEmpListByMoreTJ" resultType="Emp">
    select * from t_emp where 1=1
    <if test="ename != '' and ename != null">
    	and ename = #{ename}
    </if>
    <if test="age != '' and age != null">
    	and age = #{age}
    </if>
    <if test="sex != '' and sex != null">
    	and sex = #{sex}
    </if>
</select>
```

### 6.2 where

```xml
<select id="getEmpListByMoreTJ2" resultType="Emp">
    select * from t_emp
    <where>
        <if test="ename != '' and ename != null">
        	ename = #{ename}
        </if>
        <if test="age != '' and age != null">
            and age = #{age}
        </if>
        <if test="sex != '' and sex != null">
        	and sex = #{sex}
        </if>
    </where>
</select>
```

> where和if一般结合使用：
>
> 1. 若where标签中的if条件都不满足，则where标签没有任何功能，即不会添加where关键字
> 2. 若where标签中的if条件满足，则where标签会自动添加where关键字，并将条件最前方多余的
>    and去掉
>
> <b style="color: #FF0000"> 注意：where标签不能去掉条件最后多余的and </b>

### 6.3 trim

```xml
<select id="getEmpListByMoreTJ" resultType="Emp">
    select * from t_emp
    <trim prefix="where" suffixOverrides="and">
        <if test="ename != '' and ename != null">
        	ename = #{ename} and
        </if>
        <if test="age != '' and age != null">
        	age = #{age} and
        </if>
        <if test="sex != '' and sex != null">
        	sex = #{sex}
        </if>
    </trim>
</select>
```

> trim用于去掉或添加标签中的内容
> 常用属性：
>
> * `prefix`：在trim标签中的内容的前面添加某些内容
> * `prefixOverrides`：在trim标签中的内容的前面去掉某些内容
> * `suffix`：在trim标签中的内容的后面添加某些内容
> * `suffixOverrides`：在trim标签中的内容的后面去掉某些内容

### 6.4 choose、when、otherwise

> choose、when、otherwise相当于if...else if..else

```xml
<!--List<Emp> getEmpListByChoose(Emp emp);-->
<select id="getEmpListByChoose" resultType="Emp">
    select <include refid="empColumns"></include> from t_emp
    <where>
        <choose>
            <when test="ename != '' and ename != null">
            	ename = #{ename}
            </when>
            <when test="age != '' and age != null">
            	age = #{age}
            </when>
            <when test="sex != '' and sex != null">
            	sex = #{sex}
            </when>
            <when test="email != '' and email != null">
            	email = #{email}
            </when>
        </choose>
    </where>
</select>
```

### 6.5 foreach

```xml
<!--int insertMoreEmp(List<Emp> emps);-->
<insert id="insertMoreEmp">
	insert into t_emp values
    <foreach collection="emps" item="emp" separator=",">
    	(null,#{emp.ename},#{emp.age},#{emp.sex},#{emp.email},null)
    </foreach>
</insert>

<!--int deleteMoreByArray(int[] eids);-->
<delete id="deleteMoreByArray">
    delete from t_emp where
    <foreach collection="eids" item="eid" separator="or">
    	eid = #{eid}
    </foreach>
</delete>

<!--int deleteMoreByArray(int[] eids);-->
<delete id="deleteMoreByArray">
    delete from t_emp where eid in
    <foreach collection="eids" item="eid" separator="," open="(" close=")">
    	#{eid}
    </foreach>
</delete>
```

> 属性：
>
> 1. `collection`：设置要循环的数组或集合
> 2. `item`：表示集合或数组中的每一个数据
> 3. `separator`：设置循环体之间的分隔符
> 4. `open`：设置foreach标签中的内容的开始符
> 5. `close`：设置foreach标签中的内容的结束符

### 6.6 SQL片段

> sql片段，可以记录一段公共sql片段，在使用的地方通过include标签进行引入

```xml
<sql id="empColumns">
	eid,ename,age,sex,did
</sql>
select <include refid="empColumns"></include> from t_emp
```



## 7. MyBatis的缓存

### 7.1 MyBatis的一级缓存

> 一级缓存是SqlSession级别的，通过同一个SqlSession查询的数据会被缓存，下次查询相同的数据，就会从缓存中直接获取，不会从数据库重新访问。

使一级缓存失效的四种情况：

1. 不同的SqlSession对应不同的一级缓存
2. 同一个SqlSession但是查询条件不同
3. 同一个SqlSession两次查询期间执行了任何一次增删改操作
4. 同一个SqlSession两次查询期间手动清空了缓存

### 7.2 MyBatis的二级缓存

> 二级缓存是SqlSessionFactory级别，通过同一个SqlSessionFactory创建的SqlSession查询的结果会被
> 缓存；此后若再次执行相同的查询语句，结果就会从缓存中获取。

二级缓存开启的条件：

1. 在核心配置文件中，设置全局配置属性`cacheEnabled="true"`，默认为true，不需要设置
2. 在映射文件中设置标签`<cache />`
3. 二级缓存必须在SqlSession关闭或提交之后有效
4. 查询的数据所转换的实体类类型必须实现序列化的接口

<b style="color: #FF0000"> 使二级缓存失效的情况： 两次查询之间执行了任意的增删改，会使一级和二级缓存同时失效 </b>



### 7.3 二级缓存的相关配置

在mapper配置文件中添加的cache标签可以设置一些属性：

* eviction属性：缓存回收策略
  LRU（Least Recently Used） – 最近最少使用的：移除最长时间不被使用的对象。
  FIFO（First in First out） – 先进先出：按对象进入缓存的顺序来移除它们。
  SOFT – 软引用：移除基于垃圾回收器状态和软引用规则的对象。
  WEAK – 弱引用：更积极地移除基于垃圾收集器状态和弱引用规则的对象。
  默认的是 LRU。
* flushInterval属性：刷新间隔，单位毫秒
  默认情况是不设置，也就是没有刷新间隔，缓存仅仅调用语句时刷新
  size属性：引用数目，正整数
  代表缓存最多可以存储多少个对象，太大容易导致内存溢出
* readOnly属性：只读，true/false
  true：只读缓存；会给所有调用者返回缓存对象的相同实例。因此这些对象不能被修改。这提供了
  很重要的性能优势。
  false：读写缓存；会返回缓存对象的拷贝（通过序列化）。这会慢一些，但是安全，因此默认是
  false。

### 7.4 MyBatis缓存查询的顺序

* 先查询二级缓存，因为二级缓存中可能会有其他程序已经查出来的数据，可以拿来直接使用。
* 如果二级缓存没有命中，再查询一级缓存
* 如果一级缓存也没有命中，则查询数据库
* SqlSession关闭之后，一级缓存中的数据会写入二级缓存



## 8. MyBatis的逆向工程

* 正向工程：先创建Java实体类，由框架负责根据实体类生成数据库表。Hibernate是支持正向工程
  的。
* 逆向工程：先创建数据库表，由框架负责根据数据库表，反向生成如下资源：
  * Java实体类
  * Mapper接口
  * Mapper映射文件

### 8.1 创建逆向工程

#### 8.1.1 添加依赖和插件

```xml
<!-- 依赖MyBatis核心包 -->
<dependencies>
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.7</version>
    </dependency>
</dependencies>
<!-- 控制Maven在构建过程中相关配置 -->
<build>
    <!-- 构建过程中用到的插件 -->
    <plugins>
        <!-- 具体插件，逆向工程的操作是以构建过程中插件形式出现的 -->
        <plugin>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-maven-plugin</artifactId>
            <version>1.3.0</version>
            <!-- 插件的依赖 -->
            <dependencies>
                <!-- 逆向工程的核心依赖 -->
                <dependency>
                    <groupId>org.mybatis.generator</groupId>
                    <artifactId>mybatis-generator-core</artifactId>
                    <version>1.3.2</version>
                </dependency>
                <!-- 数据库连接池 -->
                <dependency>
                    <groupId>com.mchange</groupId>
                    <artifactId>c3p0</artifactId>
                    <version>0.9.2</version>
                </dependency>
                <!-- MySQL驱动 -->
                <dependency>
                    <groupId>mysql</groupId>
                    <artifactId>mysql-connector-java</artifactId>
                    <version>8.0.15</version>
                </dependency>
            </dependencies>
        </plugin>
    </plugins>
</build>
```

#### 8.1.2 创建MyBatis的核心配置文件

> 使用模板创建mybatis-config 即可

#### 8.1.3 创建逆向工程的配置文件

> 文件名必须是：generatorConfig.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
    PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
    "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
    <!--
        targetRuntime: 执行生成的逆向工程的版本
        MyBatis3Simple: 生成基本的CRUD（清新简洁版）
        MyBatis3: 生成带条件的CRUD（奢华尊享版）
    -->
    <context id="DB2Tables" targetRuntime="MyBatis3Simple">
        <!-- 数据库的连接信息 -->
        <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
            connectionURL="jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC"
            userId="root"
            password="333">
        </jdbcConnection>
        <!-- javaBean的生成策略-->
        <javaModelGenerator targetPackage="com.atguigu.mybatis.bean"
        targetProject=".\src\main\java">
            <property name="enableSubPackages" value="true" />
            <property name="trimStrings" value="true" />
        </javaModelGenerator>
        <!-- SQL映射文件的生成策略 -->
        <sqlMapGenerator targetPackage="com.atguigu.mybatis.mapper"
        targetProject=".\src\main\resources">
        	<property name="enableSubPackages" value="true" />
        </sqlMapGenerator>
        <!-- Mapper接口的生成策略 -->
        <javaClientGenerator type="XMLMAPPER"
        targetPackage="com.atguigu.mybatis.mapper" targetProject=".\src\main\java">
        	<property name="enableSubPackages" value="true" />
        </javaClientGenerator>
        <!-- 逆向分析的表 -->
        <!-- tableName设置为*号，可以对应所有表，此时不写domainObjectName -->
        <!-- domainObjectName属性指定生成出来的实体类的类名 -->
        <table tableName="t_emp" domainObjectName="Emp"/>
        <table tableName="t_dept" domainObjectName="Dept"/>
    </context>
</generatorConfiguration>
```

#### 8.1.4 执行MBG插件的generate目标

![1647658068122](../AppData/Roaming/Typora/typora-user-images/1647658068122.png)

效果：

![1647658078421](../AppData/Roaming/Typora/typora-user-images/1647658078421.png)

### 8.2 QBC查询

```java
@Test
public void testMBG() throws IOException {
    InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
    SqlSession sqlSession = new
    SqlSessionFactoryBuilder().build(is).openSession(true);
    EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);
    EmpExample empExample = new EmpExample();
    //创建条件对象，通过andXXX方法为SQL添加查询添加，每个条件之间是and关系
   empExample.createCriteria().andEnameLike("a").andAgeGreaterThan(20).andDidIsNotNull();
    //将之前添加的条件通过or拼接其他条件
    empExample.or().andSexEqualTo("男");
    List<Emp> list = mapper.selectByExample(empExample);
    for (Emp emp : list) {
        System.out.println(emp);
    }
}
```



## 9. 分页插件

### 9.1 分页插件的使用

#### 9.1.1 配置分页插件

> 添加分页插件的依赖

```xml
<!-- https://mvnrepository.com/artifact/com.github.pagehelper/pagehelper -->
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper</artifactId>
    <version>5.2.0</version>
</dependency>
```

#### 9.1.2 配置分页插件

在MyBatis的核心配置文件中配置插件

```xml
<plugins>
    <!--设置分页插件-->
    <plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
</plugins>
```

### 9.2 分页插件的使用

#### 9.2.1 开启分页

> 在查询功能之前使用`PageHelper.startPage(int pageNum, int pageSize)`开启分页功能
>
> 1. pageNum：当前页的页码
> 2. pageSize：每页显示条数

#### 9.2.2 获取list集合后的操作

> 使用`PageInfo<T> pageInfo = new PageInfo<>(List<T> list, intnavigatePages)`获取分页相关数据

相关数据：

|            属性             |             含义             |
| :-------------------------: | :--------------------------: |
|           pageNum           |         当前页的页码         |
|          pageSize           |        每页显示的条数        |
|            size             |     当前页显示的真实条数     |
|            total            |           总记录数           |
|            pages            |            总页数            |
|           prePage           |         上一页的页码         |
|          nextPage           |         下一页的页码         |
|   isFirstPage/isLastPage    |    是否为第一页/最后一页     |
| hasPreviousPage/haxNextPage |    是否存在上一页/下一页     |
|        navigatePages        |       导航分页的页码数       |
|      navigatepageNums       | 导航分页的页码, [1, 2,3,4,5] |


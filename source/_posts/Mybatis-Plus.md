---
title: Mybatis-Plus
author: 小小千辰
tags:
  - Mybatis
categories:
  - 千辰的小小笔记
abbrlink: 5d2bcff8
date: 2022-01-17 00:00:00
updated: 2022-01-18 00:00:00
---



# Mybatis-Plus

## 一、如何使用MP

1. 新建的spring boot工程

2. 指定maven的mp坐标

   ```xml
   <dependency>
       <groupId>com.baomidou</groupId>
       <artifactId>mybatis-plus-boot-starter</artifactId>
       <version>3.5.0</version>
   </dependency>
   ```

   指定数据库的驱动

   ```xml
   <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
       <version>8.0.15</version>
       <scope>runtime</scope>
   </dependency>
   ```

3. 创建实体类，1）定义属性，2）指定主键的类型

   ![image-20220117221424692](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241813530.png)_指定主键类型_

4. 创建Dao接口，需要继承`BaseMapper<实体.class>`

   ![image-20220117221551700](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241813433.png)_样例_

   

5. 在springboot的启动上，加入`@MapperScan(value="指定Dao接口的包名")`

   ![image-20220117221741723](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241813278.png)

   

6. 测试使用：

   在测试类或Service注入Dao接口，框架实现动态代理创建Dao的实现类对象。

   调用BaseMapper中的方法，完成CRUD



## 二、配置 mybatis 打印日志

`application.yml`

```yml
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```



## 三、CRUD基本用法

> CRUD 的操作是来自 BaseMapper 中的方法。BaseMapper 中共有 17 个方法，
>
> **17个CRUD方法中，insert占1个，update占2个，delete占4个，select占10个**
>
> CRUD 操作都有多个不同参数的方法。继承 BaseMapper 可以其中的方法。

BaseMapper 方法列表：

![image-20220118220533526](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241814320.png)

### 1.insert操作

```java
@Test
public void testInsertUser() {
    User user = new User();
    user.setName("张三");
    user.setAge(18);
    user.setEmail("zhangsan@sina.com");
    // INSERT INTO user ( name, email, age ) VALUES ( ?, ?, ? )
    int rows = userMapper.insert(user);
    System.out.println("影响的行数是: " + rows);
}
```

> `insert()`返回值 int，数据插入成功的行数，成功的记录数。`getId()`获取主键值

### 2.update操作

> update操作可分为部分字段更新（null字段不更新）和全字段更新

```java
@Test
public void testUpdate() {
    User user = new User();
    user.setId(3);
    user.setName("wangwu");
    user.setEmail("wangwu@126.com");
    user.setAge(12);
    // UPDATE user SET name=?, email=?, age=? WHERE id=?
    int rows = userMapper.updateById(user);
    System.out.println("rows: " + rows);
}
```

> 如果实体类属性为基本类型，则在进行部分字段更新时没有更新该字段的话，该字段会依照默认值更新。

```java
// 测试更新方法：实体类的属性是基本类型 - int age
@Test
public void testUpdate() {
    User user = new User();
    user.setId(4);
    user.setName("思思");
    // UPDATE user SET name=?, age=? WHERE id=?
    int rows = userMapper.updateById(user);
    System.out.println("rows: " + rows);
}
```

> **email 没有赋值，是 null ，所有没有出现在 set 语句中; age 有默认 0，被更新了**



### 3.delete操作

1. 根据 id 删除

   ```java
   /**
   * 按主键删除一条数据
   * 方法: deleteById()
   * 参数: 主键值
   * 返回值: 删除的成功行数
   */
   @Test
   public void testDeleteById() {
       // DELETE FROM user WHERE id=?
       int rows = userMapper.deleteById(4);
       System.out.println("deleteById: " + rows);
   }
   ```

2. 根据 Map 中条件删除

   > 注：删除条件封装在 Map 中，key 是列名，value 是值，多个 key 之间 and 联接。

   ```java
   /**
   * 按条件删除, 条件是封装到map对象中
   * 方法: deleteByMap(map对象)
   * 返回值: 删除的成功行数
   */
   @Test
   public void testDeleteByMap() {
       // 创建map对象, 保存条件值
       Map<String, Object> map = new HashMap<>();
       // put("表的字段名", 条件值)  可以封装多个条件
       map.put("name", "wangwu");
       map.put("age", 12);
       // DELETE FROM user WHERE name = ? AND age = ?
       int rows = userMapper.deleteByMap(map);
       System.out.println("DeleteByMap rows: " + rows);
   }
   ```

3. 批量删除

   ```java
   /**
   * 批处理方式: 使用多个主键值, 删除数据
   * 方法: deleteBatchIds()
   * 参数: Collection<? extends Serializable> var1
   * 返回值: 删除的成功行数
   */
   @Test
   public void testDeleteByBatchId() {
       List<Integer> ids = Stream.of(1, 2, 3, 4, 5).collect(Collectors.toList());
       // DELETE FROM user WHERE id IN ( ? , ? , ? , ? , ? )
       int rows = userMapper.deleteBatchIds(ids);
       System.out.println("DeleteByBatchId rows :" + rows);
   }
   ```

   

### 4.select操作

1. 根据 id 主键查询

   ```java
   /**
   * 查询 selectById, 根据主键值查询
   * 参数: 主键值
   * 返回值: 实体对象
   */
   @Test
   public void testSelectById() {
       // SELECT id,name,email,age FROM user WHERE id=?
       // 如果根据主键没有找到数据, 得到的返回值是null
       User user = userMapper.selectById(5);
       System.out.println("selectById: " + user);
   }
   ```

2. 批量查询记录

   ```java
   /**
   * 实现批处理查询, 根据多个主键值查询, 获取到List
   * 方法: selectBatchIds
   * 参数: id的集合
   * 返回值: List<T>
   */
   @Test
   public void testSelectBatchIds() {
       List<Integer> ids = Stream.of(5, 7, 9, 14).collect(Collectors.toList());
       // SELECT id,name,email,age FROM user WHERE id IN ( ? , ? , ? , ? )
       List<User> users = userMapper.selectBatchIds(ids);
       users.forEach(user -> {
           System.out.println(user);
       });
   }
   ```

3. 使用 Map 的条件查询

   ```java
   /**
    * 使用Map做多条件查询
    * 方法: selectMap
    * 参数: Map<String, Object>
    * 返回值: List<T>
    */
   @Test
   public void testSelectMap() {
       // 创建map集合, 封装查询条件
       Map<String, Object> map = new HashMap<>();
       // key是字段名, value是字段值, 多个key是and连接
       map.put("email", "zhangsan@sina.com");
       map.put("age", "18");
       // SELECT id,name,email,age FROM user WHERE email = ? AND age = ?
       List<User> users = userMapper.selectByMap(map);
       users.forEach(user -> {
           System.out.println(user);
       });
   }
   ```

   

## 四、ActiveRecord（AR）

> ActiveRecord 是什么:
>
> * 每一个数据库表对应创建一个类，类的每一个对象实例对应于数据库中表的一行记录; 通常表的每个字段在类中都有相应的 Field;
> *  ActiveRecord 负责**把自己持久化**. 在 ActiveRecord 中封装了对数据库的访问，通过对象自己实现 CRUD，实现优雅的数据库操作。
> *  ActiveRecord 也封装了部分业务逻辑。可以作为业务对象使用。

> **这种操作方式不需要使用其对应接口的实现类**



![image-20220118222715584](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241814517.png)_dept表设计_



![image-20220118223221562](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241814261.png)_bean实体类_



![image-20220118223001203](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241814303.png)_mapper_

> 这种操作方式跟普通的操作方式没什么两样, 就是方法的调用者变了

```java
@Test
public void testARInsert() {
    Dept dept = new Dept();
    dept.setName("销售部");
    dept.setMobile("010-1231233");
    dept.setManager(1);
    // 调用实体对象自己的方法, 完成对象自身到数据库的添加操作
    boolean result = dept.insert();
    System.out.println("AR Insert: " + result);
}
```



## 五、表和列

### 1.主键类型

> 一般就用`AUTO`

![image-20220118223523359](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241814925.png)

### 2.指定表名和字段名

![image-20220118223717821](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241814624.png)_user_address表_

![image-20220118223752714](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241814709.png)_表和字段一一对应_



### 3.驼峰命名

> 列名使用下划线，属性名是驼峰命名方式。MyBatis 默认支持这种规则。

![image-20220118224030472](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241814977.png)_customer表_

![image-20220118224105292](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241814287.png)_bean实体类_



## 六、自定义sql

> 这个自定义sql基本上就跟`mybatis`差不多了

### 1.表定义

![image-20220118224317267](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241814247.png)_student表_

### 2.创建实体

```java
public class Student {

    @TableId(value = "id", type = IdType.AUTO)
    private Integer id;
    private String name;
    private Integer age;
    private String email;
    private Integer status;
	// get | set
}
```

### 3.创建mapper

```java
public interface StudentMapper extends BaseMapper<Student> {
    List<Student> selectByName(String name);
}
```

### 4.新建 sql 映射 xml 文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.teng.mybatisplus.mapper.StudentMapper">
    <select id="selectByName" resultType="com.teng.mybatisplus.bean.Student">
        select id, name, age, email, status
        from student
        where name = #{name}
    </select>
</mapper>
```

### 5.配置 xml 文件位置

`application.yml`

```yml
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
  # 配置路径
  mapper-locations: classpath*:mapper/*Mapper.xml
```

### 6.测试

```java
@Test
public void testSelectByName() {
    // select id, name, age, email, status from student where name = ?
    List<Student> studentList = studentMapper.selectByName("张三");
    // Student{id=1, name='张三', age=18, email='zhangsan@126.com', status=1}
    // Student{id=2, name='张三', age=20, email='zhangsan20@126.com', status=0}
    studentList.forEach(stu -> {
        System.out.println(stu);
    });
}
```



## 七、条件构造器

### 1.allEq

```java
/**
 * 1) Map对象中有key的value是null
 * 使用的是: qw.allEq(param, true);
 * 结果: WHERE (name = ? AND age IS NULL)
 *
 * 2) Map对象中有key的value是null
 * 使用的是: qw.allEq(param, false);
 * 结果: WHERE (name = ?)
 *
 *  结论:
 *      allEq(map, boolean) <默认为true>
 *      true: 处理null值, where条件加入 字段 is null
 *      false: 忽略null值, 不作为where条件
 */
@Test
public void testAllEq() {
    QueryWrapper<Student> qw = new QueryWrapper<>();
    // 组装条件
    Map<String, Object> param = new HashMap<>();
    param.put("name", "张三");
    param.put("age", null);

    qw.allEq(param, false);
    List<Student> students = studentMapper.selectList(qw);
    students.forEach(stu -> System.out.println(stu));
}
```



### 2.基本比较操作

* **eq**
  * 等于 =
* **ne**
  * 不等于 <>
* **gt**
  * 大于 >
* **ge**
  * 大于等于 >=
* **lt**
  * 小于 <
* **le**
  * 小于等于 =<
* **between**
  * BETWEEN 值1 AND 值2
* **notBetween**
  * NOT BETWEEN 值1 AND 值2
* **in**
  * 字段 IN (value.get(0), value.get(1), ...)
* **notIn**
  * 字段 NOT IN (v0, v1, ...)

```java
@Test
public void testEq() {
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    //SELECT id,user_name,password,name,age,email FROM tb_user WHERE password = ? AND age >= ? AND name IN (?,?,?)
    wrapper.eq("password", "123456")
            .ge("age", 20)
            .in("name", "李四", "王五", "赵六");
   List<User> users = this.userMapper.selectList(wrapper);
   users.forEach(user -> System.out.println(user));
}
```

### 3.模糊查询

* **like**
  * LIKE '%值%'
  * 例: `like("name", "王") ---> name like '%王%'`
* **notLike**
  * NOT LIKE '%值%'
  * 例: `notLike("name", "王") ---> name not like '%王%'`
* **likeLeft**
  * LIKE '%值'
  * 例: `likeLeft("name", "王") ---> name like '%王'`
* **likeRight**
  * LIKE '值%'
  * 例: `likeRight("name", "王") ---> name like '王%'`

```java
@Test
public void testWrapper() {
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    // SELECT id,user_name,password,name,age,email FROM tb_user WHERE name LIKE ?
    // Parameters: %曹%(String)
    wrapper.like("name", "曹");
    List<User> users = this.userMapper.selectList(wrapper);
    users.forEach(user -> System.out.println(user));
}
```

### 4.排序

* **orderBy**
  * 排序：ORDER BY 字段, ...
  * 例: `orderBy(true, true, "id", "name") ---> order by id ASC,name ASC`
* **orderByAsc**
  * 排序：ORDER BY 字段, ... ASC
  * 例: `orderByAsc("id", "name") ---> order by id ASC,name ASC`
* **orderByDesc**
  * 排序：ORDER BY 字段, ... DESC
  * 例: `orderByDesc("id", "name") ---> order by id DESC,name DESC`

```java
@Test
public void testWrapper() {
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    //SELECT id,user_name,password,name,age,email FROM tb_user ORDER BY age DESC
    wrapper.orderByDesc("age");
    List<User> users = this.userMapper.selectList(wrapper);
    users.forEach(user -> System.out.println(user));
}
```

![image-20220120173501443](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241814651.png)_orderBy_

### 5.逻辑查询

* **or**
  * 拼接 OR
  * 主动调用 `or `表示紧接着下一个**方法**不是用 `and `连接!(不调用 `or `则默认为使用 `and `连接)
* **and**
  * AND 嵌套
  * 例: `and(i -> i.eq("name", "李白").ne("status", "活着")) ---> and (name = '李白' and status<> '活着')`

```java
@Test
public void testOr(){
    QueryWrapper<Student> qw= new QueryWrapper<>();
    //WHERE name = ? OR age = ?
    qw.eq("name","张三")
            .or()
            .eq("age",22);
    print(qw);
}
```

### 6.select

> 在MP查询中，默认查询所有的字段，如果有需要也可以通过select方法进行指定字段。

```java
@Test
public void testWrapper() {
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    //SELECT id,name,age FROM tb_user WHERE name = ? OR age = ?
    wrapper.eq("name", "李四")
            .or()
            .eq("age", 24)
            .select("id", "name", "age");
    List<User> users = this.userMapper.selectList(wrapper);
    users.forEach(user -> System.out.println(user));
}
```

### 7.其余

* **groupBy**

  ```java
  /**
   * groupBy：分组
   */
  @Test
  public void testGroupby(){
      QueryWrapper<Student> qw = new QueryWrapper<>();
      qw.select("name, count(*) personNumbers"); // select name, count(*) personNumbers,
      qw.groupBy("name");
      print(qw);
  }
  ```

* **inSql()**

  ```java
  /**
   * inSql() : 使用子查询
   */
  @Test
  public void testInSQL(){
      QueryWrapper<Student> qw = new QueryWrapper<>();
      // WHERE age IN (select age from student where id=1)
      qw.inSql("age","select age from student where id=1");
      print(qw);
  }
  ```

  

* **notInSql()**

  ```java
  /**
   * notInSql() : 使用子查询
   */
  @Test
  public void testNotInSQL(){
      QueryWrapper<Student> qw = new QueryWrapper<>();
      // WHERE age NOT IN (select age from student where id=1)
      qw.notInSql("age","select age from student where id=1");
      print(qw);
  }
  ```

* **exists | notExists**

  ```java
  @Test
  public void testExists(){
      QueryWrapper<Student> qw= new QueryWrapper<>();
      //SELECT id,name,age,email,status FROM student
      // WHERE EXISTS (select id from student where age > 20)
      //qw.exists("select id from student where age > 90");
  
      //SELECT id,name,age,email,status FROM student WHERE
      // NOT EXISTS (select id from student where age > 90)
  
      qw.notExists("select id from student where age > 90");
      print(qw);
  }
  ```



## 八、分页查询

1. 配置文件

   ```java
   import com.baomidou.mybatisplus.annotation.DbType;
   import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
   import com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor;
   import org.mybatis.spring.annotation.MapperScan;
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   
   /**
    * @Configuration标注的类就相当于xml文件
    */
   @Configuration
   // 配置mapper文件路径
   @MapperScan("com.teng.mybatisplus.mapper")
   public class MybatisPlusConfig {
   
       /***
        * 定义方法，返回的返回值是java 对象，这个对象是放入到spring容器中
        * 使用@Bean修饰方法
        * @Bean等同于<bean></bean>
        */
       @Bean
       public MybatisPlusInterceptor mybatisPlusInterceptor() {
           MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
           interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.H2));
           return interceptor;
       }
   }
   ```

2. 分页查询

   ```java
   /**
    * 分页:
    * 1. 统计记录数使用count(*)
    *      SELECT COUNT(*) AS total FROM student WHERE (age > ?)
    * 2.实现分页, 在SQL语句的末尾计入 limit 3
    *      SELECT id,name,age,email,status FROM student WHERE (age > ?) LIMIT ?
    */
   @Test
   public void testPage() {
       QueryWrapper<Student> qw = new QueryWrapper<>();
       qw.gt("age", 22);
       IPage<Student> page = new Page<>();
       // 设置分页数据
       page.setCurrent(1); // 第一页
       page.setSize(3); // 每页的记录数
   
       IPage<Student> result = studentMapper.selectPage(page, qw);
   
       // 获取分页后的数据
       List<Student> students = result.getRecords();
       System.out.println("students.size()=" + students.size());
       // 分页的信息
       System.out.println("页数: " + result.getPages());
       System.out.println("总记录数: " + result.getTotal());
       System.out.println("当前页码: " + result.getCurrent());
       System.out.println("每页的记录数: " + result.getSize());
   
   }
   ```

## 九、MP生成器

1. 导入模板引擎

   ```xml
   <!--模版引擎-->
   <dependency>
       <groupId>com.baomidou</groupId>
       <artifactId>mybatis-plus-generator</artifactId>
       <version>3.5.1</version>
   </dependency>
   <dependency>
       <groupId>org.freemarker</groupId>
       <artifactId>freemarker</artifactId>
       <version>2.3.30</version>
   </dependency>
   ```

2. 创建生成类

   

   > 文档：[代码生成器（新）](https://baomidou.com/pages/779a6e/#%E5%BF%AB%E9%80%9F%E5%85%A5%E9%97%A8)

   ```java
   public class AutoMapper {
       public static void main(String[] args) {
           List list = new ArrayList<>();
           list.add("student");
           list.add("dept");
           String url = "jdbc:mysql://localhost:3306/mybatisplus?serverTimezone=UTC";
           FastAutoGenerator.create(url, "root", "333")
                   .globalConfig(builder -> {
                       builder.author("Thousand_Star")
                               .outputDir(System.getProperty("user.dir") + "/src/main/java")  // 设置代码的生成位置, 磁盘的目录
                               .commentDate("yyyy-MM-dd")
                               .fileOverride(); // 覆盖已生成文件
                   })
                   .packageConfig(builder -> {
                       builder.parent("com.teng")
                               .moduleName("order")
                               .pathInfo(Collections.singletonMap(OutputFile.mapperXml, System.getProperty("user.dir") + "/src/main/resources/mapper"));
                   })
                   .strategyConfig(builder -> {
                       builder.addInclude(list).serviceBuilder().formatServiceFileName("%sService").formatServiceImplFileName("%sServiceImpl");
   
                   })
                   .templateEngine(new FreemarkerTemplateEngine()) // 使用Freemarker引擎模板，默认的是Velocity引擎模板
                   .execute();
       }
   }
   ```


---
title: JDBC简单使用
author: 小小千辰
tags:
  - JDBC
  - Java
categories:
  - 千辰的小小笔记
abbrlink: eb13c1a4
date: 2021-06-15 22:38:20
updated: 2021-06-15 22:38:20
---

## Idea连接Mysql

> 最近看很多人被连接数据库折磨的要死，然后我就不要脸的基于好友**大萝卜**的文章二次开发一下。
>
> [萝卜的个人博客](https://blog.csdn.net/m0_49902448)
>
> 我的文章看的不懂的话，可以看萝卜的文章。
>
> [原文连接](https://blog.csdn.net/m0_49902448/article/details/114442083?spm=1001.2014.3001.5501)



### 所需软件及Jar包

> Jar 包下载网站，不过这个网站老是进不去。
>
> [Jar包下载网站](https://commons.apache.org/)
>
> 
>
> **这里是本文所用到的Jar包**
>
> 链接：https://pan.baidu.com/s/1dVeRLGT62fI2mGyjG-zetg 
> 提取码：qfqi 
>
> 然后你还需要一个可视化工具，我用的是Navicat，如果你已经有了SQLyog 请移步本文开头萝卜的文章。
>
> [NaviCat工具及破解工具](https://wws.lanzoux.com/b01tqirzc)
>
> [NaviCat破解教程](https://www.bilibili.com/video/BV1JK411V7bQ?from=search&seid=4049643803512066367)
>
>**数据库切记选择8.0及以上！！！**



### 正文

#### 导入Jar包

* 首先在项目刚建好的那个**src**文件夹下面新建一个文件夹叫做**lib**（当然这个是随意的，不过一般都叫这个）

* 然后将下载好的Jar包复制进去（在文件夹里面里面直接Ctrl+C） 

* 在lib文件夹Ctrl+V 然后会出现如下界面
  * ![image-20210619110619684](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241826508.png)
* 右键lib文件夹
  * ![image-20210619110938268](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241826435.png)
  * ![image-20210619110946586](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241826639.png)

> **到这里就导入成功了！！！**



#### 创建数据库

* 数据库名字：**JDBC**，字符集：**utf8**，排序规则：**utf8_general_ci**
  * ![image-20210619111634576](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241827936.png)
* 创建users表
  * ![image-20210619111808219](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241827379.png)



#### 创建配置文件

> 这些配置文件创建好之后，以后在Java程序中就不用多次写连接数据库的代码了
>
> **配置文件要放在src目录下，切记切记，如果后续出了问题记得检查路径是否出错**
>
> 
>
> > **这个是配置文件可不是txt文件哦，记得打开后缀查看！**
>
> 
>
> * 配置文件一（**db.properties**）
>
>   ```sql
>   driver=com.mysql.cj.jdbc.Driver
>   url=jdbc:mysql://localhost:3306/JDBC?useUnicode=true&characterEncoding=utf8&useSSL=false&serverTimezone=UTC
>   username=root
>   password=333
>   ```
>
>   * 解释
>
>     * 3306后面的JDBC为数据库名称，上文让设置了
>     * username 是用户名
>     * password 是密码  
>     * **一定要改成自己的！！下个文件同理**
>
>   * 配置文件二（**dbcpconfig.properties**）
>
>     * ```sql
>       # 连接设置 这里面的名字, 是DBCP数据源中定义好的
>       driverClassName=com.mysql.cj.jdbc.Driver
>       url=jdbc:mysql://localhost:3306/JDBC?useUnicode=true&characterEncoding=utf8&useSSL=false&serverTimezone=UTC
>       username=root
>       password=333
>                                           
>       #<!-- 初始化连接 -->
>       initialSize=10
>                                           
>       #最大连接数量
>       maxActive=50
>                                           
>       #<!-- 最大空闲连接 -->
>       maxIdle=20
>                                           
>       #<!-- 最小空闲连接 -->
>       minIdle=5
>                                           
>       #<!-- 超时等待时间以毫秒为单位 6000毫秒/1000等于60秒 -->
>       maxWait=60000
>       #JDBC驱动建立连接时附带的连接属性属性的格式必须为这样：【属性名=property;】
>       #注意："user" 与 "password" 两个属性会被明确地传递，因此这里不需要包含他们。
>       connectionProperties=useUnicode=true;characterEncoding=UTF8
>                                           
>       #指定由连接池所创建的连接的自动提交（auto-commit）状态。
>       defaultAutoCommit=true
>                                           
>       #driver default 指定由连接池所创建的连接的只读（read-only）状态。
>       #如果没有设置该值，则“setReadOnly”方法将不被调用。（某些驱动并不支持只读模式，如：Informix）
>       defaultReadOnly=
>                                           
>       #driver default 指定由连接池所创建的连接的事务级别（TransactionIsolation）。
>       #可用值为下列之一：（详情可见javadoc。）NONE,READ_UNCOMMITTED, READ_COMMITTED, REPEATABLE_READ, SERIALIZABLE
>       defaultTransactionIsolation=READ_UNCOMMITTED
>       ```
>
> 
>
> * **最后效果应该是这样的~**
>   * ![image-20210619115648928](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241827721.png)



#### utilis配置

>  在src目录下面创建一个文件夹编写代码测试连接数据库是否成功！

* 一

```java
import java.io.InputStream;
import java.sql.*;
import java.util.Properties;

public class JdbcUtils {
        private static String driver = null;
        private static String url = null;
        private static String username = null;
        private static String password = null;

        static {

            try{
                InputStream in = JdbcUtils.class.getClassLoader().getResourceAsStream("db.properties");
                Properties properties = new Properties();
                properties.load(in);

                driver = properties.getProperty("driver");
                url = properties.getProperty("url");
                username = properties.getProperty("username");
                password = properties.getProperty("password");

                //1.驱动只用加载一次
                Class.forName(driver);

            } catch (Exception e) {
                e.printStackTrace();
            }
        }

        //获取连接
        public static Connection getConnection() throws SQLException {
            return DriverManager.getConnection(url, username, password);
        }
        //释放连接资源
        public static void release(Connection conn, Statement st, ResultSet rs){
            if(conn!=null){
                try {
                    conn.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
            if(st!=null){
                try {
                    st.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
            if(rs!=null){
                try {
                    rs.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
        }
    }

```

* 二

```java
import javax.sql.DataSource;
import java.io.InputStream;
import java.sql.*;
import java.util.Properties;

public class JdbcUtils_DBCP {

    private static DataSource dataSource = null;

    static {

        try{
            InputStream in = JdbcUtils.class.getClassLoader().getResourceAsStream("dbcpconfig.properties");
            Properties properties = new Properties();
            properties.load(in);

            //创建数据源 工厂模式-->创建对象
            dataSource = BasicDataSourceFactory.createDataSource(properties);

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    //获取连接
    public static Connection getConnection() throws SQLException {
        return dataSource.getConnection(); //从数据源中获取连接
    }
    //释放连接资源
    public static void release(Connection conn, Statement st, ResultSet rs){
        if(conn!=null){
            try {
                conn.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        if(st!=null){
            try {
                st.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        if(rs!=null){
            try {
                rs.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
    }
}

```

* 三

```java
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class TestDBCP {
    public static void main(String[] args) {
        Connection conn = null;
        PreparedStatement st = null;

        try {
            conn = JdbcUtils_DBCP.getConnection();

            String sql = "INSERT INTO users(`id`,`password`) values(?,?)";
            st = conn.prepareStatement(sql);//预编译sql, 先写sql, 然后不执行
            //手动给参数赋值
            st.setString(1,"01");//这里一会需要与注册界面连接来
            st.setString(2,"123456");//这里一会需要与注册界面连接来
            //执行
            int i=st.executeUpdate();
            if(i>0){
                System.out.println("插入成功");
            }
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }finally{
            JdbcUtils_DBCP.release(conn, st,null);
        }
    }
}

```

![image-20210619120311120](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241827061.png)

弄完之后长这样~~

然后就可以执行TestDBCP文件，如果输出**插入成功**那么恭喜你连接成功了！！



### 结语

> **关于swing连接数据库请移步萝卜的文章！！**
>
> 点赞关注评论    
>
> 一次就成功！！！








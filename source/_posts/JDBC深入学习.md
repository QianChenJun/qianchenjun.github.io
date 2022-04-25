---
title: JDBC深入学习
author: 小小千辰
tags:
  - JDBC
  - Java
categories:
  - 千辰的小小笔记
abbrlink: f3a010d2
date: 2021-09-07 22:38:34
updated: 2021-09-13 00:00:00
---

> 这个东西很难吗？
>
> ~~难，超级难~~
>
> 别怕不难
>
> 该有的东西文章里面都有，拿走改改就用了（正经人谁手写JDBC啊）。

## 数据库连接

### 方式一

```java
	@Test
    public void testConnection4() throws Exception {

        // 1.提供另外的三个基本信息
        String url = "jdbc:mysql://localhost:3306/test?";
        String user = "root";
        String password = "333";

        // 2.加载Driver （这个也可以省略了...）
        Class.forName("com.mysql.cj.jdbc.Driver");

        // 3.获取连接
        Connection conn = DriverManager.getConnection(url,user,password);
        System.out.println(conn);
    }
```

<br>

### 方式二（最常用）

```java
	/**
     * @Description 获取数据库的连接
     * @ClassName JDBCUtils
     * @Author HIFI
     * @date 2021/9/5 - 23:53
     */
    public static Connection getConnection() throws Exception {
        // 1.读取配置文件中的4个基本信息
        InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("jdbc.properties");

        Properties pros = new Properties();
        pros.load(is);

        String user = pros.getProperty("user");
        String password = pros.getProperty("password");
        String url = pros.getProperty("url");
        String driverClass = pros.getProperty("driverClass");

        // 2.加载驱动
        Class.forName(driverClass);

        // 3.获取连接
        Connection conn = DriverManager.getConnection(url, user, password);

        return conn;
    }
```

> 其中，配置文件【jdbc.properties】：此配置文件声明在工程的src下
>
> ```properties
> user=root
> password=333
> url=jdbc:mysql://localhost:3306/test
> driverClass=com.mysql.cj.jdbc.Driver
> ```

<br>
<br>

## JDBCUtils

```java
import java.io.InputStream;
import java.sql.*;
import java.util.Properties;

/**
 * @Description 通用的连接数据库以及关闭数据库
 * @ClassName JDBCUtils
 * @Author HIFI
 * @date 2021/9/5 - 23:53
 */
public class JDBCUtils {

    /** 获取数据库的连接
     *
     * @return Connection
     * @throws Exception
     */
    public static Connection getConnection() throws Exception {
        // 1.读取配置文件中的4个基本信息
        InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("jdbc.properties");

        Properties pros = new Properties();
        pros.load(is);

        String user = pros.getProperty("user");
        String password = pros.getProperty("password");
        String url = pros.getProperty("url");
        String driverClass = pros.getProperty("driverClass");

        // 2.加载驱动
        Class.forName(driverClass);

        // 3.获取连接
        Connection conn = DriverManager.getConnection(url, user, password);

        return conn;
    }

    /**
     * 关闭连接和Statement的操作
     * @param conn
     * @param ps
     */
    public static void closeResource(Connection conn, Statement ps) {
        try {
            if (ps != null)
                ps.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
        try {
            if (conn != null)
                conn.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    /**
     * 关闭连接和Statement以及ResultSet的操作
     * @param conn
     * @param ps
     */
    public static void closeResource(Connection conn, Statement ps, ResultSet rs) {
        try {
            if (ps != null)
                ps.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
        try {
            if (conn != null)
                conn.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
        try {
            if (rs != null)
                rs.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

<br>
<br>

## PreparedStatement实现CRUD

### PreparedStatement的理解

<div class="success">
    <blockquote>
        <ol>
            <li>PreparedStatement 是Statement的子接口</li>
            <li>An object that represents a precompiled SQL statement. </li>
            <li>可以解决Statement的sql注入问题，拼串问题</ol>
        </ol>
    </blockquote>
</div>

<br>

### 通用增删改

```java
//通用的增删改操作
	//sql中占位符的个数与可变形参的长度相同！
	public void update(String sql,Object ...args){ 
		Connection conn = null;
		PreparedStatement ps = null;
		try {
			//1.获取数据库的连接
			conn = JDBCUtils.getConnection();
			//2.预编译sql语句，返回PreparedStatement的实例
			ps = conn.prepareStatement(sql);
			//3.填充占位符
			for(int i = 0;i < args.length;i++){
				ps.setObject(i + 1, args[i]);//小心参数声明错误！！
			}
			//4.执行
			ps.execute();
		} catch (Exception e) {
			e.printStackTrace();
		}finally{
			//5.资源的关闭
			JDBCUtils.closeResource(conn, ps);
		}
	}
```

<br>

### 通用的查询

#### 查询一条数据

```java
	/*
        查询一条数据
     */
    public <T> T getInstance(Class<T> clazz,String sql, Object... args) {
        Connection conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;
        try {
            // 1.获取数据库连接
            conn = JDBCUtils.getConnection();

            // 2.预编译sql
            ps = conn.prepareStatement(sql);
            for (int i = 0; i < args.length; i++) {
                ps.setObject(i + 1, args[i]);
            }

            // 3.获取结果集
            rs = ps.executeQuery();
            // 获取结果集的元数据：ResultSetMetaData
            ResultSetMetaData rsmd = rs.getMetaData();
            // 通过元数据获取列数
            int columnCount = rsmd.getColumnCount();

            // 4.处理结果集
            if (rs.next()) {
                // 创建对象
                T t = clazz.getDeclaredConstructor().newInstance();// 使用反射创建对象
                // 处理结果集一行数据的每一列
                for (int i = 0; i < columnCount; i++) {
                    // 获取列值
                    Object columnValue = rs.getObject(i + 1);

                    // 获取列名getColumnLabel  这个方法是可以获取别名的
                    String columnLabel = rsmd.getColumnLabel(i + 1);

                    // 给t对象的指定的columnName属性，赋值为columnLabel：通过反射实现
                    Field field = clazz.getDeclaredField(columnLabel); // 拿到columnName属性
                    field.setAccessible(true); // 给权限
                    field.set(t, columnValue);
                }
                return t;
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            // 5.关闭数据库链接
            JDBCUtils.closeResource(conn, ps, rs);
        }
        
        return null;
    }
```

<br>

#### 查询多条数据

```java
    /*
        查询多条数据
     */
    public <T> List<T> getForList(Class<T> clazz, String sql, Object... args) {
        Connection conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;
        try {
            // 1.获取数据库连接
            conn = JDBCUtils.getConnection();

            // 2.预编译sql
            ps = conn.prepareStatement(sql);
            for (int i = 0; i < args.length; i++) {
                ps.setObject(i + 1, args[i]);
            }

            // 3.获取结果集
            rs = ps.executeQuery();
            // 获取结果集的元数据：ResultSetMetaData
            ResultSetMetaData rsmd = rs.getMetaData();
            // 通过元数据获取列数
            int columnCount = rsmd.getColumnCount();
            // 创建集合对象
            ArrayList<T> list = new ArrayList<T>();
            // 4.处理结果集
            while (rs.next()) {
                // 创建对象
                T t = clazz.getDeclaredConstructor().newInstance();// 使用反射创建对象
                // 处理结果集一行数据的每一列
                for (int i = 0; i < columnCount; i++) {
                    // 获取列值
                    Object columnValue = rs.getObject(i + 1);

                    // 获取列名getColumnLabel  这个方法是可以获取别名的
                    String columnLabel = rsmd.getColumnLabel(i + 1);

                    // 给t对象的指定的columnName属性，赋值为columnLabel：通过反射实现
                    Field field = clazz.getDeclaredField(columnLabel); // 拿到columnName属性
                    field.setAccessible(true); // 给权限
                    field.set(t, columnValue);
                }
                // 向集合中添加数据
                list.add(t);
            }
            return list;
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            // 5.关闭数据库链接
            JDBCUtils.closeResource(conn, ps, rs);
        }

        return null;
    }
```

<br>

<br>


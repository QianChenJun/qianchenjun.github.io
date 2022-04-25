---
title: JSP
author: 小小千辰
tags:
  - JSP
  - JavaWeb
categories:
  - 千辰的小小笔记
abbrlink: 78b18e67
date: 2021-10-04 12:07:32
updated: 2021-10-04 12:07:32
---

## 何为jsp

**什么是JSP**

1. jsp 的全换是 java server pages。Java 的服务器页面。
2. jsp 的主要作用是代替 Servlet 程序回传 html 页面的数据。
3. 因为 Servlet 程序回传 html 页面数据是一件非常繁锁的事情。开发成本和维护成本都极高。

> Servlet 回传 html 页面数据的代码：

```java
public class PringHtml extends HttpServlet {
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException,IOException {
		// 通过响应的回传流回传 html 页面数据
		resp.setContentType("text/html; charset=UTF-8");
		PrintWriter writer = resp.getWriter();
		writer.write("<!DOCTYPE html>\r\n");
		writer.write(" <html lang=\"en\">\r\n");
		writer.write(" <head>\r\n");
		writer.write(" <meta charset=\"UTF-8\">\r\n");
		writer.write(" <title>Title</title>\r\n");
		writer.write(" </head>\r\n");
		writer.write(" <body>\r\n");
		writer.write(" 这是 html 页面数据 \r\n");
		writer.write(" </body>\r\n");
		writer.write("</html>\r\n");
		writer.write("\r\n");
	}
}
```

> jsp 回传一个简单 html 页面的代码

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
	<title>Title</title>
</head>
<body>
	这是 html 页面数据
</body>
</html>
```

<br>

## jsp 的本质是什么

**jsp 页面本质上是一个 Servlet 程序。**

当我们第一次访问 jsp 页面的时候。

Tomcat 服务器会帮我们把 jsp 页面翻译成为一个 java 源文件。

并且对它进行编译成 为.class 字节码程序。

我们打开 java 源文件不难发现其里面的内容是：

![image-20211004121535663](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241825779.png)

我们跟踪原代码发现，HttpJspBase 类。它直接地继承了 HttpServlet 类。

也就是说。jsp 翻译出来的 java 类，它间接了继承了 HttpServlet 类。也就是说，翻译出来的是一个 Servlet程序。

![image-20211004121621288](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241825787.png)

**总结，通过翻译的 java 源代码我们就可以得到结果，jsp 就是 Servlet 程序。**

大家也可以去观察翻译出来的 Servlet 程序的源代码，不难发现。其底层实现，也是通过输出流。把 html 页面数据回传给客户端。

```java
public void _jspService(final javax.servlet.http.HttpServletRequest request, final javax.servlet.http.HttpServletResponse response) throws java.io.IOException, javax.servlet.ServletException {
	final java.lang.String _jspx_method = request.getMethod();
	if (!"GET".equals(_jspx_method) && !"POST".equals(_jspx_method) && !"HEAD".equals(_jspx_method)
		&& !javax.servlet.DispatcherType.ERROR.equals(request.getDispatcherType())) {
		response.sendError(HttpServletResponse.SC_METHOD_NOT_ALLOWED, "JSPs only permit GET POST or HEAD");
		return;
	}
    
	final javax.servlet.jsp.PageContext pageContext;
	javax.servlet.http.HttpSession session = null;
	final javax.servlet.ServletContext application;
	final javax.servlet.ServletConfig config;
	javax.servlet.jsp.JspWriter out = null;
	final java.lang.Object page = this;
	javax.servlet.jsp.JspWriter _jspx_out = null;
	javax.servlet.jsp.PageContext _jspx_page_context = null;
    
	try {
		response.setContentType("text/html;charset=UTF-8");
		pageContext = _jspxFactory.getPageContext(this, request, response, null, true, 8192, true);
		_jspx_page_context = pageContext;
		application = pageContext.getServletContext();
		config = pageContext.getServletConfig();
		session = pageContext.getSession();
		out = pageContext.getOut();
		_jspx_out = out;
        
		out.write("\r\n");
		out.write("\r\n");
		out.write("<html>\r\n");
		out.write("<head>\r\n");
		out.write(" <title>Title</title>\r\n");
		out.write("</head>\r\n");
		out.write("<body>\r\n");
		out.write(" a.jsp 页面\r\n");
		out.write("</body>\r\n");
		out.write("</html>\r\n");
	} catch (java.lang.Throwable t) {
		if (!(t instanceof javax.servlet.jsp.SkipPageException)){
			out = _jspx_out;
			if (out != null && out.getBufferSize() != 0)
				try {
					if (response.isCommitted()) {
						out.flush();
					} else {
						out.clearBuffer();
                    }
				} catch (java.io.IOException e) {}
            if (_jspx_page_context != null) _jspx_page_context.handlePageException(t);
			else throw new ServletException(t);
			}
		} finally {
			_jspxFactory.releasePageContext(_jspx_page_context);
		}
	}
```

<br>

## jsp 的三种语法

### jsp 头部的 page 指令

**jsp 的 page 指令可以修改 jsp 页面中一些重要的属性，或者行为。**

`<%@ page contentType="text/html;charset=UTF-8" language="java" %`

| 属性         | 表示                                                         |
| ------------ | ------------------------------------------------------------ |
| language     | jsp 翻译后是什么语言文件。暂时只支持 java                    |
| contentType  | jsp 返回的数据类型是什么。也是源码中 response.setContentType()参数值 |
| pageEncoding | 当前 jsp 页面文件本身的字符集                                |
| import       | 跟 java 源代码中一样。用于导包，导类                         |
| autoFlush    | 设置当 out 输出流缓冲区满了之后，是否自动刷新冲级区。默认值是 true |
| buffer       | 设置 out 缓冲区的大小。默认是8kb                             |
| errorPage    | 设置当 jsp 页面运行时出错，自动跳转去的错误页面路径。        |
| isErrorPage  | 设置当前 jsp 页面是否是错误信息页面。默认是 false。如果是 true可以 |
| session      | 设置访问当前 jsp 页面，是否会创建 HttpSession 对象。默认是true |
| extends      | 设置 jsp 翻译出来的 java 类默认继承谁                        |

<br>

### jsp 中常用的脚本

#### 声明脚本（极少使用）

声明脚本的格式：`<%! 声明 java 代码 %>`

**作用：可以给 jsp 翻译出来的 java 类定义属性和方法甚至是静态代码块。内部类等。**

> 练习

1. 声明类属性

```jsp
<%!
	private Integer id;
	private String name;
    private static Map<String,Object> map;
%>
```

```java
private Integer id;
private String name;
private static Map<String,Object> map;
```

1. 声明类属性

```jsp
<%!
	private Integer id;
	private String name;
    private static Map<String,Object> map;
%>
```

```java
private Integer id;
private String name;
private static Map<String,Object> map;
```

2. 声明static静态代码块

```jsp
<%!
	static {
		map = new HashMap<String,Object>();
		map.put("key1", "value1");
		map.put("key2", "value2");
		map.put("key3", "value3");
	}
%>
```

```java
static {
    map = new HashMap<String,Object>();
    map.put("key1", "value1");
    map.put("key2", "value2");
    map.put("key3", "value3");
}
```

3. 声明类方法

```jsp
<%!
	public int abc(){
		return 12;
	}
%>
```

```java
public int abc(){
    return 12;
}
```

4. 声明内部类

```jsp
<%!
	public static class A {
		private Integer id = 12;
		private String abc = "abc";
	}
%>
```

```java
public static class A {
    private Integer id = 12;
    private String abc = "abc";
}
```

<br>

#### 表达式脚本（常用）

表达式脚本的格式：`<%=表达式%>`

**作用：jsp页面上输出数据**

**表达式脚本的特点**

1. 所有的表达式脚本都会被翻译到`_jspService()` 方法中
2. 表达式脚本都会被翻译成为 `out.print()`输出到页面上
3. 由于表达式脚本翻译的内容都在`_jspService()` 方法中,所以`_jspService()`方法中的对象都可以直接使用。
4. 表达式脚本中的表达式不能以分号结束。

> 练习

1. 输出整数

```jsp
<%=12 %>
```

```java
out.write(12)
```

2. 输出浮点型

```jsp
<%=12.12 %>
```

```java
out.write(12.12)
```

3. 输出字符串

```jsp
<%="我是字符串" %>
```

```java
out.write("我是字符串")
```

4. 输出对象

```jsp
<%=map %>
<%=request.getParameter("username") %>
```

```java
out.write(map)
out.write(request.getParameter("username"))
```

<br>

#### 代码脚本

代码脚本的格式：

```jsp
<%
	java语句
%>
```

**作用：可以在 jsp 页面中，编写我们自己需要的功能（写的是 java 语句）**

**代码脚本的特点**

1. 代码脚本翻译之后都在`_jspService` 方法中
2. 代码脚本由于翻译到`_jspService()`方法中，所以在`_jspService()`方法中的现有对象都可以直接使用
3. 还可以由多个代码脚本块组合完成一个完整的 java 语句
4. 代码脚本还可以和表达式脚本一起组合使用，在 jsp 页面上输出

> 练习

1. if 语句

```jsp
<%
	int i = 13 ;
	if (i == 12) {
%>
	<h1>国哥好帅</h1>
<%
	} else {
%>
	<h1>国哥又骗人了！</h1>
<%
	}
%>
```

```java
int i = 13;
if (i == 12) {
    out.write("\r\n");
    out.write("		<h1>国哥好帅</h1>		")
    out.write("		")
} else {
    out.write("\r\n");
    out.write("		<h1>国哥又骗人了！</h1>		")
    out.write("		")
}
```

2. for 循环语句

```jsp
<table border="1" cellspacing="0">
<%
	for (int j = 0; j < 10; j++) {
%>
	<tr>
		<td>第 <%=j + 1%>行</td>
	</tr>
<%
	}
%>
</table>
```

3. 翻译后 java 文件中_jspService 方法内的代码都可以写

```jsp
<%
	String username = request.getParameter("username");
	System.out.println("用户名的请求参数值是：" + username);
%>
```

<br>

## jsp 九大内置对象

jsp 中的内置对象，是指 Tomcat 在翻译 jsp 页面成为 Servlet 源代码后，内部提供的九大对象，叫内置对象。

| 对象名      | 内容               |
| ----------- | ------------------ |
| request     | 请求对象           |
| response    | 相应对象           |
| pageContext | jsp的上下文对象    |
| session     | 会话对象           |
| application | ServletContext对象 |
| config      | ServletConfig对象  |
| out         | jsp输出流对象      |
| page        | 指向当前jsp的对象  |
| exception   | 异常对象           |

<br>

## jsp 四大域对象

| 对象名                           | 内容                                                       |
| -------------------------------- | ---------------------------------------------------------- |
| pageContext (PageContextImpl 类) | 当前 jsp 页面范围内有效                                    |
| request (HttpServletRequest 类） | 一次请求内有效                                             |
| session (HttpSession 类)         | 一个会话范围内有效（打开浏览器访问服务器，直到关闭浏览器） |
| application (ServletContext 类)  | 整个 web 工程范围内都有效（只要 web 工程不停止，数据都在） |

域对象是可以像 Map 一样存取数据的对象。**四个域对象功能一样。不同的是它们对数据的存取范围。**

虽然四个域对象都可以存取数据。在使用上它们是有优先顺序的。

四个域在使用的时候，优先顺序分别是，他们**从小到大**的范围的顺序。

pageContext ====>>> request ====>>> session ====>>> application

<br>

## jsp 中的 out 输出和 response.getWriter 输出的区别

response 中表示响应，我们经常用于设置返回给客户端的内容（输出） 

out 也是给用户做输出使用的。

![image-20211004123617200](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241825934.png)_输出流程_

由于 jsp 翻译之后，底层源代码都是使用 out 来进行输出。**所以一般情况下，我们在 jsp 页面中统一使用 out 来进行输出。**避免打乱页面输出内容的顺序。

`out.write()` 输出字符串没有问题 

`out.print()` 输出任意数据都没有问题（都转换成为字符串后调用的 write 输出） 

<div class="success">
    <blockquote>
        深入源码，浅出结论：在 jsp 页面中，可以统一使用 out.print()来进行输出
    </blockquote>
</div>

<br>

## jsp 的常用标签

### jsp 静态包含

`<%@ include file=""%>` 就是静态包含

1. file 属性指你要包含的 jsp 页面的路径

2. 地址中的第一个斜杠 / 表示为 `http://ip:port/工程路径/` 映射到代码的 web 目录

**静态包含的特点：**

1. 静态包含不会翻译被包含的 jsp 页面。

2. 静态包含其实是把被包含的 jsp 页面的代码拷贝到包含的位置执行输出。

`<%@ include file="/include/footer.jsp"%>`

### jsp 动态包含

`<jsp:include page=""></jsp:include>` 就是动态包含

1. page 属性是指定你要包含的 jsp 页面的路径

2. 动态包含也可以像静态包含一样。把被包含的内容执行输出到包含位置

**动态包含的特点：**

1. 动态包含会把包含的 jsp 页面也翻译成为 java 代码

2. 动态包含底层代码使用如下代码去调用被包含的 jsp 页面执行输出。

   `JspRuntimeLibrary.include(request, response, "/include/footer.jsp", out, false);`

3. 动态包含，还可以传递参数

```jsp
<jsp:include page="/include/footer.jsp">
	<jsp:param name="username" value="bbj"/>
	<jsp:param name="password" value="root"/>
</jsp:include>
```

![image-20211004124628855](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241825722.png)_动态包含的底层实现原理_

### jsp 转发

`<jsp:forward page=""></jsp:forward>` 就是转发标签

1. 功能就是请求转发
2. page 属性设置请求转发的路径

`<jsp:forward page="/scope2.jsp"**></**jsp:forward>`

<br>

## jsp 练习题

> 练习一：在 jsp 页面输出九九乘法表

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
    <head>
        <title>九九乘法表</title>
        <style type="text/css">
            table {
                width: 650px;
            }
        </style>
    </head>
    <%--练习一：打印九九乘法表--%>
    <body>
        <h1 align="center">九九乘法表</h1>
        <table align="center">
            <tr>
                <% for (int i = 1; i <= 9; i++) { %>
                    <% for (int j = 1; j <= i; j++) { %>
                         <td><%= j + "x" + i + "=" + (i * j)%></td>
                        <% } %>
            </tr>
                    <% } %>

        </table>
    </body>
</html>
```

![image-20211004125034088](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241826898.png)_九九乘法表_

> 练习二：jsp 输出一个表格，里面有10个学生的信息

![image-20211004125314031](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241825135.png)_思路_

1. Student类（JavaBean）

```java
public class Student {
    private Integer id;
    private String name;
    private Integer age;
    private String phone;
}
```

2. SearchStudentServlet程序

```java
public class SearchStudentServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 1.获取请求的参数

        // 2.发sql语句到数据库去查询学生信息
        List<Student> studentList = new ArrayList<>();
        for (int i = 0; i < 10; i++) {
            int temp = i + 1;
            studentList.add(new Student(temp, "name_" + temp, 18 + temp, "phone" + temp));
        }
        // 3.保存查询到的学生信息到request域中
        req.setAttribute("stuList", studentList);

        // 4.请求转发
        req.getRequestDispatcher("/test/showStudent.jsp").forward(req, resp);

    }
}
```

3. showStudent.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
    <head>
        <title>输出十个学生信息</title>
        <style type="text/css">
            table {
                border: 1px solid skyblue;
                width: 600px;
                border-collapse: collapse;
            }
            td {
                border: 1px solid skyblue;
            }
            h1 {
                color: lightskyblue;
            }
        </style>
    </head>
    <body>
        <%
            List<Student> studentList = (List<Student>) request.getAttribute("stuList");
            for (int i = 0; i < 10; i++) {
                int temp = i + 1;
                studentList.add(new Student(temp, "name_" + temp, 18 + temp, "phone" + temp));
            }
        %>
        <h1 align="center">输出10个学生的信息</h1>
        <table align="center">
            <tr>
                <td>编号</td>
                <td>年龄</td>
                <td>姓名</td>
                <td>电话</td>
            </tr>
            <% for (Student student : studentList) {%>
                <tr>
                    <td><%=student.getId()%></td>
                    <td><%=student.getAge()%></td>
                    <td><%=student.getName()%></td>
                    <td><%=student.getPhone()%></td>
                </tr>
            <% } %>
        </table>
    </body>
</html>
```

![image-20211004125339879](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241826853.png)_查询结果_

<br>

## Listener 监听器

> 1. Listener 监听器它是 JavaWeb 的三大组件之一。JavaWeb 的三大组件分别是：Servlet 程序、Filter 过滤器、Listener 监 听器。
>
> 2. Listener 它是 JavaEE 的规范，就是接口 
>
> 3. 监听器的作用是，监听某种事物的变化。然后通过回调函数，反馈给客户（程序）去做一些相应的处理。 

### ServletContextListener 监听器 

> * ServletContextListener 它可以监听 ServletContext 对象的创建和销毁。 
>
> * ServletContext 对象在 web 工程启动的时候创建，在 web 工程停止的时候销毁。 
>
> * 监听到创建和销毁之后都会分别调用 ServletContextListener 监听器的方法反馈。

#### 两个方法

```java
public interface ServletContextListener extends EventListener {
	/**
	* 在 ServletContext 对象创建之后马上调用，做初始化
	*/
	public void contextInitialized(ServletContextEvent sce);
    /**
    * 在 ServletContext 对象销毁之后调用
    */
    public void contextDestroyed(ServletContextEvent sce);
}
```

#### 使用 ServletContextListener 监听器监听 ServletContext 对象。 

1. 编写一个类去实现 ServletContextListener 

2. 实现其两个回调方法 

```java
public class MyServletContextListenerImpl implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent sce) {
    	System.out.println("ServletContext 对象被创建了");
    }
    @Override
    public void contextDestroyed(ServletContextEvent sce) {
    	System.out.println("ServletContext 对象被销毁了");
    }
}
```

3. 到 web.xml 中去配置监听器

```xml
<!--配置监听器-->
<listener>
	<listener-class>com.atguigu.listener.MyServletContextListenerImpl</listener-class>
</listener>
```


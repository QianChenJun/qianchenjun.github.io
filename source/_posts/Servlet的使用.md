---
title: Servlet的使用
author: 小小千辰
tags:
  - Servlet
  - JavaWeb
categories:
  - 千辰的小小笔记
abbrlink: 43aac5ed
date: 2021-10-01 16:37:49
updated: 2021-10-01 16:37:49
---



## Servlet技术

### 什么是Servlet

1. Servlet 是 JavaEE 规范之一。规范就是接口
2. Servlet 就 JavaWeb 三大组件之一。三大组件分别是：Servlet 程序、Filter 过滤器、Listener 监听器。 
3. Servlet 是运行在服务器上的一个 java 小程序，**它可以接收客户端发送过来的请求，并响应数据给客户端**。

<br>



### 手动实现Servlet

1. 编写一个类去实现 Servlet 接口 
2. 实现 service 方法，处理请求，并响应数据 
3. 到 web.xml 中去配置 servlet 程序的访问地址

> `Servlet`程序的实例代码：

```java
public class HelloServlet implements Servlet {
    /**
    * service 方法是专门用来处理请求和响应的
    * @param servletRequest
    * @param servletResponse
    * @throws ServletException
    * @throws IOException
    */
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
    	System.out.println("Hello Servlet 被访问了");
    }
}
```

> `web.xml` 中的配置：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" version="4.0">
    <!-- servlet 标签给 Tomcat 配置 Servlet 程序 -->
    <servlet>
    	<!--servlet-name 标签 Servlet 程序起一个别名（一般是类名） -->
    	<servlet-name>HelloServlet</servlet-name>
    	<!--servlet-class 是 Servlet 程序的全类名-->
    	<servlet-class>com.atguigu.servlet.HelloServlet</servlet-class>
    </servlet>
    
    <!--servlet-mapping 标签给 servlet 程序配置访问地址-->
    <servlet-mapping>
    	<!--servlet-name 标签的作用是告诉服务器，我当前配置的地址给哪个 Servlet 程序使用-->
    	<servlet-name>HelloServlet</servlet-name>
    	<!--  url-pattern 标签配置访问地址 <br/> / 斜杠在服务器解析的时候，表示地址为：http://ip:port/工程路径 <br/> /hello 表示地址为：http://ip:port/工程径/hello <br/>  -->
    	<url-pattern>/hello</url-pattern>
    </servlet-mapping>
</web-app>
```

<br>



### url 地址到 Servlet 程序的访问

![image-20211001164913878](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241812409.png)_url 地址到 Servlet 程序的访问_

<br>

### Servlet的生命周期

1. 执行`Servlet`的构造器方法
2. 执行`init`初始化方法

3. 执行`service`方法
4. 执行`destroy`方法

<div class="success">
    <blockquote>
        第一、二步，是在第一次访问的时候创建 Servlet 程序会调用。<br>
        第三步，每次访问都会调用。<br>
        第四步，在 web 工程停止的时候调用。<br>
    </blockquote>
</div>


<br>

### GET 和 POST 请求的分发处理

```java
public class HelloServlet implements Servlet {
    /**
    * service 方法是专门用来处理请求和响应的
    * @param servletRequest
    * @param servletResponse
    * @throws ServletException
    * @throws IOException
    */
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("3 service === Hello Servlet 被访问了");
        // 类型转换（因为它有 getMethod()方法）
        HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
        // 获取请求的方式
        String method = httpServletRequest.getMethod();
        if ("GET".equals(method)) {
            doGet();
        } else if ("POST".equals(method)) {
            doPost();
        }
    }
    /**
    * 做 get 请求的操作
    */
    public void doGet(){
        System.out.println("get 请求");
        System.out.println("get 请求");
    }
    /**
    * 做 post 请求的操作
    */
    public void doPost(){
        System.out.println("post 请求");
        System.out.println("post 请求");
    }
}
```

<br>

### 通过继承HttpServlet实现Servlet程序

一般实际项目开发中，都是使用继承`HttpServlet`类的方式去实现`Servlet`程序。

1. 编写一个类去继承`HttpServlet`类
2. 根据业务需要重写`doGet()`或`doPost()`方法
3. 到`web.xml`中配置`Servlet`程序的访问地址

> `Servlet`类的代码：

```java
public class HelloServlet2 extends HttpServlet {
    /**
    * doGet()在 get 请求的时候调用
    * @param req
    * @param resp
    * @throws ServletException
    * @throws IOException
    */
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    	System.out.println("HelloServlet2 的 doGet 方法");
    }
    /**
    * doPost（）在 post 请求的时候调用
    * @param req
    * @param resp
    * @throws ServletException
    * @throws IOException
    */
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
    IOException {
    	System.out.println("HelloServlet2 的 doPost 方法");
    }
}
```

> `web.xml` 中的配置

```xml
<servlet>
    <servlet-name>HelloServlet2</servlet-name>
    <servlet-class>com.atguigu.servlet.HelloServlet2</servlet-class>
    </servlet>
<servlet-mapping>
    <servlet-name>HelloServlet2</servlet-name>
    <url-pattern>/hello2</url-pattern>
</servlet-mapping>
```

<br>

### 使用 IDEA 创建 Servlet 程序

<div class="danger">
    <blockquote>
        我跟你讲，新版的idea好像是没有，至少我没有。<br>
		请参考该教程先将该选项弄出来。<br>
		<a href="https://blog.csdn.net/weixin_45518468/article/details/116902257)">IDEA创建Servlet最新方法 Idea版本2021.1及以下（超详细）</a>
    </blockquote>
</div> 

> 菜单：new->Servlet程序

![image-20211001170340019](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241812012.png)

> 配置 Servlet 的信息：

![image-20211001170400736](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241812099.png)

<br>

### Servlet类的继承体系

![image-20211001170430208](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241812700.png)_Servlet类的继承体系_

<br>

<br>

## ServletConfig类

* `ServletConfig` 类从类名上来看，就知道是 `Servlet` 程序的配置信息类。
* `Servlet` 程序和 `ServletConfig` 对象都是由 `Tomcat` 负责创建，我们负责使用。 
* `Servlet` 程序默认是第一次访问的时候创建，`ServletConfig` 是每个 `Servlet` 程序创建时，就创建一个对应的 `ServletConfig` 对象。

<br>

### ServletConfig类的三大作用

1. 可以获取`Servlet`程序的别名，`servlet-name`的值
2. 获取初始化参数 `init-param`
3. 获取`ServletContext`对象

> `web.xml`中的配置

```xml
<!-- servlet 标签给 Tomcat 配置 Servlet 程序 -->
<servlet>
    <!--servlet-name 标签 Servlet 程序起一个别名（一般是类名） -->
    <servlet-name>HelloServlet</servlet-name>
    <!--servlet-class 是 Servlet 程序的全类名-->
    <servlet-class>com.atguigu.servlet.HelloServlet</servlet-class>
    
    <!--init-param 是初始化参数-->
    <init-param>
        <!--是参数名-->
        <param-name>username</param-name>
        <!--是参数值-->
        <param-value>root</param-value>
    </init-param>
    <init-param>
        <param-name>url</param-name>
        <param-value>www.baidu.com</param-value>
    </init-param>
    
</servlet>

<!--servlet-mapping 标签给 servlet 程序配置访问地址-->
<servlet-mapping>
<!--servlet-name 标签的作用是告诉服务器，我当前配置的地址给哪个 Servlet 程序使用-->
    <servlet-name>HelloServlet</servlet-name>
    <!--
    url-pattern 标签配置访问地址 <br/>
    / 斜杠在服务器解析的时候，表示地址为：http://ip:port/工程路径 <br/>
    /hello 表示地址为：http://ip:port/工程路径/hello <br/>
    -->
    <url-pattern>/hello</url-pattern>
</servlet-mapping>
```

> `Servlet`中的代码（仅`init`方法）：

```java
@Override
public void init(ServletConfig servletConfig) throws ServletException {
    System.out.println("2 init 初始化方法");
    // 1、可以获取 Servlet 程序的别名 servlet-name 的值
    System.out.println("HelloServlet 程序的别名是:" + servletConfig.getServletName());
    // 2、获取初始化参数 init-param
    System.out.println("初始化参数 username 的值是;" + servletConfig.getInitParameter("username"));
    System.out.println("初始化参数 url 的值是;" + servletConfig.getInitParameter("url"));
    // 3、获取 ServletContext 对象
    System.out.println(servletConfig.getServletContext());
}
```

![image-20211002221541199](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241812268.png)_运行结果_

<div class="danger">
    <blockquote>
        <b>注意：</b>
    </blockquote>
</div>

![image-20211001171513099](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241813931.png)

<br>

<br>

## ServletContext类

### 什么是 ServletContext？

1. ServletContext 是一个接口，它表示 Servlet 上下文对象 
2. 一个 web 工程，只有一个 ServletContext 对象实例。 
3. ServletContext 对象是一个**域对象**。 
4. ServletContext 是在 web 工程部署启动的时候创建。在 web 工程停止的时候销毁。 

> 什么是域对象？
>
> 域对象，是可以像 Map 一样存取数据的对象，叫域对象。 
>
> 这里的域指的是存取数据的操作范围，整个 web 工程。
>
> |        |     存数据      |     取数据      |      删除数据      |
> | :----: | :-------------: | :-------------: | :----------------: |
> |  Map   |     `put()`     |     `get()`     |     `remove()`     |
> | 域对象 | `setAttrbute()` | `getAttrbute()` | `removeAttrbute()` |

<br>

### ServletContext 类的四个作用

1. 获取 web.xml 中配置的上下文参数 context-param 
2. 获取当前的工程路径，格式: /工程路径 
3. 获取工程部署后在服务器硬盘上的绝对路径 
4. 像 Map 一样存取数据

> `ServletContext` 演示代码：

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws
ServletException, IOException {
    // 1、获取 web.xml 中配置的上下文参数 context-param
    ServletContext context = getServletConfig().getServletContext();
    String username = context.getInitParameter("username");
    System.out.println("context-param 参数 username 的值是:" + username);
    System.out.println("context-param 参数 password 的值是:" + context.getInitParameter("password"));
    // 2、获取当前的工程路径，格式: /工程路径
    System.out.println( "当前工程路径:" + context.getContextPath() );
    // 3、获取工程部署后在服务器硬盘上的绝对路径
    /**
    * / 斜杠被服务器解析地址为:http://ip:port/工程名/ 映射到 IDEA 代码的 web 目录<br/>
    */
    System.out.println("工程部署的路径是:" + context.getRealPath("/"));
    System.out.println("工程下 css 目录的绝对路径是:" + context.getRealPath("/css"));
    System.out.println("工程下 imgs 目录 1.jpg 的绝对路径是:" + context.getRealPath("/imgs/1.jpg"));
}
```

> `web.xml` 中的配置：

```xml
<!--context-param 是上下文参数(它属于整个 web 工程)-->
<context-param>
    <param-name>username</param-name>
    <param-value>context</param-value>
</context-param>
<!--context-param 是上下文参数(它属于整个 web 工程)-->
<context-param>
    <param-name>password</param-name>
    <param-value>root</param-value>
</context-param>
```

<br>

> ServletContext 像 Map 一样存取数据
>
> ContextServlet1 代码：

```java
public class ContextServlet1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 获取ServletContext对象
        ServletContext context = getServletContext();
        System.out.println(context);
        
        // 没有设置前怎么获取都是null
        System.out.println("Context1 中获取域数据key1的值是："+context.getAttribute("key1"));

        context.setAttribute("key1", "value1");

        System.out.println("Context1 中获取域数据key1的值是："+context.getAttribute("key1"));
        
    }
}
```

![image-20211002221253765](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241812184.png)_运行结果_

> ContextServlet2 代码：

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException,
IOException {
    ServletContext context = getServletContext();
    System.out.println(context);
    System.out.println("Context2 中获取域数据 key1 的值是:"+ context.getAttribute("key1"));
}
```

![image-20211003162424323](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241812603.png)_运行结果_

<br>

<br>

## HTTP 协议

### 什么是 HTTP 协议

什么是协议?

协议是指双方，或多方，相互约定好，大家都需要遵守的规则，叫协议

所谓 HTTP 协议，就是指，客户端和服务器之间通信时，发送的数据，需要遵守的规则，叫 HTTP 协议

HTTP 协议中的数据又叫报文。

<br>

### 请求的 HTTP 协议格式

> 客户端给服务器发送数据叫请求。
>
> 服务器给客户端回传数据叫响应。

#### GET 请求

1. 请求行
   1. 请求的方式		`GET`
   2. 请求的资源路径		`[+?+请求参数]`
   3. 请求的协议的版本号		`HTTP/1.1`
2. 请求头
   1. `key:value` 组成 不同的键值对，表示不同的含义。

![image-20211002222252805](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241812954.png)_一次完整的 GET 请求_

<br>

#### POST请求

1. 请求行
   1. 请求的方式		`POST`
   2. 请求的资源路径		`[+?+请求参数]`
   3. 请求的协议的版本号		`HTTP/1.1`
2. 请求头
   1. `key:value`		不同的请求头，有不同的含义

![image-20211002222624619](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241812427.png)_一次完整的 POST 请求_

<br>

#### 常用的请求头的说明

1. `Accept`：表示客户端可以接收的数据类型
2. `Accpet-Languege`：表示客户端可以接收的语言类型
3. `User-Agent`：表示客户端浏览器的信息
4. `Host`：表示请求时的服务器 ip 和端口号

<br>

#### 哪些是 GET 请求，哪些是 POST

1. `GET` 请求有哪些
   1. `form` 标签 `method=get`
   2. `a` 标签
   3. `link` 标签引入 `css`
   4. `Script` 标签引入 `js` 文件
   5. `img` 标签引入图片
   6. `iframe` 引入 html 页面
   7. 在浏览器地址栏中输入地址后敲回车

1. `POST`请求有哪些
   1. `form` 标签 `method=post`

<br>

### 响应的 HTTP 协议格式

1. 响应行
   1. 响应的协议和版本号
   2. 响应状态码
   3. 相应状态描述符
2. 响应头
   1. `key:value`	不同的响应头，有其不同的含义
3. 响应体 --->> 就是回传给客户端的数据

![image-20211002223326908](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241813024.png)_响应_

<br>

### 常用的状态码

`[200]` 表示请求成功

`[302]` 表示请求重定向

`[404]` 表示请求服务器已经收到了，但是你要的数据不存在（请求地址错误） 

`[500]` 表示服务器已经收到请求，但是服务器内部错误（代码错误）

<br>



### MIME 类型说明

MIME 是 HTTP 协议中数据类型。

MIME 的英文全称是"Multipurpose Internet Mail Extensions" 多功能 Internet 邮件扩充服务。

MIME 类型的格式是“大类型/小 类型”，并与某一种文件的扩展名相对应。

常见的MIME类型：

| 文件               | MIME类型     |                                   |
| ------------------ | ------------ | --------------------------------- |
| 超文本标记语言文本 | .html , .htm | text/html                         |
| 普通文本           | .txt         | text/plain                        |
| RTF 文本           | .rtf         | application/rtf                   |
| GIF 图形           | .gif         | image/gif                         |
| JPEG 图形          | .jpeg，.jpg  | image/jpeg au                     |
| 声音文件           | .au          | audio/basic                       |
| MIDI 音乐文件      | mid,.midi    | audio/midi,audio/x-midi RealAudio |
| 音乐文件           | .ra，.ram    | audio/x-pn-realaudio              |
| MPEG 文件          | .mpg，.mpeg  | video/mpeg                        |

> 谷歌浏览器查看 HTTP 协议的方式

![image-20211002223702379](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241813977.png)_谷歌浏览器_



<br>

## HttpServletRequest 类

<div class="success">
    <blockquote>
        每次只要有请求进入 Tomcat 服务器，Tomcat 服务器就会把请求过来的 HTTP 协议信息解析好封装到 Request 对象中。 然后传递到 `service` 方法（`doGet` 和 `doPost`）中给我们使用。我们可以通过 HttpServletRequest 对象，获取到所有请求的 信息。
    </blockquote>
</div>

<br>

### HttpServletRequest 类的常用方法 

| 方法名                 | 作用                                                         |
| ---------------------- | ------------------------------------------------------------ |
| getRequestURI()        | 获取请求的资源路径                                           |
| getRequestURL()        | 获取请求的统一资源定位符（绝对路径）                         |
| getRemoteHost()        | 获取客户端的 ip 地址                                         |
| getHeader()            | 获取请求头                                                   |
| getParameter()         | 获取请求的参数                                               |
| getParameterValues()   | 获取请求的参数（多个值的时候使用）                           |
| getMethod()            | 获取请求的方式 GET 或 POST viii. setAttribute(key, value); 设置域数据 |
| getAttribute(key)      | 获取域数据                                                   |
| getRequestDispatcher() | 获取请求转发对象                                             |

> 常用API示例代码

```java
public class RequestAPIServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
    IOException {
        // i.getRequestURI() 获取请求的资源路径
        System.out.println("URI => " + req.getRequestURI());
        // ii.getRequestURL() 获取请求的统一资源定位符（绝对路径）
        System.out.println("URL => " + req.getRequestURL());
        // iii.getRemoteHost() 获取客户端的 ip 地址
        /**
        * 在 IDEA 中，使用 localhost 访问时，得到的客户端 ip 地址是 ===>>> 127.0.0.1<br/>
        * 在 IDEA 中，使用 127.0.0.1 访问时，得到的客户端 ip 地址是 ===>>> 127.0.0.1<br/>
        * 在 IDEA 中，使用 真实 ip 访问时，得到的客户端 ip 地址是 ===>>> 真实的客户端 ip 地址<br/>
        */
        System.out.println("客户端 ip 地址 => " + req.getRemoteHost());
        // iv.getHeader() 获取请求头
        System.out.println("请求头 User-Agent ==>> " + req.getHeader("User-Agent"));
        // vii.getMethod() 获取请求的方式 GET 或 POST
        System.out.println( "请求的方式 ==>> " + req.getMethod() );
    }
}
```

<br>

### 如何获取请求参数

> 表单：

```html
<body>
    <form action="http://localhost:8080/07_servlet/parameterServlet" method="get">
        用户名：<input type="text" name="username"><br/>
        密码：<input type="password" name="password"><br/>
        兴趣爱好：<input type="checkbox" name="hobby" value="cpp">C++
        <input type="checkbox" name="hobby" value="java">Java
        <input type="checkbox" name="hobby" value="js">JavaScript<br/>
        <input type="submit">
    </form>
</body>
```

> Java：

```java
public class ParameterServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
    IOException {
        // 获取请求参数
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        String[] hobby = req.getParameterValues("hobby");
        System.out.println("用户名：" + username);
        System.out.println("密码：" + password);
        System.out.println("兴趣爱好：" + Arrays.asList(hobby));
    }
}
```

<br>

### doGet 请求的中文乱码解决：

```java
// 获取请求参数
String username = req.getParameter("username");

//1 先以 iso8859-1 进行编码
//2 再以 utf-8 进行解码
username = new String(username.getBytes("iso-8859-1"), "UTF-8");
```

<br>

### doPOST 请求的中文乱码解决：

```java
@Override
protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
IOException {
    // 设置请求体的字符集为 UTF-8，从而解决 post 请求的中文乱码问题
    req.setCharacterEncoding("UTF-8");
    System.out.println("-------------doPost------------");
    
    // 获取请求参数
    String username = req.getParameter("username");
    String password = req.getParameter("password");
    String[] hobby = req.getParameterValues("hobby");
    System.out.println("用户名：" + username);
    System.out.println("密码：" + password);
    System.out.println("兴趣爱好：" + Arrays.asList(hobby));
}
```

<br>

### 请求的转发

<div class="success">
    <blockquote>
        请求转发是指，服务器收到请求后，从一次资源跳转到另一个资源的操作叫请求转发。
    </blockquote>
</div>

![image-20211002225808422](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241813553.png)_请求转发_

> Servlet1 代码：

```java
public class Servlet1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
    IOException {
        // 获取请求的参数（办事的材料）查看
        String username = req.getParameter("username");
        System.out.println("在 Servlet1（柜台 1）中查看参数（材料）：" + username);
        // 给材料 盖一个章，并传递到 Servlet2（柜台 2）去查看
        req.setAttribute("key1","柜台 1 的章");
        // 问路：Servlet2（柜台 2）怎么走
        /**
        * 请求转发必须要以斜杠打头，/ 斜杠表示地址为：http://ip:port/工程名/ , 映射到 IDEA 代码的 web 目录
        <br/>
        *
        */
        RequestDispatcher requestDispatcher = req.getRequestDispatcher("/servlet2");
        // RequestDispatcher requestDispatcher = req.getRequestDispatcher("http://www.baidu.com");
        // 走向 Sevlet2（柜台 2）
        requestDispatcher.forward(req,resp);
    }
}
```

> Servlet2 代码：

```java
public class Servlet2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
    IOException {
        // 获取请求的参数（办事的材料）查看
        String username = req.getParameter("username");
        System.out.println("在 Servlet2（柜台 2）中查看参数（材料）：" + username);
        // 查看 柜台 1 是否有盖章
        Object key1 = req.getAttribute("key1");
        System.out.println("柜台 1 是否有章：" + key1);
        // 处理自己的业务
        System.out.println("Servlet2 处理自己的业务 ");
    }
}
```

<br>

### base 标签的使用

![image-20211002225947853](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241813198.png)_base标签_

```html
<!DOCTYPE html>
<html lang="zh_CN">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--base 标签设置页面相对路径工作时参照的地址 href 属性就是参数的地址值-->
    <base href="http://localhost:8080/07_servlet/a/b/">
</head>
<body>
    这是 a 下的 b 下的 c.html 页面<br/>
    <a href="../../index.html">跳回首页</a><br/>
</body>
</html>
```

<br>

### Web 中的相对路径和绝对路径

在 javaWeb 中，路径分为相对路径和绝对路径两种：

1. 相对路径
   1. `[.]`  &nbsp; &nbsp;&nbsp;&nbsp;     &nbsp;&nbsp;&nbsp;表示当前目录
   2. `[..]`&nbsp;            &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;表示上一级目录
   3. `[资源名]`&nbsp;&nbsp;&nbsp;表示当前目录/资源名
2. 绝对路径
   1. `http://ip:port/工程路径/资源路径`



> 在实际开发中，路径都使用绝对路径，而不简单的使用相对路径。
>
> 1. 绝对路径
> 2. base+相对

<br>

### web 中 / 斜杠的不同意义

在 web 中 `/` 斜杠 是一种绝对路径

1. `/` 斜杠 如果被浏览器解析，得到的地址是：`http://ip:port/`
   1. `<a href="/">斜杠</a>`

2. `/` 斜杠，如果被服务器解析，得到的地址是：`http://ip:port/工程路径`
   1. `/servlet1`
   2. `servletContext.getRealPath(“/”);`
   3. `request.getRequestDispatcher(“/”);`

**特殊情况： response.sendRediect(“/”); 把斜杠发送给浏览器解析。得到 `http://ip:port/`**

<br>

## HttpServletResponse 类

<div class="success">
    <blockquote>
        HttpServletResponse 类和 HttpServletRequest 类一样。每次请求进来，Tomcat 服务器都会创建一个 Response 对象传 递给 Servlet 程序去使用。<br>
        HttpServletRequest 表示请求过来的信息，<br>
        HttpServletResponse 表示所有响应的信息，<br>
        我们如果需要设置返回给客户端的信息，都可以通过 HttpServletResponse 对象来进行设置
    </blockquote>
</div>

<br>

### 两个输出流的说明

| 字节流 | getOutputStream(); | 常用于下载（传递二进制数据） |
| ------ | ------------------ | ---------------------------- |
| 字符流 | getWriter();       | 常用于回传字符串（常用）     |

两个流同时只能使用一个。

使用了字节流，就不能再使用字符流，反之亦然，否则就会报错。

![image-20211002232024842](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241813787.png)_同时使用的报错信息_

<br>

### 如何往客户端回传数据

> 要求：往客户端回传字符串数据

```java
public class ResponseIOServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
    IOException {
        // 要求 ： 往客户端回传 字符串 数据。
        PrintWriter writer = resp.getWriter();
        writer.write("response's content!!!");
    }
}
```

<br>

### 响应的乱码解决

> 解决响应中文乱码方案一（不推荐使用）：

```java
// 设置服务器字符集为 UTF-8
resp.setCharacterEncoding("UTF-8");
// 通过响应头，设置浏览器也使用 UTF-8 字符集
resp.setHeader("Content-Type", "text/html; charset=UTF-8");
```

> 解决响应中文乱码方案二（推荐）：

```java
// 它会同时设置服务器和客户端都使用 UTF-8 字符集，还设置了响应头
// 此方法一定要在获取流对象之前调用才有效
resp.setContentType("text/html; charset=UTF-8");
```

<br>

### 请求重定向

<div class="success">
    <blockquote>
        请求重定向，是指客户端给服务器发请求，然后服务器告诉客户端说。我给你一些地址。你去新地址访问。叫请求重定向（因为之前的地址可能已经被废弃）。
    </blockquote>
</div>

![image-20211002232252541](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241813235.png)_何为请求重定向_

> 请求重定向的第一种方案：

```java
// 设置响应状态码 302 ，表示重定向，（已搬迁） 
resp.setStatus(302); 
// 设置响应头，说明 新的地址在哪里 
resp.setHeader("Location", "http://localhost:8080");
```

>  请求重定向的第二种方案（推荐使用）：

```java
resp.sendRedirect("http://localhost:8080");
```

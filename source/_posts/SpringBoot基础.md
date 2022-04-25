---
title: SpringBoot基础
author: 小小千辰
tags:
  - Spring
  - SpringBoot
categories:
  - 千辰的小小笔记
abbrlink: a8fdf5f8
date: 2022-01-23 00:00:00
updated: 2022-01-23 00:00:00
---



## 一、创建一个Spring项目

### 1. 创建新模块，选择Spring Initializr，并配置模块相关基础信息

![image-20211116125259385](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241810102.png)

* <font color="#ff0000"><b>注意</b></font>：这个过程是需要联网的。JDK选择本机安装的即可（推荐8或11）

### 2.选择当前模块需要使用的技术集

![image-20211116125615728](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241810610.png)

### 3.开发控制器类

```java
//Rest模式
@RestController
@RequestMapping("/books")
public class BookController {
    @GetMapping
    public String getById(){
        System.out.println("springboot is running...");
        return "springboot is running...";
    }
}
```

### 4.运行自动生成的Application类

![image-20211116130152452](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241810720.png)

```
访问路径：   http://localhost:8080/books
```



### 5.总结

1. 开发SpringBoot程序可以根据向导进行联网快速制作
2. SpringBoot程序需要基于JDK8以上版本进行制作
3. SpringBoot程序中需要使用何种功能通过勾选选择技术，也可以手工添加对应的要使用的技术（后期讲解）
4. 运行SpringBoot程序通过运行Application程序入口进行

## 二、SpringBoot几大关键因素

### 1.parent

> SpringBoot将所有的技术版本的<font color="#ff0000"><b>常见使用方案</b></font>都给开发者整理了出来，以后开发者使用时直接用它提供的版本方案，就不用担心冲突问题了，相当于SpringBoot做了无数个技术版本搭配的列表，这个技术搭配列表的名字叫做<font color="#ff0000"><b>parent</b></font>。

<font color="#ff0000"><b>注意</b></font>：parent只是定义出来，并没有直接使用，仅仅是给了开发者一个说明书，需要使用什么依赖还得是开发者进行选择。

#### Springboot实现parent

* 项目中的pom.xml中继承了一个坐标

  ```xml
  <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-parent</artifactId>
      <version>2.5.4</version>
  </parent>
  ```

* 打开后可以查阅到其中又继承了一个坐标

  ```xml
  <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-dependencies</artifactId>
      <version>2.5.4</version>
  </parent>
  ```

* 这个坐标中定义了两组信息，第一组是各式各样的依赖版本号属性，下面列出依赖版本属性的局部，可以看的出来，定义了若干个技术的依赖版本号

  ```xml
  <properties>
      <activemq.version>5.16.3</activemq.version>
      <aspectj.version>1.9.7</aspectj.version>
      <assertj.version>3.19.0</assertj.version>
      <commons-codec.version>1.15</commons-codec.version>
      <commons-dbcp2.version>2.8.0</commons-dbcp2.version>
      <commons-lang3.version>3.12.0</commons-lang3.version>
      <commons-pool.version>1.6</commons-pool.version>
      <commons-pool2.version>2.9.0</commons-pool2.version>
      <h2.version>1.4.200</h2.version>
      <hibernate.version>5.4.32.Final</hibernate.version>
      <hibernate-validator.version>6.2.0.Final</hibernate-validator.version>
      <httpclient.version>4.5.13</httpclient.version>
      <jackson-bom.version>2.12.4</jackson-bom.version>
      <javax-jms.version>2.0.1</javax-jms.version>
      <javax-json.version>1.1.4</javax-json.version>
      <javax-websocket.version>1.1</javax-websocket.version>
      <jetty-el.version>9.0.48</jetty-el.version>
      <junit.version>4.13.2</junit.version>
  </properties>
  ```

* 第二组是各式各样的的依赖坐标信息，可以看出依赖坐标定义中没有具体的依赖版本号，而是引用了第一组信息中定义的依赖版本属性值

  ```xml
  <dependencyManagement>
      <dependencies>
          <dependency>
              <groupId>org.hibernate</groupId>
              <artifactId>hibernate-core</artifactId>
              <version>${hibernate.version}</version>
          </dependency>
          <dependency>
              <groupId>junit</groupId>
              <artifactId>junit</artifactId>
              <version>${junit.version}</version>
          </dependency>
      </dependencies>
  </dependencyManagement>
  ```

<font color="#ff0000"><b>关注</b></font>：上面的依赖坐标定义是出现在<dependencyManagement>标签中的，其实是对引用坐标的依赖管理，并不是实际使用的坐标。因此当你的项目中继承了这组parent信息后，在不使用对应坐标的情况下，前面的这组定义是不会具体导入某个依赖的

#### 总结

1. 开发SpringBoot程序要继承spring-boot-starter-parent
2. spring-boot-starter-parent中定义了若干个依赖管理
3. 继承parent模块可以避免多个依赖使用相同技术时出现依赖版本冲突
4. 继承parent的形式也可以采用引入依赖的形式实现效果

### 2.starter

> SpringBoot把所有的技术使用的固定搭配格式都给开发出来，以后你用某个技术，就不用一次写一堆依赖了，还容易写错，我给你做一个东西，代表一堆东西，开发者使用的时候，直接用我做好的这个东西就好了，对于这样的固定技术搭配，SpringBoot给它起了个名字叫做<font color="#ff0000"><b>starter</b></font>。
>
> starter定义了使用某种技术时对于依赖的固定搭配格式，也是一种最佳解决方案，<font color="#ff0000"><b>使用starter可以帮助开发者减少依赖配置</b></font>

#### starter与parent的区别

​	<font color="#ff0000"><b>starter</b></font>是一个坐标中定了若干个坐标，以前写多个的，现在写一个，<font color="#ff0000"><b>是用来减少依赖配置的书写量的</b></font>

​	<font color="#ff0000"><b>parent</b></font>是定义了几百个依赖版本号，以前写依赖需要自己手工控制版本，现在由SpringBoot统一管理，这样就不存在版本冲突了，<font color="#ff0000"><b>是用来减少依赖冲突的</b></font>

#### 实际开发应用方式

- 实际开发中如果需要用什么技术，先去找有没有这个技术对应的starter

  - 如果有对应的starter，直接写starter，而且无需指定版本，版本由parent提供
  - 如果没有对应的starter，手写坐标即可

- 实际开发中如果发现坐标出现了冲突现象，确认你要使用的可行的版本号，使用手工书写的方式添加对应依赖，覆盖SpringBoot提供给我们的配置管理

  - 方式一：直接写坐标

  - 方式二：覆盖<properties>中定义的版本号，就是下面这堆东西了，哪个冲突了覆盖哪个就OK了

    ```xml
    <properties>
        <activemq.version>5.16.3</activemq.version>
        <aspectj.version>1.9.7</aspectj.version>
        <assertj.version>3.19.0</assertj.version>
        <commons-codec.version>1.15</commons-codec.version>
        <commons-dbcp2.version>2.8.0</commons-dbcp2.version>
        <commons-lang3.version>3.12.0</commons-lang3.version>
        <commons-pool.version>1.6</commons-pool.version>
        <commons-pool2.version>2.9.0</commons-pool2.version>
        <h2.version>1.4.200</h2.version>
        <hibernate.version>5.4.32.Final</hibernate.version>
        <hibernate-validator.version>6.2.0.Final</hibernate-validator.version>
        <httpclient.version>4.5.13</httpclient.version>
        <jackson-bom.version>2.12.4</jackson-bom.version>
        <javax-jms.version>2.0.1</javax-jms.version>
        <javax-json.version>1.1.4</javax-json.version>
        <javax-websocket.version>1.1</javax-websocket.version>
        <jetty-el.version>9.0.48</jetty-el.version>
        <junit.version>4.13.2</junit.version>
    </properties>
    ```

#### 总结

1. 开发SpringBoot程序需要导入坐标时通常导入对应的starter
2. 每个不同的starter根据功能不同，通常包含多个依赖坐标
3. 使用starter可以实现快速配置的效果，达到简化配置的目的

### 3.引导类

#### 引导类的作用

> 配置说完了，我们发现SpringBoot确实帮助我们减少了很多配置工作，下面说一下程序是如何运行的。目前程序运行的入口就是SpringBoot工程创建时自带的那个类了，带有main方法的那个类，运行这个类就可以启动SpringBoot工程的运行

```java
@SpringBootApplication
public class Springboot0101QuickstartApplication {
    public static void main(String[] args) {
        SpringApplication.run(Springboot0101QuickstartApplication.class, args);
    }
}
```

​	SpringBoot本身是为了加速Spring程序的开发的，而Spring程序运行的基础是需要创建自己的Spring容器对象（IoC容器）并将所有的对象交给Spring的容器管理，也就是一个一个的Bean。那还了SpringBoot加速开发Spring程序，这个容器还在吗？这个疑问不用说，一定在。当前这个类运行后就会产生一个Spring容器对象，并且可以将这个对象保存起来，通过容器对象直接操作Bean。

```java
@SpringBootApplication
public class Springboot0101QuickstartApplication {
    public static void main(String[] args) {
        ConfigurableApplicationContext ctx = SpringApplication.run(Springboot0101QuickstartApplication.class, args);
        BookController bean = ctx.getBean(BookController.class);
        System.out.println("bean======>" + bean);
    }
}
```

​	通过上述操作不难看出，其实SpringBoot程序启动还是创建了一个Spring容器对象。这个类在SpringBoot程序中是所有功能的入口，称这个类为<font color="#ff0000"><b>引导类</b></font>。

​	作为一个引导类最典型的特征就是当前类上方声明了一个注解<font color="#ff0000"><b>@SpringBootApplication</b></font>

#### 总结

1. SpringBoot工程提供引导类用来启动程序
2. SpringBoot工程启动后创建并初始化Spring容器



## 三、SpringBoot的基础配置

### 1.属性配置

> SpringBoot通过配置文件application.properties就可以修改默认的配置

配置文件里面有着大量的提示，只需要记得大概名字即可。

<img src="https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241810511.png" alt="image-20211123165719091" />

1. SpringBoot程序可以在application.properties文件中进行属性配置
2. application.properties文件中只要输入要配置的属性关键字就可以根据提示进行设置
3. SpringBoot将配置信息集中在一个文件中写，不管你是服务器的配置，还是数据库的配置，总之都写在一起，逃离一个项目十几种配置文件格式的尴尬局面

**[配置文件官方文档](https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#application-properties)**

#### 常用配置

**设置服务器端口**

```properties
server.port=80
```

**关闭运行日志图表（banner)**

```properties
spring.main.banner-mode=off
```

**设置运行日志的显示级别**

```properties
logging.level.root=debug
```

### 2.配置文件分类

#### 分类

- application.properties（properties格式）

```properties
server.port=80
```

- application.yml（yml格式）

```yml
server:
  port: 81
```

- application.yaml（yaml格式）

```yaml
server:
  port: 82
```

**其中yml格式是主流格式**



#### 优先级

1. 配置文件间的加载优先级	properties（最高）>  yml  >  yaml（最低）
2. 不同配置文件中相同配置按照加载优先级相互覆盖，不同配置文件中不同配置全部保留 



### 3.yaml文件

SpringBoot的配置以后主要使用yml结尾的这种文件格式，并且在书写时可以通过提示的形式加载正确的格式。但是这种文件还是有严格的书写格式要求的。下面就来说一下具体的语法格式。

​	YAML（YAML Ain't Markup Language），一种数据序列化格式。具有容易阅读、容易与脚本语言交互、以数据为核心，重数据轻格式的特点。常见的文件扩展名有两种：

- .yml格式（主流）

- .yaml格式

  对于文件自身在书写时，具有严格的语法格式要求，具体如下：

1. 大小写敏感
2. 属性层级关系使用多行描述，**每行结尾使用冒号结束**
3. 使用缩进表示层级关系，同层级左侧对齐，只允许使用空格（不允许使用Tab键）
4. 属性值前面添加空格（属性名与属性值之间使用冒号+空格作为分隔）
5. #号 表示注释

  上述规则不要死记硬背，按照书写习惯慢慢适应，并且在Idea下由于具有提示功能，慢慢适应着写格式就行了。核心的一条规则要记住，<font color="#ff0000"><b>数据前面要加空格与冒号隔开</b></font>

​	下面列出常见的数据书写格式，熟悉一下

```yaml
boolean: TRUE  						#TRUE,true,True,FALSE,false，False均可
float: 3.14    						#6.8523015e+5  #支持科学计数法
int: 123       						#0b1010_0111_0100_1010_1110    #支持二进制、八进制、十六进制
null: ~        						#使用~表示null
string: HelloWorld      			#字符串可以直接书写
string2: "Hello World"  			#可以使用双引号包裹特殊字符
date: 2018-02-17        			#日期必须使用yyyy-MM-dd格式
datetime: 2018-02-17T15:02:31+08:00  #时间和日期之间使用T连接，最后使用+代表时区
```

​	此外，yaml格式中也可以表示数组，在属性名书写位置的下方使用减号作为数据开始符号，每行书写一个数据，减号与数据间空格分隔

```yaml
subject:
	- Java
	- 前端
	- 大数据
enterprise:
	name: itcast
    age: 16
    subject:
    	- Java
        - 前端
        - 大数据
likes: [王者荣耀,刺激战场]			#数组书写缩略格式
users:							 #对象数组格式一
  - name: Tom
   	age: 4
  - name: Jerry
    age: 5
users:							 #对象数组格式二
  -  
    name: Tom
    age: 4
  -   
    name: Jerry
    age: 5			    
users2: [ { name:Tom , age:4 } , { name:Jerry , age:5 } ]	#对象数组缩略格式
```

**总结**

1. yaml语法规则
   - 大小写敏感
   - 属性层级关系使用多行描述，每行结尾使用冒号结束
   - 使用缩进表示层级关系，同层级左侧对齐，只允许使用空格（不允许使用Tab键）
   - 属性值前面添加空格（属性名与属性值之间使用冒号+空格作为分隔）
   - #号 表示注释
2. 注意属性名冒号后面与数据之间有一个**空格**
3. 字面值、对象数据格式、数组数据格式



### 4.yaml文件数据读取

对于yaml文件中的数据，其实你就可以想象成这就是一个小型的数据库，里面保存有若干数据，每个数据都有一个独立的名字，如果你想读取里面的数据，肯定是支持的，下面就介绍3种读取数据的方式

#### 读取单一数据

​	yaml中保存的单个数据，可以使用Spring中的注解直接读取，使用@Value可以读取单个数据，属性名引用方式：<font color="#ff0000"><b>${一级属性名.二级属性名……}</b></font>

<img src="https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241811954.png" alt="image-20211126180433356" style="zoom:80%;" />

​	记得使用@Value注解时，要将该注入写在某一个指定的Spring管控的bean的属性名上方。现在就可以读取到对应的单一数据行了

**总结**

1. 使用@Value配合SpEL读取单个数据
2. 如果数据存在多层级，依次书写层级名称即可



#### 读取全部数据

​	读取单一数据可以解决读取数据的问题，但是如果定义的数据量过大，这么一个一个书写肯定会累死人的，SpringBoot提供了一个对象，能够把所有的数据都封装到这一个对象中，这个对象叫做Environment，使用自动装配注解可以将所有的yaml数据封装到这个对象中

<img src="https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241811264.png" alt="image-20211126180738569" style="zoom:80%;" />

​	数据封装到了Environment对象中，获取属性时，通过Environment的接口操作进行，具体方法时getProperties（String），参数填写属性名即可

**总结**

1. 使用Environment对象封装全部配置信息
2. 使用@Autowired自动装配数据到Environment对象中



#### 读取对象数据

​	单一数据读取书写比较繁琐，全数据封装又封装的太厉害了，每次拿数据还要一个一个的getProperties（）,总之用起来都不是很舒服。由于Java是一个面向对象的语言，很多情况下，我们会将一组数据封装成一个对象。SpringBoot也提供了可以将一组yaml对象数据封装一个Java对象的操作

​	首先定义一个对象，并将该对象纳入Spring管控的范围，也就是定义成一个bean，然后使用注解@ConfigurationProperties指定该对象加载哪一组yaml中配置的信息。

<img src="https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241811887.png" alt="image-20211126181126382" style="zoom:80%;" />

​	这个@ConfigurationProperties必须告诉他加载的数据前缀是什么，这样当前前缀下的所有属性就封装到这个对象中。记得数据属性名要与对象的变量名一一对应啊，不然没法封装。其实以后如果你要定义一组数据自己使用，就可以先写一个对象，然后定义好属性，下面到配置中根据这个格式书写即可。

​	<img src="https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241811293.png" alt="image-20211126181423432" style="zoom:80%;" />

​	<font color="#f0f"><b>温馨提示</b></font>

​		细心的小伙伴会发现一个问题，自定义的这种数据在yaml文件中书写时没有弹出提示，是这样的，咱们到原理篇再揭秘如何弹出提示。

**总结**

1. 使用@ConfigurationProperties注解绑定配置信息到封装类中
2. 封装类需要定义为Spring管理的bean，否则无法进行属性注入



#### yaml文件中的数据引用

​	如果你在书写yaml数据时，经常出现如下现象，比如很多个文件都具有相同的目录前缀

```yaml
center:
	dataDir: /usr/local/fire/data
    tmpDir: /usr/local/fire/tmp
    logDir: /usr/local/fire/log
    msgDir: /usr/local/fire/msgDir
```

或者

```yaml
center:
	dataDir: D:/usr/local/fire/data
    tmpDir: D:/usr/local/fire/tmp
    logDir: D:/usr/local/fire/log
    msgDir: D:/usr/local/fire/msgDir
```

​	这个时候你可以使用引用格式来定义数据，其实就是搞了个变量名，然后引用变量了，格式如下：

```yaml
baseDir: /usr/local/fire
	center:
    dataDir: ${baseDir}/data
    tmpDir: ${baseDir}/tmp
    logDir: ${baseDir}/log
    msgDir: ${baseDir}/msgDir
```

​	还有一个注意事项，在书写字符串时，如果需要使用转义字符，需要将数据字符串使用双引号包裹起来

```yaml
lesson: "Spring\tboot\nlesson"
```

**总结**

1. 在配置文件中可以使用${属性名}方式引用属性值
2. 如果属性中出现特殊字符，可以使用双引号包裹起来作为字符解析



## 四、基于SpringBoot实现SSMP整合

### 1.整合整合JUnit

SpringBoot技术的定位用于简化开发，再具体点是简化Spring程序的开发。所以在整合任意技术的时候，如果你想直观感触到简化的效果，你必须先知道使用非SpringBoot技术时对应的整合是如何做的，然后再看基于SpringBoot的整合是如何做的，才能比对出来简化在了哪里。

​	我们先来看一下不使用SpringBoot技术时，Spring整合JUnit的制作方式

```java
//加载spring整合junit专用的类运行器
@RunWith(SpringJUnit4ClassRunner.class)
//指定对应的配置信息
@ContextConfiguration(classes = SpringConfig.class)
public class AccountServiceTestCase {
    //注入你要测试的对象
    @Autowired
    private AccountService accountService;
    @Test
    public void testGetById(){
        //执行要测试的对象对应的方法
        System.out.println(accountService.findById(2));
    }
}

```

​	其中核心代码是前两个注解，第一个注解@RunWith是设置Spring专用于测试的类运行器，简单说就是Spring程序执行程序有自己的一套独立的运行程序的方式，不能使用JUnit提供的类运行方式了，必须指定一下，但是格式是固定的，琢磨一下，<font color="#ff0000"><b>每次都指定一样的东西，这个东西写起来没有技术含量啊</b></font>，第二个注解@ContextConfiguration是用来设置Spring核心配置文件或配置类的，简单说就是加载Spring的环境你要告诉Spring具体的环境配置是在哪里写的，虽然每次加载的文件都有可能不同，但是仔细想想，如果文件名是固定的，这个貌似也是一个固定格式。似然<font color="#ff0000"><b>有可能是固定格式，那就有可能每次都写一样的东西，也是一个没有技术含量的内容书写</b></font>

​	SpringBoot就抓住上述两条没有技术含量的内容书写进行开发简化，能走默认值的走默认值，能不写的就不写，具体格式如下

```java
@SpringBootTest
class Springboot04JunitApplicationTests {
    //注入你要测试的对象
    @Autowired
    private BookDao bookDao;
    @Test
    void contextLoads() {
        //执行要测试的对象对应的方法
        bookDao.save();
        System.out.println("two...");
    }
}
```

​	看看这次简化成什么样了，一个注解就搞定了，而且还没有参数，再体会SpringBoot整合其他技术的优势在哪里，就两个字——<font color="#ff0000"><b>简化</b></font>。使用一个注解@SpringBootTest替换了前面两个注解。至于内部是怎么回事？和之前一样，只不过都走默认值。

​	这个时候有人就问了，你加载的配置类或者配置文件是哪一个？就是我们前面启动程序使用的引导类。如果想手工指定引导类有两种方式，第一种方式使用属性的形式进行，在注解@SpringBootTest中添加classes属性指定配置类

```java
@SpringBootTest(classes = Springboot04JunitApplication.class)
class Springboot04JunitApplicationTests {
    //注入你要测试的对象
    @Autowired
    private BookDao bookDao;
    @Test
    void contextLoads() {
        //执行要测试的对象对应的方法
        bookDao.save();
        System.out.println("two...");
    }
}
```

​	第二种方式回归原始配置方式，仍然使用@ContextConfiguration注解进行，效果是一样的

```java
@SpringBootTest
@ContextConfiguration(classes = Springboot04JunitApplication.class)
class Springboot04JunitApplicationTests {
    //注入你要测试的对象
    @Autowired
    private BookDao bookDao;
    @Test
    void contextLoads() {
        //执行要测试的对象对应的方法
        bookDao.save();
        System.out.println("two...");
    }
}
```

​	<font color="#f0f"><b>温馨提示</b></font>

​		使用SpringBoot整合JUnit需要保障导入test对应的starter，由于初始化项目时此项是默认导入的，所以此处没有提及，其实和之前学习的内容一样，用什么技术导入对应的starter即可。

**总结**

1. 导入测试对应的starter
2. 测试类使用@SpringBootTest修饰
3. 使用自动装配的形式添加要测试的对象
4. 测试类如果存在于引导类所在包或子包中无需指定引导类
5. 测试类如果不存在于引导类所在的包或子包中需要通过classes属性指定引导类



### 2.整合MyBatis

整合完JUnit下面再来说一下整合MyBatis，这个技术是大部分公司都要使用的技术，务必掌握。如果对Spring整合MyBatis不熟悉的小伙伴好好复习一下，下面列举出原始整合的全部内容，以配置类的形式为例进行

- 导入坐标，MyBatis坐标不能少，Spring整合MyBatis还有自己专用的坐标，此外Spring进行数据库操作的jdbc坐标是必须的，剩下还有mysql驱动坐标，本例中使用了Druid数据源，这个倒是可以不要

  ```xml
  <dependencies>
      <dependency>
          <groupId>com.alibaba</groupId>
          <artifactId>druid</artifactId>
          <version>1.1.16</version>
      </dependency>
      <dependency>
          <groupId>org.mybatis</groupId>
          <artifactId>mybatis</artifactId>
          <version>3.5.6</version>
      </dependency>
      <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <version>5.1.47</version>
      </dependency>
      <!--1.导入mybatis与spring整合的jar包-->
      <dependency>
          <groupId>org.mybatis</groupId>
          <artifactId>mybatis-spring</artifactId>
          <version>1.3.0</version>
      </dependency>
      <!--导入spring操作数据库必选的包-->
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-jdbc</artifactId>
          <version>5.2.10.RELEASE</version>
      </dependency>
  </dependencies>
  ```

- Spring核心配置

  ```java
  @Configuration
  @ComponentScan("com.itheima")
  @PropertySource("jdbc.properties")
      public class SpringConfig {
  }
  ```

- MyBatis要交给Spring接管的bean

  ```java
  //定义mybatis专用的配置类
  @Configuration
  public class MyBatisConfig {
  //    定义创建SqlSessionFactory对应的bean
      @Bean
      public SqlSessionFactoryBean sqlSessionFactory(DataSource dataSource){
          //SqlSessionFactoryBean是由mybatis-spring包提供的，专用于整合用的对象
          SqlSessionFactoryBean sfb = new SqlSessionFactoryBean();
          //设置数据源替代原始配置中的environments的配置
          sfb.setDataSource(dataSource);
          //设置类型别名替代原始配置中的typeAliases的配置
          sfb.setTypeAliasesPackage("com.itheima.domain");
          return sfb;
      }
  //    定义加载所有的映射配置
      @Bean
      public MapperScannerConfigurer mapperScannerConfigurer(){
          MapperScannerConfigurer msc = new MapperScannerConfigurer();
          msc.setBasePackage("com.itheima.dao");
          return msc;
      }
  
  }
  ```

- 数据源对应的bean，此处使用Druid数据源

  ```java
  @Configuration
  public class JdbcConfig {
      @Value("${jdbc.driver}")
      private String driver;
      @Value("${jdbc.url}")
      private String url;
      @Value("${jdbc.username}")
      private String userName;
      @Value("${jdbc.password}")
      private String password;
  
      @Bean("dataSource")
      public DataSource dataSource(){
          DruidDataSource ds = new DruidDataSource();
          ds.setDriverClassName(driver);
          ds.setUrl(url);
          ds.setUsername(userName);
          ds.setPassword(password);
          return ds;
      }
  }
  ```

- 数据库连接信息（properties格式）

  ```properties
  jdbc.driver=com.mysql.jdbc.Driver
  jdbc.url=jdbc:mysql://localhost:3306/spring_db?useSSL=false
  jdbc.username=root
  jdbc.password=root
  ```

  上述格式基本上是简格式了，要写的东西还真不少。下面看看SpringBoot整合MyBaits格式

**步骤①**：创建模块时勾选要使用的技术，MyBatis，由于要操作数据库，还要勾选对应数据库

![image-20211129092156020](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241811340.png)

![image-20211129092210993](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241811447.png)

​	或者手工导入对应技术的starter，和对应数据库的坐标

```xml
<dependencies>
    <!--1.导入对应的starter-->
    <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>2.2.0</version>
    </dependency>

    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```

**步骤②**：配置数据源相关信息，没有这个信息你连接哪个数据库都不知道

```yaml
#2.配置相关信息
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db
    username: root
    password: root
```

​	完了，就这么多，没了。有人就很纳闷，这就结束了？对，这就结束了，SpringBoot把配置中所有可能出现的通用配置都简化了。下面就可以写一下MyBatis程序运行需要的Dao（或者Mapper）就可以运行了

**实体类**

```java
public class Book {
    private Integer id;
    private String type;
    private String name;
    private String description;
}
```

**映射接口（Dao）**

```java
@Mapper
public interface BookDao {
    @Select("select * from tbl_book where id = #{id}")
    public Book getById(Integer id);
}
```

**测试类**

```java
@SpringBootTest
class Springboot05MybatisApplicationTests {
    @Autowired
    private BookDao bookDao;
    @Test
    void contextLoads() {
        System.out.println(bookDao.getById(1));
    }
}
```

​	完美，开发从此变的就这么简单。再体会一下SpringBoot如何进行第三方技术整合的，是不是很优秀？具体内部的原理到原理篇再展开讲解

​	<font color="#ff0000"><b>注意</b></font>：当前使用的SpringBoot版本是2.5.4，对应的坐标设置中Mysql驱动使用的是8x版本。当SpringBoot2.4.3（不含）版本之前会出现一个小BUG，就是MySQL驱动升级到8以后要求强制配置时区，如果不设置会出问题。解决方案很简单，驱动url上面添加上对应设置就行了

```yaml
#2.配置相关信息
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
    username: root
    password: root
```

​	这里设置的UTC是全球标准时间，你也可以理解为是英国时间，中国处在东八区，需要在这个基础上加上8小时，这样才能和中国地区的时间对应的，也可以修改配置不写UTC，写Asia/Shanghai也可以解决这个问题。

```yaml
#2.配置相关信息
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=Asia/Shanghai
    username: root
    password: root
```

​	如果不想每次都设置这个东西，也可以去修改mysql中的配置文件mysql.ini，在mysqld项中添加default-time-zone=+8:00也可以解决这个问题。其实方式方法很多，这里就说这么多吧。

​	此外在运行程序时还会给出一个提示，说数据库驱动过时的警告，根据提示修改配置即可，弃用**com.mysql.jdbc.Driver**，换用<font color="#ff0000"><b>com.mysql.cj.jdbc.Driver</b></font>。前面的例子中已经更换了驱动了，在此说明一下。

```tex
Loading class `com.mysql.jdbc.Driver'. This is deprecated. 
The new driver class is `com.mysql.cj.jdbc.Driver'. 
The driver is automatically registered via the SPI and manual loading of the driver class is generally unnecessary.
```

**总结**

1. 整合操作需要勾选MyBatis技术，也就是导入MyBatis对应的starter

2. 数据库连接相关信息转换成配置

3. 数据库SQL映射需要添加@Mapper被容器识别到

4. MySQL 8.X驱动强制要求设置时区

   - 修改url，添加serverTimezone设定
   - 修改MySQL数据库配置

5. 驱动类过时，提醒更换为com.mysql.cj.jdbc.Driver

   

### 3.整合MyBatis-Plus

​	做完了两种技术的整合了，各位小伙伴要学会总结，我们做这个整合究竟哪些是核心？总结下来就两句话

- 导入对应技术的starter坐标
- 根据对应技术的要求做配置

​    虽然看起来有点虚，但是确实是这个理儿，下面趁热打铁，再换一个技术，看看是不是上面这两步。

​	接下来在MyBatis的基础上再升级一下，整合MyBaitsPlus（简称MP），国人开发的技术，符合中国人开发习惯，谁用谁知道。来吧，一起做整合

**步骤①**：导入对应的starter

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.3</version>
</dependency>
```

​	关于这个坐标，此处要说明一点，之前我们看的starter都是spring-boot-starter-？？？，也就是说都是下面的格式

```tex
Spring-boot-start-***
```

​	而这个坐标的名字书写比较特殊，是第三方技术名称在前，boot和starter在后。此处简单提一下命名规范，后期原理篇会再详细讲解

| starter所属 | 命名规则                                                    | 示例                                                  |
| ----------- | ----------------------------------------------------------- | ----------------------------------------------------- |
| 官方提供    | spring-boot-starter-技术名称                                | spring-boot-starter-web <br/>spring-boot-starter-test |
| 第三方提供  | 第三方技术名称-spring-boot-starter                          | druid-spring-boot-starter                             |
| 第三方提供  | 第三方技术名称-boot-starter（第三方技术名称过长，简化命名） | mybatis-plus-boot-starter                             |

<font color="#f0f"><b>温馨提示</b></font>

​	有些小伙伴在创建项目时想通过勾选的形式找到这个名字，别翻了，没有。截止目前，SpringBoot官网还未收录此坐标，而我们Idea创建模块时读取的是SpringBoot官网的Spring Initializr，所以也没有。如果换用阿里云的url创建项目可以找到对应的坐标

**步骤②**：配置数据源相关信息

```yaml
#2.配置相关信息
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db
    username: root
    password: root
```

​	没了，就这么多，剩下的就是写MyBaitsPlus的程序了

**映射接口（Dao）**

```java
@Mapper
public interface BookDao extends BaseMapper<Book> {
}
```

​	核心在于Dao接口继承了一个BaseMapper的接口，这个接口中帮助开发者预定了若干个常用的API接口，简化了通用API接口的开发工作。

<img src="https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241811408.png" />

​	下面就可以写一个测试类进行测试了，此处省略。

<font color="#f0f"><b>温馨提示</b></font>

​	目前数据库的表名定义规则是tbl_模块名称，为了能和实体类相对应，需要做一个配置，相关知识各位小伙伴可以到MyBatisPlus课程中去学习，此处仅给出解决方案。配置application.yml文件，添加如下配置即可，设置所有表名的通用前缀名

```yaml
mybatis-plus:
  global-config:
    db-config:
      table-prefix: tbl_		#设置所有表的通用前缀名称为tbl_
```

**总结**

1. 手工添加MyBatis-Plus对应的starter
2. 数据层接口使用BaseMapper简化开发
3. 需要使用的第三方技术无法通过勾选确定时，需要手工添加坐标



### 4.整合Druid

​	使用SpringBoot整合了3个技术了，发现套路基本相同，导入对应的starter，然后做配置，各位小伙伴需要一直强化这套思想。下面再整合一个技术，继续深入强化此思想。

​	前面整合MyBatis和MP的时候，使用的数据源对象都是SpringBoot默认的数据源对象，下面我们手工控制一下，自己指定了一个数据源对象，Druid。

​	在没有指定数据源时，我们的配置如下：

```yaml
#2.配置相关信息
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=Asia/Shanghai
    username: root
    password: root
```

​	此时虽然没有指定数据源，但是根据SpringBoot的德行，肯定帮我们选了一个它认为最好的数据源对象，这就是HiKari。通过启动日志可以查看到对应的身影。

```tex
2021-11-29 09:39:15.202  INFO 12260 --- [           main] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Starting...
2021-11-29 09:39:15.208  WARN 12260 --- [           main] com.zaxxer.hikari.util.DriverDataSource  : Registered driver with driverClassName=com.mysql.jdbc.Driver was not found, trying direct instantiation.
2021-11-29 09:39:15.551  INFO 12260 --- [           main] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Start completed.
```

​	上述信息中每一行都有HiKari的身影，如果需要更换数据源，其实只需要两步即可。

1. 导入对应的技术坐标
2. 配置使用指定的数据源类型

​    下面就切换一下数据源对象

**步骤①**：导入对应的坐标（注意，是坐标，此处不是starter）

```xml
<dependencies>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.1.16</version>
    </dependency>
</dependencies>
```

**步骤②**：修改配置，在数据源配置中有一个type属性，专用于指定数据源类型

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
    username: root
    password: root
    type: com.alibaba.druid.pool.DruidDataSource
```

​	这里其实要提出一个问题的，目前的数据源配置格式是一个通用格式，不管你换什么数据源都可以用这种形式进行配置。但是新的问题又来了，如果对数据源进行个性化的配置，例如配置数据源对应的连接数量，这个时候就有新的问题了。每个数据源技术对应的配置名称都一样吗？肯定不是啊，各个厂商不可能提前商量好都写一样的名字啊，怎么办？就要使用专用的配置格式了。这个时候上面这种通用格式就不能使用了，怎么办？还能怎么办？按照SpringBoot整合其他技术的通用规则来套啊，导入对应的starter，进行相应的配置即可。

**步骤①**：导入对应的starter

```xml
<dependencies>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid-spring-boot-starter</artifactId>
        <version>1.2.6</version>
    </dependency>
</dependencies>
```

**步骤②**：修改配置

```yaml
spring:
  datasource:
    druid:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
      username: root
      password: root
```

​	注意观察，配置项中，在datasource下面并不是直接配置url这些属性的，而是先配置了一个druid节点，然后再配置的url这些东西。言外之意，url这个属性时druid下面的属性，那你能想到吗？除了这4个常规配置外，还有druid专用的其他配置。通过提示功能可以打开druid相关的配置查阅

<img src="https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241811522.png" alt="image-20211129112610729" />

​	与druid相关的配置超过200条以上，这就告诉你，如果想做druid相关的配置，使用这种格式就可以了，这里就不展开描述了，太多了。

​	这是我们做的第4个技术的整合方案，还是那两句话：<font color="#ff0000"><b>导入对应starter，使用对应配置</b></font>。没了，SpringBoot整合其他技术就这么简单粗暴。

**总结**

1. 整合Druid需要导入Druid对应的starter
2. 根据Druid提供的配置方式进行配置
3. 整合第三方技术通用方式
   - 导入对应的starter
   - 根据提供的配置格式，配置非默认值对应的配置项




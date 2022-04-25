---
title: SpringBoot运维
author: 小小千辰
tags:
  - Spring
  - SpringBoot
categories:
  - 千辰的小小笔记
abbrlink: 8bb48816
date: 2022-01-29 13:39:25
updated: 2022-01-30 13:39:25
---





## SpringBoot程序的打包和运行

![image-20220129142809887](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241807859.png)

![image-20220129142800069](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241807775.png)

> 1. 开发部门使用Git、SVN等版本控制工具上传工程到版本服务器
> 2. 服务器使用版本控制工具下载工程
> 3. 服务器上使用Maven工具在当前真机环境下重新构建项目
> 4. 启动服务

### 程序打包

#### IDEA环境下的打包

![image-20220129142925474](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241807762.png)

#### 命令行打包

```java
mvn package
```

打包后会产生一个与工程名类似的jar文件，其名称是由模块名+版本号+.jar组成的。



### 程序运行

```java
java -jar 工程包名.jar
```

&emsp;&emsp;执行程序打包指令后，程序正常运行，与在Idea下执行程序没有区别。

&emsp;&emsp;<b style="color: #FF0000">注意</b>：如果你的计算机中没有安装java的jdk环境，是无法正确执行上述操作的，因为程序执行使用的是java指令。

&emsp;&emsp;<b style="color: #FF0000">注意：在使用向导创建SpringBoot工程时，pom.xml文件中会有如下配置，这一段配置千万不能删除，否则打包后无法正常执行程序。</b>

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

<blockquote class="yellow">
    <b style="color: #FF0000">spring-boot-maven-plugin</b>插件用于将当前程序打包成一个可以独立运行的程序包
</blockquote>

**总结**

1. `SpringBoot`工程可以基于`java`环境下独立运行`jar`文件启动服务
2. `SpringBoot`工程执行`mvn`命令`package`进行打包
3. 执行`jar`命令：`java –jar 工程名.jar`



### 命令行启动常见问题及解决方案

```shell
# 查询端口
netstat -ano
# 查询指定端口
netstat -ano |findstr "端口号"
# 根据进程PID查询进程名称
tasklist |findstr "进程PID号"
# 根据PID杀死任务
taskkill /F /PID "进程PID号"
# 根据进程名称杀死任务
taskkill -f -t -im "进程名称"
```





### SpringBoot项目快速启动（Linux版）

> 需要满足的条件
>
> 1. Linux系统安装了jdk，mysql（版本与jar包来源尽可能相近）
> 2. jar包里面的数据库相关信息（账号密码）与Linux保持一致
> 3. 开放mysql的3306端口
> 4. 开放Linux数据库允许任何ip访问

1. 将打包好的jar包上传至Linux服务器（我这里选择了 /usr/app/ 目录）

   <blockquote class="danger">
       这个打包好的jar包需要你保证Linux的mysql账号密码与你打包之前的文件相同，以及mysql版本的大体一致性。否则一定会出错！
   </blockquote>

2. 开放mysql的3306端口

   ```shell
   firewall-cmd --permanent --add-port=3306/tcp  # 开放端口
   firewall-cmd --reload # 刷新状态
   ```

3. 使用navicat连接远程的mysql（这个时候可能会出现不让连接的情况）

   ```sql
   # 在你的Linux操作数据库
   use mysql;  # 使用 mysql 这个库
   update user set host = '%' where user ='root'; # 允许任何ip链接
   flush privileges; # 刷新权限
   ```

4. 创建数据库以及对应的表（顺便将你的数据传输进去）

5. 在Linux放置jar的目录

   ```java
   java -jar 工程包名.jar
   ```

6. 大功告成！



> 我失败了....
>
> 这个地方我的mysql版本差距太大了...



## 配置高级

### 临时属性设置

> 当程序打好包后需要更改部分设置。

&emsp;&emsp;SpringBoot提供了灵活的配置方式，如果你发现你的项目中有个别属性需要重新配置，可以使用临时属性的方式快速修改某些配置。

```shell
java –jar springboot.jar –-server.port=80
```

<b style="color: #FF0000">注意：这里面书写的格式是properties文件格式</b>

&emsp;&emsp;多个属性：空格隔开即可

```shell
java –jar springboot.jar –-server.port=80 --logging.level.root=debug
```



#### 属性加载优先级

> [官方链接](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-external-config)

![image-20220129144302230](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241807155.png)_官网提供的所有配置的优先级_

**总结**

1. 使用jar命令启动SpringBoot工程时可以使用临时属性替换配置文件中的属性
2. 临时属性添加方式：java –jar 工程名.jar –-属性名=值
3. 多个临时属性之间使用空格分隔
4. 临时属性必须是当前boot工程支持的属性，否则设置无效



#### 开发中使用临时属性

&emsp;&emsp;打开SpringBoot引导类的运行界面，在里面找到配置项。其中Program arguments对应的位置就是添加临时属性。

![image-20220129144546509](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241807635.png)_配置临时参数_







### 配置文件分类

> SpringBoot提供了配置文件和临时属性的方式来对程序进行配置。
>
> - 类路径下配置文件（一直使用的是这个，也就是resources目录中的application.yml文件）
> - 类路径下config目录下配置文件
> - 程序包所在目录中配置文件
> - 程序包所在目录中config目录下配置文件

当四个文件同时存在的时候会有一个优先级的问题：

1. `file ：config/application.yml` **【最高】**
2. `file ：application.yml`
3. `classpath：config/application.yml`
4. `classpath：application.yml`  **【最低】**



#### 使用场景

- 场景A：你作为一个开发者，你做程序的时候为了方便自己写代码，配置的数据库肯定是连接你自己本机的，咱们使用4这个级别，也就是之前一直用的application.yml。
- 场景B：现在项目开发到了一个阶段，要联调测试了，连接的数据库是测试服务器的数据库，肯定要换一组配置吧。你可以选择把你之前的文件中的内容都改了，目前还不麻烦。
- 场景C：测试完了，一切OK。你继续写你的代码，你发现你原来写的配置文件被改成测试服务器的内容了，你要再改回来。现在明白了不？场景B中把你的内容都改掉了，你现在要重新改回来，以后呢？改来改去吗？

&emsp;&emsp;解决方案很简单，用上面的3这个级别的配置文件就可以快速解决这个问题，再写一个配置就行了。两个配置文件共存，因为config目录中的配置加载优先级比你的高，所以配置项如果和级别4里面的内容相同就覆盖了，这样是不是很简单？

&emsp;&emsp;级别1和2什么时候使用呢？程序打包以后就要用这个级别了，管你程序里面配置写的是什么？我的级别高，可以轻松覆盖你，就不用考虑这些配置冲突的问题了。



**总结**

1. 配置文件分为4种

   - 项目类路径配置文件：服务于开发人员本机开发与测试
   - 项目类路径config目录中配置文件：服务于项目经理整体调控
   - 工程路径配置文件：服务于运维人员配置涉密线上环境
   - 工程路径config目录中配置文件：服务于运维经理整体调控

2. 多层级配置文件间的属性采用叠加并覆盖的形式作用于程序



### 自定义配置文件

**方式一：使用临时属性设置配置文件名，注意仅仅是名称，不要带扩展名**

![image-20220129145122498](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241807630.png)

**方式二：使用临时属性设置配置文件路径，这个是全路径名**

![image-20220129145128926](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241807905.png)

也可以设置加载多个配置文件

![image-20220129145146015](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241807244.png)

**总结**

1. 配置文件可以修改名称，通过启动参数设定
2. 配置文件可以修改路径，通过启动参数设定
3. 微服务开发中配置文件通过配置中心进行设置

## 多环境开发

### yaml单一文件

```yaml
spring:
	profiles:
		active: pro		# 启动pro
---
spring:
	profiles: pro       # 环境名称
server:
	port: 80
---
spring:
	profiles: dev
server:
	port: 81
```

> 每一组的环境中间使用三个减号分隔开

上述格式已经过时，标准格式如下

```yaml
spring:
	config:
    	activate:
        	on-profile: pro
```

**总结**

1. 多环境开发需要设置若干种常用环境，例如开发、生产、测试环境
2. yaml格式中设置多环境使用---区分环境设置边界
3. 每种环境的区别在于加载的配置属性不同
4. 启用某种环境时需要指定启动时使用该环境



### yaml多文件（掌握）

**主配置文件**

```yaml
spring:
	profiles:
		active: pro		# 启动pro
```

**application-pro.yaml**

```yaml
server:
	port: 80
```

**application-dev.yaml**

```yaml
server:
	port: 81
```

&emsp;&emsp;文件的命名规则为：application-环境名.yml。

&emsp;&emsp;在配置文件中，如果某些配置项所有环境都一样，可以将这些项写入到主配置中，只有哪些有区别的项才写入到环境配置文件中。

- 主配置文件中设置公共配置（全局）
- 环境分类配置文件中常用于设置冲突属性（局部）

**总结**

1. 可以使用独立配置文件定义环境属性
2. 独立配置文件便于线上系统维护更新并保障系统安全性



### properties多文件（了解）

&emsp;&emsp;SpringBoot最早期提供的配置文件格式是properties格式的，这种格式的多环境配置也了解一下吧。

**主配置文件**

```properties
spring.profiles.active=pro
```

**环境配置文件**

**application-pro.properties**

```properties
server.port=80
```

**application-dev.properties**

```properties
server.port=81
```

​		文件的命名规则为：application-环境名.properties。

**总结**

1. properties文件多环境配置仅支持多文件格式





### 多环境开发独立配置文件书写技巧

**准备工作**

&emsp;&emsp;将所有的配置根据功能对配置文件中的信息进行拆分，并制作成独立的配置文件，命名规则如下

- application-devDB.yml
- application-devRedis.yml
- application-devMVC.yml

**使用**

&emsp;&emsp;使用include属性在激活指定环境的情况下，同时对多个环境进行加载使其生效，多个环境间使用逗号分隔

```yaml
spring:
	profiles:
    	active: dev
        include: devDB,devRedis,devMVC
```

​		比较一下，现在相当于加载dev配置时，再加载对应的3组配置，从结构上就很清晰，用了什么，对应的名称是什么

<b style="color: #FF0000">注意</b>

&emsp;&emsp;<b style="color: #FF0000">当主环境dev与其他环境有相同属性时，主环境属性生效；其他环境中有相同属性时，最后加载的环境属性生效</b>

**改良**

&emsp;&emsp;SpringBoot从2.4版开始使用group属性替代include属性，降低了配置书写量。简单来说就是都写好所以的配置，用哪个就选择哪个。

```yaml
spring:
	profiles:
    	active: dev  # 更换环境修改这里即可
        group:
        	"dev": devDB,devRedis,devMVC
      		"pro": proDB,proRedis,proMVC
      		"test": testDB,testRedis,testMVC
```

**总结**

1. 多环境开发使用group属性设置配置文件分组，便于线上维护管理





### 多环境开发控制

<blockquote class="danger">
    maven和SpringBoot同时设置多环境会产生冲突。
</blockquote>

<blockquote class="success">
    但是呢，maven是做什么的？项目构建管理的，最终生成代码包的，SpringBoot是干什么的？简化开发的。简化，又不是其主导作用。最终还是要靠maven来管理整个工程，所以SpringBoot应该听maven的。
    <ul>
        <li>先在maven环境中设置用什么具体的环境</li>
        <li>在SpringBoot中读取maven设置的环境即可</li>
    </ul>
</blockquote>

**maven中设置多环境（使用属性方式区分环境）**

```xml
<profiles>
    <profile>
        <id>env_dev</id>
        <properties>
            <profile.active>dev</profile.active>
        </properties>
        <activation>
            <activeByDefault>true</activeByDefault>		<!--默认启动环境-->
        </activation>
    </profile>
    <profile>
        <id>env_pro</id>
        <properties>
            <profile.active>pro</profile.active>
        </properties>
    </profile>
</profiles>
```

**SpringBoot中读取maven设置值**

```yaml
spring:
	profiles:
    	active: @profile.active@
```

​		上面的@属性名@就是读取maven中配置的属性值的语法格式。

**总结**

1. 当Maven与SpringBoot同时对多环境进行控制时，以Mavn为主，SpringBoot使用@..@占位符读取Maven对应的配置属性值
2. 基于SpringBoot读取Maven配置属性的前提下，如果在Idea下测试工程时pom.xml每次更新需要手动compile方可生效





## 日志

> 日志的主要作用如下：
>
> - 编程期调试代码
> - 运营期记录信息
> - 记录日常运营重要信息（峰值流量、平均响应时长……）
> - 记录应用报错信息（错误堆栈）
> - 记录运维过程数据（扩容、宕机、报警……）

### 代码中使用日志工具记录日志

&emsp;&emsp;日志的使用格式非常固定，直接上操作步骤：

**步骤①**：添加日志记录操作

```java
@RestController
@RequestMapping("/books")
public class BookController extends BaseClass{
    private static final Logger log = LoggerFactory.getLogger(BookController.class);
    @GetMapping
    public String getById(){
        log.debug("debug...");
        log.info("info...");
        log.warn("warn...");
        log.error("error...");
        return "springboot is running...2";
    }
}
```

&emsp;&emsp;上述代码中log对象就是用来记录日志的对象，下面的log.debug，log.info这些操作就是写日志的API了。

**步骤②**：设置日志输出级别

&emsp;&emsp;日志设置好以后可以根据设置选择哪些参与记录。这里是根据日志的级别来设置的。日志的级别分为6种，分别是：

- TRACE：运行堆栈信息，使用率低
- DEBUG：程序员调试代码使用
- INFO：记录运维过程数据
- WARN：记录运维过程报警数据
- ERROR：记录错误堆栈信息
- FATAL：灾难信息，合并计入ERROR

&emsp;&emsp;一般情况下，开发时候使用DEBUG，上线后使用INFO，运维信息记录使用WARN即可。下面就设置一下日志级别：

```yaml
# 开启debug模式，输出调试信息，常用于检查系统运行状况
debug: true
```

&emsp;&emsp;这么设置太简单粗暴了，日志系统通常都提供了细粒度的控制

```yaml
# 开启debug模式，输出调试信息，常用于检查系统运行状况
debug: true

# 设置日志级别，root表示根节点，即整体应用日志级别
logging:
	level:
    	root: debug
```

&emsp;&emsp;还可以再设置更细粒度的控制

**步骤③**：设置日志组，控制指定包对应的日志输出级别，也可以直接控制指定包对应的日志输出级别

```yaml
logging:
	# 设置日志组
    group:
    	# 自定义组名，设置当前组中所包含的包
        ebank: com.itheima.controller
    level:
    	root: warn
        # 为对应组设置日志级别
        ebank: debug
    	# 为对包设置日志级别
        com.itheima.controller: debug
```

&emsp;&emsp;说白了就是总体设置一下，每个包设置一下，如果感觉设置的麻烦，就先把包分个组，对组设置，没了，就这些。

**总结**

1. 日志用于记录开发调试与运维过程消息
2. 日志的级别共6种，通常使用4种即可，分别是DEBUG，INFO,WARN,ERROR
3. 可以通过日志组或代码包的形式进行日志显示级别的控制



### 优化日志对象创建代码

&emsp;&emsp;写代码的时候每个类都要写创建日志记录对象，这个可以优化一下，使用前面用过的lombok技术给我们提供的工具类即可。

```java
@RestController
@RequestMapping("/books")
public class BookController extends BaseClass{
    private static final Logger log = LoggerFactory.getLogger(BookController.class);
}
```

&emsp;&emsp;导入lombok后使用注解搞定，日志对象名为log

```java
@Slf4j		//这个注解替代了下面那一行
@RestController
@RequestMapping("/books")
public class BookController extends BaseClass{
    private static final Logger log = LoggerFactory.getLogger(BookController.class);	//这一句可以不写了
}
```

**总结**

1. 基于lombok提供的@Slf4j注解为类快速添加日志对象



### 日志输出格式控制

​		日志已经能够记录了，但是目前记录的格式是SpringBoot给我们提供的，如果想自定义控制就需要自己设置了。先分析一下当前日志的记录格式。

![image-20220130205953985](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241807551.png)

​		对于单条日志信息来说，日期，触发位置，记录信息是最核心的信息。级别用于做筛选过滤，PID与线程名用于做精准分析。了解这些信息后就可以DIY日志格式了。本课程不做详细的研究，有兴趣的小伙伴可以学习相关的知识。下面给出课程中模拟的官方日志模板的书写格式，便于大家学习。

```yaml
logging:
	pattern:
    	console: "%d %clr(%p) --- [%16t] %clr(%-40.40c){cyan} : %m %n"
```

**总结**

1. 日志输出格式设置规则



### 日志文件

​		日志信息显示，记录已经控制住了，下面就要说一下日志的转存了。日志不能仅显示在控制台上，要把日志记录到文件中，方便后期维护查阅。

​		对于日志文件的使用存在各种各样的策略，例如每日记录，分类记录，报警后记录等。这里主要研究日志文件如何记录。

​		记录日志到文件中格式非常简单，设置日志文件名即可。

```yaml
logging:
	file:
    	name: server.log
```

​		虽然使用上述格式可以将日志记录下来了，但是面对线上的复杂情况，一个文件记录肯定是不能够满足运维要求的，通常会每天记录日志文件，同时为了便于维护，还要限制每个日志文件的大小。下面给出日志文件的常用配置方式：

```yaml
logging:
	logback:
    	rollingpolicy:
        	max-file-size: 3KB
            file-name-pattern: server.%d{yyyy-MM-dd}.%i.log
```

​		以上格式是基于logback日志技术设置每日日志文件的设置格式，要求容量到达3KB以后就转存信息到第二个文件中。文件命名规则中的%d标识日期，%i是一个递增变量，用于区分日志文件。

**总结**

1. 日志记录到文件
2. 日志文件格式设置

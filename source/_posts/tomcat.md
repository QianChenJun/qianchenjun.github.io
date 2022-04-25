---
title: Tomcat的使用
author: 小小千辰
tags:
  - Tomcat
  - JavaWeb
categories:
  - 千辰的小小笔记
abbrlink: acc4324d
date: 2021-09-27 00:00:00
updated: 2021-09-27 00:00:00

---



## 创建动态web工程

### 新建模块选择Java

![image-20210927230001278](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241804965.png)

<br>

### 右键选择添加依赖

![image-20210927230121533](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241804664.png)



### 选择Web Application，并且默认创建web.xml

![image-20210927230229982](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241804119.png)



> 可以看到如下文件即为创建成功

![image-20210927230414571](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241804885.png)



## 添加Jar包的方式

### 在WEB-INF目录下创建Jar包

![image-20210927230610211](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241804742.png)



### 导入Jar包

![image-20210927230651949](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241804765.png)



### 创建LIbraries

![image-20210927230733766](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241804855.png)

> 选择Java

![image-20210927230750771](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241804155.png)

> 选择刚才的Jar包

![image-20210927230839345](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241804770.png)

> 选择作用工程

![image-20210927230923471](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241804493.png)

> Apply即可

![image-20210927231000042](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241804580.png)



## Artifacts的Fix

![image-20210927231056045](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241805297.png)

> 选择第一个，然后Apply

![image-20210927231108367](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241805843.png)



## Tomcat的配置

![image-20210927231202162](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241805660.png)

> 新建Tomcat local

![image-20210927231242084](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241805385.png)

> 选择tomcat安装目录，以及该tomcat名字（对应自己的工程）

![image-20210927231403441](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241805613.png)

> 解决下方Fix问题

![image-20210927231443750](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241805671.png)

> 选择本工程，应用即可

![image-20210927231520345](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202204241805150.png)




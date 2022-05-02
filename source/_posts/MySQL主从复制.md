---
title: MySQL主从复制
tags:
  - mysql
categories:
  - 千辰的小小笔记
abbrlink: c813917a
date: 2022-05-02 10:46:03
updated: 2022-05-02 10:46:03
---

## 介绍

MySQL主从复制是一个异步的复制过程，底层是基于MySq1数据库自带的**二进制日志**功能。就是一台或多台MySQL数据库（slave，即从库）从另一台MySQL数据库（master，即**主库**）进行日志的复制然后再解析日志并应用到自身，最终实现**从库**的数据和**主库**的数据保持一致。MySQL主从复制是MySQL数据库自带功能，无需借助第三方工具。

MySQL复制过程分为三步：

* master将改变记录到二进制位日志（binary log）
* slave将master的binary log拷贝到它的中继日志（relay log）
* slave重做中继日志的时间，将改变应用到自己的的数据库中

![image-20220502123118613](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202205021313053.png)

*<!--more-->*

## 配置

### 前置条件

提前准备好两台服务器，分别安装MySQL并启动服务成功

* 主库Master：192.168.200.100
* 从库Slave：192.168.200.101

> 这里我使用了VMware来安装虚拟机，配置好一台虚拟机后通过复制虚拟机来制作另外一台虚拟机。
>
> **注意：这里如果另一台虚拟机是复制出来的话会导致两个MySQL的UUID相同，后面操作会有个小坑。**



### 主库Master

1. 修改MySQL数据库配置文件 `/etc/my.cnf`

   ![image-20220502123727198](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202205021313054.png)

2. 重启MySQL服务

   ```shell
   systemct restart mysqld
   ```

3. 登录MySQL数据库，执行下面的SQL

   ```sql
   GRANT REPLICATION SLAVE ON *.* to 'tom'@'%' identified by 'Root@123456';
   ```

   注：上面SQL的作用是创建一个用户**tom**，密码为**Root@123456**，并且给tom用户授予**REPLICATION SLAVE**权限。常用于建立复制时所需要用到的用户权限，也就是slave必须被master授权具有该权限的用户，才能通过该用户复制。

4. 登录MySQL数据库，执行下面的SQL，记录下结果中**File**和**Position**的值

   ```sql
   show master status;
   ```

   ![image-20220502124443083](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202205021313055.png)

   注：上面的SQL的作用是查看Master的状态，执行完此SQL后不要再执行任何操作





### 从库Slave

1. 修改MySQL数据库配置文件 `/etc/my.cnf`

   ![image-20220502124639846](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202205021313056.png)

2. 重启MySQL服务

   ```shell
   systemct restart mysqld
   ```

3. 登录MysQL数据库，执行下面的SQL

   ```sql
   change master to master_host='192.168.200.100',master_user='tom',master_password='Root@123456',master_log_file='mysql-bin.000002',master_log_pos=42634;
   
   start slave;
   ```

   注：`master_host`是主数据库的`ip`，`master_user`和`master_password`是刚才创建的用户名和密码，`master_log_file`和`master_log_pos`是主库的状态。

4. 查看从库的状态

   ```sql
   show slave status;
   ```

   ![image-20220502125417011](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202205021313058.png)

   日志文件中显示这两个地方是Yes则代表成功



### 从库状态No的处理

**问题：从库所在虚拟机是主库复制出来的**

查看UUID：

```sql
show variables like '%server_uuid%';
```

![image-20220502125959457](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202205021313059.png)

> 这会导致主机和从机的UUID一致。

解决：分别在主库和从库执行以下命令

```shell
rm -rf ./mysql/data/auto.cnf
rm -rf /var/lib/mysql/auto.cnf
```

执行完毕后重启服务器




---
title: MySQL主从复制
tags:
  - mysql
categories:
  - 千辰的小小笔记
abbrlink: c813917a
date: 2022-05-02 10:46:03
---

## 方式一：MySQL自带

### 介绍

MySQL主从复制是一个异步的复制过程，底层是基于MySq1数据库自带的**二进制日志**功能。就是一台或多台MySQL数据库（slave，即从库）从另一台MySQL数据库（master，即**主库**）进行日志的复制然后再解析日志并应用到自身，最终实现**从库**的数据和**主库**的数据保持一致。MySQL主从复制是MySQL数据库自带功能，无需借助第三方工具。

MySQL复制过程分为三步：

* master将改变记录到二进制位日志（binary log）
* slave将master的binary log拷贝到它的中继日志（relay log）
* slave重做中继日志的时间，将改变应用到自己的的数据库中

![image-20220502123118613](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202205021313053.png)

*<!--more-->*

### 配置

#### 前置条件

提前准备好两台服务器，分别安装MySQL并启动服务成功

* 主库Master：192.168.200.100
* 从库Slave：192.168.200.101

> 这里我使用了VMware来安装虚拟机，配置好一台虚拟机后通过复制虚拟机来制作另外一台虚拟机。
>
> **注意：这里如果另一台虚拟机是复制出来的话会导致两个MySQL的UUID相同，后面操作会有个小坑。**



#### 主库Master

1. 修改MySQL数据库配置文件 `/etc/my.cnf`

   ![image-20220502123727198](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202205021313054.png)

2. 重启MySQL服务

   ```shell
   systemctl restart mysqld
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





#### 从库Slave

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



#### 从库状态No的处理

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





## 方式二：Canal实现数据库复制

> canal是阿里巴巴旗下的一款开源项目，纯Java开发。基于数据库增量日志解析，提供增量数据订阅&消费，目前主要支持了MySQL。

> 注意：需要复制的两个MySQL数据库需要保证**完全一致**，即库名称、表名称和表结构完全一致。

### 环境搭建

> canal的原理是基于mysql binlog技术，所以这里一定需要开启mysql的binlog写入功能

1. 检查binlog功能是否开启

   ```mysql
   mysql> show variables like 'log_bin';
   +---------------+-------+
   | Variable_name | Value |
   +---------------+-------+
   | log_bin 		|  OFF  |
   +---------------+-------+
   1 row in set (0.00 sec)
   ```

2. 如果显示状态为OFF表示该功能未开启，开启binlog功能

   ```mysql
   (1)修改 mysql 的配置文件 my.cnf
   vi /etc/my.cnf
   追加内容：
   	log-bin=mysql-bin #binlog文件名
   	binlog_format=ROW #选择row模式
   	server_id=1 #mysql实例id,不能和canal的slaveId重复
   (2)重启 mysql：systemctl restart mysqld
   
   (3)登录 mysql 客户端，查看 log_bin 变量
    mysql> show variables like 'log_bin';
    +---------------+-------+
    | Variable_name | Value |
    +---------------+-------+
    | log_bin | ON|
    +---------------+-------+
    1 row in set (0.00 sec)
    ————————————————
    如果显示状态为ON表示该功能已开启
   ```

3. 在mysql里面添加以下的相关用户和权限

   ```mysql
   CREATE USER 'canal'@'%' IDENTIFIED BY 'canal';
   GRANT SHOW VIEW, SELECT, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO
   'canal'@'%';
   FLUSH PRIVILEGES;
   ```

   > 这一步的作用其实就是创建一个用户来完成数据库的操作。需要这个用户拥有远程访问数据库的功能，同样也是可以通过修改root的权限来让root用户拥有远程登陆的权限。

### 下载和安装Canal

下载地址：[Canal的Github仓库](https://github.com/alibaba/canal/releases)

1. 解压Canal

   ![image-20220617221239952](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202206172212860.png)

2. 修改配置文件

   ![](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202206172214867.png)

   1. 将address更换成虚拟机的地址

   ![](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202206172214867.png)

   2. 修改用户名和密码（我这里的root拥有远程访问的权限）

      ![image-20220617221650732](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202206172216178.png)

   3. 修改同步数据库的规则

      ![image-20220617221852520](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202206172218705.png)

      > 注：
      >
      > mysql 数据解析关注的表，Perl正则表达式.
      >
      > 多个正则之间以逗号(,)分隔，转义符需要双斜杠(\\) 
      >
      > 常见例子：
      >
      > 1. 所有表：`.* or .*\\..*`
      > 2. canal schema下所有表： `canal\\..*`
      > 3. canal下的以canal打头的表：`canal\\.canal.*`
      > 4. canal schema下的一张表：`canal.test1`
      > 5. 多个规则组合使用：`canal\\..*,mysql.test1,mysql.test2 (逗号分隔)`
      >
      > **注意：此过滤条件只针对row模式的数据有效(ps. mixed/statement因为不解析sql，所以无法准确提取tableName进行过滤)**

3. 进入bin目录启动

   `sh bin/startup.sh`

   ![image-20220617222137764](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202206172221548.png)

> 截至目前我们在Linux的配置已经完成了



### SpringBoot操作

![image-20220617222255555](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202206172222943.png)

#### pom文件

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!--mysql-->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
    </dependency>

    <dependency>
        <groupId>commons-dbutils</groupId>
        <artifactId>commons-dbutils</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-jdbc</artifactId>
    </dependency>

    <dependency>
        <groupId>com.alibaba.otter</groupId>
        <artifactId>canal.client</artifactId>
    </dependency>
</dependencies>
```

#### application.yml

```yml
server:
  # 端口
  port: 10000

spring:
  application:
    # 服务名称
    name: canal-client
  profiles:
    # 环境设置
    active: dev
  # mysql数据库连接
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/guli?serverTimezone=GMT%2B8
    username: root
    password: 333
```

#### CanalClient.java

```java
import com.alibaba.otter.canal.client.CanalConnector;
import com.alibaba.otter.canal.client.CanalConnectors;
import com.alibaba.otter.canal.protocol.CanalEntry.*;
import com.alibaba.otter.canal.protocol.Message;
import com.google.protobuf.InvalidProtocolBufferException;
import org.apache.commons.dbutils.DbUtils;
import org.apache.commons.dbutils.QueryRunner;
import org.springframework.stereotype.Component;

import javax.annotation.Resource;
import javax.sql.DataSource;
import java.net.InetSocketAddress;
import java.sql.Connection;
import java.sql.SQLException;
import java.util.List;
import java.util.Queue;
import java.util.concurrent.ConcurrentLinkedQueue;

@Component
public class CanalClient {

    //sql队列
    private Queue<String> SQL_QUEUE = new ConcurrentLinkedQueue<>();

    @Resource
    private DataSource dataSource;

    /**
     * canal入库方法
     */
    public void run() {

        CanalConnector connector = CanalConnectors.newSingleConnector(new InetSocketAddress("192.168.200.100",
                11111), "example", "", "");
        int batchSize = 1000;
        try {
            connector.connect();
            connector.subscribe(".*\\..*");
            connector.rollback();
            try {
                while (true) {
                    //尝试从master那边拉去数据batchSize条记录，有多少取多少
                    Message message = connector.getWithoutAck(batchSize);
                    long batchId = message.getId();
                    int size = message.getEntries().size();
                    if (batchId == -1 || size == 0) {
                        Thread.sleep(1000);
                    } else {
                        dataHandle(message.getEntries());
                    }
                    connector.ack(batchId);

                    //当队列里面堆积的sql大于一定数值的时候就模拟执行
                    if (SQL_QUEUE.size() >= 1) {
                        executeQueueSql();
                    }
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            } catch (InvalidProtocolBufferException e) {
                e.printStackTrace();
            }
        } finally {
            connector.disconnect();
        }
    }

    /**
     * 模拟执行队列里面的sql语句
     */
    public void executeQueueSql() {
        int size = SQL_QUEUE.size();
        for (int i = 0; i < size; i++) {
            String sql = SQL_QUEUE.poll();
            System.out.println("[sql]----> " + sql);

            this.execute(sql.toString());
        }
    }

    /**
     * 数据处理
     *
     * @param entrys
     */
    private void dataHandle(List<Entry> entrys) throws InvalidProtocolBufferException {
        for (Entry entry : entrys) {
            if (EntryType.ROWDATA == entry.getEntryType()) {
                RowChange rowChange = RowChange.parseFrom(entry.getStoreValue());
                EventType eventType = rowChange.getEventType();
                if (eventType == EventType.DELETE) {
                    saveDeleteSql(entry);
                } else if (eventType == EventType.UPDATE) {
                    saveUpdateSql(entry);
                } else if (eventType == EventType.INSERT) {
                    saveInsertSql(entry);
                }
            }
        }
    }

    /**
     * 保存更新语句
     *
     * @param entry
     */
    private void saveUpdateSql(Entry entry) {
        try {
            RowChange rowChange = RowChange.parseFrom(entry.getStoreValue());
            List<RowData> rowDatasList = rowChange.getRowDatasList();
            for (RowData rowData : rowDatasList) {
                List<Column> newColumnList = rowData.getAfterColumnsList();
                StringBuffer sql = new StringBuffer("update " + entry.getHeader().getTableName() + " set ");
                for (int i = 0; i < newColumnList.size(); i++) {
                    sql.append(" " + newColumnList.get(i).getName()
                            + " = '" + newColumnList.get(i).getValue() + "'");
                    if (i != newColumnList.size() - 1) {
                        sql.append(",");
                    }
                }
                sql.append(" where ");
                List<Column> oldColumnList = rowData.getBeforeColumnsList();
                for (Column column : oldColumnList) {
                    if (column.getIsKey()) {
                        //暂时只支持单一主键
                        sql.append(column.getName() + "=" + column.getValue());
                        break;
                    }
                }
                SQL_QUEUE.add(sql.toString());
            }
        } catch (InvalidProtocolBufferException e) {
            e.printStackTrace();
        }
    }

    /**
     * 保存删除语句
     *
     * @param entry
     */
    private void saveDeleteSql(Entry entry) {
        try {
            RowChange rowChange = RowChange.parseFrom(entry.getStoreValue());
            List<RowData> rowDatasList = rowChange.getRowDatasList();
            for (RowData rowData : rowDatasList) {
                List<Column> columnList = rowData.getBeforeColumnsList();
                StringBuffer sql = new StringBuffer("delete from " + entry.getHeader().getTableName() + " where ");
                for (Column column : columnList) {
                    if (column.getIsKey()) {
                        //暂时只支持单一主键
                        sql.append(column.getName() + "=" + column.getValue());
                        break;
                    }
                }
                SQL_QUEUE.add(sql.toString());
            }
        } catch (InvalidProtocolBufferException e) {
            e.printStackTrace();
        }
    }

    /**
     * 保存插入语句
     *
     * @param entry
     */
    private void saveInsertSql(Entry entry) {
        try {
            RowChange rowChange = RowChange.parseFrom(entry.getStoreValue());
            List<RowData> rowDatasList = rowChange.getRowDatasList();
            for (RowData rowData : rowDatasList) {
                List<Column> columnList = rowData.getAfterColumnsList();
                StringBuffer sql = new StringBuffer("insert into " + entry.getHeader().getTableName() + " (");
                for (int i = 0; i < columnList.size(); i++) {
                    sql.append(columnList.get(i).getName());
                    if (i != columnList.size() - 1) {
                        sql.append(",");
                    }
                }
                sql.append(") VALUES (");
                for (int i = 0; i < columnList.size(); i++) {
                    sql.append("'" + columnList.get(i).getValue() + "'");
                    if (i != columnList.size() - 1) {
                        sql.append(",");
                    }
                }
                sql.append(")");
                SQL_QUEUE.add(sql.toString());
            }
        } catch (InvalidProtocolBufferException e) {
            e.printStackTrace();
        }
    }

    /**
     * 入库
     * @param sql
     */
    public void execute(String sql) {
        Connection con = null;
        try {
            if(null == sql) return;
            con = dataSource.getConnection();
            QueryRunner qr = new QueryRunner();
            int row = qr.execute(con, sql);
            System.out.println("update: "+ row);
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            DbUtils.closeQuietly(con);
        }
    }
}
```

#### CanalApplication.java

```java
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import xyz.qianchen.canal.client.CanalClient;

import javax.annotation.Resource;

@SpringBootApplication
public class CanalApplication implements CommandLineRunner {
    @Resource
    private CanalClient canalClient;

    public static void main(String[] args) {
        SpringApplication.run(CanalApplication.class, args);
    }

    @Override
    public void run(String... strings) throws Exception {
        //项目启动，执行canal客户端监听
        canalClient.run();
    }
}
```

> 创建好所有的文件后启动Spring Boot程序



### 测试

虚拟机操作

![image-20220617222940848](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202206172229105.png)

![image-20220617223112684](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202206172231789.png)

本地数据库

![image-20220617223030566](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202206172230530.png)














---
title: Redis简单使用
author: 小小千辰
tags:
  - Redis
categories:
  - 千辰的小小笔记
abbrlink: c4599bc1
date: 2022-05-02 09:32:52
---

## Redis基本配置

> [Redis官网](https://redis.io/)

### Redis的安装

#### Linux

1. 将Redis安装包上传到Linux
2. 解压安装包，命令：`tar -zxvg redis-4.0.0.tar.gz -C /usr/local`
3. 安装Redis的依赖环境gcc，命令：`yum install gcc-c++`
4. 进入 `/usr/local/redis-4.0.0`，进行编译，命令：`make`
5. 进入 redis 的 src 目录，进行安装，命令：`make install`

#### Windows

解压绿色版本即可。

### Redis的启动

#### Linux

1. 进入 Redis 的src路径，命令：`cd /usr/local/redis-4.0.0/src`
2. 打开 Redis 的服务器，命令：`./redis-server`
3. 打开 Redis 的客户端，命令：`./redis-cli`

![image-20220430104337768](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202205011702255.png)

注意：**此时的服务端启动会霸占整个屏幕**

优化启动：

1. 进入 Redis 的安装路径，命令：`cd /usr/local/redis-4.0.0`

2. 编辑 Redis 的配置文件，命令：`vim redis.conf`

3. 输入 `/daemonize`

   ![image-20220430104620471](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202205011702256.png)

4. 将 no 改成 yes，保存即可

5. 在当前目录 `redis-4.0.0` 启动 Redis，命令：`src/redis-server ./redis.conf`

   ![image-20220430104934443](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202205011702257.png)



#### Windows

> 双击启动即可

![image-20220430111454453](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202205011702258.png)



### Redis设置密码

1. 编辑 Redis 的配置文件，命令：`vim redis.conf`

2. 查找 `requirepass`，命令：`/requirepass`

   ![image-20220430110654190](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202205011702259.png)

   > 打开注释，并把后面的字母改成自己的密码

3. 重启 Redis 服务



### Redis设置远程访问

> 默认 Redis 不设置的话是只能本机访问。
>
> <b style="color: #FF0000">注意：如果需要远程访问，一定要设置密码</b>

1. 编辑 Redis 的配置文件，命令：`vim redis.conf`

2. 查找 `bind`，命令：`/bind`

   ![image-20220430110957357](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202205011702260.png)

3. 将 `bind 127.0.0.1` 注释掉

4. 重启服务器

> 在 Windows 测试访问，命令：`.\redis-cli.exe -h 192.168.222.130 -p 6379 -a 333`
>
> 参数：
>
> 1. -h：host，指定ip
> 2. -p：port，指定端口
> 3. -a：auth，指定密码
>
> ![image-20220430111240037](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202205011702261.png)





## Redis常用命令

### Redis 常用的数据类型

![image-20220430111934350](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202205011702262.png)_Redis常用的数据类型_

### 字符串 string 操作命令

Redis 中字符串类型的常用命令：

|           命令            |                            含义                             |
| :-----------------------: | :---------------------------------------------------------: |
|      `SET key value`      |                     设置指定 `key` 的值                     |
|         `GET key`         |                     获取指定 `key` 的值                     |
| `SETEX key seconds value` | 设置指定 `key` 的值，并将 `key` 的过期时间设为 `seconds` 秒 |
|     `SETNX key value`     |            只有在 `key` 不存在时设置 `key` 的值             |

操作演示：

![image-20220430115818489](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202205011702263.png)

### 哈希 hash 操作命令

Redis hash 是一个string类型的 field 和 value 的映射表，hash特别适合用于存储对象。

常用命令：

|          命令          |                       含义                       |
| :--------------------: | :----------------------------------------------: |
| `HSET key field value` | 将哈希表 `key` 中的字段 `field` 的值设为 `value` |
|    `HGET key field`    |          获取存储在哈希表中指定字段的值          |
|    `HDEL key field`    |           删除存储在哈希表中的指定字段           |
|      `HKEYS key`       |               获取哈希表中所有字段               |
|      `HVALS key`       |                获取哈希表中所有值                |
|     `HGETALL key`      |     获取在哈希表中指定 `key` 的所有字段和值      |

![image-20220430114539477](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202205011702264.png)_Hash结构图示_

操作演示：

![image-20220430163333768](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202205011702265.png)



### 列表 list 操作命令

Redis列表是简单的字符串列表，**按照插入序列排序**。

常用命令：



|            命令             |                             含义                             |
| :-------------------------: | :----------------------------------------------------------: |
| `LPUSH key value1 [value2]` |                 将一个或多个值插入到列表头部                 |
|   `LRANGE key start stop`   |                   获取列表指定范围内的元素                   |
|         `RPOP key`          |                  移除并获取列表最后一个元素                  |
|         `LLEN key`          |                         获取列表长度                         |
| `BRPOP key1 [key2] timeout` | 移除并获取列表的最后一个元素，如果列表没有元素会阻塞列表<br>直到等待超时或发现可弹出元素为止 |

![image-20220430163826113](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202205011702266.png)_列表结构图示_

操作演示：

![image-20220430164415452](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202205011702267.png)

### 无序集合 set 操作命令

Redis set是string类型的**无序**集合。集合成员是**唯一**的，这就意味着**集合中不能出现重复的数据**。

常用命令：

|             命令             |           含义           |
| :--------------------------: | :----------------------: |
| `SADD key member1 [member2]` | 向集合添加一个或多个成员 |
|        `SMEMBERS key`        |   返回集合中所有的成员   |
|         `SCARD key`          |     获取集合的成员数     |
|     `SINTER key1 [key2]`     |  返回给定所有集合的交集  |
|     `SUNION key1 [key2]`     |  返回所有给定集合的并集  |
|     `SDIFF key1 [key2]`      |  返回给定所有集合的差集  |
| `SREM key member1 [member2]` | 移除集合中一个或多个成员 |

![image-20220430164856975](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202205011702268.png)_无序集合结构图示_

操作演示：

![image-20220430165712386](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202205011702269.png)



### 有序集合 sorted set 操作命令

Redis sorted set有序集合是string类型元素的集合，且不允许重复的成员。每个元素都会关联一个double类型的分数(score)。redis正是通过分数来为集合中的成员进行从小到大排序。有序集合的成员是唯一的，但分数却可以重复。
常用命令：

|                    命令                    |                          含义                          |
| :----------------------------------------: | :----------------------------------------------------: |
| `ZADD key socre1 member1 [score2 member2]` | 向有序集合添加一个或多个成员，或者更新已存在成员的分数 |
|    `ZRANGE key start stop [WITHSCORES]`    |       通过索引区间返回有序集合中指定区间内的成员       |
|       `ZINCRBY key increment member`       |     有序集合中对指定成员的分数加上增量 `increment`     |
|       `ZREM key member [member ...]`       |             移除有序集合中的一个或多个成员             |

![image-20220430170244192](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202205011702270.png)_有序集合结构体图示_

操作演示：

![image-20220430171133223](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202205011702271.png)



### 通用命令

|      命令      |                             含义                             |
| :------------: | :----------------------------------------------------------: |
| `KEYS pattern` |          查找所有符合给定模式`（pattern）`的 `key`           |
|  `EXISTS key`  |                   检查给定 `key `是否存在                    |
|   `TYPE key`   |                  返回 key 所储存的值的类型                   |
|   `TTL key`    | 返回给定 `key `的剩余生存时间`（TTL，time to live）`，以秒为单位 |
|   `DEL key`    |             该命令用于在 `key `存在时删除 `key`              |

操作演示：

![image-20220430171720850](https://cdn.jsdelivr.net/gh/QianChenJun/cloudimage@main/img/202205011702272.png)


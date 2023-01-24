---
title: Redis 在 Mac 上的安装与使用
date: 2016-08-12 16:34:08
categories: 
- 教程
tags: Database
---

### Redis

Redis是一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。

Redis是一个速度非常快的非关系数据库（non-relational database），它可以存储键（key）与5种不同类型的值（value）之间的映射（mapping），可以将存储在内存的键值对数据持久化到硬盘，可以使用复制特性来扩展读性能，还可以使用客户端分片来扩展写性能。
<!-- more -->

一些数据库和缓存服务器的特性与功能对比：

名称 | 类型 | 数据存储选项 | 查询类型 | 附加功能
---- | ---- | ---- | ---- | ----
Redis | 使用内存存储（in-memory） 的非关系数据库	| 字符串、列表、集合、散列表、有序集合	| 每种数据类型都有自己的专属命令， 另外还有批量操作（bulk operation）和不完全（partial）的事务支持	| 发布与订阅， 主从复制（master/slave replication）， 持久化， 脚本（存储过程，stored procedure）
memcached | 使用内存存储的键值缓存	| 键值之间的映射	| 创建命令、读取命令、更新命令、删除命令以及其他几个命令	| 为提升性能而设的多线程服务器
MySQL	| 关系数据库	| 每个数据库可以包含多个表， 每个表可以包含多个行； 可以处理多个表的视图（view）； 支持空间（spatial）和第三方扩展 |	SELECT 、 INSERT 、 UPDATE 、 DELETE 、函数、存储过程	| 支持ACID性质（需要使用InnoDB）， 主从复制和主主复制 （master/master replication）
PostgreSQL	| 关系数据库	| 每个数据库可以包含多个表， 每个表可以包含多个行； 可以处理多个表的视图； 支持空间和第三方扩展；支持可定制类型 |	SELECT 、 INSERT 、 UPDATE 、 DELETE 、内置函数、自定义的存储过程	| 支持ACID性质，主从复制， 由第三方支持的多主复制 （multi-master replication）
MongoDB	| 使用硬盘存储（on-disk）的非关系文档存储	| 每个数据库可以包含多个表， 每个表可以包含多个无schema （schema-less）的BSON文档	| 创建命令、读取命令、更新命令、删除命令、条件查询命令，等等	| 支持map-reduce操作，主从复制，分片， 空间索引（spatial index）



### Redis 安装

我使用的安装环境是 OS X EI Capitan 10.11.6。

首先，在[Redis官网](http://redis.io/download)中下载稳定的 .tar.gz 压缩包，解压后，进入该文件夹，输入命令：

``` bash
sudo make
```

然后是一个可选的命令，来查看redis内部的一些东西是否OK：

``` bash
make test
```

然后安装：

``` bash
sudo make install
```

此外，也可以按照官网推荐的方式来下载安装。

### Redis 启动

Starting redis:

``` bash
redis-server /path/to/redis.conf
```

第二个参数为当前目录到配置文件 redis.conf 的路径，执行后可看到：
![icon](http://i2.piimg.com/4851/36f197a62e06769e.png)

这个表示redis服务端已开启。然后在另一个Terminal中启动客户端：

``` bash
redis-cli
```
这将打开一个redis提示符：

``` bash
127.0.0.1:6379>
```
输入ping命令，得到pong的回应：

![icon](http://i1.piimg.com/4851/07e331b5fe00d188.png)

说明redis在本地启动成功！

### Redis 使用

Redis 提供5种数据结构：

结构类型 | 结构存储的值
---- | ----
STRING | 可以是字符串、整数或者浮点数
LIST | 一个链表，链表上的每个节点都包含了一个字符串
SET | 包含字符串的无序收集器（unordered collection），并且被包含的每个字符串都是独一无二、各不相同的
HASH | 包含键值对的无序散列表
ZSET （有序集合） | 字符串成员（member）与浮点数分值（score）之间的有序映射，元素的排列顺序由分值的大小决定

#### String

应用于 String 类型的操作主要有 SET、GET和DEL。

命令 | 行为
---- | ----
GET | 获取存储在给定键中的值
SET | 设置存储在给定键中的值
DEL | 删除存储在给定键中的值（这个命令可以用于所有类型）

用法如下：

```
127.0.0.1:6379> set hello world
OK
127.0.0.1:6379> get hello
"world"
127.0.0.1:6379> del hello
(integer) 1
127.0.0.1:6379> get hello
(nil)
```

#### List

命令 | 行为
---- | ----
RPUSH | 将给定值推入到列表的右端
LRANGE | 获取列表在给定范围上的所有值
LINDEX | 获取列表在给定位置上的单个元素
LPOP | 从列表的左端弹出一个值，并返回被弹出的值

用法如下：

```
127.0.0.1:6379> rpush list-key item
(integer) 1
127.0.0.1:6379> rpush list-key item2
(integer) 2
127.0.0.1:6379> rpush list-key item
(integer) 3
127.0.0.1:6379> lrange list-key 0 -1
1) "item"
2) "item2"
3) "item"
127.0.0.1:6379> lindex list-key 1
"item2"
127.0.0.1:6379> lpop list-key
"item"
127.0.0.1:6379> lrange list-key 0 -1
1) "item2"
2) "item"
```

#### Set


命令 | 行为
---- | ----
SADD |	将给定元素添加到集合
SMEMBERS |	返回集合包含的所有元素
SISMEMBER |	检查给定元素是否存在于集合中
SREM |	如果给定的元素存在于集合中，那么移除这个元素

用法如下：

```
127.0.0.1:6379> sadd set-key item
(integer) 1
127.0.0.1:6379> sadd set-key item2
(integer) 1
127.0.0.1:6379> sadd set-key item3
(integer) 1
127.0.0.1:6379> sadd set-key item
(integer) 0
127.0.0.1:6379> smembers set-key
1) "item3"
2) "item2"
3) "item"
127.0.0.1:6379> sismember set-key item4
(integer) 0
127.0.0.1:6379> sismember set-key item
(integer) 1
127.0.0.1:6379> srem set-key item2
(integer) 1
127.0.0.1:6379> srem set-key item2
(integer) 0
127.0.0.1:6379> smembers set-key
1) "item3"
2) "item"
```

此外，集合还有 SINTER、SUNION、SDIFF 操作来实现交集、并集、差集运算。

#### Hash

命令 | 行为
---- | ----
HSET | 在散列里面关联起给定的键值对
HGET | 获取指定散列键的值
HGETALL | 获取散列包含的所有键值对
HDEL | 如果给定键存在于散列里面，那么移除这个键

用法：

```
127.0.0.1:6379> hset hash-key sub-key1 value1
(integer) 1
127.0.0.1:6379> hset hash-key sub-key2 value2
(integer) 1
127.0.0.1:6379> hset hash-key sub-key1 value1
(integer) 0
127.0.0.1:6379> hgetall hash-key
1) "sub-key1"
2) "value1"
3) "sub-key2"
4) "value2"
127.0.0.1:6379> hdel hash-key sub-key2
(integer) 1
127.0.0.1:6379> hdel hash-key sub-key2
(integer) 0
127.0.0.1:6379> hget hash-key sub-key1
"value1"
127.0.0.1:6379> hgetall hash-key
1) "sub-key1"
2) "value1"
```

#### Zset

命令 | 行为
---- | ----
ZADD | 将一个带有给定分值的成员添加到有序集合里面
ZRANGE | 根据分值的排序顺序，获取有序集合在给定位置范围内的所有元素
ZRANGEBYSCORE | 获取有序集合在给定分值范围内的所有元素
ZREM | 如果给定成员存在于有序集合，那么移除这个成员

用法：

```
127.0.0.1:6379> zadd zset-key 728 member1
(integer) 1
127.0.0.1:6379> zadd zset-key 982 member0
(integer) 1
127.0.0.1:6379> zadd zset-key 982 member0
(integer) 0
127.0.0.1:6379> zrange zset-key 0 -1 withscores
1) "member1"
2) "728"
3) "member0"
4) "982"
127.0.0.1:6379> zrangebyscore zset-key 0 800 withscores
1) "member1"
2) "728"
127.0.0.1:6379> zrem zset-key member1
(integer) 1
127.0.0.1:6379> zrem zset-key member1
(integer) 0
127.0.0.1:6379> zrange zset-key 0 -1 withscores
1) "member0"
2) "982"
```

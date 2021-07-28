<h1 id="Redis" align="center">Redis</h1>
<!-- @import "[TOC]" {cmd="toc"} -->

<!-- code_chunk_output -->

- [1. NoSQL 数据库](#1-nosql-数据库)
- [2. Redis 概述](#2-redis-概述)
- [3. 常用五大数据类型](#3-常用五大数据类型)
  - [3.1. 字符串 string](#31-字符串-string)

<!-- /code_chunk_output -->

# 1. NoSQL 数据库

NoSQL 泛指非关系型的数据库：
  - non-relational 非关系型
  - Not Only SQL 不仅仅是 SQL

特点：
  - 不遵循 SQL 标准。
  - 不支持 ACID（原子性、一致性、隔离性、持久性）。
  - 远超于 SQL 的性能。

适用场景：
  - 对数据高并发的读写
  - 海量数据的读写
  - 对数据高可扩展性的

不适用场景：
  - 需要事务支持
  - 基于 SQL 的结构化查询存储，处理复杂的关系，需要即席查询（用户根据自己的需求，灵活的选择查询条件）。


# 2. Redis 概述

> Redis 是一个开源（BSD许可）的，内存中的数据结构存储系统，它可以用作数据库、缓存和消息中间件。 它支持多种类型的数据结构，如 字符串（strings）， 散列（hashes）， 列表（lists）， 集合（sets）， 有序集合（sorted sets） 与范围查询， bitmaps， hyperloglogs 和 地理空间（geospatial） 索引半径查询。 Redis 内置了 复制（replication），LUA脚本（Lua scripting）， LRU驱动事件（LRU eviction），事务（transactions） 和不同级别的 磁盘持久化（persistence）， 并通过 Redis哨兵（Sentinel）和自动 分区（Cluster）提供高可用性（high availability）。

- 一个开源的 key-value 存储系统。
- value 支持多种数据类型：字符串（strings），散列（hashes），列表（lists），集合（sets），有序集合（sorted sets）。
- 这些数据类型都支持 push/pop, add/remove 及取交集并集和差集及更丰富的操作，而且这些操作都是原子性的。
- 为了保证效率，数据都是缓存在内存中。Redis 会周期性的把更新的数据写入磁盘或者把修改操作写入追加的记录文件。


应用场景：
- 配合关系型数据库做高速缓存
- 分布式架构中做 session 共享
- 多样的数据结构存储持久化数据（验证码、记住我、消息、注册中心）


**Redis 是单线程 + 多路 IO 复用技术**



Redis 默认端口是 6379。
默认有 16 个数据库，标号从 0 开始，初始默认使用 0 号库。

```shell
# 运行 Redis 客户端连接上 Redis 数据库
redis-cli
127.0.0.1:6379>

# 切换数据库
127.0.0.1:6379> select 2
OK
127.0.0.1:6379[2]>

# 查看当前数据库 key 的数量
127.0.0.1:6379> dbsize
(integer) 2

# 清空当前库
flushdb
# 通杀全部库
flushall
```

Key 操作

```shell
# 插入数据
127.0.0.1:6379> set aBadString 松松
# 取值
127.0.0.1:6379> get aBadString
"松松"

# 查看 key, 支持正则, 大小写敏感
127.0.0.1:6379> keys *
1) "aBadString"
127.0.0.1:6379> keys aBadString
1) "aBadString"
127.0.0.1:6379> keys abadstring
(empty array)
127.0.0.1:6379> keys a*
1) "aBadString"

# 判断某个 key 是否存在. 1 表示存在, 0 表示不存在
127.0.0.1:6379> exists aBadString
(integer) 1
127.0.0.1:6379> exists abadstring
(integer) 0

# 查看 key 的数据类型
127.0.0.1:6379> type aBadString
string
127.0.0.1:6379> type abadstring
none

# 删除指定 key 的数据
127.0.0.1:6379> del aBadString
(integer) 1
# 非阻塞删除
127.0.0.1:6379> unlink aBadString
(integer) 1

# 设置过期时间 (秒)
127.0.0.1:6379> expire aBadString 10
(integer) 1

# 查看还有多少秒过期. -1表示永不过期, -2表示已过期
ttl aBadString
(integer) -1
# 还有四秒过期
127.0.0.1:6379> ttl aBadString
(integer) 4
# 已经过期
127.0.0.1:6379> ttl aBadString
(integer) -2
```


# 3. 常用五大数据类型

## 3.1. 字符串 string

string 类型是二进制安全的，最多可以是 512 M。

```shell
# 插入
127.0.0.1:6379> set k1 v1
OK
# 取值
127.0.0.1:6379> get k1
"v1"

# 覆盖
127.0.0.1:6379> set k1 v11
OK
127.0.0.1:6379> get k1
"v11"

# 追加. 返回字符串长度
127.0.0.1:6379> append k1 456
(integer) 6
127.0.0.1:6379> get k1
"v11456"

# 获取字符串长度
127.0.0.1:6379> strlen k1
(integer) 6

# 不覆盖地插入值. k1 存在, 不覆盖
127.0.0.1:6379> setnx k1 v1
(integer) 0
127.0.0.1:6379> get k1
"v11456"
# k2 存在, 则添加
127.0.0.1:6379> setnx k2 v2
(integer) 1
127.0.0.1:6379> get k2
"v2"
```
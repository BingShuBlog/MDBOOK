# Redis

## **NoSql数据库**

 

### 1 **R**edis介绍

##### 1.1 **什么是NoSql**

为了解决高并发、高可用、高可扩展，大数据存储等一系列问题而产生的数据库解决方案，就是NoSql。

 

NoSql，叫非关系型数据库，它的全名Not only sql。它不能替代关系型数据库，只能作为关系型数据库的一个良好补充。

 

##### 1.2 **NoSql的分类**

- 键值(Key-Value)存储数据库

相关产品： Tokyo Cabinet/Tyrant、**Redis**、Voldemort、Berkeley DB

典型应用： 内容缓存，主要用于处理大量数据的高访问负载。 

数据模型： 一系列键值对

优势： 快速查询

劣势： 存储的数据缺少结构化



- 列存储数据库

相关产品：Cassandra, **HBase**, Riak

典型应用：分布式的文件系统

数据模型：以列簇式存储，将同一列数据存在一起

优势：查找速度快，可扩展性强，更容易进行分布式扩展

 劣势：功能相对局限



- 文档型数据库

相关产品：CouchDB、**MongoDB**

典型应用：Web应用（与Key-Value类似，Value是结构化的）

数据模型： 一系列键值对

 优势：数据结构要求不严格

 劣势： 查询性能不高，而且缺乏统一的查询语法

-  图形(Graph)数据库

相关数据库：Neo4J、InfoGrid、Infinite Graph

典型应用：社交网络

数据模型：图结构

优势：利用图结构相关算法。

劣势：需要对整个图做计算才能得出结果，不容易做分布式的集群方案。

 

## 1.3 **什么是redis**

Redis是使用c语言开发的一个高性能键值数据库。Redis可以通过一些键值类型来存储数据。

键值类型：

1. String字符类型
2. map散列类型
3. list列表类型
4. set集合类型
5. sortedset有序集合类型

 

## 1.4 **redis历史发展**

​	2008年，意大利的一家创业公司Merzia推出了一款基于MySQL的网站实时统计系统LLOOGG，然而没过多久该公司的创始人 Salvatore Sanfilippo便 对MySQL的性能感到失望，于是他决定亲自为LLOOGG量身定做一个数据库，并于2009年开发完成，这个数据库就是Redis。 不过Salvatore Sanfilippo并不满足只将Redis用于LLOOGG这一款产品，而是希望更多的人使用它，于是在同一年Salvatore Sanfilippo将Redis开源发布，并开始和Redis的另一名主要的代码贡献者Pieter Noordhuis一起继续着Redis的开发，直到今天。

​	Salvatore Sanfilippo自己也没有想到，短短的几年时间，Redis就拥有了庞大的用户群体。Hacker News在2012年发布了一份数据库的使用情况调查，结果显示有近12%的公司在使用Redis。国内如新浪微博、街旁网、知乎网，国外如GitHub、Stack Overflow、Flickr等都是Redis的用户。

​	VMware公司从2010年开始赞助Redis的开发， Salvatore Sanfilippo和Pieter Noordhuis也分别在3月和5月加入VMware，全职开发Redis。

 

## 1.5 **redis的应用场景**

- 缓存（数据查询、短连接、新闻内容、商品内容等等）。（**最多使用**）
- 分布式集群架构中的session分离。
- 聊天室的在线好友列表。
- 任务队列。（秒杀、抢购、12306等等）
- 应用排行榜。
- 网站访问统计。
- 数据过期处理（可以精确到毫秒）



# 1 **redis安装**

## 1.1 **redis下载**

官网地址：<http://redis.io/>

下载地址：<http://download.redis.io/releases/redis-3.0.0.tar.gz>

![img](file:///C:\Users\ABC\AppData\Local\Temp\ksohtml\wps1A68.tmp.jpg) 

 

## 1.2 **redis的安装**

redis的安装环境会安装到linux系统中。

 

第一步：安装VMware，并且在VMware中安装centos系统（参考linux教程）。

第二步：将redis的压缩包，上传到linux系统

第三步：对redis的压缩包进行解压缩

```
Redis解压缩之后的文件是用c语言写的源码文件
命令：tar -zxf redis-3.0.0.tar.gz

第四步：安装c语言环境（安装centos之后，自带c语言环境）
命令： yum install gcc-c++

第五步：编译redis源码
命令： cd redis-3.0.0
       make
       
第六步：安装redis
命令： make install PREFIX=/usr/local/redis19
```



**连接操作相关的命令**

- 默认直接连接  远程连接-h 192.168.1.20 -p 6379
- ping：测试连接是否存活如果正常会返回pong
- echo：打印
- select：切换到指定的数据库，数据库索引号 `index` 用数字值指定，以 `0` 作为起始索引值
- quit：关闭连接（connection）
- auth：简单密码认证

**服务端相关命令**

- time：返回当前服务器时间
- client list: 返回所有连接到服务器的客户端信息和统计数据  参见http://redisdoc.com/server/client_list.html
- client kill ip:port：关闭地址为 `ip:port` 的客户端
- save：将数据同步保存到磁盘
- bgsave：将数据异步保存到磁盘
- lastsave：返回上次成功将数据保存到磁盘的Unix时戳
- shundown：将数据同步保存到磁盘，然后关闭服务
- info：提供服务器的信息和统计
- config resetstat：重置info命令中的某些统计数据
- config get：获取配置文件信息
- config set：动态地调整 Redis 服务器的配置(configuration)而无须重启，可以修改的配置参数可以使用命令 `CONFIG GET *` 来列出
- config rewrite：Redis 服务器时所指定的 `redis.conf` 文件进行改写
- monitor：实时转储收到的请求
- slaveof：改变复制策略设置

**发布订阅相关命令**

- psubscribe：订阅一个或多个符合给定模式的频道 例如psubscribe news.* tweet.*
- publish：将信息 `message` 发送到指定的频道 `channel 例如publish msg "good morning"`
- pubsub channels：列出当前的活跃频道 例如PUBSUB CHANNELS news.i*
- pubsub numsub：返回给定频道的订阅者数量 例如PUBSUB NUMSUB news.it news.internet news.sport news.music
- pubsub numpat：返回客户端订阅的所有模式的数量总和
- punsubscribe：指示客户端退订所有给定模式。
- subscribe：订阅给定的一个或多个频道的信息。例如 subscribe msg chat_room
- unsubscribe：指示客户端退订给定的频道。



**对KEY操作的命令**

- exists(key)：确认一个key是否存在
- del(key)：删除一个key
- type(key)：返回值的类型
- keys(pattern)：返回满足给定pattern的所有key
- randomkey：随机返回key空间的一个
- keyrename(oldname, newname)：重命名key
- dbsize：返回当前数据库中key的数目
- expire：设定一个key的活动时间（s）
- ttl：获得一个key的活动时间
- move(key, dbindex)：移动当前数据库中的key到dbindex数据库
- flushdb：删除当前选择数据库中的所有key
- flushall：删除所有数据库中的所有key

**对String操作的命令**

- set(key, value)：给数据库中名称为key的string赋予值value
- get(key)：返回数据库中名称为key的string的value
- getset(key, value)：给名称为key的string赋予上一次的value
- mget(key1, key2,…, key N)：返回库中多个string的value
- setnx(key, value)：添加string，名称为key，值为value
- setex(key, time, value)：向库中添加string，设定过期时间time
- mset(key N, value N)：批量设置多个string的值
- msetnx(key N, value N)：如果所有名称为key i的string都不存在
- incr(key)：名称为key的string增1操作
- incrby(key, integer)：名称为key的string增加integer
- decr(key)：名称为key的string减1操作
- decrby(key, integer)：名称为key的string减少integer
- append(key, value)：名称为key的string的值附加value
- substr(key, start, end)：返回名称为key的string的value的子串

**对List操作的命令**

- rpush(key, value)：在名称为key的list尾添加一个值为value的元素
- lpush(key, value)：在名称为key的list头添加一个值为value的 元素
- llen(key)：返回名称为key的list的长度
- lrange(key, start, end)：返回名称为key的list中start至end之间的元素
- ltrim(key, start, end)：截取名称为key的list
- lindex(key, index)：返回名称为key的list中index位置的元素
- lset(key, index, value)：给名称为key的list中index位置的元素赋值
- lrem(key, count, value)：删除count个key的list中值为value的元素
- lpop(key)：返回并删除名称为key的list中的首元素
- rpop(key)：返回并删除名称为key的list中的尾元素
- blpop(key1, key2,… key N, timeout)：lpop命令的block版本。
- brpop(key1, key2,… key N, timeout)：rpop的block版本。
- rpoplpush(srckey, dstkey)：返回并删除名称为srckey的list的尾元素，并将该元素添加到名称为dstkey的list的头部

**对Set操作的命令**

- sadd(key, member)：向名称为key的set中添加元素member
- srem(key, member) ：删除名称为key的set中的元素member
- spop(key) ：随机返回并删除名称为key的set中一个元素
- smove(srckey, dstkey, member) ：移到集合元素
- scard(key) ：返回名称为key的set的基数
- sismember(key, member) ：member是否是名称为key的set的元素
- sinter(key1, key2,…key N) ：求交集
- sinterstore(dstkey, (keys)) ：求交集并将交集保存到dstkey的集合
- sunion(key1, (keys)) ：求并集
- sunionstore(dstkey, (keys)) ：求并集并将并集保存到dstkey的集合
- sdiff(key1, (keys)) ：求差集
- sdiffstore(dstkey, (keys)) ：求差集并将差集保存到dstkey的集合
- smembers(key) ：返回名称为key的set的所有元素
- srandmember(key) ：随机返回名称为key的set的一个元素

**对Hash操作的命令**

- hset(key, field, value)：向名称为key的hash中添加元素field
- hget(key, field)：返回名称为key的hash中field对应的value
- hmget(key, (fields))：返回名称为key的hash中field i对应的value
- hmset(key, (fields))：向名称为key的hash中添加元素field
- hincrby(key, field, integer)：将名称为key的hash中field的value增加integer
- hexists(key, field)：名称为key的hash中是否存在键为field的域
- hdel(key, field)：删除名称为key的hash中键为field的域
- hlen(key)：返回名称为key的hash中元素个数
- hkeys(key)：返回名称为key的hash中所有键
- hvals(key)：返回名称为key的hash中所有键对应的value
- hgetall(key)：返回名称为key的hash中所有的键（field）及其对应的value









## 连接操作相关的命令

**quit**：关闭连接（connection）

**auth**：简单密码认证

## 对value操作的命令

**exists(key)**：确认一个key是否存在

**del(key)**：删除一个key

**type(key)**：返回值的类型

**keys(pattern)**：返回满足给定pattern的所有key

**randomkey**：随机返回key空间的一个key

**rename(oldname, newname)**：将key由oldname重命名为newname，若newname存在则删除newname表示的key

**dbsize**：返回当前数据库中key的数目

**expire**：设定一个key的活动时间（s）

**ttl**：获得一个key的活动时间

**select(index)**：按索引查询

**move(key, dbindex)**：将当前数据库中的key转移到有dbindex索引的数据库

**flushdb**：删除当前选择数据库中的所有key

**flushall**：删除所有数据库中的所有key

## 对String操作的命令

**set(key, value)**：给数据库中名称为key的string赋予值value

**get(key)**：返回数据库中名称为key的string的value

**getset(key, value)**：给名称为key的string赋予上一次的value

**mget(key1, key2,…, key N)**：返回库中多个string（它们的名称为key1，key2…）的value

**setnx(key, value)**：如果不存在名称为key的string，则向库中添加string，名称为key，值为value

**setex(key, time, value)**：向库中添加string（名称为key，值为value）同时，设定过期时间time

**mset(key1, value1, key2, value2,…key N, value N)**：同时给多个string赋值，名称为key i的string赋值value i

**msetnx(key1, value1, key2, value2,…key N, value N)**：如果所有名称为key i的string都不存在，则向库中添加string，名称key i赋值为value i

**incr(key)**：名称为key的string增1操作

**incrby(key, integer)**：名称为key的string增加integer

**decr(key)**：名称为key的string减1操作

**decrby(key, integer)**：名称为key的string减少integer

**append(key, value)**：名称为key的string的值附加value

**substr(key, start, end)**：返回名称为key的string的value的子串

## 对List操作的命令

**rpush(key, value)**：在名称为key的list尾添加一个值为value的元素

**lpush(key, value)**：在名称为key的list头添加一个值为value的 元素

**llen(key)**：返回名称为key的list的长度

**lrange(key, start, end)**：返回名称为key的list中start至end之间的元素（下标从0开始，下同）

**ltrim(key, start, end)**：截取名称为key的list，保留start至end之间的元素

**lindex(key, index)**：返回名称为key的list中index位置的元素

**lset(key, index, value)**：给名称为key的list中index位置的元素赋值为value

**lrem(key, count, value)**：删除count个名称为key的list中值为value的元素。count为0，删除所有值为value的元素，count>0从头至尾删除count个值为value的元素，count<0从尾到头删除|count|个值为value的元素。

**lpop(key)**：返回并删除名称为key的list中的首元素

**rpop(key)**：返回并删除名称为key的list中的尾元素

**blpop(key1, key2,… key N, timeout)**：lpop命令的block版本。即当timeout为0时，若遇到名称为key i的list不存在或该list为空，则命令结束。如果timeout>0，则遇到上述情况时，等待timeout秒，如果问题没有解决，则对key i+1开始的list执行pop操作。

**brpop(key1, key2,… key N, timeout)**：rpop的block版本。参考上一命令。

**rpoplpush(srckey, dstkey)**：返回并删除名称为srckey的list的尾元素，并将该元素添加到名称为dstkey的list的头部

## 对Set操作的命令

**sadd(key, member)**：向名称为key的set中添加元素member

**srem(key, member)** ：删除名称为key的set中的元素member

**spop(key)** ：随机返回并删除名称为key的set中一个元素

**smove(srckey, dstkey, member)** ：将member元素从名称为srckey的集合移到名称为dstkey的集合

**scard(key)** ：返回名称为key的set的基数

**sismember(key, member)** ：测试member是否是名称为key的set的元素

**sinter(key1, key2,…key N)** ：求交集

**sinterstore(dstkey, key1, key2,…key N)** ：求交集并将交集保存到dstkey的集合

**sunion(key1, key2,…key N)** ：求并集

**sunionstore(dstkey, key1, key2,…key N)** ：求并集并将并集保存到dstkey的集合

**sdiff(key1, key2,…key N)** ：求差集

**sdiffstore(dstkey, key1, key2,…key N)** ：求差集并将差集保存到dstkey的集合

**smembers(key)** ：返回名称为key的set的所有元素

**srandmember(key)** ：随机返回名称为key的set的一个元素

## 对zset（sorted set）操作的命令

**zadd(key, score, member)**：向名称为key的zset中添加元素member，score用于排序。如果该元素已经存在，则根据score更新该元素的顺序。

**zrem(key, member)** ：删除名称为key的zset中的元素member

**zincrby(key, increment, member)** ：如果在名称为key的zset中已经存在元素member，则该元素的score增加increment；否则向集合中添加该元素，其score的值为increment

**zrank(key, member)** ：返回名称为key的zset（元素已按score从小到大排序）中member元素的rank（即index，从0开始），若没有member元素，返回“nil”

**zrevrank(key, member)** ：返回名称为key的zset（元素已按score从大到小排序）中member元素的rank（即index，从0开始），若没有member元素，返回“nil”

**zrange(key, start, end)**：返回名称为key的zset（元素已按score从小到大排序）中的index从start到end的所有元素

**zrevrange(key, start, end)**：返回名称为key的zset（元素已按score从大到小排序）中的index从start到end的所有元素

**zrangebyscore(key, min, max)**：返回名称为key的zset中score >= min且score <= max的所有元素

**zcard(key)**：返回名称为key的zset的基数

**zscore(key, element)**：返回名称为key的zset中元素element的score

**zremrangebyrank(key, min, max)**：删除名称为key的zset中rank >= min且rank <= max的所有元素

**zremrangebyscore(key, min, max)** ：删除名称为key的zset中score >= min且score <= max的所有元素

**zunionstore / zinterstore(dstkeyN, key1,…,keyN, WEIGHTS w1,…wN, AGGREGATE SUM|MIN|MAX)**：对N个zset求并集和交集，并将最后的集合保存在dstkeyN中。对于集合中每一个元素的score，在进行AGGREGATE运算前，都要乘以对于的WEIGHT参数。如果没有提供WEIGHT，默认为1。默认的AGGREGATE是SUM，即结果集合中元素的score是所有集合对应元素进行SUM运算的值，而MIN和MAX是指，结果集合中元素的score是所有集合对应元素中最小值和最大值。

## 对Hash操作的命令

**hset(key, field, value)**：向名称为key的hash中添加元素field<—>value

**hget(key, field)**：返回名称为key的hash中field对应的value

**hmget(key, field1, …,field N)**：返回名称为key的hash中field i对应的value

**hmset(key, field1, value1,…,field N, value N)**：向名称为key的hash中添加元素field i<—>value i

**hincrby(key, field, integer)**：将名称为key的hash中field的value增加integer

**hexists(key, field)**：名称为key的hash中是否存在键为field的域

**hdel(key, field)**：删除名称为key的hash中键为field的域

**hlen(key)**：返回名称为key的hash中元素个数

**hkeys(key)**：返回名称为key的hash中所有键

**hvals(key)**：返回名称为key的hash中所有键对应的value

**hgetall(key)**：返回名称为key的hash中所有的键（field）及其对应的value

## 持久化

**save**：将数据同步保存到磁盘

**bgsave**：将数据异步保存到磁盘

**lastsave**：返回上次成功将数据保存到磁盘的Unix时戳

**shundown**：将数据同步保存到磁盘，然后关闭服务

## 远程服务控制

**info**：提供服务器的信息和统计

**monitor**：实时转储收到的请求

**slaveof**：改变复制策略设置

**config**：在运行时配置Redis服务器





# 1 **Redis的主从复制**

## 1.1 **什么是主从复制**

 

​	持久化保证了即使redis服务重启也不会丢失数据，因为redis服务重启后会将硬盘上持久化的数据恢复到内存中，但是当redis服务器的硬盘损坏了可能会导致数据丢失，如果**通过redis的主从复制机制就可以避免这种单点故障**，如下图：

 

 

![img](file:///C:\Users\ABC\AppData\Local\Temp\ksohtml\wps5661.tmp.png)

 

说明：

n 主redis中的数据有两个副本（replication）即从redis1和从redis2，即使一台redis服务器宕机其它两台redis服务也可以继续提供服务。

n 主redis中的数据和从redis上的数据保持实时同步，当主redis写入数据时通过主从复制机制会复制到两个从redis服务上。

n 只有一个主redis，可以有多个从redis。

n 主从复制不会阻塞master，在同步数据时，master 可以继续处理client 请求

n 一个redis可以即是主又是从，如下图：

 

 

![img](file:///C:\Users\ABC\AppData\Local\Temp\ksohtml\wps5662.tmp.png)

 

## 1.2 **主从复制设置**

### 1.2.1 **主机配置**

无需配置

### 1.2.2 **从机配置**

第一步：复制出一个从机

命令：cp bin/ bin2 –r



第二步：修改从机的redis.conf

命令：Slaveof masterip masterport

slaveof 192.168.242.137 6379

 

第三步：改从机的port地址为6380

命令：在redis.conf中修改

 

第四步：清除从机中的持久化文件

命令：rm -rf appendonly.aof dump.rdb

 

第五步：启动从机

命令： ./redis-server redis.conf

 

第六步：启动6380的客户端

命令： ./redis-cli -p 6380  

注意：

​	主机一旦发生增删改操作，那么从机会将数据同步到从机中

​	从机不能执行写操作

 

 

# 2 **Redis集群**

## 2.1 **redis-cluster架构图**

![img](file:///C:\Users\ABC\AppData\Local\Temp\ksohtml\wps5675.tmp.png) 

架构细节:

(1)所有的redis节点彼此互联(PING-PONG机制),内部使用二进制协议优化传输速度和带宽.

(2)节点的fail是通过集群中**超过半数的节点检测**失效时才生效.

(3)客户端与redis节点直连,不需要中间proxy层.客户端不需要连接集群所有节点,连接集群中任何一个可用节点即可

(4)redis-cluster把所有的物理节点映射到[0-16383]slot上,cluster 负责维护node<->slot<->value

Redis 集群中内置了 16384 个哈希槽，当需要在 Redis 集群中放置一个 key-value 时，redis 先对 key 使用 crc16 算法算出一个结果，然后把结果对 16384 求余数，这样每个 key 都会对应一个编号在 0-16383 之间的哈希槽，redis 会根据节点数量大致均等的将哈希槽映射到不同的节点

 

示例如下：

![img](file:///C:\Users\ABC\AppData\Local\Temp\ksohtml\wps5676.tmp.png)![img](file:///C:\Users\ABC\AppData\Local\Temp\ksohtml\wps5677.tmp.png)![img](file:///C:\Users\ABC\AppData\Local\Temp\ksohtml\wps5678.tmp.png)![img](file:///C:\Users\ABC\AppData\Local\Temp\ksohtml\wps5688.tmp.png)![img](file:///C:\Users\ABC\AppData\Local\Temp\ksohtml\wps5689.tmp.png)![img](file:///C:\Users\ABC\AppData\Local\Temp\ksohtml\wps568A.tmp.png)![img](file:///C:\Users\ABC\AppData\Local\Temp\ksohtml\wps568B.tmp.png)![img](file:///C:\Users\ABC\AppData\Local\Temp\ksohtml\wps568C.tmp.png)![img](file:///C:\Users\ABC\AppData\Local\Temp\ksohtml\wps568D.tmp.png)![img](file:///C:\Users\ABC\AppData\Local\Temp\ksohtml\wps568E.tmp.png) 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

![img](file:///C:\Users\ABC\AppData\Local\Temp\ksohtml\wps569F.tmp.png) 

 

 

 

![img](file:///C:\Users\ABC\AppData\Local\Temp\ksohtml\wps56A0.tmp.png) 

 

 

 

 

 

## 2.2 **redis-cluster**投票:容错

![img](file:///C:\Users\ABC\AppData\Local\Temp\ksohtml\wps56A1.tmp.png)

 

(1)集群中所有master参与投票,如果半数以上master节点与其中一个master节点通信超过(cluster-node-timeout),认为该master节点挂掉.

(2):什么时候整个集群不可用(cluster_state:fail)? 

Ø 如果集群任意master挂掉,且当前master没有slave，则集群进入fail状态。也可以理解成集群的[0-16383]slot映射不完全时进入fail状态。

Ø 如果集群超过半数以上master挂掉，无论是否有slave，集群进入fail状态。

 

 

## 2.3 **安装ruby**

集群管理工具（redis-trib.rb）是使用ruby脚本语言编写的。

 

第一步：安装ruby

[root@itheima bin2]# yum install ruby

[root@itheima bin2]# yum install rubygems

 

第二步：将以下文件上传到linux系统

![img](file:///C:\Users\ABC\AppData\Local\Temp\ksohtml\wps56A2.tmp.jpg) 

 

 

第三步：安装ruby和redis接口

[root@itheima ~]# gem install redis-3.0.0.gem

 

第四步：将redis-3.0.0包下src目录中的以下文件拷贝到redis19/redis-cluster/

![img](file:///C:\Users\ABC\AppData\Local\Temp\ksohtml\wps56B2.tmp.jpg) 

[root@itheima src]# cd /usr/local/redis19/

[root@itheima redis19]# mkdir redis-cluster

[root@itheima redis19]# cd /root/redis-3.0.0/src/

[root@itheima src]# cp redis-trib.rb  /usr/local/redis19/redis-cluster

 

第五步：查看是否拷贝成功

![img](file:///C:\Users\ABC\AppData\Local\Temp\ksohtml\wps56B3.tmp.jpg) 

 

## 2.4 **搭建集群**

搭建集群最少也得需要3台主机，如果每台主机再配置一台从机的话，则最少需要6台机器。

 

端口设计如下：7001-7006

 

第一步：复制出一个7001机器

[root@itheima redis19]# cp bin ./redis-cluster/7001 –r

 

第二步：如果存在持久化文件，则删除

[root@itheima 7001]# rm -rf appendonly.aof dump.rdb

 

第三步：设置集群参数

![img](file:///C:\Users\ABC\AppData\Local\Temp\ksohtml\wps56B4.tmp.jpg) 

 

第四步：修改端口

![img](file:///C:\Users\ABC\AppData\Local\Temp\ksohtml\wps56C5.tmp.jpg) 

 

第五步：复制出7002-7006机器

[root@itheima redis-cluster]# cp 7001/ 7002 -r

[root@itheima redis-cluster]# cp 7001/ 7003 -r

[root@itheima redis-cluster]# cp 7001/ 7004 -r

[root@itheima redis-cluster]# cp 7001/ 7005 -r

[root@itheima redis-cluster]# cp 7001/ 7006 –r

 

第六步：修改7002-7006机器的端口

 

第七步：启动7001-7006这六台机器

![img](file:///C:\Users\ABC\AppData\Local\Temp\ksohtml\wps56C6.tmp.jpg) 

 

第八步：修改start-all.sh文件的权限

[root@itheima redis-cluster]# chmod u+x start-all.sh

 

[root@itheima redis-cluster]# ./start-all.sh

 

第九步：创建集群

 

 

 

 

## 2.5 **连接集群**

[root@itheima 7001]# ./redis-cli -h 192.168.242.137 -p 7001 **–c**

 

-c：指定是集群连接

 

![img](file:///C:\Users\ABC\AppData\Local\Temp\ksohtml\wps56C7.tmp.jpg) 
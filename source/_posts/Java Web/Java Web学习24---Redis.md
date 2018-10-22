---
layout: post
title: Redis
comments: true
toc: true
date: 2017-01-13 11:52:55
tags:
	- Java
	- Redis
	- 策略模式
---

Redis是用C语言开发的一个开源的高性能键值对（key-value）数据库。它通过提供多种键值数据类型来适应不同场景下的存储需求，目前为止Redis支持的键值数据类型如下：
* 字符串类型
* 散列类型
* 列表类型
* 集合类型
* 有序集合类型。

<!--more-->

## <font color=orange> NoSQL </font>

`NoSQL`，泛指非关系型的数据库，NoSQL即Not-Only SQL，它可以作为关系型数据库的良好补充。随着互联网web2.0网站的兴起，非关系型的数据库现在成了一个极其热门的新领域，非关系数据库产品的发展非常迅速。而传统的关系数据库在应付web2.0网站，特别是超大规模和高并发的SNS类型的web2.0纯动态网站已经显得力不从心，暴露了很多难以克服的问题: 
* `High performance`: 对数据库高并发读写的需求 
* `Huge Storage`: 对海量数据的高效率存储和访问的需求
* `High Scalability && High Availability`: 对数据库的高可扩展性和高可用性的需求 

### <font color=orange> 常见的NoSQL数据库 </font>
#### <font color=orange> 键值(Key-Value)存储数据库 </font>
相关产品： Tokyo Cabinet/Tyrant、<font color=red>Redis</font>、Voldemort、Berkeley DB
典型应用： 内容缓存，主要用于处理大量数据的高访问负载。 
数据模型： 一系列键值对
优势： 快速查询
劣势： 存储的数据缺少结构化
#### <font color=orange> 列存储数据库 </font>
相关产品：Cassandra, HBase, Riak
典型应用：分布式的文件系统
数据模型：以列簇式存储，将同一列数据存在一起
优势：查找速度快，可扩展性强，更容易进行分布式扩展
劣势：功能相对局限
#### <font color=orange> 文档型数据库 </font>
相关产品：CouchDB、MongoDB
典型应用：Web应用（与Key-Value类似，Value是结构化的）
数据模型： 一系列键值对
优势：数据结构要求不严格
劣势： 查询性能不高，而且缺乏统一的查询语法
#### <font color=orange> 图形(Graph)数据库 </font>
相关数据库：Neo4J、InfoGrid、Infinite Graph
典型应用：社交网络
数据模型：图结构
优势：利用图结构相关算法。
劣势：需要对整个图做计算才能得出结果，不容易做分布式的集群方案。

## <font color=orange> Redis </font>
Redis是C语言开发，建议在linux上运行. Redis 3.0版本主要增加了Redis集群功能, 源码编译依赖gcc环境, 没有安装gcc的可以执行: `yum install gcc-c++`.
Redis的应用场景: 
* 缓存（数据查询、短连接、新闻内容、商品内容等等）。（最多使用）
* 分布式集群架构中的session分离。
* 聊天室的在线好友列表。
* 任务队列。（秒杀、抢购、12306等等）
* 应用排行榜。
* 网站访问统计。
* 数据过期处理（可以精确到毫秒）

### <font color=orange> 下载、安装和运行Redis </font>
[官网下载地址](http://download.Redis.io/releases/), 这里我们使用3.0版本进行安装.
一个Redis进程就是一个Redis实例，一台服务器可以同时有多个Redis实例，不同的Redis实例提供不同的服务端口对外提供服务，每个Redis实例之间互相影响。每个Redis实例都包括自己的数据库，数据库中可以存储自己的数据。一个Redis实例最多可提供16个数据库，下标从0到15，客户端默认连接第0号数据库，也可以通过select选择连接哪个数据库. 
* 下载
	* redis: `wget http://download.Redis.io/releases/Redis-3.0.0.tar.gz`
* 解压
	* 解压: `tar -zxvf Redis-3.0.0.tar.gz`
* 安装
	* 指定目录安装
		* 创建安装目录: `mkdir -p /usr/local/Redis`
		* 编译: `make`
		* 指定安装目录安装: `make PREFIX=/usr/local/Redis/ install`
	* 可以安装到`/usr/local/bin目录中`, 这样运行Redis的话就不用输入目录了
		* 在解压目录中运行: `make`
		* 在解压目录中运行: `sudo make install`
* 拷贝配置文件到安装目录下
	* 进入源码目录，里面有一份配置文件 Redis.conf，然后将其拷贝到安装路径下
	* 运行: `cp Redis.conf /usr/local/Redis/bin/`
* 安装目录下的文件
	* Redis-server: Redis服务器
	* Redis-cli: Redis命令行客户端
	* Redis-benchmark: Redis性能测试工具
	* Redis-check-aof: AOF文件修复工具
	* Redis-check-dump: RDB文件检查工具
	* Redis-sentinel: Redis集群管理工具(3.0以后版本才有)
* 启动: Redis默认端口是: 6379
	* 前端模式: 前端模式启动的缺点是ssh命令窗口关闭则Redis-server程序结束，不推荐使用此方法。
		* `*/Redis-server`
	* 后端模式: 修改`Redis.conf`配置文件， 修改`daemonize yes`以后端模式启动。
		* 进入安装: `cd /usr/local/Redis/bin`
		* 运行: `./Redis-server ./Redis.conf`即可
* 停止Redis
	* 运行: `/usr/local/Redis/Redis-cli shutdown`
* 连接Redis服务器
	* 运行: `Redis-cli [-h host] [-p port]`
* 向Redis服务端发送命令
	* `ping`: 用来测试客户端和服务器的连接是否正常, 正常则返回`PONG`
	* `set`: 向Redis设置数据
		* 如: `set name Lucy`
	* `get`: 向Redis取数据
		* 如: `get name`
	* `del`: 删除指定key
	* `Keys *`: 查看当前库中所有的key值
	* `select 下标`: Redis最多可以提供16个数据库, 下标从0到15, 默认连接的是0号下标, 该命令可以切换数据库.
	* `flushall`: 清空数据库, 会把该Redis实例下面的数据库都情况.<font color=red>所以不同应用应该使用不同的Redis实例, 而不是同一个Redis实例下的不同数据库.</font>
* 设置开机启动
	* 首先把redis解压目录中的`/redis-x.x.x/utils/redis_init_script`拷贝到`/etc/init.d/redis`
```
cp redis-4.0.0/utils/redis_init_script /etc/init.d/redis
```
	* 修改`/etc/init.d/redis`
```
#!/bin/sh
#
# Simple Redis init.d script conceived to work on Linux systems
# as it does use of the /proc filesystem.
# 这里两行不能少, 拷贝的没有需要添加上
# chkconfig: 2345 90 10    
# description: Start and Stop redis

REDISPORT=6379
#安装的启动路径
EXEC=/usr/local/bin/redis-server
CLIEXEC=/usr/local/bin/redis-cli
PIDFILE=/var/run/redis_${REDISPORT}.pid
#配置文件位置
CONF="/usr/local/bin/redis.conf"
case "$1" in
    start)
        if [ -f $PIDFILE ]
        then
                echo "$PIDFILE exists, process is already running or crashed"
        else
                echo "Starting Redis server..."
                $EXEC $CONF
        fi
        ;;
    stop)
        if [ ! -f $PIDFILE ]
        then
                echo "$PIDFILE does not exist, process is not running"
        else
                PID=$(cat $PIDFILE)
                echo "Stopping ..."
                $CLIEXEC -p $REDISPORT shutdown
                while [ -x /proc/${PID} ]
                do
"/etc/init.d/redis" 44L, 1155C
```
	* 开机启动设置
		* 添加redis服务: `chkconfig --add redis`
		* 设置为开机启动: `chkconfig redis on`
		* 打开redis: `service redis start`
		* 关闭redis: `service redis stop`
* 阿里云ECS中需要额外设置
	* 添加`安全组规则`, 开放`6379端口`
	* 修改`redis.conf`文件, 找到`requirepass`字段, 去掉注释, 添加授权密码
	* 修改`redis.conf`文件, 注释掉`/bind 127.0.0.1`以允许所有ip访问



## <font color=orange> Jedis </font>
Redis不仅是使用命令来操作，现在基本上主流的语言都有客户端支持，比如java、C、C#、C++、php、Node.js、Go等。 
在官方网站里列一些Java的客户端，有Jedis、Redisson、JRedis、JDBC-Redis、等其中官方推荐使用Jedis和Redisson。 在企业中用的最多的就是Jedis，下面我们就重点学习下[Jedis](https://github.com/xetorthio/jedis )。 

### <font color=orange> 使用Jedis </font>
Linux服务器中如果已经安装好Redis服务, 并已经启动, 如果Linux有防火墙, 需要先开启Redis端口, 默认: `6379`
* 开启防火墙

>	永久开启8080端口: `firewall-cmd --add-port=8080/tcp --zone=public --permanent`
	查询所有已经开启的端口: `firewall-cmd --zone=public --list-ports`
	移除8080端口:`firewall-cmd --permanent --zone=public --remove-port=8080/tcp`
	刷新防火墙使设置生效: `firewall-cmd --reload`
	重启防火墙: `sudo systemctl restart firewalld`

* Maven: 
```xml
<dependency>
    <groupId>Redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>${jedis.version}</version>
</dependency>
```
* Spring配置文件中: 
```xml
<!-- 连接池配置 -->
<bean id="jedisPoolConfig" class="Redis.clients.jedis.JedisPoolConfig">
    <!-- 最大连接数 -->
    <property name="maxTotal" value="30" />
    <!-- 最大空闲连接数 -->
    <property name="maxIdle" value="10" />
    <!-- 每次释放连接的最大数目 -->
    <property name="numTestsPerEvictionRun" value="1024" />
    <!-- 释放连接的扫描间隔（毫秒） -->
    <property name="timeBetweenEvictionRunsMillis" value="30000" />
    <!-- 连接最小空闲时间 -->
    <property name="minEvictableIdleTimeMillis" value="1800000" />
    <!-- 连接空闲多久后释放, 当空闲时间>该值 且 空闲连接>最大空闲连接数 时直接释放 -->
    <property name="softMinEvictableIdleTimeMillis" value="10000" />
    <!-- 获取连接时的最大等待毫秒数,小于零:阻塞不确定的时间,默认-1 -->
    <property name="maxWaitMillis" value="1500" />
    <!-- 在获取连接的时候检查有效性, 默认false -->
    <property name="testOnBorrow" value="true" />
    <!-- 在空闲时检查有效性, 默认false -->
    <property name="testWhileIdle" value="true" />
    <!-- 连接耗尽时是否阻塞, false报异常,ture阻塞直到超时, 默认true -->
    <property name="blockWhenExhausted" value="false" />
</bean>
	
<!-- Redis单机(非集群) 通过连接池 -->
<bean id="jedisPool" class="Redis.clients.jedis.JedisPool" destroy-method="close">
    <constructor-arg name="poolConfig" ref="jedisPoolConfig"/>
    <constructor-arg name="host" value="192.168.1.184"/>
    <constructor-arg name="port" value="6379"/>
</bean>
```
* 测试类: 
```java

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext-dao.xml")
public class JedisTest {
    @Resource(name = "jedisPool")
    private JedisPool pool;
    
    @Test
    public void testJedis() throws Exception {
        //创建连接
        Jedis jedis = new Jedis("192.168.1.184", 6379);

        jedis.set("name", "名称");

        System.out.println(jedis.get("name"));

        jedis.close();
    }

    @Test
    public void testJedisPool() throws Exception {
        //创建连接池
        JedisPool pool = new JedisPool("192.168.1.184", 6379);

        Jedis jedis = pool.getResource();

        jedis.set("name", "JedisPool");

        System.out.println(jedis.get("name"));

        jedis.close();
    }
    
    @Test
    public void testSpringJedisPool() {
        Jedis jedis = pool.getResource();

        jedis.set("name", "名称");

        System.out.println(jedis.get("name"));
    }
}
```

## <font color=orange> Redis中数据类型 </font>

### <font color=orange> 字符串string </font>
Redis中没有使用C语言的字符串表示，而是自定义一个数据结构叫SDS（simple dynamic string）即简单动态字符串。
Redis的字符串是二进制安全的，什么是二进制安全？简单理解就是存入什么数据取出的还是什么数据。Redis中的sds不像c语言处理字符串那样遇到'\0'字符则认证字符串结束，它不会对存储进去的二进制数据进行处理，存入什么数据取出还是什么数据。
#### <font color=orange> 命令 </font>
* 赋值: `SET key value`
* 取值: `GET key`, 当键不存在返回nil
* 取值时同时对key进行赋值操作: `GETSET key value`
* 删除: `Del key`
* 递增数字: `INCR key`
	* 当存储的字符串是整数时，Redis提供了一个实用的命令INCR，其作用是让当前键值递增，并返回递增后的值。 
* 增加指定的整数: `INCRBY key increment`
* 递减数值: `DECR key`
* 减少指定的整数: `DECRBY key decrement`
* 向尾部追加值: `APPEND key value`
	* APPEND的作用是向键值的末尾追加value。如果键不存在则将该键的值设置为value，即相当于 SET key value。返回值是追加后字符串的总长度。 
* 获取字符串长度: `STRLEN key`
* 同时设置多个键值: `MSET key value [key value …]`
* 同时获取多个键值: `MGET key [key …]`

### <font color=orange> 列表list</font>
列表类型（list）可以存储一个有序的字符串列表，常用的操作是向列表两端添加元素，或者获得列表的某一个片段。
列表类型内部是使用双向链表（double linked list）实现的，所以向列表两端添加元素的时间复杂度为0(1)，获取越接近两端的元素速度就越快。这意味着即使是一个有几千万个元素的列表，获取头部或尾部的10条记录也是极快的。
#### <font color=orange> 命令 </font>
* 向列表左边增加元素: `LPUSH key value [value ...]`
* 向列表右边增加元素: `RPUSH key value [value ...]`
* 查看列表: `LRANGE key start stop`
	* 索引从0开始。索引可以是负数，如：“-1”代表最后边的一个元素。 
* 从列表左边弹出一个元素: `LPOP key`
	* 第一步是将列表左边的元素从列表中移除，第二步是返回被移除的元素值。 
* 从列表右边弹出一个元素: `RPOP key`
	* 第一步是将列表右边的元素从列表中移除，第二步是返回被移除的元素值。 
* 获取列表中元素的个数: `LLEN key`
* 删除列表中指定的值: `LREM key count value`
	* 删除列表中前count个值为value的元素，返回实际删除的元素个数
	* 当count>0时， LREM会从列表左边开始删除。 
	* 当count<0时， LREM会从列表后边开始删除。 
	* 当count=0时， LREM删除所有值为value的元素
* 获得/设置指定索引的元素值 
	* `LINDEX key index`
	* `LSET key index value`
* 只保留列表指定片段，指定范围和LRANGE一致: `LTRIM key start stop`
* 向列表中插入元素: `LINSERT key BEFORE|AFTER pivot value`
	* 该命令首先会在列表中从左到右查找值为pivot的元素，然后根据第二个参数是BEFORE还是AFTER来决定将value插入到该元素的前面还是后面。
* 将元素从一个列表转移到另一个列表中: `RPOPLPUSH source destination`

### <font color=orange> 散列hash</font>
hash叫散列类型，它提供了字段和字段值的映射。字段值只能是字符串类型，不支持散列类型、集合类型等其它类型。
#### <font color=orange> 命令 </font>
* 赋值
	* 一次赋值一个字段: `HSET key field value`
	* 一次赋值多个字段: `HMSET key field value [field value ...]`
	* 4) `HSET`命令不区分插入和更新操作，当执行插入操作时HSET命令返回1，当执行更新操作时返回0.
* 取值
	* 一次取值一个字段: `HGET key field`
	* 一次取值多个字段: `HMGET key field [field ...]`
	* 取出所有的字段: `HGETALL key`
* 删除字段: `HDEL key field [field ...]`
	* 可以删除一个或多个字段，返回值是被删除的字段个数 
* 增加数字: `HINCRBY key field increment`
* 判断字段是否存在: `HEXISTS key field`
* 当字段不存在时赋值: `HSETNX key field value`
* 只获取字段名: `HKEYS key`
* 只获取字段值: `HVALS key`
* 获取字段数量: `HLEN key`
### <font color=orange> 集合set</font>
在集合中的每个元素都是不同的，且没有顺序。
#### <font color=orange> 命令 </font>
* 增加元素: `SADD key member [member ...]`
* 删除元素: `SREM key member [member ...]` 
* 获得集合中的所有元素: `SMEMBERS key`
* 判断元素是否在集合中: `SISMEMBER key member`
* 属于A并且不属于B的元素: `SDIFF key [key ...]`
* 集合的交集运算: `SINTER key [key ...]`
* 集合的并集运算: `SUNION key [key ...]`
* 获得集合中元素的个数: `SCARD key`
* 从集合中弹出一个元素: `SPOP key`
	* 由于集合是无序的，所有SPOP命令会从集合中随机选择一个元素弹出

### <font color=orange> 有序集合sorted set</font>
在集合类型的基础上有序集合类型为集合中的每个元素都关联一个分数，这使得我们不仅可以完成插入、删除和判断元素是否存在在集合中，还能够获得分数最高或最低的前N个元素、获取指定分数范围内的元素等与分数有关的操作。 
在某些方面有序集合和列表类型有些相似。 
1、二者都是有序的。 
2、二者都可以获得某一范围的元素。 
但是，二者有着很大区别： 
1、列表类型是通过链表实现的，获取靠近两端的数据速度极快，而当元素增多后，访问中间数据的速度会变慢。 
2、有序集合类型使用散列表实现，所有即使读取位于中间部分的数据也很快。 
3、列表中不能简单的调整某个元素的位置，但是有序集合可以（通过更改分数实现） 
4、有序集合要比列表类型更耗内存。
 
#### <font color=orange> 命令 </font>
* 增加元素: `ZADD key score member [score member ...]`
* 获取元素的分数: `ZSCORE key member`
* 删除元素: `ZREM key member [member ...]`
* 获得排名在某个范围的元素列表
	* 照元素分数从小到大的顺序返回索引从start到stop之间的所有元素（包含两端的元素): `ZRANGE key start stop [WITHSCORES]`
	* 照元素分数从大到小的顺序返回索引从start到stop之间的所有元素（包含两端的元素）: `ZREVRANGE key start stop [WITHSCORES]`
	* 如果需要获得元素的分数的可以在命令尾部加上WITHSCORES参数 
* 获得指定分数范围的元素: `ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset count]`
* 增加某个元素的分数，返回值是更改后的分数: `ZINCRBY  key increment member`
* 获得集合中元素的数量: `ZCARD key`
* 获得指定分数范围内的元素个数: `ZCOUNT key min max`
* 按照排名范围删除元素: `ZREMRANGEBYRANK key start stop`
* 获取元素的排名
	* 从小到大: `ZRANK key member`
	* 从大到小: `ZREVRANK key member`

### <font color=orange> keys命令 </font>
Redis在实际使用过程中更多的用作缓存，然而缓存的数据一般都是需要设置生存时间的，即：到期后数据销毁。 

* 设置key的生存时间（单位：秒）key在多少秒后会自动删除: `EXPIRE key seconds`
* 查看key生于的生存时间: `TTL key`		
* 清除生存时间: `PERSIST key`				
* 生存时间设置单位为：毫秒: `PEXPIRE key milliseconds`
* 返回满足给定pattern 的所有key: `keys xxx*`
* 确认一个key 是否存在: `exists xxx`
* 删除一个key: `del xxx`
* 重命名key: `rename xxx new_xxx`
* 返回值的类型: `type xxx`

### <font color=orange> 服务器命令 </font>
* 测试连接是否存活: `ping`
* 在命令行打印一些内容: `echo xxx`
* 选择数据库: `select index`
	* index从0~15
* 退出连接: `quit`
* 返回当前数据库中key 的数目: `dbsize`
* 获取服务器的信息和统计: `info`
* 删除当前选择数据库中的所有key: `flushdb`
* 删除所有数据库中的所有key: `flushall`

## <font color=orange> 持久化 </font>
Redis的高性能是由于其将所有数据都存储在了内存中，为了使Redis在重启之后仍能保证数据不丢失，需要将数据从内存中同步到硬盘中，这一过程就是持久化。
	Redis支持两种方式的持久化，一种是RDB方式，一种是AOF方式。可以单独使用其中一种或将二者结合使用。
	
### <font color=orange> RDB持久化 </font>### 
RDB方式的持久化是通过快照（snapshotting）完成的，当符合一定条件时Redis会自动将内存中的数据进行快照并持久化到硬盘。
RDB是Redis默认采用的持久化方式，在redis.conf配置文件中默认有此下配置：

```
save 900 1
save 300 10
save 60 10000
```
* 存储速度快
* 服务器断电会造成数据丢失(数据完整性不能保证)

### <font color=orange> AOF持久化 </font>

默认情况下Redis没有开启AOF（append only file）方式的持久化，可以通过appendonly参数开启：
```
appendonly yes
```
开启AOF持久化后每执行一条会更改Redis中的数据的命令，Redis就会将该命令写入硬盘中的AOF文件。AOF文件的保存位置和RDB文件的位置相同，都是通过dir参数设置的，默认的文件名是appendonly.aof，可以通过appendfilename参数修改：
```
appendfilename appendonly.aof
```

* 持久化良好, 能保证数据的完整性
* 大大降低了redis的

## <font color=orange> 主从复制 </font>
持久化保证了即使redis服务重启也会丢失数据，因为redis服务重启后会将硬盘上持久化的数据恢复到内存中，但是当redis服务器的硬盘损坏了可能会导致数据丢失，如果通过redis的主从复制机制就可以避免这种单点故障.
* 主redis中的数据有两个副本（replication）即从redis1和从redis2，即使一台redis服务器宕机其它两台redis服务也可以继续提供服务。
* 主redis中的数据和从redis上的数据保持实时同步，当主redis写入数据时通过主从复制机制会复制到两个从redis服务上。
* 只有一个主redis，可以有多个从redis。
* 主从复制不会阻塞master，在同步数据时，master 可以继续处理client 请求
* 一个redis可以即是主又是从, 从redis下面还可以有从redis.

### <font color=orange> 主从配置 </font>
#### <font color=green> 主redis配置 </font>
无需特殊配置。
#### <font color=green> 从redis配置 </font>
修改从redis服务器上的redis.conf文件，添加slaveof 主redisip  主redis端口.
```
#该redis对应的主redis服务器是192.168.1.111 端口是6379
slaveof 192.168.1.111 6379
```

## <font color=orange> Redis集群 </font>

### <font color=orange> 集群特点 </font>
* 1、所有的redis节点彼此互联(PING-PONG机制),内部使用二进制协议优化传输速度和带宽.
* 2、节点的fail是通过集群中超过半数的节点检测失效时才生效(所以集群数量最好是奇数).
* 3、客户端与redis节点直连,不需要中间proxy层.客户端不需要连接集群所有节点,连接集群中任何一个可用节点即可
* 4、redis-cluster把所有的物理节点映射到[0-16383]slot上,cluster 负责维护node<->slot<->value之间的存储. Redis 集群中内置了 16384 个哈希槽，当需要在 Redis 集群中放置一个 key-value 时，redis 先对 key 使用 crc16 算法算出一个结果，然后把结果对 16384 求余数，这样每个 key 都会对应一个编号在 0-16383 之间的哈希槽，redis会根据节点数量大致均等的将哈希槽映射到不同的节点.

### <font color=orange> Redis集群的搭建</font>
Redis集群中至少应该有三个节点。要保证集群的高可用，需要每个节点有一个备份机。Redis集群至少需要6台服务器。

#### <font color=orange> 搭建redis实例 </font>
注意需要开启Redis配置文件`redis.config`中的集群设置
```
 cluster-enabled yes
```
还需要开启对应Redis服务器的对应端口.
```
#开启redis默认6379端口
firewall-cmd --add-port=6379/tcp --zone=public --permanent
#刷新防火墙并生效
firewall-cmd --reload
```

然后启动每个实例
```
cd redis安装目录
./redis-server redis.conf
```

#### <font color=orange> 集群搭建环境 </font>
* 使用ruby脚本搭建集群。需要ruby的运行环境。
```
yum install ruby
yum install rubygems
```
* 安装ruby脚本运行使用的包
```
wget https://rubygems.global.ssl.fastly.net/gems/redis-3.0.0.gem
gem install redis-3.0.0.gem 
```
* 在redis的`源码/src/redis-trib.rb`这个就是搭建redis集群的脚本, 使用ruby脚本搭建集群
```
./redis-trib.rb create --replicas 1 xxx.xxx.xxx.xxx:6379 xxx.xxx.xxx.xxx:6379 xxx.xxx.xxx.xxx:6379 xxx.xxx.xxx.xxx:6379 xxx.xxx.xxx.xxx:6379 xxx.xxx.xxx.xxx:6379
```
#### <font color=orange> 连接集群</font>
```
#-c：代表连接的是redis集群
Redis-cli [-h host] [-p port] -c
```

### <font color=orange> 使用Jedis测试 </font>
* spring配置文件
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
       xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
	http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd
	http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.2.xsd
	http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.2.xsd">

    <!-- Redis连接池配置 -->
    <bean id="jedisPoolConfig" class="redis.clients.jedis.JedisPoolConfig">
        <!-- 最大连接数 -->
        <property name="maxTotal" value="30" />
        <!-- 最大空闲连接数 -->
        <property name="maxIdle" value="10" />
        <!-- 每次释放连接的最大数目 -->
        <property name="numTestsPerEvictionRun" value="1024" />
        <!-- 释放连接的扫描间隔（毫秒） -->
        <property name="timeBetweenEvictionRunsMillis" value="30000" />
        <!-- 连接最小空闲时间 -->
        <property name="minEvictableIdleTimeMillis" value="1800000" />
        <!-- 连接空闲多久后释放, 当空闲时间>该值 且 空闲连接>最大空闲连接数 时直接释放 -->
        <property name="softMinEvictableIdleTimeMillis" value="10000" />
        <!-- 获取连接时的最大等待毫秒数,小于零:阻塞不确定的时间,默认-1 -->
        <property name="maxWaitMillis" value="1500" />
        <!-- 在获取连接的时候检查有效性, 默认false -->
        <property name="testOnBorrow" value="true" />
        <!-- 在空闲时检查有效性, 默认false -->
        <property name="testWhileIdle" value="true" />
        <!-- 连接耗尽时是否阻塞, false报异常,ture阻塞直到超时, 默认true -->
        <property name="blockWhenExhausted" value="false" />
    </bean>

    <!--Redis集群版-->
    <bean id="jedisCluster" class="redis.clients.jedis.JedisCluster">
        <constructor-arg>
            <set>
                <bean class="redis.clients.jedis.HostAndPort">
                    <constructor-arg name="host" value="192.168.1.181"/>
                    <constructor-arg name="port" value="6379"/>
                </bean>
                <bean class="redis.clients.jedis.HostAndPort">
                    <constructor-arg name="host" value="192.168.1.182"/>
                    <constructor-arg name="port" value="6379"/>
                </bean>
                <bean class="redis.clients.jedis.HostAndPort">
                    <constructor-arg name="host" value="192.168.1.183"/>
                    <constructor-arg name="port" value="6379"/>
                </bean>
                <bean class="redis.clients.jedis.HostAndPort">
                    <constructor-arg name="host" value="192.168.1.184"/>
                    <constructor-arg name="port" value="6379"/>
                </bean>
                <bean class="redis.clients.jedis.HostAndPort">
                    <constructor-arg name="host" value="192.168.1.185"/>
                    <constructor-arg name="port" value="6379"/>
                </bean>
                <bean class="redis.clients.jedis.HostAndPort">
                    <constructor-arg name="host" value="192.168.1.186"/>
                    <constructor-arg name="port" value="6379"/>
                </bean>
            </set>
        </constructor-arg>
        <constructor-arg name="poolConfig" ref="jedisPoolConfig"/>
    </bean>
</beans>
```
* 测试类
```java
package com.coppco.test;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import redis.clients.jedis.HostAndPort;
import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisCluster;
import redis.clients.jedis.JedisPool;

import javax.annotation.Resource;
import java.util.HashSet;
import java.util.Set;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext-dao.xml")
public class TestJedis {
	
    @Resource
    private JedisCluster jedisCluster;

    /**
     * 测试直接连接
     */ 
    @Test
    public void testJedisCluster() {
        //创建Cluster对象, 构造参数是Set类型, 每个元素是HostAndPort类型
        Set<HostAndPort> hostAndPorts = new HashSet<>();
        hostAndPorts.add(new HostAndPort("192.168.1.181", 6379));
        hostAndPorts.add(new HostAndPort("192.168.1.182", 6379));
        hostAndPorts.add(new HostAndPort("192.168.1.183", 6379));
        hostAndPorts.add(new HostAndPort("192.168.1.184", 6379));
        hostAndPorts.add(new HostAndPort("192.168.1.185", 6379));
        hostAndPorts.add(new HostAndPort("192.168.1.186", 6379));
        JedisCluster cluster = new JedisCluster(hostAndPorts);
        //直接使用JedisCluster操作redis, 自带连接池, JedisCluster对象可以是单例

        cluster.set("name", "集群");

        System.out.println(cluster.get("name"));
        cluster.close();
    }
    
    /**
     * 测试通过spring连接
     */ 
    @Test
    public void testJedisClusterSpring() {
        jedisCluster.set("name", "jedisCluster");
        System.out.println(jedisCluster.get("name"));
    }
}
```

## <font color=orange> 策略模式 </font>
实际项目中, 无可避免的使用到单机版和集群的Redis, 这就需要我们来回切换单机版和集群版的Redis, 如果每次更换代码, 风险比较大.我们可以使用策略模式.
### <font color=orange> 策略模式定义 </font>
策略模式是指对一系列的算法定义，并将每一个算法封装起来，而且使它们还可以相互替换。策略模式让算法独立于使用它的客户而独立变化。
* 它的组成
	* 一个抽象类
	* 具体的实现类
	* 一个抽象类的引用, 给客户端使用

### <font color=orange> JedisClient的使用 </font>
#### <font color=orange> 抽象类 </font>
```java
package com.coppco.jedis;

public interface JedisClient {
	String set(String key, String value);
	String get(String key);
	Boolean exists(String key);
	Long expire(String key, int seconds);
	Long ttl(String key);
	Long incr(String key);
	Long hset(String key, String field, String value);
	String hget(String key, String field);
	Long hdel(String key, String... field);
}
```
#### <font color=orange> 单机版 </font>
* spring配置文件`applicationContent.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
       xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
	http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd
	http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.2.xsd
	http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.2.xsd">

    <!--注解扫描器-->
    <context:component-scan/>
    <!-- Redis连接池配置 -->
    <bean id="jedisPoolConfig" class="redis.clients.jedis.JedisPoolConfig">
        <!-- 最大连接数 -->
        <property name="maxTotal" value="30" />
        <!-- 最大空闲连接数 -->
        <property name="maxIdle" value="10" />
        <!-- 每次释放连接的最大数目 -->
        <property name="numTestsPerEvictionRun" value="1024" />
        <!-- 释放连接的扫描间隔（毫秒） -->
        <property name="timeBetweenEvictionRunsMillis" value="30000" />
        <!-- 连接最小空闲时间 -->
        <property name="minEvictableIdleTimeMillis" value="1800000" />
        <!-- 连接空闲多久后释放, 当空闲时间>该值 且 空闲连接>最大空闲连接数 时直接释放 -->
        <property name="softMinEvictableIdleTimeMillis" value="10000" />
        <!-- 获取连接时的最大等待毫秒数,小于零:阻塞不确定的时间,默认-1 -->
        <property name="maxWaitMillis" value="1500" />
        <!-- 在获取连接的时候检查有效性, 默认false -->
        <property name="testOnBorrow" value="true" />
        <!-- 在空闲时检查有效性, 默认false -->
        <property name="testWhileIdle" value="true" />
        <!-- 连接耗尽时是否阻塞, false报异常,ture阻塞直到超时, 默认true -->
        <property name="blockWhenExhausted" value="false" />
    </bean>

    <!-- Redis单机(非集群) 通过连接池 -->
    <bean id="jedisPool" class="redis.clients.jedis.JedisPool" destroy-method="close">
        <constructor-arg name="poolConfig" ref="jedisPoolConfig"/>
        <constructor-arg name="host" value="192.168.1.184"/>
        <constructor-arg name="port" value="6379"/>
    </bean>
</beans>
    <bean id="jedisClientPool" class="com.coppco.jedis.JedisClientPool"/>
```
* 实现类
```java
package com.coppco.jedis;

import org.springframework.beans.factory.annotation.Autowired;

import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisPool;

public class JedisClientPool implements JedisClient {
	
	@Autowired
	private JedisPool jedisPool;

	@Override
	public String set(String key, String value) {
		Jedis jedis = jedisPool.getResource();
		String result = jedis.set(key, value);
		jedis.close();
		return result;
	}

	@Override
	public String get(String key) {
		Jedis jedis = jedisPool.getResource();
		String result = jedis.get(key);
		jedis.close();
		return result;
	}

	@Override
	public Boolean exists(String key) {
		Jedis jedis = jedisPool.getResource();
		Boolean result = jedis.exists(key);
		jedis.close();
		return result;
	}

	@Override
	public Long expire(String key, int seconds) {
		Jedis jedis = jedisPool.getResource();
		Long result = jedis.expire(key, seconds);
		jedis.close();
		return result;
	}

	@Override
	public Long ttl(String key) {
		Jedis jedis = jedisPool.getResource();
		Long result = jedis.ttl(key);
		jedis.close();
		return result;
	}

	@Override
	public Long incr(String key) {
		Jedis jedis = jedisPool.getResource();
		Long result = jedis.incr(key);
		jedis.close();
		return result;
	}

	@Override
	public Long hset(String key, String field, String value) {
		Jedis jedis = jedisPool.getResource();
		Long result = jedis.hset(key, field, value);
		jedis.close();
		return result;
	}

	@Override
	public String hget(String key, String field) {
		Jedis jedis = jedisPool.getResource();
		String result = jedis.hget(key, field);
		jedis.close();
		return result;
	}

	@Override
	public Long hdel(String key, String... field) {
		Jedis jedis = jedisPool.getResource();
		Long result = jedis.hdel(key, field);
		jedis.close();
		return result;
	}

}
```
#### <font color=orange> 集群版 </font>
* spring配置文件`applicationContent.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
       xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
	http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd
	http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.2.xsd
	http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.2.xsd">

    <!--注解扫描器-->
    <context:component-scan/>
    <!-- Redis连接池配置 -->
    <bean id="jedisPoolConfig" class="redis.clients.jedis.JedisPoolConfig">
        <!-- 最大连接数 -->
        <property name="maxTotal" value="30" />
        <!-- 最大空闲连接数 -->
        <property name="maxIdle" value="10" />
        <!-- 每次释放连接的最大数目 -->
        <property name="numTestsPerEvictionRun" value="1024" />
        <!-- 释放连接的扫描间隔（毫秒） -->
        <property name="timeBetweenEvictionRunsMillis" value="30000" />
        <!-- 连接最小空闲时间 -->
        <property name="minEvictableIdleTimeMillis" value="1800000" />
        <!-- 连接空闲多久后释放, 当空闲时间>该值 且 空闲连接>最大空闲连接数 时直接释放 -->
        <property name="softMinEvictableIdleTimeMillis" value="10000" />
        <!-- 获取连接时的最大等待毫秒数,小于零:阻塞不确定的时间,默认-1 -->
        <property name="maxWaitMillis" value="1500" />
        <!-- 在获取连接的时候检查有效性, 默认false -->
        <property name="testOnBorrow" value="true" />
        <!-- 在空闲时检查有效性, 默认false -->
        <property name="testWhileIdle" value="true" />
        <!-- 连接耗尽时是否阻塞, false报异常,ture阻塞直到超时, 默认true -->
        <property name="blockWhenExhausted" value="false" />
    </bean>

    <!--Redis集群版-->
    <bean id="jedisCluster" class="redis.clients.jedis.JedisCluster">
        <constructor-arg name="nodes">
            <set>
                <bean class="redis.clients.jedis.HostAndPort">
                    <constructor-arg name="host" value="192.168.1.181"/>
                    <constructor-arg name="port" value="6397"/>
                </bean>
                <bean class="redis.clients.jedis.HostAndPort">
                    <constructor-arg name="host" value="192.168.1.182"/>
                    <constructor-arg name="port" value="6397"/>
                </bean>
                <bean class="redis.clients.jedis.HostAndPort">
                    <constructor-arg name="host" value="192.168.1.183"/>
                    <constructor-arg name="port" value="6397"/>
                </bean>
                <bean class="redis.clients.jedis.HostAndPort">
                    <constructor-arg name="host" value="192.168.1.184"/>
                    <constructor-arg name="port" value="6397"/>
                </bean>
                <bean class="redis.clients.jedis.HostAndPort">
                    <constructor-arg name="host" value="192.168.1.185"/>
                    <constructor-arg name="port" value="6397"/>
                </bean>
                <bean class="redis.clients.jedis.HostAndPort">
                    <constructor-arg name="host" value="192.168.1.186"/>
                    <constructor-arg name="port" value="6397"/>
                </bean>
            </set>
        </constructor-arg>
        <constructor-arg name="poolConfig" ref="jedisPoolConfig"/>
    </bean>
    <bean id="jedisClientCluster" class="com.coppco.jedis.JedisClientCluster"/>
</beans>

```
* 实现类
```java
package com.coppco.jedis;

import org.springframework.beans.factory.annotation.Autowired;

import redis.clients.jedis.JedisCluster;

public class JedisClientCluster implements JedisClient {
	
	@Autowired
	private JedisCluster jedisCluster;

	@Override
	public String set(String key, String value) {
		return jedisCluster.set(key, value);
	}

	@Override
	public String get(String key) {
		return jedisCluster.get(key);
	}

	@Override
	public Boolean exists(String key) {
		return jedisCluster.exists(key);
	}

	@Override
	public Long expire(String key, int seconds) {
		return jedisCluster.expire(key, seconds);
	}

	@Override
	public Long ttl(String key) {
		return jedisCluster.ttl(key);
	}

	@Override
	public Long incr(String key) {
		return jedisCluster.incr(key);
	}

	@Override
	public Long hset(String key, String field, String value) {
		return jedisCluster.hset(key, field, value);
	}

	@Override
	public String hget(String key, String field) {
		return jedisCluster.hget(key, field);
	}

	@Override
	public Long hdel(String key, String... field) {
		return jedisCluster.hdel(key, field);
	}
}
```
# Redis

## 0. 概述

### 0.1 NoSQL

- Not Only Sql

> NoSQL特点

解耦

1、方便扩展（数据之间没有关系，很好扩展）

2、大数据量高性能 （一秒写8万次，读取11万次，缓存记录级，是一种细粒度的缓存）

3、数据类型是多样型的

4、键值对

### 0.2 场景分析

> 架构师需要面对的问题

```bash
# 1.商品的基本信息
	名称、价格、商家信息 mysql
  
# 2.商品的描述、评论
	文档型数据库 mysql
  
# 3.图片
	分布式文件系统 FastDFS
	- TFS
	- GFS
	- HDFS
	- OSS

# 4.商品关键字（搜索）
	- 搜索引擎 solr elasticsearch
	- ISearch: 多隆
	
# 5.商品热门的波段信息
	- 内存数据库
	- Redis、Tair、Memache
	
# 6.商品的交易
	第三方接口整合
# 使用统一的数据服务层 UDSL

```

### 0.3 Redis概述

Redis （Remote Dictionary Server）远程字典服务

> Redis能干嘛

1、内存存储，持久化，（rdb，aof）

2、效率高，可用于高速缓存

3、发布订阅系统

4、地理信息分析

5、计时器、计数器



#### redis基本知识

- 默认有16个数据库，默认使用 db 1；select切换
- redis是单线程的，CPU不是瓶颈，内存是瓶颈



```shell
select 1 # 切换数据库
DBSIZE # 数据库大小
keys * # 获取db所有的key
flushall # 清空所有数据
flushdb # 清空当前db

set name 1
get name
EXISTS name # 是否存在
move name # 移除
EXPIRE name 10 # 单点登陆可以设置过期时间
ttl name # 查看过期时间
```

> redis是单线程，还这么快？

1、多线程（cpu上下文切换，争夺资源）不一定比单线程效率高

2、高性能的服务器不一定是多线程的

核心：redis将所有数据全部放在内存中，所以效率高。对于内存系统来说，没有上下文切换，效率是最高的

#### redis配置文件

> 单位 redis.conf 对大小写不敏感

![image-20210709142601019](redis.assets\image-20210709142601019.png)

> 包含: 可以包含多个配置文件的引入

![image-20210709142638136](redis.assets\image-20210709142638136.png)

> 网络

```bash
bind 127.0.0.1 ::1 # 绑定ip
protected-mode yes # 保护模式
port 6379	# 端口
```

> 通用配置

```bash
daemonize yes # 以守护进程的方式进行，默认为no 需要开启 yes
pidfile /var/run/redis/redis-server.pid # 守护进程的 pid 文件
loglevel notice # 日志级别 
# This can be one of:
# debug (a lot of information, useful for development/testing)
# verbose (many rarely useful info, but not a mess like the debug level)
# notice (moderately verbose, what you want in production probably)
# warning (only very important / critical messages are logged)

logfile /var/log/redis/redis-server.log # 日志文件夹
databases 16	# 数据库数量
```

> 快照

持久化，在规定的时间内执行了多少次，则会持久化到文件， .rdb .aof

redis是内存数据库，如果没有持久化，那么数据断电及失

```bash
#   save ""

save 900 1		# 如果900内，至少有一个1 key 进行了修改，我们进行持久化操作
save 300 10 	# 300s内 至少10 key 进行了修改，我们进行持久化
save 60 10000 

stop-writes-on-bgsave-error yes # 持久化错误之后是否进行继续操作，默认开启
rdbcompression yes # 是否需要 压缩rdb文件，需要消耗一些资源
rdbchecksum yes # 保存rdb文件的时候，进行错误的检查校验
dir ./  # rdb文件保存的目录

```

> security

```bash
# 设置redis密码
config set requirepass "123456"
auto 123456 # 使用密码进行登陆
```

> 限制clients

```bash
maxclients 10000 # 设置最大客户端的数量
maxmemory <bytes> # 最大内存
maxmemory-policy noeviction # 内存到达上线之后的处理策略
		# 移除一些过期的key
```

> APPEND ONLY MODE  aof配置

```bash
appendly no # 默认不开启，默认使用rdb方式持久化，
appendfilename "appendonly.aof" # 配置文件名称

# appendfsync always # 每次修改都会同步
appendfsync everysec # 每秒同步
# appendfsync no	 # 不同步

```



#### RDB(Redis DataBase)

在指定的时间间隔内将内存中的数据集快照写入磁盘， snapshot快照，它恢复时是将快照文件直接读到内存里

Redis会单独创建 fork 一个子进程来进行持久化，会先将数据写入到一个临时文件中，待持久化过程结束，再用这个临时文件替换上次持久化好的文件。整个过程，主进程是不进行IO操作的。缺点是最后一次持久化的数据可能丢失

> 触发机制

- save的规则满足的情况下，会自动触发rdb规则
- 执行flushall命令，也会触发我们的rdb规则
- 退出redis，也会产生rdb文件！

> 如果恢复rdb文件

- 只需要将rdb文件放在我们redis启动目录就可以，redis启动的时候回自动检查dump.rdb
- `config get dir` 查看路径 dump.rdb



#### AOF(Append Only File)

将我们所有命令都记录下来，恢复的时候就把这个文件全部在执行一遍

优点：每次修改都同步，文件完整性会更加好

没秒同步一次，可能会丢失一秒的数据

从不同步，效率最高

缺点：aof数据文件远大于rdb，修复的速度也比rdb慢



### 0.4 Redis集群

#### 主从复制

master ， slave 

主机以写为主，从机以读为主，读写分离，减缓服务器压力

作用：

- 数据冗余
- 故障恢复
- 负载均衡
- 高可用

```bash
info replication # 查看当前库的信息 
# 通过命令配置
slaveof 127.0.0.1 6379 # 在从机中配置主机
slaveof no one # 主机断开，从机手动设置为主节点

# 配置文件配置，永久的

```

- 增量复制
- 全量复制

#### 哨兵模式

配置哨兵配置文件 sentinel.conf

```bash
sentinel monitor myredis 127.0.0.1 6379 1
```

优点：

哨兵集群，基于主从复制

主从可以切换，故障可以转移，系统可用性好

缺点：

redis不好在线扩容，集群容量一旦达到上限，扩容十分麻烦

实现哨兵的配置十分麻烦，里面有很多选择





## 1. 安装配置

https://blog.csdn.net/erlian1992/article/details/54382443

- windows安装

  [下载](https://github.com/tporadowski/redis/releases),解压

- linux安装

  ```shell
  wget http://download.redis.io/releases/redis-6.0.8.tar.gz
  tar xzf redis-6.0.8.tar.gz
  cd redis-6.0.8
  make
  
  cd src
  ./redis-server
  # ./redis-server ../redis.conf #指定配置文件启动
  ```

- Ubuntu apt 安装

  ```shell
  sudo apt update
  sudo apt install redis-server
  # 启动
  redis-server
  
  sudo vi /etc/redis/redis.conf # 配置文件
  ```

### 1.1 启动问题

- window启动

  ```shell
  # 配置环境变量存在问题一直不能直接cmd打开启动，需要在redis目录下cmd 然后输入 
  redis-server redis.windows.conf
  # 打开客户端 
  redis-cli
  ```

  

```shell
# 连接远程redis
redis-cli -h host -p port -a password
# 手动修改配置 持久化
CONFIG SET dir "D:\\Program Files\\redis"
CONFIG SET save "900 1 300 10 60 10000 10 1" # 修改save的频率
CONFIG SET appendonly yes 
```

- 持久化配置问题

### 1.2 redis相关问题

- redis默认支持16个数据库，对外都是从0开始的递增数字命名
- 客户端与redis建立连接后悔自动选择0号数据库，可用select 1来切换



### 1.3 redis远程连接配置

- 为了开发方便，开放redis的连接权限，不推荐

  ```shell
  vi redis.conf
  # bind 127.0.0.1
  protected-mode no
  ```

- 使用nginx代理

  ```shell
  # 在nginx.conf 代理中设置
  stream {    # stream 模块配置和 http 模块在相同级别
      upstream redis {
          server 127.0.0.1:6379 max_fails=3 fail_timeout=30s;
      }
      server {
          listen 16379;
          proxy_connect_timeout 1s;
          proxy_timeout 3s;
          proxy_pass redis;
      }
  }
  
  $ sudo service nginx reload
  ```


### 1.4 性能测试

redis-benchmark

![image-20210703214019402](redis.assets\image-20210703214019402.png)

```shell
redis-benchmark -h localhost -p 6379 -c 100 -n 10000
```



## 2. 数据类型

###  2.1 String

- key-value

- string类型是二进制安全的，可以包含任何数据，jpg图像或者序列化对象

- 最大存储512MB

- 实例

  ```shell
  SET key "王晨"
  GET key
  APPEND key "hello" # 追加字符串
  STRLEN key # 字符串长度
  
  incr views # 加1 可用于浏览量
  decr views # 减1
  INCRBY views 10 # +10
  DECRBY views 10 # -10
  
  GETRANGE key 0 3 # 截取字符串 [0,3]
  GETRANGE key 0 -1 # 截取所有字符串
  SETRANGE key 1 xx # 从第1个索引开始替换字符串
  
  # setex(set with expire) # 设置过期时间
  # setnx(set if not exist) # 不存在再设置 如果存在则失败
  
  mset k1 v1 k2 v2 k3 v3 # 同时设置多个值
  mget k1 k2 k3
  msetnx k1 v1 # 
  
  # 对象
  set user:1 {name:wc,age:27}
  # user:{id}:{filed}
  mset user:1:name zhangsan user:1:age 2
  mget user:1:name user:1:age
  
  getset # 先获取再设置 
  ```

- 使用场景

  - 计数器
  - 统计多单位的数量
  - 粉丝数
  - 对象缓存数据

### 2.2 Hash

- redis hash 是一个键值对集合

- string类型的field和value的映射表，本质和string类型没有区别

- 场景：存储、读取、修改用户属性；编程中的对象

- 实例

  ```shell
  HMSET runoob field1 "Hello" field2 "World"
  HGET runoob field1
  HGET runoob field2
  HGETALL runoob
  HDEL runoob field1
  
  hlen myhash 
  hkeys myhash # 只获取所有的field
  hkeys myhash # 只后驱所有的value
  hincrby myhash field3 1
  hincrby myhash field -1 
  
  hsetnx myhash field4 hello # 如果不存在则可以设置，如果存在则不可设置
  ```
  
- 应用

  - hash变更的数据 user name age 

### 2.3 List

- 字符串列表，按照插入顺序排列，

- 可以添加一个元素到列表的头部或者尾部

- 存储2^32-1 个元素

- 场景：最新消息排列时间线、消息队列

- 实例

  ```shell
  
  lpush runoob redis # lpush在头部加
  rpush runoob redis # rpush在尾部加
  lrange runoob 0 10
  blpop key1 timeout # 移除并获取列表的第一个元素,若没有元素就阻塞
  brpop key1 timeout # 移除并获取列表的最后一个元素
  lindex key index # 根据索引获取
  lpop key # 移除获取第一个元素
  lpush key value # 插入到列表头部
  rpop key # 移除列表最后一个元素返回移除元素值
  llen # 返回列表长度
  lrem list count value
  ltrim mylist 1 2 # 通过下标截取长度
  rpoplpush mylist myotherlist # 移除列表最后一个元素，将他移动到新的列表中
  
  linsert # 将某个具体的value插入到列表中
  linsert mylist before "world" "other"
  linsert mylist after world new 
  
  ```

  > 小结

  - 实际上是一个链表
  - 在两边插入或者改动值，效率最高，获取中间元素，效率低一些，链表结构的特点：插入快，查询慢
  - 可以实现消息队列（rpush，lpop），栈（lpush，lpop）

### 2.4 Set

- Set是string类型的无序集合，唯一的

- 操作的复杂度都是O(1)

- 添加一个string元素到key对应的set集合中，成功返回1，元素已存在返回0

- 实例

  ```shell
  sadd runoob redis
  sadd runoob mongodb
  
  smembers key # 返回集合中的所有成员
  smember key member # 判断member元素是否是集合key的成员
  sismember key hello
  
  scard key # 获取集合成员数
  
  sinter key1 [key2] # 返回给定所有集合的交集
  sunion key1 [key2] # 返回集合的并
  
  smove source destination member # 集合间移动元素
  spop key # 移除并返回集合中的一个随机元素
  
  srandmember myset # 随机抽选一个元素
  srandmember myset 2 # 随机抽选出指定类型的元素
  spop myset # 随机弹出一个值
  
  
  ```
  
- 应用场景：

  - 共同关注，共同爱好，推荐好友

### 2.5 zset 

- 有序集合，每个元素都会关联一个double类型的分数

- score可重复

- 场景：排行榜、带权重的消息队列

- 实例

  ```shell
  # zadd key score member
  zadd runoob 0 redis
  zadd runoob 0 rabbitmq
  ZRANGEBYSCORE runoob 0 1000
  
  zrange salary 0 -1
  zrem salary xiaohong   # 移除
  zcard salary # 获取有序集合中的个数
  
  ```

- 应用：

  - 排行榜应用实现

  - 存储班级成绩表，工资表排序 

    

### 2.6 geospatial 地理位置



```shell
geoadd china:city 116.40 39.90  beijing # 添加 经纬

geopos china:city beijin # 查看

geodist china:city beijin shanghai km # 计算距离 

georadius china:city 110 30 1000 km # 以给定的经纬度为中心，通过半径来查询

geohash china:city beijing shanghai # 返回当前经纬度的hash字符串 将二维降维到一维

```

> GEO 底层的实现原理其实就是Zset 我们可以使用Zset命令来操作GEO



### 2.7 Hyperloglog

> 什么是基数？ 找不重复的元素的个数

- Hyperloglog 基数统计的算法

```shell
pfadd mykey a b c d e f
pfadd mykey2 i j z x c b n 
pfcount mykey2
pfmeger mykey3 mykey mykey2 # 合并
pfcount mykey3

```

- 应用：
  - 网页的UV



### 2.8 Bitmaps位图

> 位存储

统计用户信息，活跃，不活跃！ 登陆、未登陆！ 打卡，未打卡！ 两个状态的都可以使用bitmaps

都是操作二进制位来进行记录，就只有0和1两个状态

```shell
setbit sign 0 0
bitcount sign
```





## 3. redis功能

### 3.1 通用命令

```shell
redis-cli
redis-cli -h host -p port -a password

del key # 删除key
dump key # 序列化给定key，返回被序列化的值
exists key # 判断key是否存在
expire key seconds # 设置过期时间 秒
expireat key timestamp # unix时间戳
persist key # 移除过期时间
renamenx key newkey # 修改key的名称
type key # 返回储存类型
# 127.0.0.1 ::1
```

### 3.2 发布订阅

```shell
# 第一个redis-cli客户端
subscribe runoobChat # 订阅
# 第二个客户端 redis-cli
publish runoobChat "Redis PUBLISH test" # 发布

punsubscribe [pattern[pattern ...]] # 退订
unsubscribe channel # 退订给定的频道

```

- 应用场景：
  - 构建即使通信应用，实时广播，实时提醒

> 原理

通过 subscribe 命令订阅某频道后，redis-server 里维护了一个字典，字典的键就是一个个频道，字典的值则是一个链表。链表中保存了所有订阅这个channel的客户端，subscribe命令的关键 就是将客户添加到给定 channel 的订阅链表中

publish命令向订阅者发送消息，redis-server会使用给定的频道作为键，在它维护的channel字典中查找记录了订阅这个频道的所有客户端的链表，遍历这个链表，将消息发布给所有订阅者





### 3.3 事务

- 事务的执行不是原子性

```shell
multi # 事务开始
set a aaa
set b bbb
set c ccc
exec # 事务结束
# 若b处失败，则a不会回滚 c会继续

discard # 取消事务

watch money # 加锁
unwatch # 解锁

```

> 监控！Watch 是乐观锁

**悲观锁**

什么时候都会出问题，无论做什么都会加锁

**乐观锁**

什么时候都不会出问题，所以不会上锁! 更新数据的时候去判断一下，在此期间是否有人修改过这个数据

获取version

更新的时候比较version

> Redis监测测试



### 3.4 服务器命令

```shell
ping # 查看是否连接成功
select 1 # 切换数据库
bgsave # 异步保存当前数据库的数据到 磁盘
save # 同步保存数据
lastsave # 返回最近一次保存时间
client list # 获取client数量
client set connection-name # 设置当前连接名称
client getname # 获取连接名称
config get parameter # 获取配置参数
dbsize # 获取当前数据库key的数量
flushdb # 删除当前数据的所有key
flushall # 删除所有key

```



## 4. redis SDK操作

### 4.1 python基本操作

- [参考网址](https://www.runoob.com/w3cnote/python-redis-intro.html)

```python
import redis

pool = redis.ConnectionPool(host='localhost', port=6379,db=0,decode_responses=True)
r=redis.Redis(connection_pool=pool)
r.set('name', 'runoob')  # 设置 name 对应的值
print(r.get('name'))  # 取出键 name 对应的值

# string 操作 
set(name, value, ex=None, px=None, nx=False, xx=False) # ex-过期时间(秒) px-过期(毫秒) nx- 为true只有不存在时set xx-为true只有存在的时候才set
mset(*args, **kwargs) # 批量设置值
r.mset({"k1":"v1", "k2":"v2"})
mget(keys, *args) # 批量获取
getset(name, value) # 设置新值并获取原来的值
getrange(key, start, end) # 截取
setrange(key,offset,value) # 指定字符替换
append(key, value) # 

# hash 操作
hset(name, key, value) # 单个增加或修改
hkeys(name) # 获取hash所有key
hget(name,[keys]) # 获取hash 对应key的值
hsetnx(name,key,value) # key不存在时创建

hmset(name, mapping) # 批量增加 mapping-字典 hmset方法已过期
r.hset("hash2",mapping={"k6": "v6", "k7": "v7"})
hgetall(name) # 获取所有键值 返回一个字典mapping
hlen(name) # 获键值对个数
hvals(name) # 获取所有值
hexists(name,key) # key是否存在
hdel(name,*keys) # 删除指定的键值对
hincrby(name, key, amount=1) # 自增
hincrbyfloat(name, key, amount=1.0) # 浮点数 自增
hscan(name, cursor=0, match=None, count=None) # 分片读取
hscan_iter(name, match=None, count=None)# 分批读取

# list
lpush(name,values) # 添加到列表最左边
lpushx(name,value) # 只有name存在时才添加
lrange(name,start,end) # 取出索引号 s - e (-1为最后一个元素)
rpush(name,value) # 添加到右边
rpushx(name,value)
lset(name,index,value) # 指定索引号修改
lrem(name,value,num) # 要删除的值，num 从前到后删除几个 0为全部
lpop(name) # 获取左边第一个元素 返回并移除
rpop(name)# 右边
ltrim(name, start, end) # 删除索引之外的值
lindex(name, index) # 根据索引号取值
rpoplpush(src, dst) # 移动
blpop(keys, timeout)
brpop(keys, timeout)

# set
sadd(name,values)
scard(name) # 获取个数
smembers(name) # 获取所有成员

#zset
zadd
zcard
r.zrange( name, start, end, desc=False, withscores=False, score_cast_func=float)
zrem(name,values)

# 迭代器
for i in r.hscan_iter("hash1"):
    print(i)
sscan_iter
zscan_iter
```

### 4.2 Jedis

> 什么是jedis 是 redis 官方推荐的 java 连接开发工具！使用java 操作redis 的中间件
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

```

> redis是单线程，还这么快？

1、多线程（cpu上下文切换，争夺资源）不一定比单线程效率高

2、高性能的服务器不一定是多线程的

核心：redis将所有数据全部放在内存中，所以效率高。对于内存系统来说，没有上下文切换，效率是最高的



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

###  2.1 字符串

- key-value

- string类型是二进制安全的，可以包含任何数据，jpg图像或者序列化对象

- 最大存储512MB

- 实例

  ```shell
  SET wc "王晨"
  GET wc
  ```

### 2.2 Hash

- redis hash 是一个键值对集合

- string类型的field和value的映射表

- 场景：存储、读取、修改用户属性；编程中的对象

- 实例

  ```shell
  HMSET runoob field1 "Hello" field2 "World"
  HGET runoob field1
  HGET runoob field2
  HGETALL runoob
  ```

### 2.3 List

- 字符串列表，按照插入顺序排列，

- 可以添加一个元素到列表的头部或者尾部

- 存储2^32-1 个元素

- 场景：最新消息排列时间线、消息队列

- 实例

  ```shell
  lpush runoob redis # lpush在头部加
  lrange runoob 0 10
  blpop key1 timeout # 移除并获取列表的第一个元素,若没有元素就阻塞
  brpop key1 timeout # 移除并获取列表的最后一个元素
  lindex key index # 根据索引获取
  lpop key # 移除获取第一个元素
  lpush key value # 插入到列表头部
  rpop key # 移除列表最后一个元素返回移除元素值
  ```

### 2.4 Set

- Set是string类型的无序集合

- 操作的复杂度都是O(1)

- 添加一个string元素到key对应的set集合中，成功返回1，元素已存在返回0

- 场景：利用唯一性统计

- 实例

  ```shell
  sadd runoob redis
  sadd runoob mongodb
  smembers key # 返回集合中的所有成员
  smember key member # 判断member元素是否是集合key的成员
  scard key # 获取集合成员数
  sinter key1 [key2] # 返回给定所有集合的交集
  sunion key1 [key2] # 返回集合的并
  smove source destination member # 集合间移动元素
  spop key # 移除并返回集合中的一个随机元素
  
  ```

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
  ```



## 3. redis命令

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
```

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



## 4. redis python操作

### 4.1 基本操作

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


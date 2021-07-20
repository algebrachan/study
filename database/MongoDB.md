## MongoDB

### 1.简介

- MongoDB是由c++编写的，是一个基于分布式文件存储的开源数据库系统 
- 旨在为WEB应用提供可扩展的高性能数据存储解决方案。
- NoSQL数据库 
- [教程](https://www.runoob.com/mongodb/mongodb-tutorial.html)
- compass客户端

#### 业务应用场景

- 数据库高并发读写的需求
- 对海量数据的高效存储的访问需求
- 对数据库的搞可扩展性的高可用性：不需要事先定义结构
- 具体场景：社交场景、游戏场景、物流场景、物联网场景、视频
- 共同特点：数据量大，写入操作频繁，价值较低的数据对事务性不高
- 

#### 业务场景

- 对数据库高并发读写的需求
- 对海量数据的高效存储和访问的需求
- 对数据库的高可扩展性和高可用性的需求
- 社交场景、游戏场景、物流场景
- 特点：数据量大、写入操作频繁、价值较低的数据

| SQL术语/概念 | MongoDB术语/概念 | 解释/说明                           |
| :----------- | :--------------- | :---------------------------------- |
| database     | database         | 数据库                              |
| table        | collection       | 数据库表/集合                       |
| row          | document         | 数据记录行/文档                     |
| column       | field            | 数据字段/域                         |
| index        | index            | 索引                                |
| table joins  |                  | 表连接,MongoDB不支持                |
| primary key  | primary key      | 主键,MongoDB自动将_id字段设置为主键 |

![img](MongoDB.assets\Figure-1-Mapping-Table-to-Collection-1.png)

#### 数据类型 

数据结构是BSON类似JSON

| 数据类型           | 描述                                                         |
| :----------------- | :----------------------------------------------------------- |
| String             | 字符串。存储数据常用的数据类型。在 MongoDB 中，UTF-8 编码的字符串才是合法的。 |
| Integer            | 整型数值。用于存储数值。根据你所采用的服务器，可分为 32 位或 64 位。 |
| Boolean            | 布尔值。用于存储布尔值（真/假）。                            |
| Double             | 双精度浮点值。用于存储浮点值。                               |
| Min/Max keys       | 将一个值与 BSON（二进制的 JSON）元素的最低值和最高值相对比。 |
| Array              | 用于将数组或列表或多个值存储为一个键。                       |
| Timestamp          | 时间戳。记录文档修改或添加的具体时间。                       |
| Object             | 用于内嵌文档。                                               |
| Null               | 用于创建空值。                                               |
| Symbol             | 符号。该数据类型基本上等同于字符串类型，但不同的是，它一般用于采用特殊符号类型的语言。 |
| Date               | 日期时间。用 UNIX 时间格式来存储当前日期或时间。你可以指定自己的日期时间：创建 Date 对象，传入年月日信息。 |
| **Object ID**      | 对象 ID。用于创建文档的 ID。类似唯一主键                     |
| Binary Data        | 二进制数据。用于存储二进制数据。                             |
| Code               | 代码类型。用于在文档中存储 JavaScript 代码。                 |
| Regular expression | 正则表达式类型。用于存储正则表达式。                         |

#### 数据库保留名

- admin：root数据库，保存用户，权限；一些特定的服务器端命令也只能从这个数据库运行
- local：永远不会被复制，用来存储限于本地单台服务器的任意集合
- config：当mongodb用于分片设置时，config数据库在内部使用，用于保存分片的相关信息

#### 索引

mongodb使用的是 b-tree

单索引

复合索引





### 2.操作

- 连接：mongodb://admin:123456@10.50.63.63:27017/database

- 查询语句

  | 操作       | 格式                     | 范例                                        | RDBMS中的类似语句       |
  | :--------- | :----------------------- | :------------------------------------------ | :---------------------- |
  | 等于       | `{<key>:<value>`}        | `db.col.find({"by":"菜鸟教程"}).pretty()`   | `where by = '菜鸟教程'` |
  | 小于       | `{<key>:{$lt:<value>}}`  | `db.col.find({"likes":{$lt:50}}).pretty()`  | `where likes < 50`      |
  | 小于或等于 | `{<key>:{$lte:<value>}}` | `db.col.find({"likes":{$lte:50}}).pretty()` | `where likes <= 50`     |
  | 大于       | `{<key>:{$gt:<value>}}`  | `db.col.find({"likes":{$gt:50}}).pretty()`  | `where likes > 50`      |
  | 大于或等于 | `{<key>:{$gte:<value>}}` | `db.col.find({"likes":{$gte:50}}).pretty()` | `where likes >= 50`     |
  | 不等于     | `{<key>:{$ne:<value>}}`  | `db.col.find({"likes":{$ne:50}}).pretty()`  | `where likes != 50`     |

- and条件：使用 , 隔开

- or 条件：$or: [{key1: value1}, {key2:value2}]

    

```shell
# 创建数据库 数据库有集合才会持久化到磁盘
use runoob
show dbs
# 数据库得有数据才会显示
db.runoob.insert({"name":"菜鸟教程"})

# 删除数据库
db.dropDatabase()
# 创建 删除集合
db.createCollection("rb") # 显式创建
# 隐式创建 在插入文档的时候自动创建
show tables / show collections
db.rb.drop()


# 插入文档
db.col.insert({title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: '菜鸟教程',
    url: 'http://www.runoob.com',
    tags: ['mongodb', 'database', 'NoSQL'],
    likes: 100
})
# 批量插入
db.col.insertMany([{},{}])
# 查询
db.col.find()
db.col.findOne()
# 查询文档 投影查询(只显示指定字段)
db.collection.find(query, projection)


# 更新文档
db.collection.update(
   <query>,
   <update>,
   {
     upsert: <boolean>,
     multi: <boolean>,
     writeConcern: <document>
   }
)
db.col.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}},{multi:true})
# 以上语句只会修改第一条发现的文档，如果你要修改多条相同的文档，则需要设置 multi 参数为 true。
db.collection.save(
   <document>,
   {
     writeConcern: <document>
   }
) 
# 覆盖修改
db.col.update({_id:"1"},{likenum:NumberInt(1001)})
# 局部修改
db.col.update({_id:"2"},{$set:{likenum:NumberInt(1001)}})


# 删除文档
db.collection.remove(
   <query>,
   {
     justOne: <boolean>,
     writeConcern: <document>
   }
)
db.col.remove({})

# 统计查询
db.col.count({})


# limit() 限制 记录条数 skip()方法来跳过指定数量的数据 
# 分页查询
db.COLLECTION_NAME.find().limit(NUMBER).skip(NUMBER)


# 排序 -1 降序 1 正序
db.COLLECTION_NAME.find().sort({KEY:1})
# 通过正则表达式，进行模糊查询


# 索引
db.col.getIndexs()
db.col.createIndex(keys, options)
db.col.dropIndex(index)


# 聚合 类似 sql中的count*
db.COLLECTION_NAME.aggregate(AGGREGATE_OPERATION)

```



### 3.副本集

两种类型：

- 主节点：数据操作的主要连接点，可读写
- 次要节点：数据冗余备份节点，可以读或选举

三种角色：

- 主要成员：就是主节点
- 副本成员：备份数据，读操作
- 仲裁者：选举

```yaml
# mongod.conf

storage:
  dbPath: /var/lib/mongodb
  journal:
    enabled: true
#  engine:
#  mmapv1:
#  wiredTiger:

# where to write logging data.
systemLog:
  destination: file
  logAppend: true
  path: /var/log/mongodb/mongod.log
# network interfaces
net:
  port: 27017
  bindIp: 127.0.0.1

# how the process runs
processManagement:
  fork: true
  timeZoneInfo: /usr/share/zoneinfo
  
replication:
  replSetName: myrs

sharding:
  # 分片角色
  clusterRole: shardsvr
```

[使用docker来创建副本集](https://blog.51cto.com/bigboss/2296445)



```shell
/usr/local/mongodb/bin/mongo --host=xx --port=27017 -uadmin -p123456
# 初始化 主节点
rs.initiate()
rs.config()
rs.status()
# 添加副本节点
rs.add("192.168.1.126:27018")
# 添加冲裁节点
rs.addArb("192.168.1.126:27019")

# 从节点 设置奴隶节点，可以读数据
rs.slaveOk()
```



#### 主节点选举原则

触发条件

1) 主节点故障

2) 主节点网络不可达

3) 人工干预(rs.stepDown(600))



### 4.分片集群

分片 (sharding) 是一种跨多台机器分布数据的方法

分片集群包含组件：

- 分片（存储）：每个分片包含分片数据的子集
- mongos（路由）: 充当查询路由器，在客户端应用程序和分片集群之间提供接口
- config servers（调度配置）：配置服务器存储群集的元数据和配置设置。

![image-20210630154145342](MongoDB.assets\image-20210630154145342.png)

副本集的创建同上

路由节点创建

```shell
# 路由节点不需要 data
storage:
  dbPath: /var/lib/mongodb
  journal:
    enabled: true
#  engine:
#  mmapv1:
#  wiredTiger:

# where to write logging data.
systemLog:
  destination: file
  logAppend: true
  path: /var/log/mongodb/mongod.log
# network interfaces
net:
  port: 27017
  bindIp: 127.0.0.1

# how the process runs
processManagement:
  fork: true
  timeZoneInfo: /usr/share/zoneinfo

sharding:
  # 分片角色
  configDB: myconfigrs /xxx:27019,xxx:27119,
```

### 5.安全认证

```shell
use admin
db.createUser({user:"myroot",pwd:"123456",roles:["root"]})
db.createUser({user:"admin",pwd:"123456",roles:["root"]})
# 创建一个普通用户
db.createUser({user:"fadmin",pwd:"furnace123456",roles:[{role:"root",db:"furnace"}]})
db.auth("furnace","furnace123456")

#开启认证
security:
  authorization:enabled
  
```

role: https://www.cnblogs.com/igoodful/p/13706220.html

分片集群也需要设置安全认证


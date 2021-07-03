## MongoDB

### 1.简介

- MongoDB是由c++编写的，是一个基于分布式文件存储的开源数据库系统
- 旨在为WEB应用提供可扩展的高性能数据存储解决方案。
- [教程](https://www.runoob.com/mongodb/mongodb-tutorial.html)

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
# 创建数据库
use runoob
show dbs
# 数据库得有数据才会显示
db.runoob.insert({"name":"菜鸟教程"})

# 删除数据库
db.dropDatabase()
# 创建 删除集合
db.createCollection("rb")
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
# 查询
db.col.find()

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

# 删除文档
db.collection.remove(
   <query>,
   {
     justOne: <boolean>,
     writeConcern: <document>
   }
)
# 查询文档
db.collection.find(query, projection)

# limit() 限制 记录条数 skip()方法来跳过指定数量的数据 
db.COLLECTION_NAME.find().limit(NUMBER).skip(NUMBER)

# 排序 -1 降序 1 正序
db.COLLECTION_NAME.find().sort({KEY:1})

# 索引
db.collection.createIndex(keys, options)

# 聚合 类似 sql中的count*
db.COLLECTION_NAME.aggregate(AGGREGATE_OPERATION)

```


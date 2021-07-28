# MySQL

## 1. 前言

> 关系型数据库: 行、列

mysql、oracle、sql server、DB2、SQLite



> 非关系型数据库:  nosql

redis  mongodb	

**DBMS**

数据库管理系统



**mysql**

体积小、速度快、成本低

中小型网站、大型网站、集群

5.7版本

8.0版本



### 1.1 数据库引擎

|              | MyISAM | InnoDB        |
| ------------ | ------ | ------------- |
| 事务支持     | 不支持 | 支持          |
| 数据行锁定   | 不支持 | 支持          |
| 外键约束     | 不支持 | 支持          |
| 全文索引     | 支持   | 不支持        |
| 表空间的大小 | 较小   | 较大，约为2倍 |

- MYISAM：节约空间，速度快
- INNODB：安全，事务的处理，多表用户操作



> 在物理空间存在的位置

data目录下

- InnoDB 在数据库表中只有一个*.frm文件，
- MyISAM
  - *.frm   表结构的定义文件
  - *.MYD  数据文件(data)
  - *.MYI   索引文件(index)





### 安装

[菜鸟安装教程](https://www.runoob.com/w3cnote/windows10-mysql-installer.html)



### 基本命令

```sql
 mysql (-h hostname) -uroot -p 
 -- 输入密码
123456

-- 修改密码
update mysql user set authentication_string=password('123456') where user='root' and Host = 'localhost';
flush privileges;  -- 刷新权限

show databases;
use school;		-- 切换数据库
show tables;

exit; -- 退出

 -- 显示当前正在执行的mysql连接
 show processlist
 describe student;
 
create user username identified with mysql_native_password by 'password'; 
grant all on 你的数据库名称.* to 'username'@'%'; 
flush privileges;
```

### 遇到的问题
- ERROR 2059 (HY000): Authentication plugin 'caching_sha2_password' cannot be loaded？
```shell
MySQL8.0.11版本默认的认证方式是caching_sha2_password 
navicat连接不支持该认证方式 Navicat支持mysql_native_password
解决方案，修改加密规则
# 修改账户密码加密规则并更新用户密码
ALTER USER 'root'@'localhost' IDENTIFIED BY '123456' PASSWORD EXPIRE NEVER;
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456'; 
FLUSH PRIVILEGES;   #刷新权限 
alter user 'root'@'%' identified by '123456'; #重置密码

# 重启mysql
```

- ERROR 2003 (HY000): Can't connect to MySQL server on '10.50.63.63' (10061)
```shell
远程服务器端口未开放
$ sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
注释 # bind-address = 127.0.0.1
user里面的 Host 字段设置为 %
```

- mysql忘记密码



## 2. 操作数据库

### 2.1 基本操作

```sql
create database if not exists westos;	-- 创建数据库

drop database if exists westos; 	-- 删除数据库

use westos;		-- 使用数据库

show databases;		-- 查看所有数据库

create table [if not exists] `table_name` (
	'字段名' 列类型 [属性] [索引] [注释],
    ...
    '字段名' 列类型 [属性] [索引] [注释],
)engine=INNODB DEFAULT CHARSET=utf8

-- 修改表
alter table teacher rename as teacher1; -- 修改表名
alter table teacher1 add age int(11); -- 新增字段
alter table teacher1 modify age varchar(11); -- 修改约束
alter table teacher1 change age age1 int(1); -- 重命名约束

alert table drop age1; -- 删除表字段
drop table if exits teacher1; -- 删除表
```

### 2.2 数据库的列类型

> 数值

- tinyint        十分小的数据      1个字节

- smallint     较小的数据          2个字节

- **int               标准的整数          4个字节**

- bigint         较大的数据           8个字节

- float           单精度浮点数       4个字节

- double      双精度浮点数        8个字节

- decimal     字符串形式的浮点数  金融计算的时候，一般使用decimal

  

> 字符串

- char          字符串固定大小的    0~255

- **varchar     可变字符串              0~65535**     String

- tinytext       微型文本                2^8-1

- **text             文本串                     2^16-1**      保存大文本

  

> 时间日期

java.util.Date



- date    YYYY-MM-DD      日期格式
- time    HH:mm:ss          时间格式
- **datetime     YYYY-MM-DD  HH:mm:ss   最常用的时间格式**
- **timestamp    时间戳，  1970.1.1到现在的毫秒数**
- year  年份表示



> null

- 没有值，未知
- 注意，不要使用null去计算



### 2.3 数据库字段属性

Unsigned:

- 无符号的整数
- 声明了该列不能声明为负数



zerofill:

- 0填充的
- 不足的位数，使用0来填充， int(3) , 5 ----- 005



自增：

- 通常理解为自增，自动在上一条的记录上 +1（默认）
- 通常用来设计唯一的主键 ~ index，必须是整数类型
- 可以自定义设计主键自增的起始值和步长



非空 NULL not null

- 假设设置为 not null，如果不给它赋值，就会报错
- NULL，如果不填写值，默认就是null



默认：

- 设置默认的值



**每个表必须存在5个字段**

id   主键

`version`	乐观锁

is_delete	伪删除

gmt_create 创建时间

gmt_update 修改时间



## 3. MySQL数据管理

### 3.1 外键

> 方式一、在创建表的时候，增加约束（麻烦，比较复杂）

删除有外键关系的表的时候，必须要先删除引用别人的表（从表），再删除被引用的表（主表）



> 方式二、创建表成功后添加外键约束

```sql
alter table `student` add constraint `FK_gradeid` foreign key(`gradeid`) references `grade`(`gradeid`);
```

以上都是物理外键

**最佳实践**

- 数据库就是单纯的表，只用来存数据，只有行和列
- 我们想使用多张表的数据，想使用外键（程序去实现）



### 3.2 DML语言

```sql
-- 插入语句
insert into 表名 ([字段名1，字段名2，字段名3]) values ('值1') ('值2')...
-- 注意事项
-- 1.字段和字段之间使用 英文逗号 隔开
-- 2.字段是可以省略的，但是后面的值必须要一一对应
-- 3.可以同时插入多条数据

-- 更新语句
update 表名 set colnum_name = value, [colnum_name = value,...] where [条件]

-- 删除语句
delete from 表名 [where 条件]
-- 清空
truncate 表名

```

> delete 和 truncate区别

- 相同点：都能删除数据，都不会删除表结构
- 不同：
  - truncate 重新设置 自增列 计数器会归零
  - truncate 不会影响事务

问题：重启数据库，现象

- InnoDB 自增会从 1开始 （存在内存当中的，断电即失）

- MyISAM 继续从上一个自增量开始 （存在文件中的，不会丢失）

  

### 3.3 DQL查询数据

```sql
-- 查询语法
select 字段,... from 表
-- 去重
select distinct 字段 from 表

select version() -- 查询系统版本
select 100*3-1 as 计算结果 -- 用来计算
select @@auto_increment_increment -- 查询自增的步长

-- where 条件子句
-- or and not
-- || && !=

-- 模糊查询
-- like 结合 %(代表0到任意两个字符) _(一个字符)  
-- in  in( , , )
-- null     not null


-- 连表查询
-- from 表 xxx join 连接的表 on 交叉条件 where 


-- 排序
order by 字段 ASC(升序) DESC(降序)
-- 分页
-- limit 起始下标 页面大小


-- 常用函数
select abs(-8) -- 绝对值
select ceiling(9.4) -- 向上取整
select floor(9.4)  -- 向下取整
select sign(10)   -- 判断一个数的符号


-- 聚合函数
count
sum
avg
min
max

```

| 操作       | 描述                                       |
| ---------- | ------------------------------------------ |
| inner join | 如果表中至少有一个匹配，就返回行           |
| left join  | 会从左表中返回所有的值，即使右表中没有匹配 |
| right join | 会从右表中返回所有的值，即使左表中没有匹配 |

> 自连接

自己的表和自己的表连接，核心：一张表拆为两张一样的表即可

### 3.4 事务

- 原子性：强调事务的不可分割
- 一致性：事务的执行的前后数据的完整性保持一致
- 持久性：一个事务执行的过程中，不应该受到其他事务的干扰
- 隔离性：事务一旦结束，数据就持久到数据库



> 脏读

指一个事务读取了另一个事务未提交的数据

> 不可重复度

在一个事务内读取表中的某一行数据

> 虚读

是指在一个事务内读取到了别的事务插入的数据，导致前后读取不一致

```sql
-- mysql 默认开启事务自动提交的

set autocommit = 0; -- 关闭
start transaction; -- 标记一个事务的开始，

insert xx;

commit;
rollback;

set autocommit = 1; -- 开启
savepoint 保存点名 -- 设置一个事务的保存点

```



### 3.5 索引

> MySQL官方对索引的定义为：索引(index)是帮助mysql高效获取数据的数据结构 0.5s 0.00001s
>
> 提取句子主干，都可以得到索引的本质：索引是数据结构

#### 索引分类

- 主键索引（primary key）
  - 唯一标识，主键不可重复，只能有一个列作为主键
- 唯一索引（unique key）
  - 避免重复的列出现，唯一索引可以重复，多个列都可以标识为 唯一索引
- 常规索引（key/index）
  - 默认的，key关键字来设置
- 全文索引（fulltext）
  - 在特定的数据引擎下才有，MyISAM中

#### 索引原则

- 索引不是越多越好
- 不要对进程变动数据加索引
- 小数据量的表不需要加索引
- 索引一般加载常用来查询的字段上



Hash 类型的索引

Btree：InnoDB的默认数据结构
















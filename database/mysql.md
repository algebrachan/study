# MySQL

## 前言

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



## 操作数据库

### 基本操作

```sql
create database if not exists westos;	-- 创建数据库

drop database if exists westos; 	-- 删除数据库

use westos;		-- 使用数据库

show databases;		-- 查看所有数据库
```

### 数据库的列类型

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



### 数据库字段属性

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



每个表必须存在5个字段

id   主键

`version`	乐观锁

is_delete	伪删除

gmt_create 创建时间

gmt_update 修改时间
















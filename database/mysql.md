# MySQL

### 安装
[菜鸟安装教程](https://www.runoob.com/w3cnote/windows10-mysql-installer.html)



### 基本命令

```shell
 mysql (-h hostname) -u username -p 
 # 输入密码

 # 显示当前正在执行的mysql连接
 show processlist
 
 
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
```

- ERROR 2003 (HY000): Can't connect to MySQL server on '10.50.63.63' (10061)
```shell
远程服务器端口未开放
$ sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
注释 # bind-address = 127.0.0.1
user里面的 Host 字段设置为 %
```

- mysql忘记密码
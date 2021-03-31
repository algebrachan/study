## 个人云服务器操作相关

### 1.安装软件

购买云服务器，ubuntu

安装相关软件

使用xshell7进行服务器连接

```shell
# 安装 mysql
sudo apt-get update
sudo apt-get install mysql-server
sudo mysql_secure_installation # 初始化
sudo vim /etc/mysql/mysql.conf.dmysqld.cnf # 修改配置文件
sudo service mysql restart
sudo service mysql stop

# 安装 nginx
sudo apt-get install nginx
sudo apt-get redis 
sudo vim /etc/nginx/nginx.conf # 修改配置
sudo service nginx start
sudo service nginx reload
sudo service nginx stop

# 安装 redis
sudo apt-get install redis
sudo vim /etc/redis/redis.conf 
sudo service redis restart
sudo service redis stop
```


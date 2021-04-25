## 系统部署相关问题总结

- ### redis

  ```shell
  # 安装
  sudo apt-get install redis-server
  
  # 修改配置文件 
  sudo vim /etc/redis/redis.conf
  # 开启远程连接注释
  # bind 127.0.0.1
  # 修改默认端口
  port 6379
  # 以守护进程方式运行
  daemonize yes
  # 关闭持久化
  # save 
  save ""
  
  # 启动redis 
  sudo redis-server /etc/redis/redis.conf
  
  # 关闭redis
  sudo redis-cli shutdown
  ```

- ### mysql

  ```shell
  # 安装 
  sudo apt-get install mysql-server mysql-client
  sudo service mysql restart
  
  # 登陆
  sudo mysql -uroot -p
  'root1234'
  
  # 创建
  create user furnace identified by 'furnace123456';
  # 配置用户权限，%为任意的ip地址 furnancedb为数据库的表
  grant all on furnacedb.* to 'furnace'@'%';
  # 刷新权限
  flush privileges;
  
  # 修改用户密码规则
   alter user 'furnace'@'%' identified by 'furnace123456' password expire never;
  alter user 'furnace'@'%' identified with mysql_native_password by 'furnace123456';
  flush privileges;
  
  #创建数据库
  create database furnacedb; 
  ```

  ```sql
  DROP TABLE IF EXISTS `server_list`;
  CREATE TABLE `server_list`  (
    `key` bigint(20) NOT NULL AUTO_INCREMENT,
    `gmt_create` datetime(0) NULL DEFAULT NULL,
    `gmt_modified` datetime(0) NULL DEFAULT NULL ON UPDATE CURRENT_TIMESTAMP(0),
    `server_ip` varchar(32) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL DEFAULT '',
    `server_name` varchar(64) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL DEFAULT '',
    `server_cpu` varchar(64) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL DEFAULT '',
    `server_os` varchar(64) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL DEFAULT '',
    `server_disk` varchar(64) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL DEFAULT '',
    `state` int(11) NULL DEFAULT 0,
    PRIMARY KEY (`key`) USING BTREE,
    UNIQUE INDEX `server_ip`(`server_ip`) USING BTREE
  ) ENGINE = InnoDB AUTO_INCREMENT = 5 CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci ROW_FORMAT = Dynamic;
  
  DROP TABLE IF EXISTS `furnace_list`;
  CREATE TABLE `furnace_list`  (
    `key` bigint(20) NOT NULL AUTO_INCREMENT,
    `gmt_create` datetime(0) NULL DEFAULT NULL,
    `gmt_modified` datetime(0) NULL DEFAULT NULL ON UPDATE CURRENT_TIMESTAMP(0),
    `furnace_id` int(11) NOT NULL DEFAULT 0,
    `furnace_series` varchar(4) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL DEFAULT '',
    `furnace_state` int(11) NOT NULL DEFAULT 0,
    `running_time` bigint(20) NOT NULL DEFAULT 0,
    `server_ip` varchar(64) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL DEFAULT '',
    PRIMARY KEY (`key`) USING BTREE,
    UNIQUE INDEX `furnace_id`(`furnace_id`, `furnace_series`) USING BTREE
  ) ENGINE = InnoDB AUTO_INCREMENT = 4777 CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci ROW_FORMAT = Dynamic;
  
  DROP TABLE IF EXISTS `broken_history_result`;
  CREATE TABLE `broken_history_result`  (
    `key` bigint(20) NOT NULL AUTO_INCREMENT,
    `gmt_update` datetime(0) NOT NULL ON UPDATE CURRENT_TIMESTAMP(0),
    `date` date NOT NULL,
    `server_ip` varchar(32) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL DEFAULT '',
    `type` int(11) NOT NULL,
    `broken_nums` int(11) NOT NULL DEFAULT 0,
    PRIMARY KEY (`key`) USING BTREE
  ) ENGINE = InnoDB AUTO_INCREMENT = 154 CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci ROW_FORMAT = Dynamic;
  ```

- ### supervisor

  ```shell
  # 安装
  sudo apt-get install supervisor
  pips install supervisor
  
  # 配置文件
  sudo vim /etc/supervisor/supervisord.conf
  # 启动
  sudo supervisord -c /etc/supervisor/supervisord.conf
  # 
  ```

  
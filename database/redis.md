# Redis

## 1. 安装配置

https://blog.csdn.net/erlian1992/article/details/54382443

```shell
# 连接远程redis
redis-cli -h host -p port -a password
# 手动修改配置 持久化
CONFIG SET dir "D:\\Program Files\\redis"
CONFIG SET save "900 1 300 10 60 10000 10 1" # 修改save的频率
CONFIG SET appendonly yes 
```

- 持久化配置问题

  - 需要手动save

  
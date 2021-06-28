## Docker



## Docker概述

之前存在的问题：开发环境 | 运维环境 不统一，不能跨平台部署

> docker 是一种容器，易移植，跨平台

> docker 通过隔离机制，打包装箱

> docker 相对于虚拟机技术 十分轻巧，容易技术,内核级别的虚拟化

> docker 是基于Go语言开发的



文档地址：https://docs.docker.com/

仓库地址：https://hub.docker.com/



> 比较Docker 和传统虚拟机技术的不同

- 传统虚拟机，虚拟出一条硬件，运行一个完整的操作系统，然后在这个系统上安装和运行软件

- 容器内的应用直接运行在宿主机的内容，容器自己没有自己的内核，也没有虚拟硬件

- 每个容器间是互相隔离的，每个容器内都有一个属于自己的文件系统，互补影响

  

> DevOps (开发、运维)

- 更快速的交付和部署
- 更便捷的升级和扩缩容
- 更简单的系统运维
- 更高效的计算资源利用



## Docker安装

### docker的基本组成

![img](docker.assets\idig8.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg)

**镜像（image）：**

docker镜像是一个模板，可以通过这个模板来创建容器服务

**容器（container）：**

docker利用容器技术，独立运行一个或者一组应用，通过镜像来创建

**仓库（repository）：**

仓库就是存放镜像的地方

阿里云，配置镜像加速

### 安装Docker

查看官网方案安装docker： https://docs.docker.com/engine/install/ubuntu/

Ubuntu在线安装

Ubuntu离线安装：下载packages中的包，并`dpkg -i *`

```shell
# 检验是否安装成功
sudo docker version   
sudo docker run hello-world
# 查看镜像
sudo docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
ubuntu        latest    7e0aa2d69a15   6 weeks ago    72.7MB
hello-world   latest    d1165f221234   3 months ago   13.3kB
```

#### 卸载

```shell
sudo apt-get purge docker-ce docker-ce-cli containerd.io

sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```

#### 配置阿里云镜像加速

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://kj2jhyp2.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```



#### 底层原理

Docker 是一个Client-Server 结构的系统，Docker的守护进程运行在主机上，通过Socket从客户端访问

DockerServer 接收到Docker-Client的指令，就会执行这个命令

![image-20210611144118289](docker.assets\image-20210611144118289.png)

**Docker为什么比VM快?**

1、docker有着比虚拟机更少的抽象层

2、docker利用的是宿主机的内核，vm需要的是Guest OS

![img](docker.assets\dingyue.nosdn.127.net&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg)





## Docker常用命令

### 帮助命令

```shell
docker version 			# 显示版本信息
docker info				# 显示系统信息 包括镜像和容器的数量
docker 命令 --help	   # 万能命令

```

帮助文档地址：https://docs.docker.com/engine/reference/commandline/docker/



### 镜像命令

`sudo docker images`

```shell
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
ubuntu        latest    7e0aa2d69a15   6 weeks ago    72.7MB
hello-world   latest    d1165f221234   3 months ago   13.3kB

# 解释
REPOSITORY 	镜像的仓库源
TAG			镜像标签
IMAGE ID	镜像id
CREATED		镜像创建时间
SIZE		镜像大小

Options:
  -a, --all             # 列出所有的镜像
  -q, --quiet           # 只显示id

```

`sudo docker search xx`	hub上搜索镜像

```shell
wangchen@rtx3080:~$ sudo docker search mysql
NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   10984     [OK]       
mariadb                           MariaDB Server is a high performing open sou…   4152      [OK] 
# Options
--filter=STARS=3000 # 搜索出来的镜像STARS大于3000
```

`sudo docker pull xx ` 	下载镜像

```shell
# 下载镜像 docker pull 镜像名[:tag]
wangchen@rtx3080:~$ sudo docker pull mysql
Using default tag: latest # 如果不写tag 默认就是latest
latest: Pulling from library/mysql
69692152171a: Pull complete 	# 分层下载 docker image的核心
1651b0be3df3: Pull complete 
951da7386bc8: Pull complete 
0f86c95aa242: Pull complete 
37ba2d8bd4fe: Pull complete 
6d278bb05e94: Pull complete 
497efbd93a3e: Pull complete 
f7fddf10c2c2: Pull complete 
16415d159dfb: Pull complete 
0e530ffc6b73: Pull complete 
b0a4a1a77178: Pull complete 
cd90f92aa9ef: Pull complete 
Digest: sha256:d50098d7fcb25b1fcb24e2d3247cae3fc55815d64fec640dc395840f8fa80969 # 签名
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest

# 两个命令等价
docker pull mysql
docker pull docker.io/library/mysql:latest 
```

`sudo docker rmi xx` 	删除镜像

```shell
wangchen@rtx3080:~$ sudo docker rmi -f 2c9028880e58 # 删除指定的镜像 id
sudo docker rm -f $(sudo docker ps -aq)
sudo docker rmi -f $(sudo docker images -aq) # 删除全部容器

```



### 容器命令

有了镜像才可以创建容器

容器可以理解为一个运行的虚拟机

```shell
docker run [可选参数] image
# 参数说明
--name="Name"	#容器名字 tomcat01...
-d				#后台方式启动
-it				#使用交互方式运行，进入容器内查看内容
-p				#指定容器的端口 -p 8080:8080
	-p 主机端口：容器端口
-P				#随机指定端口

# 启动并进入容器
wangchen@rtx3080:~$ sudo docker run -it ubuntu bash
root@44f4f30d57ac:/# exit

```

**列出所有运行的容器**

```shell
# docker ps 命令
-a #列出当前正在运行的容器+带出历史运行过的内容
-n=? # 显示最近创建的容器
-q # 只显示容器的编号

```

**退出容器**

```shell
exit	#直接容器停止并退出
Ctrl + P + Q # 容器不停止退出
```

**删除容器**

```shell
docker rm 容器id
docker rm -f $(docker ps -aq) # 删除所有容器
```

**启动停止容器**

```shell
docker start 容器id
docker restart 容器id
docker stop 容器id
docker kill 容器id
```



### 常用其他命令

```shell
# 命令 docker run -d 镜像名

# 问题 docker ps 发现 centos停止了

# 常见的坑：docker 容器使用后台运行，就必须要有一个前台进程，docker发现没有应用，就会自动停止
# nginx，容器启动后，发现自己没有提供服务，就会立刻停止，就是没有程序了

# 查看日志命令
docker logs  -f -t --tail 容器id

# 查看 容器的进程id
docker top 容器id

# 查看容器信息
docker inspect 容器id

# 进入容器
docker exec -it 容器id /bin/bash # 进入容器后开启一个新的终端
docker attach 容器ID  # 进入容器正在执行的终端 不会启动新的进程   

# 容器主机互相拷贝文件
docker cp 容器id:/home/xx /home # 容器 -->宿主机
```

## Docker实践

### 安装rabbitmq

```shell
docker pull rabbitmq:management  # 记得下载这个带web管理的
docker run -d --name rabbitmq -p 5671:5671 -p 5672:5672 -p 4369:4369 -p 25672:25672 -p 15671:15671 -p 15672:15672 rabbitmq:management
输入网址：http://IP:15672/ 账号密码：guest/guest
```



### 安装Nginx

```shell
docker search nginx
docker pull nginx
docker run -d --name nginx01 -p 3344:80 nginx
```



### 安装Tomcat

```shell
docker run -it --rm tomcat:9.0
# 之前启动的是后台 -d, 停止了容器之后是可以查到 docker run 0it --rm 一般用来测试，用完就删除

# 下载再启动 
docker pull tomcat
docker run -d 3355:8080 --name tomcat01 tomcat
# 进入容器
docker exec -it tomcat01 /bin/bash


```



### 可视化面板安装

- portainer

  ```shell
  docker run -d -p 8088:9000 \
  --restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
  ```

  

- Rancher







## Docker镜像讲解

### 镜像是什么

镜像是一种轻量级、可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，它包含运行某个软件所需的所有内容，包括代码，运行时、库、环境变量和配置文件

所有应用，直接打包docker镜像就可以直接跑起来

如何得到镜像：

- 从远程仓库下载
- 拷贝
- 自己制作一个镜像DockerFile

### Docker镜像加载原理

> UnionFS (联合文件系统)

分层。轻量级且高性能的文件系统

bootfs加载内核

rootfs加载操作系统

对于一个精简的OS，rootfs可以很小，只要包含最基本的命令，工具和程序库就可以。 因为底层直接用host的kernel 自己只需要提供rootfs就可以，对于linux发行版，bootfs基本是一致的，可以公用



### commit镜像

```shell
docker commit 提交容器成为一个新的副本

# 命令和git原理类似
docker commit -m="提交描述信息" -a="作者" 容器id 目标镜像名:[TAG]

```



### 容器数据卷

将应用和环境打包成一个镜像

数据？如果数据都在容器中，那么我们容器删除，数据就会丢失

数据可以持久化

卷技术，目录的挂载，将我们容器内的目录，挂载到linux上面

> 方式一：直接使用命令来挂载 -v

```shell
docker run -it -v 主机目录:容器内目录
```



### 安装mysql

```shell
docker run -d -p 3310:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7 
```

```shell
# 匿名挂载
-v 容器内路径
docker run -d -P --name nginx01 -v /etc/nginx nginx

# 查看所有的 volume 的情况
docker volume ls
# 具名挂载
docker run -d -P --name nginx01 -v juming-nginx:/etc/nginx nginx
# 建议使用具名挂载

-v 容器内路径	# 匿名挂载
-v 卷名：容器内路径	# 具名挂载
-v /宿主机路径:容器内路径	# 指定路径挂载

# ro 只读
# rw 可读可写

```



### Dockerfile 

dockerfile就是用来构建 docker 镜像的构建文件 命令脚本  通过脚本可以生成镜像，镜像是一层一层的

创建步骤

1、编写一个dockerfile文件

2、docker build 构建一个镜像

3、docker run 运行镜像

4、docker push 发布镜像

基础知识

1、每个保留关键字都是必须是大写字母

2、执行从上到下顺序执行

3、# 表示注释

4、每个指令都会创建一个新的镜像层，并提交！

#### 指令

![img](docker.assets\img.dongcoder.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg)

```shell
# 1.编写dockerfile文件
cat mydockerfile-ubuntu
FROM ubuntu
MAINTAINER wangchen<17326032809@126.com>

ENV MYPATH /usr/local
WORKDIR $MYPATH
RUN apt install vim

EXPORE 80

CMD echo $MYPATH
CMD echo "---end---"
CMD /bin/bash

# 2.通过这个文件构建镜像
# 命令 docker build -f dockerfile文件路径 -t 镜像名:[tag] .
sudo docker build -f /home/wangchen/mydockerfile -t ubunto01:0.1 .
# 3.测试运行

docker history 镜像id

```

> CMD和ENTRYPOINT 区别

```shell
CMD			# 指定这个容器启动的时候要运行的命令，只有最后一个会生效，可被替代
ENTRYPOINT	# 指定这个容器启动的时候要运行的命令，可以追加


```



#### dockerfile制作tomcat镜像

```shell
# 1.准备镜像文件 Tomcat压缩包，jdk的压缩包

# 2.编写dockerfile文件

```

#### 发布镜像

> dockerhub

```shell
Log in to a Docker registry.
If no server is specified, the default is defined by the daemon.

Options:
  -p, --password string   Password
      --password-stdin    Take the password from stdin
  -u, --username string   Username


sudo docker login -u wangchen0328
sudo docker push wangchen0328/nginx01:1.1

```

> 阿里云镜像

1、登陆阿里云

2、找到容器镜像服务

3、创建命名空间

4、创建容器镜像



#### 打包、加载镜像

```shell
docker save -o /home/xxx.tar IMAGE
docker load -i /home/xxx.tar IMAGE
```



## Docker网络

> 理解docker0

清空docker 

![image-20210625110758293](docker.assets\image-20210625110758293.png)

三个网络

```shell
#问题： docker 是如何处理容器网络访问的？
docker run -d -P --name tomcat01 tomcat

# linux 可以 PING 通 docker 容器内部
# 容器和容器之间是可以ping通的 通过桥接ip地址

```

> 原理

1.我们每启动一个docker容器，docker就会给docker容器分配一个ip，我们只要安装了docker，就会有一个网卡 docker0 **桥接**模式，使用的技术是 **evth-pair** 技术！

```ini
# 我们发现这个容器带来的网卡，都是一对对的
# evth-pair 就是一对的虚拟设备接口，他们都是成对出现的，一段连着协议，一段彼此相连
# evth-pair 充当一个桥梁，连接各种虚拟网络设备
# OpenStac Docker容器之间的连接 OVS连接
```

容器之间的通信

![image-20210625112919019](docker.assets\image-20210625112919019.png)

### -- link 

```shell
docker run -d -P --name tomcat03 --link tomcat02 tomcat
# 通过 --link 直接ping容器名称 
```

本质探究： --link 就是我们在hosts 配置中增加一个 tomcat02 xxx

自定义网络 不建议使用 --link

docker0问题：他不再支持容器名连接访问



### 自定义网络

- 网络模式
  - bridge：桥接  docker(默认，自己创建也使用bridge模式)
  - none：不配置网络
  - host：和宿主机共享网络
  - container：容器网络连通 （局限很大）

```shell
docker network ls # 查看docker网络
docker network rm xxx

docker run -d -P --name tomcat01 --net bridge tomcat
# docker0 默认
# 自定义网络
docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet

root@iZbp18eobx9icoi9md9hlbZ:~# docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
ce27d69eeca1   bridge    bridge    local
867b33f74adc   host      host      local
3f60b8a0603b   mynet     bridge    local
018c37eef36e   none      null      local

docker run -d -P --name tomcat01 --net mynet tomcat
# 自定义网络可以相互ping，修复了docker0的缺点
```



### 网络连通

```shell
# 容器连接网络
docker network connect mynet tomcat01
# 连通之后就是将 tomcat01 放到了 mynet 网络下
# 一个容器两个ip地址！ 阿里云服务，公网ip 私网
```



### 实战部署redis集群

分片 + 高可用 + 负载均衡

```shell
docker network create redis --subnet 172.38.0.0/16

# 通过脚本创建六个redis配置
for port in $(seq 1 6);\
do \
mkdir -p /mydata/redis/node-${port}/conf
touch /mydata/redis/node-${port}/conf/redis.conf
cat << EOF >/mydata/redis/node-${port}/conf/redis.conf
port 6379
bind 0.0.0.0
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 5000
cluster-announce-ip 172.38.0.1${port}
cluster-announce-port 6379
cluster-announce-bus-port 16379
appendonly yes
EOF
done
# 启动 服务
docker run -p 6371:6379 -p 16371:16379 --name redis-1 \
-v /mydata/redis/node-1/data:/data \
-v /mydata/redis/node-1/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.11 redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf

# 创建集群
docker exec -it redis-1 /bin/sh
/data # redis-cli --cluster create 172.38.0.11:6379 172.38.0.12:6379 172.38.0.13:6379 172.38.0.14:6379 172.38.0.15:6379 172.38.0.16:6379 --cluster-replicas l
# 进入
/data # redis-cli -c
```





## DockerCompose

### 简介

Docker Compose来轻松高效的管理容器，定义运行多个容器

> 官方介绍 

Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration. To learn more about all the features of Compose, see [the list of features](https://docs.docker.com/compose/#features).

Compose works in all environments: production, staging, development, testing, as well as CI workflows. You can learn more about each case in [Common Use Cases](https://docs.docker.com/compose/#common-use-cases).

Using Compose is basically a three-step process:

1. Define your app’s environment with a `Dockerfile` so it can be reproduced anywhere.
2. Define the services that make up your app in `docker-compose.yml` so they can be run together in an isolated environment.
3. Run `docker compose up` and the [Docker compose command](https://docs.docker.com/compose/cli-command/) starts and runs your entire app. You can alternatively run `docker-compose up` using the docker-compose binary.



Compose是Docker官方的开源项目 ，需要安装！

Dockerfile 让程序在任何地方运行 web服务。 redis、mysql、nginx ...多个容器

```yaml
version: "3.9"  # optional since v1.27.0
services:
  web:
    build: .
    ports:
      - "5000:5000"
    volumes:
      - .:/code
      - logvolume01:/var/log
    links:
      - redis
  redis:
    image: redis
volumes:
  logvolume01: {}
```

`docker-compose up` 

- 服务service，容器、应用 （web、redis、mysql ...）
- 项目project。 一组关联的容器。



### 安装

可根据官网 安装

```shell
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
```



流程：

1、创建网络

2、执行Docker-compose.yaml

3、启动服务



默认的服务名 文件名_ 服务名 _ num

多个服务器 集群 A B _num 副本数量

集群状态。 服务都不可能只有一个运行时。 弹性 、10 HA 高并发

kubectl service 负载均衡。

### 网络规则

10个服务 => 项目 (项目中的内容都在同个网络下，域名访问)

mysql:3306

10个容器实例 ： mysql  redis

停止：docker-compose down ctrl+c



### yaml 规则

```yaml
# 3层 
version:'' # 版本
services: # 服务
	服务1：web
        # 服务配置
        images
        build
        network
        ...
    服务2：redis

# 其他配置 网络/卷 全局规则
volumes:
networks:
configs:
```

多写，多看，compose.yaml 配置

- 官方文档
- 开源项目





## Docker Swarm

- 集群管理
  - manager
  - worker
- node：节点，多个节点组成了一个网络集群
- service：docker集群的核心

![Swarm mode cluster](docker.assets\swarm-diagram.png)

```shell
# 初始化 swam 
docker swarm init --advertise-addr 私网地址
# 私网 公网
# 获取令牌
docker swarm join-token manager
docker swarm join-token worker
# 加入节点
docker swarm join --token xxx xx.xx.xx.xx.:2377

docker node ls

```

### Raft协议

双主双从： 假设一个节点挂了，其他节点是否可用

Raft协议：保证大多数节点存活才可用， 只要>1 集群至少大于3台



弹性、扩容！集群！

docker-compose up 启动一个项目 单机

集群： swarm docker service

容器 => 服务 => 副本

redis 服务 => 10 个副本 (同时开启 10个redis容器)



 灰度发布：金丝雀发布

```shell
docker run 容器启动 不具有扩缩容器
docker service 服务启动 具有扩缩容器，滚动更新！

docker service create -p 8888:80 --name my-nginx nginx
# 扩容
docker service update --replicas 10 my-nginx
docker service update --replicas 1 my-nginx
# 扩容
docker service scale my-nginx=5

docker service rm my-nginx
```

### 总结

命令 -> 管理 -> api -> 调度 -> 工作节点(创建Task容器维护创建)



## Docker 其他命令

docker-compose 单机部署项目

**Docker Stack**部署，集群部署！

```shell
docker-compose up -d wordpress.yaml

docker stack deploy wordpress.yaml

```



**Docker Secret**

安全！ 配置密码，证书！

**Docker Config**

![image-20210628161757343](docker.assets\image-20210628161757343.png)

未来需要学习Golang语言，天生的并发语言


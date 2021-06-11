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




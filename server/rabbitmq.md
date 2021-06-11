# RabbitMq消息队列

## 1. 简介

RabbitMQ是一个实现了AMQP（Advanced Message Queuing Protocol）高级消息队列协议的消息队列服务，用Erlang语言。

[详细介绍](https://zhuanlan.zhihu.com/p/63700605)

### 1.0不同MQ的特点

- ActiveMQ：开源消息总线，完全支持JMS规范的消息中间件，丰富的API，多种集群架构模式
- Kafka：开源分布式发布-订阅消息系统。主要特点是基于pull的模式来处理消息消费，高吞吐量，适合产生大量数据的互联网服务的数据收集业务
- RocketMQ：阿里开源的消息中间件，纯java开发，具有高吞吐量、高可用性，适合大规模分布式系统应用，广泛应用于交易、充值、流计算、消息推送、日志流式处理、binglog分发
- RabbitMQ：使用Erlang开发的开源消息队列系统，应用于对数据一致性、稳定性和可靠性高的场景，对性能和吞吐量要求其次
- 

### 1.1 [windows安装](https://blog.csdn.net/zhm3023/article/details/82217222)

- 安装erlang [地址](https://www.erlang.org/downloads)
- 修改环境变量
- 安装 [rabbitmq](https://www.rabbitmq.com/install-windows.html)

- 启动

```shell
# 对应安装目录的 sbin下
rabbitmq-server.bat
```

- web端口：15672  
  - 用户密码：guest guest

### 1.2 [linux安装](https://www.cnblogs.com/web424/p/6761153.html)



### 1.3 docker安装

```shell
docker pull rabbitmq:management  # 记得下载这个带web管理的
docker run -d --name rabbitmq -p 5671:5671 -p 5672:5672 -p 4369:4369 -p 25672:25672 -p 15671:15671 -p 15672:15672 rabbitmq:management
# 输入网址：http://IP:15672/ 账号密码：guest/guest
```



### 1.3 基本命令



## 2.应用


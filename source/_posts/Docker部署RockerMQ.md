---
title: Docker部署RockerMQ
date: 2023-03-23 14:39:14
tags: [Docker]
---

## 安装NameServer

### 1、拉取镜像

```
docker pull rocketmqinc/rocketmq
```

### 2、创建数据目录

```
mkdir -p /docker/rocketmq/nameserver/logs /docker/rocketmq/nameserver/store
```

### 3、运行

```
docker run -d \
--restart=always \
--name rmqnamesrv \
--privileged=true \
-p 9876:9876  \
-v /docker/rocketmq/nameserver/logs:/root/logs \
-v /docker/rocketmq/nameserver/store:/root/store \
-e "MAX_POSSIBLE_HEAP=100000000" rocketmqinc/rocketmq \
sh mqnamesrv
```

## 安装broker

### 1、创建broker.conf配置文件

目录为 /opt/docker/rocketmq/broker.conf

```
brokerClusterName = DefaultCluster
brokerName = broker-a
brokerId = 0
deleteWhen = 04
fileReservedTime = 48
brokerRole = ASYNC_MASTER
flushDiskType = ASYNC_FLUSH
brokerIP1 = 主机的IP
```

### 2、启动broker

```
docker run -d \
--restart=always \
--name rmqbroker \
--link rmqnamesrv:namesrv \
-p 10911:10911 \
-p 10909:10909 \
--privileged=true \
-v /docker/rocketmq/data/broker/logs:/root/logs \
-v /docker/rocketmq/data/broker/store:/root/store \
-v /docker/rocketmq/conf/broker.conf:/opt/docker/rocketmq/broker.conf \
-e "NAMESRV_ADDR=namesrv:9876" \
-e "MAX_POSSIBLE_HEAP=200000000" rocketmqinc/rocketmq \
sh mqbroker -c /opt/docker/rocketmq/broker.conf
```

## 安装控制台

### 1、拉取镜像

```
docker pull pangliang/rocketmq-console-ng
```

### 2、启动控制台

```
docker run -d \
--restart=always \
--name rmqadmin \
-e "JAVA_OPTS=-Drocketmq.namesrv.addr=ip:9876 -Dcom.rocketmq.sendMessageWithVIPChannel=false" \
-p 8080:8080 pangliang/rocketmq-console-ng
```

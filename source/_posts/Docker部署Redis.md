---
title: Docker部署Redis
date: 2022-12-03 15:59:56
tags: [Docker]
---

拉取Redis镜像

```
sudo docker pull redis
```

运行容器

```
sudo docker run \
--restart=always \
--name redis \
-p 6379:6379 \
-v /home/opc/redis/data:/data \
-e TZ=Asia/Shanghai \
-e REDIS_PASSWORD=123456 \
-d redis:latest \
--appendonly yes
```
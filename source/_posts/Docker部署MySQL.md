---
title: Docker部署MySQL
date: 2022-12-03 15:58:30
tags: [Docker]
---

拉取MySQL镜像

```
sudo docker pull mysql:5.7
```

运行容器

```
sudo docker run \
--restart=always \
--name mysql \
-p 3306:3306 \
-v /home/opc/mysql/data:/var/lib/mysql \
-e TZ=Asia/Shanghai \
-e MYSQL_ROOT_PASSWORD=123456 \
-d mysql:5.7 \
--character-set-server=utf8mb4 \
--collation-server=utf8mb4_general_ci
```

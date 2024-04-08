---
title: Docker部署Minio
date: 2023-03-23 15:00:11
tags: [Docker]
---

### 1、创建目录

```
mkdir -p /docker/minio/data /docker/minio/config
```

### 2、拉取镜像

```
docker pull minio/minio
```

### 3、运行容器

```
docker run -p 9000:9000 -p 9090:9090 \
--name minio \
-d --restart=always \
-e "MINIO_ACCESS_KEY=admin" \
-e "MINIO_SECRET_KEY=admin123456" \
-v /docker/minio/data:/data  \
-v /docker/minio/config:/root/.minio \
minio/minio server /data --console-address ":9090" -address ":9000"
```
### 4、访问地址

```
http://ip:9090
```
---
title: CentOS7安装Docker
date: 2021-06-07 15:30:08
tags: [Docker]
---

安装需要的软件包， yum-util 提供 yum-config-manager 功能，另外两个是 devicemapper 驱动依赖的

```
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

设置 yum 源

```
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

![](1.png)

可以查看所有仓库中所有 docker 版本，并选择特定版本安装

```
yum list docker-ce --showduplicates | sort -r
```

![](2.png)

```
sudo yum install <版本号>
```

![](3.png)

启动并加入开机启动

```
sudo systemctl start docker
sudo systemctl enable docker
```

验证安装是否成功

```
sudo docker version
```

![](4.png)

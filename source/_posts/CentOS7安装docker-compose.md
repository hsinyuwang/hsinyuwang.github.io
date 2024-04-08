---
title: CentOS7安装docker-compose
date: 2021-06-07 15:30:08
tags: [Docker]
---

> centos 7中python-pip模块不存在，是因为像centos这类衍生的发行版，源跟新滞后，或者不存在。即使使用yum去search python-pip也找不到软件包。

> 为了使用安装滞后或源中不存在的安装包，需要安装扩展源EPEL。扩展源EPEL(http://fedoraproject.org/wiki/EPEL) 是由 Fedora 社区打造，为 RHEL 及衍生发行版如 CentOS、Scientific Linux 等提供高质量软件包的项目。

## 安装扩展源

```
yum -y install epel-release
```

## 安装python-pip模块

```
yum install python-pip
```

此时执行 `docker-compose version` 命令可能会出现命令不存在的问题，需要进行安装

```
cd /usr/local/bin/

wget https://github.com/docker/compose/releases/download/1.14.0-rc2/docker-compose-Linux-x86_64

rename docker-compose-Linux-x86_64 docker-compose docker-compose-Linux-x86_64

chmod +x /usr/local/bin/docker-compose
```



---
title: CentOS7开放及查看端口
date: 2020-10-14 15:30:08
tags: [服务器]
---

1、开放端口

```
firewall-cmd --zone=public --add-port=5672/tcp --permanent   # 开放5672端口

firewall-cmd --zone=public --remove-port=5672/tcp --permanent  #关闭5672端口

firewall-cmd --reload   # 配置立即生效
```

2、查看防火墙所有开放的端口

```
firewall-cmd --zone=public --list-ports
```

3、关闭防火墙

```
systemctl stop firewalld.service
```

4、查看防火墙状态

```
firewall-cmd --state
```

5、查看监听的端口

```
netstat -lnpt
```

6、检查端口被哪个进程占用

```
netstat -lnpt |grep 5672
```

7、查看进程的详细信息

```
ps 6832
```

8、中止进程

```
kill -9 6832
```

---
title: CentOS7禁止ping
date: 2020-10-07 15:30:08
tags: [服务器]
---

**一、临时生效**

1、允许ping

```
echo 0 >/proc/sys/net/ipv4/icmp_echo_ignore_all 
```

2、禁止ping

```
echo 1 >/proc/sys/net/ipv4/icmp_echo_ignore_all 
```

二、永久生效

修改文件：/etc/sysctl.conf

1、允许ping

增加一行：

```
vim /etc/sysctl.conf
net.ipv4.icmp_echo_ignore_all=0
```

2、禁止ping

增加一行：

```
vim /etc/sysctl.conf
net.ipv4.icmp_echo_ignore_all=1
```

 修改后使用以下命令使之生效：

```
sysctl -p
```

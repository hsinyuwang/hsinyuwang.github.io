---
title: CentOS7修改SSH端口
date: 2021-08-06 15:30:08
tags: [服务器]
---

**一、修改ssh配置文件sshd_config**

```
vim /etc/ssh/sshd_config
```

**二、防火墙放行**

```
firewall-cmd --zone=public --add-port=8022/tcp --permanent

firewall-cmd --reload
```

**三、向SELinux中添加修改的SSH端口**

先安装SELinux的管理工具 semanage (如果已经安装了就直接到下一步) ：

```
yum provides semanage
```

安装运行semanage所需依赖工具包 policycoreutils-python：

```
yum -y install policycoreutils-python
```

查询当前 ssh 服务端口：

```
semanage port -l | grep ssh
```

向 SELinux 中添加 ssh 端口：

```
semanage port -a -t ssh_port_t -p tcp 8022
```

重启 ssh 服务：

```
systemctl restart sshd.service
```

测试成功后，将原来的22端口注释掉即可

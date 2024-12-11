---
title: CentOS7修改SSH端口和秘钥登录
date: 2021-08-06 15:30:08
tags: [服务器]
---

## 修改ssh配置文件sshd_config

```
vim /etc/ssh/sshd_config
```
首先将原来 #Port=22 这行的#号去掉，另起一行同样键入 Port=8022 然后保存退出

## 防火墙放行

```
firewall-cmd --zone=public --add-port=8022/tcp --permanent
firewall-cmd --reload
```

重启 ssh 服务：

```
systemctl restart sshd.service
```

测试成功后，将原来的 22 端口注释掉即可（这里如果不行的话，检查 SELinux 设置、iptables 防火墙设置、firewalld 防火墙设置）

## 向SELinux中添加修改的SSH端口（修改成功可跳过）

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

## 使用 Xshell 秘钥登录

首先需要在被连接的Linux服务器上修改 /etc/ssh/sshd_config 配置文件中的参数，修改完以后，先不要重启 sshd 服务

```
PasswordAuthentication no        # 不允许密码验证登录
PubkeyAuthentication yes          # 允许公钥验证登录
AuthorizedKeysFile .ssh/authorized_keys  # 指定公钥文件路径
```

在 Xshell 端口生成一对秘钥，将公钥复制粘贴到 Linux 服务器的中 /root/.ssh/authorized_keys 文件

```
vim /root/.ssh/authorized_keys
```
![](1.png)

![](2.png)

![](3.png)

![](4.png)

![](5.png)

上图中在下拉框中已经看到了公钥，然后全选复制，最后将粘贴到Linux服务器的 /root/.ssh/authorized_keys 文件中

然后给这个文件修改权限为 600（如果 /root/.ssh/ 下没有 authorized_keys 文件，可以手动创建一个）

```
chmod 600  /root/.ssh/authorized_keys
```

最后，重点是所有设置都完成以后，记得要重启sshd服务

```
systemctl  restart sshd
```

登录测试

![](6.png)

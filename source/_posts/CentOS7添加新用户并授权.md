---
title: CentOS7添加新用户并授权
date: 2020-04-30 15:30:08
tags: [服务器]
---

1、添加新用户

（1）创建新用户

```
adduser [用户名]
```

（2）修改新用户的密码

```
passwd [用户名]
```

2、授权

（1）添加sudoers文件可写权限

```
chmod -v u+w /etc/sudoers
```

（2）修改sudoers文件

```
vim /etc/sudoers
```

在sudoers文件中找到如下位置并添加如下内容：

```
[用户名]    ALL=(ALL)    ALL（如需新用户使用sudo时不用输密码，把最后一个ALL改为NOPASSWD:ALL即可）
```

![](1.png)

（3）收回sudoers文件可写权限

```
chmod -v u-w /etc/sudoers
```


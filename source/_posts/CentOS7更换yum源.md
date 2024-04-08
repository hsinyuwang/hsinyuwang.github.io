---
title: CentOS7更换yum源
date: 2020-04-12 15:30:08
tags: [服务器]
---

一、备份当前源

```
cd /etc/yum.repos.d/
cp CentOS-Base.repo CentOS-Base-repo.bak
```

二、使用wget下载阿里yum源repo文件

```
wget http://mirrors.aliyun.com/repo/Centos-7.repo
```

三、清理旧包

```
yum clean all
```

四、把下载下来阿里云repo文件设置成为默认源

```
mv Centos-7.repo CentOS-Base.repo
```

五、生成阿里云yum源缓存并更新yum源

```
yum makecache
yum update
```

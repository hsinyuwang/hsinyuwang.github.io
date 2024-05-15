---
title: nexus搭建maven私有仓库
date: 2024-05-15 14:20:37
tags: [后端]
---


## 安装 nexus

下载地址 [https://help.sonatype.com/en/download.html](https://help.sonatype.com/en/download.html)

解压后进入到 bin 目录，执行

```
./nexus run
```

默认端口为 8081 进入后修改默认密码，完成初始化并禁用匿名访问

我们希望 nexus 的工作流程为：先在本地 releases 仓库里面找，如果没有则去snapshots 仓库里面找，如果快照仓库没有就去阿里云找，如果阿里云有则直接将其缓存到 blob 中

![](1.png)

所以需要配置一个阿里云的仓库 [地址链接](https://developer.aliyun.com/mvn/guide)

![](2.png)

![](3.png)

将创建好的 maven-aliyun 添加到 maven-public

![](4.png)

将 maven-releases 和 maven-snapshots 修改为如下配置

![](5.png)

## 配置 maven settings

在 settings.xml 指定位置添加如下信息

password 修改为实际密码
```
<server>
    <id>maven-snapshots</id>
    <username>admin</username>
    <password>password</password>
</server>
<server>
    <id>maven-releases</id>
    <username>admin</username>
    <password>password</password>
</server>
<server>
    <id>nexus-public</id>
    <username>admin</username>
    <password>password</password>
</server>
```

ip:port 修改为实际地址和端口号

```
<profile>
    <id>nexus</id>
    <repositories>
    <repository>
        <id>maven-public</id>
        <url>http://ip:port/repository/maven-public</url>
        <releases>
            <enabled>true</enabled>
        </releases>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
    </repository>
    </repositories>
</profile>
```

```
<activeProfiles>
    <activeProfile>nexus</activeProfile>
</activeProfiles>
```

## 项目配置

在项目 pom 文件中添加

```
<repositories>
    <repository>
        <id>nexus-public</id>
        <name>nexus</name>
        <url>http://ip:port/repository/maven-public</url>
        <releases>
            <enabled>true</enabled>
        </releases>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
    </repository>
</repositories>
```



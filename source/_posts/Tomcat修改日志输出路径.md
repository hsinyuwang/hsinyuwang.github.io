---
title: Tomcat修改日志输出路径
date: 2020-03-27 14:53:30
tags: [后端]
---

## 修改catalina.sh文件

进入tomcat/bin/路径下，找到catalina.sh文件，下载本地打开搜索CATALINA_OUT=，在后面修改为你想要输出的路径，设置输出路径为/home/user/logs文件夹下，路径后面跟上catalina.out文件名

![](1.png)

## 修改server.xml文件

进入tomcat/conf/路径下，找到server.xml文件，下载本地打开搜索directory=，将这里修改为你指定的输出路径，注意这里写到文件夹即可，最后不需要/

![](2.png)

## 修改server.xml文件

进入tomcat/conf/路径下，找到logging.properties文件，下载本地打开，将图中所示的四处都改为你要输出的路径，同样也是写到文件夹即可，不需要加/

![](3.png)


三个文件都覆盖后，重启tomcat
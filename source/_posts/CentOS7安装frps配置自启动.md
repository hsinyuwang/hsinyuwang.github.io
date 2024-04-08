---
title: CentOS7安装frps配置自启动
date: 2021-08-06 15:30:08
tags: [服务器]
---

1和2二选一

1、脚本安装frps

```
cd /tmp
wget --no-check-certificate https://raw.githubusercontent.com/clangcn/onekey-install-shell/master/frps/install-frps.sh -O ./install-frps.sh
chmod 777 install-frps.sh
./install-frps.sh install
```

2、程序包安装

（1）下载frp程序文件

```
https://github.com/fatedier/frp/releases
```

（2）解压文件

下载后解压到自己的目录，我这里解压到/usr/local/frp:

![](1.png)

（3）添加systemd配置文件：

```
vim /etc/systemd/system/frps.service
```

文件内容如下：

```
[Unit]

Description=FRP Server Daemon

[Service]

Type=simple

ExecStartPre=-/sbin/setcap cap_net_bind_service=+ep /usr/local/frp/frps

ExecStart=/usr/local/frp/frps -c /usr/local/frp/frps.ini

Restart=always

RestartSec=20s

User=nobody

PermissionsStartOnly=true

[Install]

WantedBy=multi-user.target
```

ExecStart的内容请根据自己frp安装目录修改。

（4）设置开机启动

```
systemctl daemon-reload
systemctl enable frp
```

（5）启动 frp

```
systemctl start frp
```

（6）查看frp是否启动

```
ps aux | grep frp
```

![](2.png)

看到这里就说明启动成功了。



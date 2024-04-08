---
title: Ubuntu使用proxychains终端代理
date: 2024-04-07 16:49:22
tags: [嵌入式]
---

1 安装

```
sudo apt install proxychains4
```

2 配置

```
sudo vim /etc/proxychains4.conf
```

3 注释掉 socks4 127.0.0.1 那一行，在最后加上代理工具的设置

```
socks5 127.0.0.1 1080
```

4 使用

在需要使用代理的命令前加上 proxychains4 例如：

```
proxychains4 git clone https://github.com/xxx/xxx.git
```
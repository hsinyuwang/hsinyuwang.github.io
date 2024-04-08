---
title: Docker部署Nginx
date: 2022-12-04 14:00:50
tags: [Docker]
---

拉取Nginx镜像

```
sudo docker pull nginx
```

运行容器

```
sudo docker run \
--restart always \
--name nginx \
-v /home/opc/nginx/nginx.conf:/etc/nginx/nginx.conf \
-v /home/opc/nginx/logs:/var/log/nginx \
-v /home/opc/nginx/cert:/cert \
-v /home/opc/myblog/html:/usr/share/nginx/html \
-p 80:80 \
-p 443:443 \
-e TZ=Asia/Shanghai \
-d nginx
```

nginx.conf 配置 SSL

```
server {
     #HTTPS的默认访问端口443。
     #如果未在此处配置HTTPS的默认访问端口，可能会造成Nginx无法启动。
     listen 443 ssl;
     
     #填写证书绑定的域名
     server_name <yourdomain>;
 
     #填写证书文件绝对路径
     ssl_certificate cert/<cert-file-name>.pem;
     #填写证书私钥文件绝对路径
     ssl_certificate_key cert/<cert-file-name>.key;
 
     ssl_session_cache shared:SSL:1m;
     ssl_session_timeout 5m;
	 
     #自定义设置使用的TLS协议的类型以及加密套件（以下为配置示例，请您自行评估是否需要配置）
     #TLS协议版本越高，HTTPS通信的安全性越高，但是相较于低版本TLS协议，高版本TLS协议对浏览器的兼容性较差。
     ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
     ssl_protocols TLSv1.1 TLSv1.2 TLSv1.3;

     #表示优先使用服务端加密套件。默认开启
     ssl_prefer_server_ciphers on;
 
 
    location / {
           root html;
           index index.html index.htm;
    }
}
```

设置 HTTP 请求自动跳转 HTTPS

```
server {
    listen 80;
    #填写证书绑定的域名
    server_name <yourdomain>;
    #将所有HTTP请求通过rewrite指令重定向到HTTPS。
    rewrite ^(.*)$ https://$host$1;
    location / {
        index index.html index.htm;
    }
}
```

一级域名跳转到 www 二级域名

```
if ($http_host ~ "^xxx.com$") {
    rewrite ^(.*) https://www.xxx.com$1 permanent;
}
```
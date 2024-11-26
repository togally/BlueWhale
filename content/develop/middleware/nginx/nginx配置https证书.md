+++
title = 'nginx配置https证书'
tags = ['nginx','https']
series_order = 1
order = 1
draft = false
+++
[Nginx或Tengine服务器配置SSL证书_数字证书管理服务（原SSL证书）(SSL Certificate)-阿里云帮助中心](https://help.aliyun.com/zh/ssl-certificate/user-guide/install-ssl-certificates-on-nginx-servers-or-tengine-servers)


```Nginx
server {
    listen 443 ssl;
    # 需要注意下最好指定某个域名或者通配
    server_name *.{domain};

    ssl_certificate /etc/nginx/ssl/default.crt;
    ssl_certificate_key /etc/nginx/ssl/default.key;
}
```




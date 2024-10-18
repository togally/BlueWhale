+++
title = 'nginx配置代理转发流量'
tags = ['nginx']
series = ["中间件"]
series_order = 1
order = 1
draft = false
+++
# nginx配置代理转发流量
## 业务背景
跟高德平台用808协议调了一个车载摄像头直播，但是高德平台提供的时http协议通讯，我们需求https因此用nginx做一个转发


## 错误配置
```properties
server {
    listen 443 ssl default_server;
    server_name test.jinyingweishi.cn;
    ssl_certificate /usr/local/ssl/cert.pem;
    ssl_certificate_key /usr/local/ssl/cert.key;
    ssl_session_timeout 5m;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
    ssl_prefer_server_ciphers on;

    location / {
        add_header Access-Control-Allow-Origin *;
        add_header Access-Control-Allow-Methods 'GET, POST, OPTIONS';
        add_header Access-Control-Allow-Headers 'DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Authorization';
        gzip_static on;
        root /opt/platform/web/supervise;
        index  index.html index.htm;
    }

    location /gaodeX {
        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header REMOTE-HOST $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass  http://ip:8080;
        proxy_connect_timeout 15s;
        proxy_send_timeout 600s;
        proxy_read_timeout 600s;
    }
}
```

## 改正配置
```properties
server {
    listen 443 ssl default_server;
    server_name test.jinyingweishi.cn;
    ssl_certificate /usr/local/ssl/cert.pem;
    ssl_certificate_key /usr/local/ssl/cert.key;
    ssl_session_timeout 5m;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
    ssl_prefer_server_ciphers on;

    location / {
        add_header Access-Control-Allow-Origin *;
        add_header Access-Control-Allow-Methods 'GET, POST, OPTIONS';
        add_header Access-Control-Allow-Headers 'DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Authorization';
        gzip_static on;
        root /opt/platform/web/supervise;
        index  index.html index.htm;
    }

    location /gaodeX/ {
        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header REMOTE-HOST $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass  http://ip:8080/;
        proxy_connect_timeout 15s;
        proxy_send_timeout 600s;
        proxy_read_timeout 600s;
    }
}
```

## 改动说明

### 针对于location匹配带不带/的区别

location /XXX <font color=grey>匹配以 /XX 开头的请求,如果是http://host/xxx可以匹配；但是如果是http://host/xxx/aaa不可以匹配</font>

location /XXX/ <font color=grey>匹配以 /XX/ 开头的请求,则没问题，如果是http://host/xxx 会先重定向到http://host/xxx/再转发</font>

### 针对于proxy_pass匹配带不带/的区别

proxy_pass http://target/XXX/ <font color=grey> 请求http://host/XXX/abc 则会转发到http://target/abc</font> 

proxy_pass http://target/XXX <font color=grey> 请求http://host/XXX/abc 则会转发到http://target/XXX/abc </font>


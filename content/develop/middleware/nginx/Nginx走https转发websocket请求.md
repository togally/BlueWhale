+++
title = 'Nginx走https转发websocket请求'
date = 2024-10-24T16:05:03+08:00
tags = ['nginx','运维']
series = ["中间件"]
draft = false
+++
## 问题背景
新项目需要用到websocket进行通讯,有一个问题就是脚手架提供的websocket通讯是走ws协议的也就是http,因此需要用nginx来转发https请求。
ps: 我们用的spring-websocket好像websocket可以支持配置一些证书的信息,没办法着急提测太懒了，回头再研究下补上。

## nginx配置
没啥好说的,固定路径转发到http请求,直接上配置
```text
 location /wss-xg {
    proxy_pass http://ip:port/xg-security/resource/websocket?$args;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;##此处Upgrade注意大小写
    proxy_set_header Connection "Upgrade";
    proxy_set_header Remote_addr $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_read_timeout 600s;
}
```

## 额外补充
1. js好像在websocket请求中，token是没法通过header去传递的(没验证,其他博客里看到的)，所有如果需要权限验证，可以在requestParam里传
2. 如果前端使用了socket.io这类框架来做socket请求处理，那么有可能会对后端的socket路径有要求。
例如对于我们这个项目，前端在调试时使用的是https://address/wss-xg,但是实际请求就是wss://address/socket.io?xxx,本来应该统一对该请求
做代理但是由于已经有了其他项目用这种方式接入了websocket所以我这里也没法用了,这里附下socket.io方式的nginx配置
```text
       location ^~/socket.io/ { 
            proxy_pass http://test.jinyingweishi.cn:28889;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;##此处Upgrade注意大小写
            proxy_set_header Connection "Upgrade";
            proxy_set_header Remote_addr $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_connect_timeout 15s;
            proxy_send_timeout 3600s;
            proxy_read_timeout 3600s;

        }
```
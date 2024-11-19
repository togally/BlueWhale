+++
title = '基于mybatis拦截器的数据权限设计'
date = 2024-10-10T16:15:59+08:00
tags = ['mybatis',"设计方案"]
series = ["开发"]
draft = true
+++
## 业务背景
![img.png](design/dataScopeConf.png)
需要在全局的角度上来做数据权限的控制，主要是从部门的纬度，来做数据权限控制
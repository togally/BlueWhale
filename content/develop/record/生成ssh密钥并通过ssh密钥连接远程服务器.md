+++
title = '生成ssh密钥并通过ssh密钥连接远程服务器'
tags = ['ssh','运维']
date = 2024-10-30T14:24:09+08:00
draft = false
+++
## 生成ssh密钥并通过ssh密钥连接远程服务器
生成 SSH 密钥对：
在本地终端运行以下命令生成 SSH 密钥对：

```shell
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
````
按照提示输入保存密钥的文件路径（默认是 ~/.ssh/id_rsa）和密码（可选）。

将公钥添加到远程服务器：
使用以下命令将公钥（默认为 ~/.ssh/id_rsa.pub）添加到远程服务器的 ~/.ssh/authorized_keys 文件中：

```shell
ssh-copy-id -i ~/.ssh/id_rsa.pub remote_user@remote_host
````
按照提示输入远程服务器的密码。完成后，你的公钥将被添加到远程服务器的 `~/.ssh/authorized
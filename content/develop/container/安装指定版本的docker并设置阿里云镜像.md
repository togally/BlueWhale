+++
title = '安装指定版本的docker并设置阿里云镜像'
tags = ['docker','运维']
series = ["容器化"]
date = 2024-11-07T17:09:46+08:00
draft = false
+++
1. 安装必要的依赖包：
```sh
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```
2. 设置 Docker 的阿里云 Yum 仓库：
```sh
sudo yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

3. 列出可用的 Docker 版本：
```sh
yum list docker-ce --showduplicates | sort -r
```

4. 安装指定版本：
```sh
sudo yum install -y docker-ce-20.10.13 docker-ce-cli-20.10.13 containerd.io
```

5. 启动并设置开机自启动：
```sh
sudo systemctl start docker
sudo systemctl enable docker
```

6. 验证安装：
```sh
sudo docker --version
sudo docker run hello-world
```

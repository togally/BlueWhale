+++
title = '大白话解析MQTT协议'
date = 2024-09-14T23:23:58+08:00
series = ["计算机网络"]
series_order = 1
order = 1
draft = false
+++
# MQTT协议入门
参考文档[维基百科](https://en.wikipedia.org/wiki/MQTT)、[阿里云消息队列MQTT版本](https://help.aliyun.com/zh/apsaramq-for-mqtt/quick-start?spm=a2c4g.11186623.0.0.67ea2784mW2VwO)、[MQTT3.1.1协议](https://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.html)

## 协议来源
mqtt协议最早在1999年被撰写。最初的目的是为了监控石油管道石油运输,所以需要一个轻量高效，占用带宽和电量少的协议。
因为那个时候类似于这种设备只能通过卫星去进行数据通讯，开销很大。

## 协议组成
mqtt协议定义了两种网络实体,broker和clients（一个消息broker和多个clients）。

broker的主要作用是接收client发送来的消息，并通过路由转发给目标client，消息的发送方和接收方都完全不需要感知到目标client的地址
或者存储任何路由表。

client是用来收发消息的客户端，它可以是任何设备，只要装载了mqtt库并且连接到了broker那么他就可以作为client来进行收发消息

## 消息流转
- client会发送 控制消息(control message) 由message + topic组成 到 broker，控制消息最小2字节，最大可以256mb
- broker会根据订阅信息，向所有订阅了topic的client转发消息
- 如果没有client订阅该消息，那么该消息会被丢弃掉，除非设置了保留消息
- broker会保留最近的一条保留数据，当用client订阅topic的时候会立即推送

## broker
broker可以部署在本地或者云上,它会维护一个由层级结构的topic，当client发送消息到某个topic后，
所有订阅了该topic的client都会收到一个该消息副本。

之所以说是层级结构的是因为topic可以用‘/’来区分层级，使用‘+’可以匹配单个层级，使用‘#’可以匹配多个层级

home/#可以匹配 home/living_room/temperature、home/bedroom/temperature

home/+/temperature 可以匹配 home/living_room/temperature 和 home/bedroom/temperature。
## 消息类型
### connect
表示等待与服务建立一个明确的连接,希望和服务建立一个连接
### disconnect
等待 MQTT 客户端完成其必须执行的所有工作，并等待 TCP/IP 会话断开连接
### publish
将请求传递给 MQTT 客户端后立即返回到应用程序线程。

![img.png](network/mqttMsg.png)

## Quality of service(QOS)
- QoS 0 最多一次 – 消息仅发送一次，客户端和代理无需采取任何额外步骤来确认发送（即发即忘）。
- QoS 1 至少一次 – 发送者多次重试消息，直到收到确认（确认发送）。
- Qos 2 恰好一次——发送者和接收者进行两级握手，以确保只接收到消息的一份副本（保证传递）。

## [阿里云云消息队列MQTT](https://help.aliyun.com/zh/apsaramq-for-mqtt/quick-start?spm=a2c4g.11186623.0.0.67ea2784mW2VwO)


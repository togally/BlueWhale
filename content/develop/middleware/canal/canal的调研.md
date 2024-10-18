+++
title = 'canal调研'
series = ["中间件"]
tags = ['canal']
series_order = 1
order = 1
date = 2024-09-13T09:29:49+08:00
draft = false
+++

## 业务背景
我们有这样的一个场景，一个主系统和N个子系统，子系统之间数据隔离,主系统有全部数据。

于是针对数据同步就有了两个需求点
- 需要可以在将N个子系统的数据实时同步到主系统
- 需要考虑主子系统之间出现网路隔离子系统仍然正常工作可操作,再网络恢复后主子系统之间进行数据的增量同步。

在这样的背景下我们准备对阿里的[cannel](https://github.com/alibaba/canal)和[dataX](https://github.com/alibaba/DataX)两个工具进行调研

## 前置条件
- 目前canal 支持源端 MySQL 版本包括 5.1.x , 5.5.x , 5.6.x , 5.7.x , 8.0.x
- mysql需要开启binglog
```mysql
# binlog查询命令
show variables like 'log_bin';

# mysql开启binlog
[mysqld]
# 打开binlog
log-bin=mysql-bin
# 选择ROW(行)模式
binlog-format=ROW
# 配置MySQL replaction需要定义，不要和canal的slaveId重复
server_id=1
# 要监控的数据库名称
binlog-do-db=my-test
```

## canal原理
canal的工作原理可以是基于主从同步,但是与主从同步不完全相同。

canal是通过伪装成从库通过dump协议从mysql拉取binlog日志，[解析binlog日志]()并将解析后的数据应用到目标数据库

主从同步通过dump协议拉取binlog日志后，通过[binlog回放]()来达到数据同步的目的

ps:

<span style="color: #00BCD4">
主从同步在断网重连的场景下,如果断网时间过长，在数据一致性这块可能会有问题（binlog位置不一致,relay log 损坏，数据冲突等原因），
需要依赖三方的工具检查和手动解决。<br><br>
</span>

<span style="color: #00BCD4">
canal针对于断网恢复后的数据一致性问题，给出了持久化binlog同步位置信息的方案（文件或者数据库都可），以保证断网后不存在数据一致性问题
</span>

![pop](canal/canal_work_principle.png)

## 业务场景匹配度
- canal支持多子库往一个主库同步的情况
- 相对于主从同步下的数据一致性问题，canal本身就给了解决方案，无需再借用第三方工具来处理


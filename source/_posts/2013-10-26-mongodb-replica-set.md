---
layout: post
title: MongoDB 高可用群集简单配置
category: 数据库
published: true
---


### Replica Set 节点类型分为三种：

	standard：常规节点，它存储一份完整的数据副本，参与选举投票，有可能成为primary节点。
	passive：存储了完整的数据副本，参与投票，不能成为primary节点。 
	arbiter：仲裁节点，只参与投票，不接收复制的数据，也不能成为primary节点。

<!--more-->

###节点规划
仅做的测试而已，所以所有节点仅是同一台服务器上的三个不同端口的 MongoDB 实例，本文配置使用2个常规节点和一个arbiter节点，arbiter节点由于不同步数据，所以负载会很小，部署对硬件没有太大的要求，
因为 MongoDB Replica Set 不允许添加两个 “本地（localhost或127.0.01）节点”，所有首先在系统的 hosts 文件中添加:

  127.0.0.1 mongodb27021
  127.0.0.1 mongodb27022
	127.0.0.1 mongodb27023

节点规划如下:

	mongodb27021:27021 主节点(Primary)
	mongodb27022:27022  从节点(Secondary)
	mongodb27023:27023 仲裁节点(Arbiter)

###启动三个节点
```
mongod.exe --port=27021 --replSet=rs_test --dbpath="..\27021\data" --logpath="..\27021\log\mongodb.log" --smallfiles
```

其它两个节点用同样的命令，将端口替换为指定的端口，及指定合适的数据库文件及日志存储目录，生产环境，可将 mongodb 安装到系统服务中（很简单 google 一下吧）。

###配置群集
mongo -port 27021 连接到其中一个节点。

#####初始化群集:
rs.initiate()
这个命令可以接收参数，直接配置群集中的各节点，这里简单点直接初始化，27021将成为 Primary 节点。

#####添加从节点
rs.add("mongodb27022:27022")
默认从节点的数据不可 “读写” ，连接到从节点，执行 “rs.salveOk()” 可以使从节点只读，以减少主节点的读压力。

####添加仲裁节点
rs.addArb("mongodb27022:27023")
仲裁节点，不存储数据，只在主节点发生故障时，参与主节点的 “推举”。

至此一个最简单的 Replica Set 高可用群集配置完成。

###检查群集
1. 在主节点接入数据，在从节点即可到刚刚接入的数据。
2. 结束到主节点，再连接到从节点用 “rs.isMaster()” 可以看到从节点，已被“推举”为主节点了。




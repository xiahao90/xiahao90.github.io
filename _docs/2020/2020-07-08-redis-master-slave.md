---
title:  redis主从配置
category: 2020
order: 97
date: 2020-07-08
---
------
### 主从配置 
参与主从的分类主节点（master）与从节点（slave），每个从节点只能有一个主节点，复制数据是单项复制从主节点复制到从节点。
** 主从复制三种方式(在从节点中配置) **
+ 在配置文件redis.conf中加入 slaveof {masterhost} {masterport} 随着redis启动生效
+ 在redis-server启动命令后加入 --slaveof {masterhost} {masterport} 随着redis启动是生效
+ 在启动后直接使用命令 {masterhost} {masterport} 

配置启动完成后，进入redis可以用info replication 命令查看主从状态
**连接流程为：** 保存主节点信息->建立socket连接->发送ping命令->权限验证->同步数据->持续复制

** 主节点192.160.1.160 6379查看复制状态 **
``` html
127.0.0.1:6379> info replication 
# Replication 
role:master 
connected_slaves:1 
slave0:ip=192.160.1.140,port=6379,state=online,offset=1752,lag=1
```
** 从节点192.160.1.140 6379查看复制状态 **
``` html
127.0.0.1:6379> info replication 
# Replication 
role:slave 
master_host:192.160.1.150 
master_port:6379 
master_link_status:up 
master_last_io_seconds_ago:6 
master_sync_in_progress:0
```
** 断开主从 **
slaveof可以断开主从关系，在从节点执行 slaveof on one 来断开主从，断开后当前节点升级成主节点

*** 主从注意事项 ***
1. 安全：主节点可以通过这只requirepass来设置密码，让所有客户端必须使用auth命令才能正常连接。那么从节点也需要配置masterauth参数才能正确的连接主节点并发起主从关系
2. 从节点只读：和所有主从关系一样从节点都是只读，默认配置是slave-read-only=yes 表示只读，别改就好
3. 网络延迟问题： 主从一般不在一台机器上，redis提供了repl-disable-tcp-nodelay参数用于控制是否关闭TCP NODELAY,默 认关闭，**说明如下**:
 + **关闭时**：主节点产生的所有命令数据都会发给及时从节点，这样主从延迟会变小，但是网络带宽要求较高，适用于同机房的内网。
 + **开启时**：主节点会合并较小的tcp包来节省带宽，默认发送时间取决于linux内核，一般是40毫秒，适合跨了机房的情况。

*** 数据同步概念 ***
 + ** 全量复制 **：首次主次连接的时候会进行全量复制，过程较长，网络开销很大。
 + ** 部分复制 **：用于处理应为网络异常情况导致的数据丢失场景，当从节点再次连接上后，主节点会补发丢失数据给从节点，如网络中断时间较长，造成主节点没能保存中断时间的是命令，任然需要全量复制。

*** 复制偏移量 ***
参与主从的节点都会维护自身的偏移量，主节点在处理完写入命令后会把字节长度做统计，在info relication 中的 master_repl_offset可以看到
``` html
127.0.0.1:6379> info replication 
# Replication 
role:master 
... 
master_repl_offset:6104
```
从节点每秒也会上报自己的偏移量给主节点，主节点也会保存从节点的复制偏移量，查看从节点的偏移量：
``` html
127.0.0.1:6379> info replication 
# Replication 
role:master 
connected_slaves:1 
slave0:ip=192.160.1.140,port=6379,state=online,offset=6104,lag=1
```

*** 复制积压缓冲区 ***
复制积压缓冲区是保存在主节点一个队列，默认是1M，有主从关系产生是被创建，主节点收到写命令的时候不但会把命令同步给从节点也会写入缓冲区，缓冲区存储的是偏移量与命令，因为是队列，所以超过设置大小时候会挤出先进去的数据。其用处是主从发生短时间内网络中断重新连接时，可根据缓冲区的偏移量进行部分数据复制，如果时间较长或数据较多大于设置的大小时候，还是需要全量复制。
*** 主节点运行id ***
每个redis在启动时都会分配一个40位的字符串作为运行id，用来分辨唯一的redis节点，用处是从节点在连接上主节点进行复制的时候是根据id来进行复制的，并不是通过ip：port来进行通过的，因为加入主节点重启后（aof或者rdb进行数据恢复），从节点在进行偏移量的复制的话数据将会不安全。因此id发生变化后，需要进行重写的全量复制。可以共 info server 查看run_id
*** psync ***
1. 在redis2.8以后可以用pasync来进行数据复制，在从节点执行psync runid offset来进行数据复制。runid是主节点运行id，offset是从节点保存的偏移量，首次执行psync ? -1进行全量复制
2. 主节点根据收到psync参数与自身情况来决定响应结果
 + 如果回复+FULLRESYNC {runid} {offset}, 那么从节点将厨房全量复制流程。 
 + 如果回复+CONTINUE，从节点将触发部分复制流程。
 + 如果回复+ERR，说明主节点版本低于Redis2.8
 
*** 注意 ***
+ 在全量复制过程中，主节点任然会接收命令。待复制完成后，会重新将缓冲器的内容发送给从节点
+ 复制数据大于6G的时候可能会触发超时，需要修改repl-timeout超时时间。
+ 缓存区1M可以根据系统产生数据频率情况与恢复时间进行调整，如网络中断最大时间为60秒，每秒产生的数据大约为100k，那么缓冲区最低需要6M才能保证部分复制功能的正常运行，可以将缓冲区设置成12M

*** 心跳 *** 
在主从关系建立后会维护长链接彼此都会发起心跳检测
+ 主节点每10秒会发起一次心跳检测从节点是否存活，可通过参数repl-ping-slave-period控制发送频率
+ 从节点每秒会通过replconf ack {offset} 命令，给主节点上报自身当前的复制偏移量。

> 本文首发我的微信公众号『[爱笑的架构师](https://mp.weixin.qq.com/s?__biz=MzIwODI1OTk1Nw==&mid=2650321391&idx=1&sn=0aea8b119ccee60a1366fffb9c040695&chksm=8f09cff5b87e46e33555e89a7d4929e4b184563b11f3cbfb7b834510c68708b125f128830acf&token=875646549&lang=zh_CN#rd)』，欢迎大家关注。

<!-- TOC -->

- [0. 目标](#0-目标)
- [1. 安装docker，运行docker](#1-安装docker运行docker)
- [2. 拉取redis镜像文件](#2-拉取redis镜像文件)
- [3. 准备好redis配置文件redis.conf](#3-准备好redis配置文件redisconf)
- [4. 启动redis实例](#4-启动redis实例)
- [5. 配置主从复制集群](#5-配置主从复制集群)
- [6. 测试主从复制效果](#6-测试主从复制效果)
- [总结](#总结)

<!-- /TOC -->

<img src="https://cdn.jsdelivr.net/gh/CoderLeixiaoshuai/assets/202102/20210504214729-2021-05-04-21-47-29.png" alt="20210504214729-2021-05-04-21-47-29">

# 0. 目标

本地搭建三个redis实例（一主两备），实现效果：主实例插入数据备实例可以复制同步过去。

# 1. 安装docker，运行docker

docker安装步骤省略，大家可以从官网下载并安装。

检查docker是否运行成功：

```plain
docker info
```
出现回显表示运行成功，可以做下一步操作了。

# 2. 拉取redis镜像文件

执行以下命令默认拉取tag为latest的官方redis镜像

```plain
docker pull redis
```

# 3. 准备好redis配置文件redis.conf

下载地址：

> 链接:https://pan.baidu.com/s/14tWHtk3mch3e3VlT9TYX1A  
>
> 提取密码:q3uk

拷贝为3三份，如：redis01.conf, redis02.conf, redis03.conf

打开所有的配置文件，修改如下配置项：

* 注释只监听本地选项，可以远程连接。#bind 127.0.0.1
* 关闭保护模式 protected-mode no
* 打开AOF持久化开关 appendonly yes
# 4. 启动redis实例

```shell
# 实例1
docker run -p 6381:6379 --name redis-server-01 -v /your/path/redis/conf/redis01.conf:/etc/redis/redis.conf -v /your/path/redis/data01:/data -d redis redis-server /etc/redis/redis.conf

# 实例2
docker run -p 6382:6379 --name redis-server-02 -v /your/path/redis/redis/conf/redis02.conf:/etc/redis/redis.conf -v /your/path/redis/data02:/data -d redis redis-server /etc/redis/redis.conf

# 实例3
docker run -p 6383:6379 --name redis-server-03 -v /your/path/redis/conf/redis03.conf:/etc/redis/redis.conf -v /your/path/redis/data03:/data -d redis redis-server /etc/redis/redis.conf
```



对以上命令简单解释：

* 参数-p 6381:6379，6381表示宿主机端口，6379表示容器实例端口，意思是将容器实例端口映射到宿主机端口。
* 参数--name redis-server-01，给容器实例命名。
* 参数-v /your/path/redis/conf/redis01.conf:/etc/redis/redis.conf，冒号前是宿主机配置文件路径，冒号后是容器的配置文件路径，意思是将容器实例的配置路径映射到宿主机的路径。
* 参数-v /your/path/redis/data01:/data，如上面意思相同。
* 选项-d表示以后台形式运行实例。
* 参数redis-server /etc/redis/redis.conf表示执行redis-server命令， /etc/redis/redis.conf表示以该配置文件启动redis实例，注意配置文件必须为redis-server命令的第一个参数。

# 5. 配置主从复制集群

检查实例运行状态：

```plain
docker ps
```


回显有三个redis实例即为正常。 查询实例1：redis-server-01 运行的内部ip

```plain
docker inspect redis-server-01
```


通过回显可以看到："IPAddress": "172.17.0.4" 我们将实例1规划为主，另外两个实例自然为备了，通过将主的ip和port配置在备的配置文件中即可实现主从复制的效果。

修改redis02.conf和redis03.conf配置文件，找到replicaof选项（redis5.0之前是slaveof），修改为：

```plain
replicaof 172.17.0.4 6379
```


修改完毕，重启实例2和实例3：

```plain
docker restart redis-server-02
docker restart redis-server-03
```


检查实例1的状态是否为主，并且挂载两个备实例：

```plain
docker exec -it redis-server-01 redis-cli
127.0.0.1:6379> info
```


回显如下表示主从复制配置成功：

```plain
# Replication
role:master
connected_slaves:2
slave0:ip=172.17.0.3,port=6379,state=online,offset=84,lag=1
slave1:ip=172.17.0.2,port=6379,state=online,offset=84,lag=1
```


# 6. 测试主从复制效果

连接redis实例1插入一条记录：

```plain
docker exec -it redis-server-01 redis-cli # 连接实例1
127.0.0.1:6379> set name ray  # 插入一条数据
OK  # 插入成功
```


连接redis实例2和实例3查看是否复制成功：

```plain
docker exec -it redis-server-02 redis-cli # 连接实例2
127.0.0.1:6379> get name
"ray"   # 可以查到，表明从实例已经将主实例的数据同步过来了
```


# 总结

搭建Redis主从复制实例需要有一点docker的基础，如果你对docker比较熟悉了，那搭建过程实在太容易了。没有docker基础，只要按照上面的命令逐个运行也可以100%成功哦。

下一篇带大家手把手搭建Redis主从复制+哨兵模式，尽请期待。

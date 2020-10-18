<!-- MarkdownTOC -->

- [Redis数据结构和常用命令](#redis数据结构和常用命令)
	- [1.  String字符串](#1-string字符串)
	- [2. Hash哈希](#2-hash哈希)
	- [3. List列表](#3-list列表)
	- [4. Set集合](#4-set集合)
	- [5. Sorted Set有序集合](#5-sorted-set有序集合)
	- [6. Redis常用命令参考](#6-redis常用命令参考)
- [Redis事务机制](#redis事务机制)
	- [1. Redis事务生命周期](#1-redis事务生命周期)
	- [2. Redis事务到底是不是原子性的？](#2-redis事务到底是不是原子性的)
	- [3. Redis为什么不支持回滚（roll back）？](#3-redis为什么不支持回滚roll-back)
	- [4. Redis事务失败场景](#4-redis事务失败场景)
	- [5. Redis事务相关命令](#5-redis事务相关命令)
		- [（1）WATCH](#1watch)
		- [（2）MULTI](#2multi)
		- [（3）UNWATCH](#3unwatch)
		- [（4）DISCARD](#4discard)
		- [（5）EXEC](#5exec)
- [Redis持久化策略](#redis持久化策略)
	- [什么是持久化？](#什么是持久化)
	- [Redis为什么要持久化？](#redis为什么要持久化)
	- [Redis如何实现持久化？](#redis如何实现持久化)
	- [RDB持久化](#rdb持久化)
	- [AOF持久化](#aof持久化)
	- [RDB和AOF的优缺点](#rdb和aof的优缺点)
- [Redis内存淘汰策略](#redis内存淘汰策略)
	- [什么是淘汰策略？](#什么是淘汰策略)
	- [如何配置最大内存？](#如何配置最大内存)
	- [淘汰策略的分类](#淘汰策略的分类)
		- [noeviction](#noeviction)
		- [allkeys-lru](#allkeys-lru)
		- [volatile-lru](#volatile-lru)
		- [allkeys-random](#allkeys-random)
		- [volatile-random](#volatile-random)
		- [volatile-ttl](#volatile-ttl)
		- [allkeys-lfu](#allkeys-lfu)
		- [volatile-lfu](#volatile-lfu)
	- [LRU算法](#lru算法)
	- [LFU算法](#lfu算法)
- [Redis内存失效策略](#redis内存失效策略)
	- [定时清除（主动）](#定时清除主动)
	- [惰性清除（被动）](#惰性清除被动)
	- [定期扫描清除（主动）](#定期扫描清除主动)
- [缓存更新策略](#缓存更新策略)
	- [Cache aside（旁路缓存）](#cache-aside旁路缓存)
	- [Cache aside踩坑](#cache-aside踩坑)
		- [踩坑一：先更新数据库，再更新缓存](#踩坑一：先更新数据库再更新缓存)
		- [踩坑二：先删缓存，再更新数据库](#踩坑二：先删缓存再更新数据库)
		- [最佳实践：先更新数据库，再删除缓存](#最佳实践：先更新数据库再删除缓存)
	- [Read through](#read-through)
	- [Write through](#write-through)
	- [Write behind](#write-behind)
- [缓存异常场景](#缓存异常场景)
	- [缓存穿透](#缓存穿透)
		- [什么是缓存穿透？](#什么是缓存穿透)
		- [缓存穿透常用的解决方案](#缓存穿透常用的解决方案)
	- [缓存击穿](#缓存击穿)
		- [什么是缓存击穿？](#什么是缓存击穿)
		- [缓存击穿危害](#缓存击穿危害)
		- [如何解决](#如何解决)
	- [缓存雪崩](#缓存雪崩)
		- [什么是缓存雪崩？](#什么是缓存雪崩)
		- [缓存雪崩解决方案](#缓存雪崩解决方案)
	- [缓存预热](#缓存预热)
		- [什么是缓存预热？](#什么是缓存预热)
		- [缓存预热的操作方法](#缓存预热的操作方法)
	- [缓存降级](#缓存降级)
- [高可用架构](#高可用架构)
	- [Replication（主从复制）](#replication主从复制)
		- [什么是主从复制？](#什么是主从复制)
		- [主从复制的作用](#主从复制的作用)
		- [主从复制实现原理](#主从复制实现原理)
			- [连接建立阶段](#连接建立阶段)
			- [数据同步阶段](#数据同步阶段)
			- [命令传播阶段](#命令传播阶段)
	- [Sentinel（哨兵模式）](#sentinel哨兵模式)
		- [为什么要引入哨兵模式？](#为什么要引入哨兵模式)
		- [什么是哨兵模式？](#什么是哨兵模式)
		- [哨兵模式的原理](#哨兵模式的原理)
			- [心跳机制](#心跳机制)
			- [故障转移](#故障转移)
	- [Cluster（集群）](#cluster集群)
		- [为什么要引入Cluster模式？](#为什么要引入cluster模式)
		- [什么是Cluster模式？](#什么是cluster模式)
		- [Cluster模式的原理](#cluster模式的原理)
			- [Redis集群TCP端口](#redis集群tcp端口)
			- [Redis集群数据分片](#redis集群数据分片)
- [常见应用场景](#常见应用场景)
- [实战篇](#实战篇)
	- [使用docker搭建redis主从复制集群](#使用docker搭建redis主从复制集群)
		- [0. 目标](#0-目标)
		- [1. 安装docker，运行docker](#1-安装docker运行docker)
		- [2. 拉取redis镜像文件](#2-拉取redis镜像文件)
		- [3. 准备好redis配置文件redis.conf](#3-准备好redis配置文件redisconf)
		- [4. 启动redis实例](#4-启动redis实例)
		- [5. 配置主从复制集群](#5-配置主从复制集群)
		- [6. 测试主从复制效果](#6-测试主从复制效果)
	- [使用docker搭建redis主从复制+哨兵模式](#使用docker搭建redis主从复制哨兵模式)
		- [0. 哨兵作用](#0-哨兵作用)
		- [1. 准备好哨兵配置文件sentinel.conf](#1-准备好哨兵配置文件sentinelconf)
		- [2. 启动sentinel哨兵实例](#2-启动sentinel哨兵实例)
		- [3. 测试哨兵模式](#3-测试哨兵模式)
- [公众号](#公众号)

<!-- /MarkdownTOC -->

# Redis数据结构和常用命令

Redis是key-value数据库，key的类型只能是String，但是value的数据类型就比较丰富了，主要包括五种：

* String
* Hash
* List
* Set
* Sorted Set

<div align="center">  <img src="https://uploader.shimo.im/f/WSnWh6fkpZLksyOX.png!thumbnail" width="300"/> </div><br>

## 1.  String字符串

**语法**

```plain
SET KEY_NAME VALUE
```
string类型是二进制安全的。意思是redis的string可以包含任何数据。比如jpg图片或者序列化的对象。
string类型是Redis最基本的数据类型，一个键最大能存储512MB。

## 2. Hash哈希

**语法**

```plain
HSET KEY_NAME FIELD VALUE
```
Redis hash 是一个键值(key=>value)对集合。
Redis hash是一个string类型的field和value的映射表，hash特别适合用于存储对象。

## 3. List列表

**语法**

```plain
//在 key 对应 list 的头部添加字符串元素
LPUSH KEY_NAME VALUE1.. VALUEN
//在 key 对应 list 的尾部添加字符串元素
RPUSH KEY_NAME VALUE1..VALUEN
//对应 list 中删除 count 个和 value 相同的元素
LREM KEY_NAME COUNT VALUE
//返回 key 对应 list 的长度
LLEN KEY_NAME 
```
Redis 列表是简单的字符串列表，按照插入顺序排序。
可以添加一个元素到列表的头部（左边）或者尾部（右边）

## 4. Set集合

**语法**

```plain
SADD KEY_NAME VALUE1...VALUEn
```
Redis的Set是string类型的无序集合。
集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)。

## 5. Sorted Set有序集合

**语法**

```plain
ZADD KEY_NAME SCORE1 VALUE1.. SCOREN VALUEN
```
Redis zset 和 set 一样也是string类型元素的集合,且不允许重复的成员。
不同的是每个元素都会关联一个double类型的分数。

redis正是通过分数来为集合中的成员进行从小到大的排序。

zset的成员是唯一的,但分数(score)却可以重复。

## 6. Redis常用命令参考

更多命令语法可以参考官网手册：

[https://www.redis.net.cn/order/](https://www.redis.net.cn/order/)

---完毕

# Redis事务机制

## 1. Redis事务生命周期

* 开启事务：使用MULTI开启一个事务
* 命令入队列：每次操作的命令都会加入到一个队列中，但命令此时不会真正被执行
* 提交事务：使用EXEC命令提交事务，开始顺序执行队列中的命令
## 2. Redis事务到底是不是原子性的？

先看关系型数据库ACID 中关于原子性的定义：

**原子性：**一个事务(transaction)中的所有操作，要么全部完成，要么全部不完成，不会结束在中间某个环节。事务在执行过程中发生错误，会被恢复(Rollback)到事务开始前的状态，就像这个事务从来没有执行过一样。

官方文档对事务的定义：

* **事务是一个单独的隔离操作**：事务中的所有命令都会序列化、按顺序地执行。事务在执行的过程中，不会被其他客户端发送来的命令请求所打断。
* **事务是一个原子操作**：事务中的命令要么全部被执行，要么全部都不执行。EXEC 命令负责触发并执行事务中的所有命令：如果客户端在使用 MULTI 开启了一个事务之后，却因为断线而没有成功执行 EXEC ，那么事务中的所有命令都不会被执行。另一方面，如果客户端成功在开启事务之后执行 EXEC ，那么事务中的所有命令都会被执行。

官方认为Redis事务是一个原子操作，这是站在执行与否的角度考虑的。但是从ACID原子性定义来看，**严格意义上讲Redis事务是非原子型的**，因为在命令顺序执行过程中，一旦发生命令执行错误Redis是不会停止执行然后回滚数据。

## 3. Redis为什么不支持回滚（roll back）？

在事务运行期间虽然Redis命令可能会执行失败，但是Redis依然会执行事务内剩余的命令而不会执行回滚操作。如果你熟悉mysql关系型数据库事务，你会对此非常疑惑，Redis官方的理由如下：

>只有当被调用的Redis命令有语法错误时，这条命令才会执行失败（在将这个命令放入事务队列期间，Redis能够发现此类问题），或者对某个键执行不符合其数据类型的操作：实际上，这就意味着只有程序错误才会导致Redis命令执行失败，这种错误很有可能在程序开发期间发现，一般很少在生产环境发现。
>支持事务回滚能力会导致设计复杂，这与Redis的初衷相违背，Redis的设计目标是功能简化及确保更快的运行速度。
>

对于官方的这种理由有一个普遍的反对观点：程序有bug怎么办？但其实回归不能解决程序的bug，比如某位粗心的程序员计划更新键A，实际上最后更新了键B，回滚机制是没法解决这种人为错误的。正因为这种人为的错误不太可能进入生产系统，所以官方在设计Redis时选用更加简单和快速的方法，没有实现回滚的机制。

## 4. Redis事务失败场景

有三种类型的失败场景：

（1）在事务提交之前，客户端执行的命令缓存（队列）失败，比如命令的语法错误(命令参数个数错误，不支持的命令等等)。如果发生这种类型的错误，Redis将向客户端返回包含错误提示信息的响应，同时Redis会清空队列中的命令并取消事务。

```plain
127.0.0.1:6379> set name xiaoming # 事务之前执行
OK
127.0.0.1:6379> multi # 开启事务
OK
127.0.0.1:6379> set name zhangsan # 事务中执行，命令入队列
QUEUED
127.0.0.1:6379> setset name zhangsan2 # 错误的命令，模拟失败场景
(error) ERR unknown command `setset`, with args beginning with: `name`, `zhangsan2`,
127.0.0.1:6379> exec # 提交事务，发现由于上条命令的错误导致事务已经自动取消了
(error) EXECABORT Transaction discarded because of previous errors.
127.0.0.1:6379> get name # 查询name，发现未被修改
"xiaoming"
```
（2）事务提交后开始顺序执行命令，之前缓存在队列中的命令有可能执行失败。
```plain
127.0.0.1:6379> multi # 开启事务
OK
127.0.0.1:6379> set name xiaoming # 设置名字
QUEUED
127.0.0.1:6379> set age 18 # 设置年龄
QUEUED
127.0.0.1:6379> lpush age 20 # 此处仅检查是否有语法错误，不会真正执行
QUEUED
127.0.0.1:6379> exec # 提交事务后开始顺序执行命令，第三条命令执行失败
1) OK
2) OK
3) (error) WRONGTYPE Operation against a key holding the wrong kind of value
127.0.0.1:6379> get name # 第三条命令失败没有将前两条命令回滚
"xiaoming"
```
（3）由于乐观锁失败，事务提交时将丢弃之前缓存的所有命令序列。
通过开启两个redis客户端并结合watch命令模拟这种失败场景。

```plain
# 客户端1
127.0.0.1:6379> set name xiaoming # 客户端1设置name
OK
127.0.0.1:6379> watch name # 客户端1通过watch命令给name加乐观锁
OK
# 客户端2
127.0.0.1:6379> get name # 客户端2查询name
"xiaoming"
127.0.0.1:6379> set name zhangsan # 客户端2修改name值
OK
# 客户端1
127.0.0.1:6379> multi # 客户端1开启事务
OK
127.0.0.1:6379> set name lisi # 客户端1修改name
QUEUED
127.0.0.1:6379> exec # 客户端1提交事务，返回空
(nil)
127.0.0.1:6379> get name # 客户端1查询name，发现name没有被修改为lisi
"zhangsan"
```
在事务过程中监控的key被其他客户端改变，则当前客户端的乐观锁失败，事务提交时将丢弃所有命令缓存队列。
## 5. Redis事务相关命令

### （1）WATCH

可以为Redis事务提供 check-and-set （CAS）行为。被WATCH的键会被监视，并会发觉这些键是否被改动过了。 如果有至少一个被监视的键在 EXEC 执行之前被修改了， 那么整个事务都会被取消， EXEC 返回nil-reply来表示事务已经失败。

### （2）MULTI

用于开启一个事务，它总是返回OK。MULTI执行之后,客户端可以继续向服务器发送任意多条命令， 这些命令不会立即被执行，而是被放到一个队列中，当 EXEC命令被调用时， 所有队列中的命令才会被执行。

### （3）UNWATCH

取消 WATCH 命令对所有 key 的监视，一般用于DISCARD和EXEC命令之前。如果在执行 WATCH 命令之后， EXEC 命令或 DISCARD 命令先被执行了的话，那么就不需要再执行 UNWATCH 了。因为 EXEC 命令会执行事务，因此 WATCH 命令的效果已经产生了；而 DISCARD 命令在取消事务的同时也会取消所有对 key 的监视，因此这两个命令执行之后，就没有必要执行 UNWATCH 了。

### （4）DISCARD

当执行 DISCARD 命令时， 事务会被放弃， 事务队列会被清空，并且客户端会从事务状态中退出。

### （5）EXEC

负责触发并执行事务中的所有命令：

如果客户端成功开启事务后执行EXEC，那么事务中的所有命令都会被执行。

如果客户端在使用MULTI开启了事务后，却因为断线而没有成功执行EXEC,那么事务中的所有命令都不会被执行。需要特别注意的是：即使事务中有某条/某些命令执行失败了，事务队列中的其他命令仍然会继续执行，Redis不会停止执行事务中的命令，而不会像我们通常使用的关系型数据库一样进行回滚。

# Redis持久化策略

## 什么是持久化？

持久化（Persistence），即把数据（如内存中的对象）保存到可永久保存的存储设备中（如磁盘）。持久化的主要应用是将内存中的对象存储在数据库中，或者存储在磁盘文件中、XML数据文件中等等。

<div align="center">  <img src="https://uploader.shimo.im/f/a4vVKqLuU7ABjtJE.png!thumbnail" width=""/> </div><br>

还可以从如下两个层面简单的理解持久化 ：

* 应用层：如果关闭(shutdown)你的应用然后重新启动则先前的数据依然存在。
* 系统层：如果关闭(shutdown)你的系统（电脑）然后重新启动则先前的数据依然存在。
## Redis为什么要持久化？

Redis是内存数据库，为了保证效率所有的操作都是在内存中完成。数据都是缓存在内存中，当你重启系统或者关闭系统，之前缓存在内存中的数据都会丢失再也不能找回。因此为了避免这种情况，Redis需要实现持久化将内存中的数据存储起来。

## Redis如何实现持久化？

Redis官方提供了不同级别的持久化方式：

* RDB持久化：能够在指定的时间间隔能对你的数据进行快照存储。
* AOF持久化：记录每次对服务器写的操作，当服务器重启的时候会重新执行这些命令来恢复原始的数据，AOF命令以redis协议追加保存每次写的操作到文件末尾。Redis还能对AOF文件进行后台重写，使得AOF文件的体积不至于过大。
* 不使用持久化：如果你只希望你的数据在服务器运行的时候存在，你也可以选择不使用任何持久化方式。
* 同时开启RDB和AOF：你也可以同时开启两种持久化方式，在这种情况下当redis重启的时候会优先载入AOF文件来恢复原始的数据，因为在通常情况下AOF文件保存的数据集要比RDB文件保存的数据集要完整。

这么多持久化方式我们应该怎么选？在选择之前我们需要搞清楚每种持久化方式的区别以及各自的优劣势。

## RDB持久化

RDB(Redis Database)持久化是把当前内存数据生成快照保存到硬盘的过程，触发RDB持久化过程分为**手动触发**和**自动触发**。

（1）手动触发

手动触发对应save命令，会阻塞当前Redis服务器，直到RDB过程完成为止，对于内存比较大的实例会造成长时间阻塞，线上环境不建议使用。

（2）自动触发

自动触发对应bgsave命令，Redis进程执行fork操作创建子进程，RDB持久化过程由子进程负责，完成后自动结束。阻塞只发生在fork阶段，一般时间很短。

在redis.conf配置文件中可以配置：

```plain
save <seconds> <changes>
```
表示xx秒内数据修改xx次时自动触发bgsave。
如果想关闭自动触发，可以在save命令后面加一个空串，即：

```plain
save ""
```
还有其他常见可以触发bgsave，如：
* 如果从节点执行全量复制操作，主节点自动执行bgsave生成RDB文件并发送给从节点。
* 默认情况下执行shutdown命令时，如果没有开启AOF持久化功能则 自动执行bgsave。

**bgsave工作机制**

<div align="center">  <img src="https://uploader.shimo.im/f/0xV52bqdNVfJ7fKB.png!thumbnail" width="300"/> </div><br>

（1）执行bgsave命令，Redis父进程判断当前是否存在正在执行的子进 程，如RDB/AOF子进程，如果存在，bgsave命令直接返回。

（2）父进程执行fork操作创建子进程，fork操作过程中父进程会阻塞，通 过info stats命令查看latest_fork_usec选项，可以获取最近一个fork操作的耗时，单位为微秒

（3）父进程fork完成后，bgsave命令返回“Background saving started”信息并不再阻塞父进程，可以继续响应其他命令。

（4）子进程创建RDB文件，根据父进程内存生成临时快照文件，完成后对原有文件进行原子替换。执行lastsave命令可以获取最后一次生成RDB的 时间，对应info统计的rdb_last_save_time选项。

（5）进程发送信号给父进程表示完成，父进程更新统计信息，具体见 info Persistence下的rdb_*相关选项。

## AOF持久化

AOF（append only file）持久化：以独立日志的方式记录每次写命令， 重启时再重新执行AOF文件中的命令达到恢复数据的目的。AOF的主要作用是解决了数据持久化的实时性，目前已经是Redis持久化的主流方式。

**AOF持久化工作机制**

开启AOF功能需要配置：appendonly yes，默认不开启。

AOF文件名 通过appendfilename配置设置，默认文件名是appendonly.aof。保存路径同 RDB持久化方式一致，通过dir配置指定。

AOF的工作流程操作：命令写入 （append）、文件同步（sync）、文件重写（rewrite）、重启加载 （load）。

<div align="center">  <img src="https://uploader.shimo.im/f/UDiS8d0jjBKhsR3W.png!thumbnail" width="200"/> </div><br>

（1）所有的写入命令会追加到aof_buf（缓冲区）中。

（2）AOF缓冲区根据对应的策略向硬盘做同步操作。

AOF为什么把命令追加到aof_buf中？Redis使用单线程响应命令，如果每次写AOF文件命令都直接追加到硬盘，那么性能完全取决于当前硬盘负载。先写入缓冲区aof_buf中，还有另一个好处，Redis可以提供多种缓冲区同步硬盘的策略，在性能和安全性方面做出平衡。

（3）随着AOF文件越来越大，需要定期对AOF文件进行重写，达到压缩的目的。

（4）当Redis服务器重启时，可以加载AOF文件进行数据恢复。

**AOF重写（rewrite）机制**

重写的目的：

* 减小AOF文件占用空间；
* 更小的AOF 文件可以更快地被Redis加载恢复。

AOF重写可以分为手动触发和自动触发：

* 手动触发：直接调用bgrewriteaof命令。
* 自动触发：根据auto-aof-rewrite-min-size和auto-aof-rewrite-percentage参数确定自动触发时机。

auto-aof-rewrite-min-size：表示运行AOF重写时文件最小体积，默认 为64MB。

auto-aof-rewrite-percentage：代表当前AOF文件空间 （aof_current_size）和上一次重写后AOF文件空间（aof_base_size）的比值。

自动触发时机

当aof_current_size>auto-aof-rewrite-minsize 并且（aof_current_size-aof_base_size）/aof_base_size>=auto-aof-rewritepercentage。

其中aof_current_size和aof_base_size可以在info Persistence统计信息中查看。

<div align="center">  <img src="https://uploader.shimo.im/f/debsx48n43gNlnEp.png!thumbnail" width="300"/> </div><br>

AOF文件重写后为什么会变小？

（1）旧的AOF文件含有无效的命令，如：del key1， hdel key2等。重写只保留最终数据的写入命令。

（2）多条命令可以合并，如lpush list a，lpush list b，lpush list c可以直接转化为lpush list a b c。

** AOF文件数据恢复**

<div align="center">  <img src="https://uploader.shimo.im/f/YwlQ7wtH4Hpm7EWP.png!thumbnail" width="300"/> </div><br>

数据恢复流程说明：

（1）AOF持久化开启且存在AOF文件时，优先加载AOF文件。

（2）AOF关闭或者AOF文件不存在时，加载RDB文件。

（3）加载AOF/RDB文件成功后，Redis启动成功。

（4）AOF/RDB文件存在错误时，Redis启动失败并打印错误信息。

## RDB和AOF的优缺点

**RDB优点**

* RDB 是一个非常紧凑的文件,它保存了某个时间点的数据集,非常适用于数据集的备份,比如你可以在每个小时报保存一下过去24小时内的数据,同时每天保存过去30天的数据,这样即使出了问题你也可以根据需求恢复到不同版本的数据集。
* RDB 是一个紧凑的单一文件,很方便传送到另一个远端数据中心，非常适用于灾难恢复。
* RDB 在保存 RDB 文件时父进程唯一需要做的就是 fork 出一个子进程,接下来的工作全部由子进程来做，父进程不需要再做其他 IO 操作，所以 RDB 持久化方式可以最大化 Redis 的性能。
* 与AOF相比,在恢复大的数据集的时候，RDB 方式会更快一些。

**AOF优点**

* 你可以使用不同的 fsync 策略：无 fsync、每秒 fsync 、每次写的时候 fsync .使用默认的每秒 fsync 策略, Redis 的性能依然很好( fsync 是由后台线程进行处理的,主线程会尽力处理客户端请求),一旦出现故障，你最多丢失1秒的数据。
* AOF文件是一个只进行追加的日志文件,所以不需要写入seek,即使由于某些原因(磁盘空间已满，写的过程中宕机等等)未执行完整的写入命令,你也也可使用redis-check-aof工具修复这些问题。
* Redis 可以在 AOF 文件体积变得过大时，自动地在后台对 AOF 进行重写： 重写后的新 AOF 文件包含了恢复当前数据集所需的最小命令集合。 整个重写操作是绝对安全的，因为 Redis 在创建新 AOF 文件的过程中，会继续将命令追加到现有的 AOF 文件里面，即使重写过程中发生停机，现有的 AOF 文件也不会丢失。 而一旦新 AOF 文件创建完毕，Redis 就会从旧 AOF 文件切换到新 AOF 文件，并开始对新 AOF 文件进行追加操作。
* AOF 文件有序地保存了对数据库执行的所有写入操作， 这些写入操作以 Redis 协议的格式保存， 因此 AOF 文件的内容非常容易被人读懂， 对文件进行分析（parse）也很轻松。 导出（export） AOF 文件也非常简单： 举个例子， 如果你不小心执行了 FLUSHALL 命令， 但只要 AOF 文件未被重写， 那么只要停止服务器， 移除 AOF 文件末尾的 FLUSHALL 命令， 并重启 Redis ， 就可以将数据集恢复到 FLUSHALL 执行之前的状态。

**RDB缺点**

* Redis 要完整的保存整个数据集是一个比较繁重的工作,你通常会每隔5分钟或者更久做一次完整的保存,万一在 Redis 意外宕机,你可能会丢失几分钟的数据。
* RDB 需要经常 fork 子进程来保存数据集到硬盘上,当数据集比较大的时候, fork 的过程是非常耗时的,可能会导致 Redis 在一些毫秒级内不能响应客户端的请求。

**AOF缺点**

* 对于相同的数据集来说，AOF 文件的体积通常要大于 RDB 文件的体积。
* 数据恢复（load）时AOF比RDB慢，通常RDB 可以提供更有保证的最大延迟时间。

**RDB和AOF简单对比总结**

RDB优点：

* RDB 是紧凑的二进制文件，比较适合备份，全量复制等场景
* RDB 恢复数据远快于 AOF

RDB缺点：

* RDB 无法实现实时或者秒级持久化；
* 新老版本无法兼容 RDB 格式。

AOF优点：

* 可以更好地保护数据不丢失；
* appen-only 模式写入性能比较高；
* 适合做灾难性的误删除紧急恢复。

AOF缺点：

* 对于同一份文件，AOF 文件要比 RDB 快照大；
* AOF 开启后，会对写的 QPS 有所影响，相对于 RDB 来说 写 QPS 要下降；
* 数据库恢复比较慢， 不合适做冷备。
# Redis内存淘汰策略

## 什么是淘汰策略？

Redis内存淘汰策略是指当缓存内存不足时，通过淘汰旧数据处理新加入数据选择的策略。

## 如何配置最大内存？

**通过配置文件配置**

修改redis.conf配置文件

```plain
maxmemory 1024mb //设置Redis最大占用内存大小为1024M
```
注意：maxmemory默认配置为0，在64位操作系统下redis最大内存为操作系统剩余内存，在32位操作系统下redis最大内存为3GB。
**通过动态命令配置**

Redis支持运行时通过命令动态修改内存大小：

```plain
127.0.0.1:6379> config set maxmemory 200mb //设置Redis最大占用内存大小为200M
127.0.0.1:6379> config get maxmemory //获取设置的Redis能使用的最大内存大小
1) "maxmemory"
2) "209715200"
```
## 淘汰策略的分类

Redis最大占用内存用完之后，如果继续添加数据，如何处理这种情况呢？实际上Redis官方已经定义了八种策略来处理这种情况：

### noeviction

默认策略，对于写请求直接返回错误，不进行淘汰。

### allkeys-lru

lru(less recently used), 最近最少使用。从所有的key中使用近似LRU算法进行淘汰。

### volatile-lru

lru(less recently used), 最近最少使用。从设置了过期时间的key中使用近似LRU算法进行淘汰。

### allkeys-random

从所有的key中随机淘汰。

### volatile-random

从设置了过期时间的key中随机淘汰。

### volatile-ttl

ttl(time to live)，在设置了过期时间的key中根据key的过期时间进行淘汰，越早过期的越优先被淘汰。

### allkeys-lfu

lfu(Least Frequently Used)，最少使用频率。从所有的key中使用近似LFU算法进行淘汰。从Redis4.0开始支持。

### volatile-lfu

lfu(Least Frequently Used)，最少使用频率。从设置了过期时间的key中使用近似LFU算法进行淘汰。从Redis4.0开始支持。

注意：当使用volatile-lru、volatile-random、volatile-ttl这三种策略时，如果没有设置过期的key可以被淘汰，则和noeviction一样返回错误。

## LRU算法

LRU(Least Recently Used)，即最近最少使用，是一种缓存置换算法。在使用内存作为缓存的时候，缓存的大小一般是固定的。当缓存被占满，这个时候继续往缓存里面添加数据，就需要淘汰一部分老的数据，释放内存空间用来存储新的数据。这个时候就可以使用LRU算法了。其核心思想是：如果一个数据在最近一段时间没有被用到，那么将来被使用到的可能性也很小，所以就可以被淘汰掉。

**LRU在Redis中的实现**

Redis使用的是近似LRU算法，它跟常规的LRU算法还不太一样。近似LRU算法通过随机采样法淘汰数据，每次随机出5个（默认）key，从里面淘汰掉最近最少使用的key。

可以通过maxmemory-samples参数修改采样数量， 如：maxmemory-samples 10

maxmenory-samples配置的越大，淘汰的结果越接近于严格的LRU算法，但因此耗费的CPU也很高。

Redis为了实现近似LRU算法，给每个key增加了一个额外增加了一个24bit的字段，用来存储该key最后一次被访问的时间。

**Redis3.0对近似LRU的优化**

Redis3.0对近似LRU算法进行了一些优化。新算法会维护一个候选池（大小为16），池中的数据根据访问时间进行排序，第一次随机选取的key都会放入池中，随后每次随机选取的key只有在访问时间小于池中最小的时间才会放入池中，直到候选池被放满。当放满后，如果有新的key需要放入，则将池中最后访问时间最大（最近被访问）的移除。

当需要淘汰的时候，则直接从池中选取最近访问时间最小（最久没被访问）的key淘汰掉就行。

## LFU算法

LFU(Least Frequently Used)，是Redis4.0新加的一种淘汰策略，它的核心思想是根据key的最近被访问的频率进行淘汰，很少被访问的优先被淘汰，被访问的多的则被留下来。

LFU算法能更好的表示一个key被访问的热度。假如你使用的是LRU算法，一个key很久没有被访问到，只刚刚是偶尔被访问了一次，那么它就被认为是热点数据，不会被淘汰，而有些key将来是很有可能被访问到的则被淘汰了。如果使用LFU算法则不会出现这种情况，因为使用一次并不会使一个key成为热点数据。

# Redis内存失效策略

Redis的key一般会设置一个过期时间，等过期之后Redis会从内存清除这些key，如何清除？一般有三种策略：定时清除、惰性清除、定时扫描清除。

## 定时清除（主动）

每个设置过期时间的key都需要创建一个定时器，到过期时间就会立即清除。

该策略可以立即清除过期的数据，对内存很友好，但是会占用大量的CPU资源去处理过期的数据，从而影响缓存的响应时间和吞吐量。

## 惰性清除（被动）

当key过期之后不会立即从内存清除，只有当访问一个key时，才会判断该key是否已过期，如果过期则清除并返回空。

该策略可以最大化地节省CPU资源，却对内存非常不友好。极端情况可能出现大量的过期key没有再次被访问，从而不会被清除，占用大量内存。

## 定期扫描清除（主动）

每隔一定的时间会扫描一定数量的数据库的expires字典中一定数量的key，并清除其中已过期的key。该策略是前两者的一个折中方案。通过调整定时扫描的时间间隔和每次扫描的限定耗时，可以在不同情况下使得CPU和内存资源达到最优的平衡效果。

**Redis中同时使用了惰性清除和定期扫描清除两种策略。**

所谓定期扫描清除，指的是 redis 默认每隔 100ms 就随机抽取一些设置了过期时间的 key，检查其是否过期，如果过期就删除。

假设 redis 里放了 10w 个 key，都设置了过期时间，你每隔几百毫秒，就检查 10w 个 key，那 redis 基本上就死了，cpu 负载会很高的，消耗在你的检查过期 key 上了。注意，这里可不是每隔 100ms 就遍历所有的设置过期时间的 key，那样就是一场性能上的灾难。实际上 redis 是每隔 100ms 随机抽取一些 key 来检查和删除的。

但是问题是，定期删除可能会导致很多过期 key 到了时间并没有被删除掉，那咋整呢？所以就是惰性删除了。这就是说，在你获取某个 key 的时候，redis 会检查一下 ，这个 key 如果设置了过期时间那么是否过期了？如果过期了此时就会删除，不会给你返回任何东西。

获取 key 的时候，如果此时 key 已经过期，就删除，不会返回任何东西。但是实际上这还是有问题的，如果定期删除漏掉了很多过期 key，然后你也没及时去查，也就没走惰性删除，此时会怎么样？如果大量过期 key 堆积在内存里，导致 redis 内存块耗尽了，咋整？答案是：走内存淘汰机制。

# 缓存更新策略

缓存更新有三种常用策略：

* Cache aside
* Read/Write through
* Write behind caching
## Cache aside（旁路缓存）

Cache aside最常用的缓存策略，数据请求的过程如下：

（1）如果是数据读请求，应用首先会判断缓存是否有该数据，缓存命中直接返回数据，缓存未命中即缓存穿透到数据库，从数据库查询数据然后回写到缓存中，最后返回数据给客户端。

（2）如果是数据写请求，首先更新数据库，然后从缓存中删除该数据。

详细流程可以结合以下流程图：

<div align="center">  <img src="https://uploader.shimo.im/f/TzJkbpWzf7ptDDC8.jpg!thumbnail" width="300"/> </div><br>

仔细看上面的流程可以发现，读请求常用的套路是先更新缓存再删缓存，有些同学可能要问为什么要删缓存，先更新数据库再更新缓存行不行？先更新缓存再更新数据库行不行？这里就涉及到几个坑，下面一一解读。

## Cache aside踩坑

Cache aside策略如果用错就会遇到深坑，下面我们来逐个踩。

### 踩坑一：先更新数据库，再更新缓存

如果一个写请求来了我们先更新数据库再更新缓存，在两个并发写请求下可能会导致脏数据。

<div align="center">  <img src="https://uploader.shimo.im/f/ehSYGQsCETtKraIy.jpg!thumbnail" width="400"/> </div><br>

请求1先更新数据库，请求2后更新数据库，预期结果是数据库中age为20，缓存中age为20，但是由于请求1比请求2后更新缓存，结果导致缓存中age为18，造成了数据库与缓存不一致，缓存中age为脏数据。

### 踩坑二：先删缓存，再更新数据库

如果写请求的处理流程是先删缓存再更新数据库，在一个读请求和一个写请求并发场景下可能会出现脏数据。

<div align="center">  <img src="https://uploader.shimo.im/f/vvAqgHH9ebifdJ2r.png!thumbnail" width="400"/> </div><br>

流程如下：

（1）写请求删除缓存数据；

（2）读请求查询缓存未击中，紧接着查询数据库，将返回的数据回写到缓存中；

（3）写请求更新数据库。

整个流程下来发现数据库中age为20，缓存中age为18，缓存和数据库数据不一致，缓存出现了脏数据。

### 最佳实践：先更新数据库，再删除缓存

在实际的系统中针对写请求推荐这种操作，但是在理论上还是存在问题。

<div align="center">  <img src="https://uploader.shimo.im/f/v7kTeZLnlo7Uol5c.png!thumbnail" width="400"/> </div><br>

流程如下：

（1）读请求先查询缓存，缓存未击中，查询数据库返回数据；

（2）写请求更新数据库，删除缓存；

（3）读请求回写缓存；

整个流程操作下来发现数据库age为20，缓存age为18，即数据库与缓存不一致，导致应用程序从缓存中读到的数据都为旧数据。

但其实上述问题发生的概率非常低，因为通常数据库更新操作比内存操作耗时多出几个数量级。如上图中最后一步回写缓存通常会在更新数据库之前完成。但是为了避免这种极端情况造成脏数据所产生的影响，我们还是要为缓存设置过期时间。

## Read through

在 Cache Aside 更新模式中，应用代码需要维护两个数据存储，一个是缓存，一个是数据库。而在 Read-Through 策略下，应用程序无需管理缓存和数据库，只需要将数据库的同步委托给缓存提供程序 Cache Provider 即可。所有数据交互都是通过抽象缓存层完成的。

<div align="center">  <img src="https://uploader.shimo.im/f/GnwGEAZVgi9sQEp5.png!thumbnail" width="400"/> </div><br>

在进行大量读取时，Read-Through 可以减少数据源上的负载，也对缓存服务的故障具备一定的弹性。如果缓存服务挂了，则缓存提供程序仍然可以通过直接转到数据源来进行操作。

Read-Through 适用于多次请求相同数据的场景。这与 Cache-Aside 策略非常相似，但是二者还是存在一些差别，这里再次强调一下：

* 在 Cache-Aside 中，应用程序负责从数据源中获取数据并更新到缓存。
* 在 Read-Through 中，此逻辑通常是由独立的缓存提供程序支持。
## Write through

Write-Through 策略下，当发生数据更新(Write)时，缓存提供程序 Cache Provider 负责更新底层数据源和缓存。缓存与数据源保持一致，并且写入时始终通过抽象缓存层到达数据源。

<div align="center">  <img src="https://uploader.shimo.im/f/Oypr2oPE23Kwe70P.png!thumbnail" width="500"/> </div><br>

## Write behind

数据更新时只更新缓存，每隔一段时间将数据刷新到数据库中。

<div align="center">  <img src="https://uploader.shimo.im/f/VT44gPVsVacJFBTk.png!thumbnail" width="500"/> </div><br>

优点是数据写入速度非常快，适用于频繁写的场景。

缺点是缓存和数据库非强一致性。

# 缓存异常场景

在实际生产环境中有时会遇到缓存穿透、缓存击穿、缓存雪崩等异常场景，为了避免异常带来巨大损失，我们需要了解每种异常发生的原因以及解决方案，帮助提升系统可靠性和高可用。

## 缓存穿透

### 什么是缓存穿透？

缓存穿透是指用户请求的数据在缓存中不存在即没有命中，同时在数据库中也不存在，导致用户每次请求该数据都要去数据库中查询一遍，然后返回空。

如果有恶意攻击者不断请求系统中不存在的数据，会导致短时间大量请求落在数据库上，造成数据库压力过大，甚至击垮数据库系统。

### 缓存穿透常用的解决方案

**（1）布隆过滤器（推荐）**

布隆过滤器（Bloom Filter，简称BF）由Burton Howard Bloom在1970年提出，是一种空间效率高的概率型数据结构。

**布隆过滤器专门用来检测集合中是否存在特定的元素。**

如果在平时我们要判断一个元素是否在一个集合中，通常会采用查找比较的方法，下面分析不同的数据结构查找效率：

* 采用线性表存储，查找时间复杂度为O(N)
* 采用平衡二叉排序树（AVL、红黑树）存储，查找时间复杂度为O(logN)
* 采用哈希表存储，考虑到哈希碰撞，整体时间复杂度也要O[log(n/m)]

当需要判断一个元素是否存在于海量数据集合中，不仅查找时间慢，还会占用大量存储空间。接下来看一下布隆过滤器如何解决这个问题。

**布隆过滤器设计思想**

布隆过滤器由一个长度为m比特的位数组（bit array）与k个哈希函数（hash function）组成的数据结构。位数组初始化均为0，所有的哈希函数都可以分别把输入数据尽量均匀地散列。

当要向布隆过滤器中插入一个元素时，该元素经过k个哈希函数计算产生k个哈希值，以哈希值作为位数组中的下标，将所有k个对应的比特值由0置为1。

当要查询一个元素时，同样将其经过哈希函数计算产生哈希值，然后检查对应的k个比特值：如果有任意一个比特为0，表明该元素一定不在集合中；如果所有比特均为1，表明该集合有可能性在集合中。为什么不是一定在集合中呢？因为不同的元素计算的哈希值有可能一样，会出现哈希碰撞，导致一个不存在的元素有可能对应的比特位为1，这就是所谓“假阳性”（false positive）。相对地，“假阴性”（false negative）在BF中是绝不会出现的。

总结一下：布隆过滤器认为不在的，一定不会在集合中；布隆过滤器认为在的，可能在也可能不在集合中。

举个例子：下图是一个布隆过滤器，共有18个比特位，3个哈希函数。集合中三个元素x，y，z通过三个哈希函数散列到不同的比特位，并将比特位置为1。当查询元素w时，通过三个哈希函数计算，发现有一个比特位的值为0，可以肯定认为该元素不在集合中。

<div align="center">  <img src="https://uploader.shimo.im/f/2hd9rhfvp8QyexvO.png!thumbnail" width="500"/> </div><br>

**布隆过滤器优缺点**

优点：

* 节省空间：不需要存储数据本身，只需要存储数据对应hash比特位
* 时间复杂度低：插入和查找的时间复杂度都为O(k)，k为哈希函数的个数

缺点：

* 存在假阳性：布隆过滤器判断存在，可能出现元素不在集合中；判断准确率取决于哈希函数的个数
* 不能删除元素：如果一个元素被删除，但是却不能从布隆过滤器中删除，这也是造成假阳性的原因了

**布隆过滤器适用场景**

* 爬虫系统url去重
* 垃圾邮件过滤
* 黑名单

**（2）返回空对象**

当缓存未命中，查询持久层也为空，可以将返回的空对象写到缓存中，这样下次请求该key时直接从缓存中查询返回空对象，请求不会落到持久层数据库。为了避免存储过多空对象，通常会给空对象设置一个过期时间。

这种方法会存在两个问题：

* 如果有大量的key穿透，缓存空对象会占用宝贵的内存空间。
* 空对象的key设置了过期时间，在这段时间可能会存在缓存和持久层数据不一致的场景。
## 缓存击穿

### 什么是缓存击穿？

缓存击穿，是指一个key非常热点，在不停的扛着大并发，大并发集中对这一个点进行访问，当这个key在失效的瞬间，持续的大并发就穿破缓存，直接请求数据库，就像在一个屏障上凿开了一个洞。

### 缓存击穿危害

数据库瞬时压力骤增，造成大量请求阻塞。

### 如何解决

**使用互斥锁（mutex key）**

这种思路比较简单，就是让一个线程回写缓存，其他线程等待回写缓存线程执行完，重新读缓存即可。

<div align="center">  <img src="https://uploader.shimo.im/f/l1i9taQTV2iNv8qm.png!thumbnail" width="500"/> </div><br>

同一时间只有一个线程读数据库然后回写缓存，其他线程都处于阻塞状态。如果是高并发场景，大量线程阻塞势必会降低吞吐量。这种情况如何解决？大家可以在留言区讨论。

如果是分布式应用就需要使用分布式锁。

**热点数据永不过期**

永不过期实际包含两层意思：

* 物理不过期，针对热点key不设置过期时间
* 逻辑过期，把过期时间存在key对应的value里，如果发现要过期了，通过一个后台的异步线程进行缓存的构建

<div align="center">  <img src="https://uploader.shimo.im/f/FMBCoGpJV86UMniL.png!thumbnail" width="500"/> </div><br>

从实战看这种方法对于性能非常友好，唯一不足的就是构建缓存时候，其余线程(非构建缓存的线程)可能访问的是老数据，对于不追求严格强一致性的系统是可以接受的。

## 缓存雪崩

### 什么是缓存雪崩？

缓存雪崩是指缓存中数据大批量到过期时间，而查询数据量巨大，请求直接落到数据库上，引起数据库压力过大甚至宕机。和缓存击穿不同的是，缓存击穿指并发查同一条数据，缓存雪崩是不同数据都过期了，很多数据都查不到从而查数据库。

### 缓存雪崩解决方案

常用的解决方案有：

* 均匀过期
* 加互斥锁
* 缓存永不过期
* 双层缓存策略

（1）均匀过期

设置不同的过期时间，让缓存失效的时间点尽量均匀。通常可以为有效期增加随机值或者统一规划有效期。

（2）加互斥锁

跟缓存击穿解决思路一致，同一时间只让一个线程构建缓存，其他线程阻塞排队。

（3）缓存永不过期

跟缓存击穿解决思路一致，缓存在物理上永远不过期，用一个异步的线程更新缓存。

（4）双层缓存策略

使用主备两层缓存：

主缓存：有效期按照经验值设置，设置为主读取的缓存，主缓存失效后从数据库加载最新值。

备份缓存：有效期长，获取锁失败时读取的缓存，主缓存更新时需要同步更新备份缓存。

## 缓存预热

### 什么是缓存预热？

缓存预热就是系统上线后，将相关的缓存数据直接加载到缓存系统，这样就可以避免在用户请求的时候，先查询数据库，然后再将数据回写到缓存。

如果不进行预热， 那么 Redis 初始状态数据为空，系统上线初期，对于高并发的流量，都会访问到数据库中， 对数据库造成流量的压力。

### 缓存预热的操作方法

* 数据量不大的时候，工程启动的时候进行加载缓存动作；
* 数据量大的时候，设置一个定时任务脚本，进行缓存的刷新；
* 数据量太大的时候，优先保证热点数据进行提前加载到缓存。
## 缓存降级

缓存降级是指缓存失效或缓存服务器挂掉的情况下，不去访问数据库，直接返回默认数据或访问服务的内存数据。

在项目实战中通常会将部分热点数据缓存到服务的内存中，这样一旦缓存出现异常，可以直接使用服务的内存数据，从而避免数据库遭受巨大压力。

降级一般是有损的操作，所以尽量减少降级对于业务的影响程度。

# 高可用架构

## Replication（主从复制）

### 什么是主从复制？

主从复制，是指将一台Redis服务器的数据，复制到其他的Redis服务器。前者称为主节点(master)，后者称为从节点(slave)；数据的复制是单向的，只能由主节点到从节点。

### 主从复制的作用

1. 数据冗余：主从复制实现了数据的热备份，是持久化之外的一种数据冗余方式。
2. 故障恢复：当主节点出现问题时，可以由从节点提供服务，实现快速的故障恢复；实际上是一种服务的冗余。
3. 负载均衡：在主从复制的基础上，配合读写分离，可以由主节点提供写服务，由从节点提供读服务，分担服务器负载；尤其是在写少读多的场景下，通过多个从节点分担读负载，可以大大提高Redis服务器的并发量。
4. 高可用基石：主从复制还是哨兵和集群能够实施的基础，因此说主从复制是Redis高可用的基础。
### 主从复制实现原理

主从复制过程主要可以分为3个阶段：连接建立阶段、数据同步阶段、命令传播阶段。

#### 连接建立阶段

该阶段的主要作用是在主从节点之间建立连接，为数据同步做好准备。

**步骤1：保存主节点信息**

slaveof命令是异步的，在从节点上执行slaveof命令，从节点立即向客户端返回ok，从节点服务器内部维护了两个字段，即masterhost和masterport字段，用于存储主节点的ip和port信息。

**步骤2：建立socket连接**

从节点每秒1次调用复制定时函数replicationCron()，如果发现了有主节点可以连接，便会根据主节点的ip和port，创建socket连接。

从节点为该socket建立一个专门处理复制工作的文件事件处理器，负责后续的复制工作，如接收RDB文件、接收命令传播等。

主节点接收到从节点的socket连接后（即accept之后），为该socket创建相应的客户端状态，并将从节点看做是连接到主节点的一个客户端，后面的步骤会以从节点向主节点发送命令请求的形式来进行。

**步骤3：发送ping命令**

从节点成为主节点的客户端之后，发送ping命令进行首次请求，目的是：检查socket连接是否可用，以及主节点当前是否能够处理请求。

从节点发送ping命令后，可能出现3种情况：

（1）返回pong：说明socket连接正常，且主节点当前可以处理请求，复制过程继续。

（2）超时：一定时间后从节点仍未收到主节点的回复，说明socket连接不可用，则从节点断开socket连接，并重连。

（3）返回pong以外的结果：如果主节点返回其他结果，如正在处理超时运行的脚本，说明主节点当前无法处理命令，则从节点断开socket连接，并重连。

**步骤4：身份验证**

如果从节点中设置了masterauth选项，则从节点需要向主节点进行身份验证；没有设置该选项，则不需要验证。从节点进行身份验证是通过向主节点发送auth命令进行的，auth命令的参数即为配置文件中的masterauth的值。

如果主节点设置密码的状态，与从节点masterauth的状态一致（一致是指都存在，且密码相同，或者都不存在），则身份验证通过，复制过程继续；如果不一致，则从节点断开socket连接，并重连。

**步骤5：发送从节点端口信息**

身份验证之后，从节点会向主节点发送其监听的端口号（前述例子中为6380），主节点将该信息保存到该从节点对应的客户端的slave_listening_port字段中；该端口信息除了在主节点中执行info Replication时显示以外，没有其他作用。

#### 数据同步阶段

主从节点之间的连接建立以后，便可以开始进行数据同步，该阶段可以理解为从节点数据的初始化。具体执行的方式是：从节点向主节点发送psync命令（Redis2.8以前是sync命令），开始同步。

数据同步阶段是主从复制最核心的阶段，根据主从节点当前状态的不同，可以分为全量复制和部分复制，后面再讲解这两种复制方式以及psync命令的执行过程，这里不再详述。

#### 命令传播阶段

数据同步阶段完成后，主从节点进入命令传播阶段；在这个阶段主节点将自己执行的写命令发送给从节点，从节点接收命令并执行，从而保证主从节点数据的一致性。

需要注意的是，命令传播是异步的过程，即主节点发送写命令后并不会等待从节点的回复；因此实际上主从节点之间很难保持实时的一致性，延迟在所难免。数据不一致的程度，与主从节点之间的网络状况、主节点写命令的执行频率、以及主节点中的repl-disable-tcp-nodelay配置等有关。

## Sentinel（哨兵模式）

### 为什么要引入哨兵模式？

Redis 的主从复制模式下，一旦主节点由于故障不能提供服务，需要手动将从节点晋升为主节点，同时还要通知客户端更新主节点地址，这种故障处理方式从一定程度上是无法接受的。

Redis 2.8 以后提供了 Redis Sentinel 哨兵机制来解决这个问题。

### 什么是哨兵模式？

Redis Sentinel 是 Redis 高可用的实现方案。Sentinel 是一个管理多个 Redis 实例的工具，它可以实现对 Redis 的监控、通知、自动故障转移。

Redis Sentinel架构图如下：

<div align="center">  <img src="https://uploader.shimo.im/f/WSboj3FYK3kO96X2.png!thumbnail" width="500"/> </div><br>

### 哨兵模式的原理

哨兵模式的主要作用在于它能够自动完成故障发现和故障转移，并通知客户端，从而实现高可用。哨兵模式通常由一组 Sentinel 节点和一组（或多组）主从复制节点组成。

#### 心跳机制

（1）Sentinel与Redis Node

Redis Sentinel 是一个特殊的 Redis 节点。在哨兵模式创建时，需要通过配置指定 Sentinel 与 Redis Master Node 之间的关系，然后 Sentinel 会从主节点上获取所有从节点的信息，之后 Sentinel 会定时向主节点和从节点发送 info 命令获取其拓扑结构和状态信息。



（2）Sentinel与Sentinel

基于 Redis 的订阅发布功能， 每个 Sentinel 节点会向主节点的 __sentinel__：hello 频道上发送该 Sentinel 节点对于主节点的判断以及当前 Sentinel 节点的信息 ，同时每个 Sentinel 节点也会订阅该频道， 来获取其他 Sentinel 节点的信息以及它们对主节点的判断。



通过以上两步所有的 Sentinel 节点以及它们与所有的 Redis 节点之间都已经彼此感知到，之后每个 Sentinel 节点会向主节点、从节点、以及其余 Sentinel 节点定时发送 ping 命令作为心跳检测， 来确认这些节点是否可达。

#### 故障转移

每个 Sentinel 都会定时进行心跳检查，当发现主节点出现心跳检测超时的情况时，此时认为该主节点已经不可用，这种判定称为**主观下线**。

之后该 Sentinel 节点会通过 sentinel ismaster-down-by-addr 命令向其他 Sentinel 节点询问对主节点的判断， 当 quorum（法定人数） 个 Sentinel 节点都认为该节点故障时，则执行**客观下线**，即认为该节点已经不可用。这也同时解释了为什么必须需要一组 Sentinel 节点，因为单个 Sentinel 节点很容易对故障状态做出误判。



>这里 quorum 的值是我们在哨兵模式搭建时指定的，后文会有说明，通常为 Sentinel节点总数/2+1，即半数以上节点做出主观下线判断就可以执行客观下线。



因为故障转移的工作只需要一个 Sentinel 节点来完成，所以 Sentinel 节点之间会再做一次选举工作， 基于 Raft 算法选出一个 Sentinel 领导者来进行故障转移的工作。

被选举出的 Sentinel 领导者进行故障转移的具体步骤如下：

（1）在从节点列表中选出一个节点作为新的主节点

* 过滤不健康或者不满足要求的节点；
* 选择 slave-priority（优先级）最高的从节点， 如果存在则返回， 不存在则继续；
* 选择复制偏移量最大的从节点 ， 如果存在则返回， 不存在则继续；
* 选择 runid 最小的从节点。

（2）Sentinel 领导者节点会对选出来的从节点执行 slaveof no one 命令让其成为主节点。

（3）Sentinel 领导者节点会向剩余的从节点发送命令，让他们从新的主节点上复制数据。

（4）Sentinel 领导者会将原来的主节点更新为从节点， 并对其进行监控， 当其恢复后命令它去复制新的主节点。

## Cluster（集群）

### 为什么要引入Cluster模式？

不管是主从模式还是哨兵模式都只能由一个master在写数据，在海量数据高并发场景，一个节点写数据容易出现瓶颈，引入Cluster模式可以实现多个节点同时写数据。

### 什么是Cluster模式？

Redis-Cluster采用无中心结构，每个节点都保存数据，节点之间互相连接从而知道整个集群状态。

<div align="center">  <img src="https://uploader.shimo.im/f/PNyUtVh8fsi0Hcan.png!thumbnail" width="500"/> </div><br>

如图所示Cluster模式其实就是多个主从复制的结构组合起来的，每一个主从复制结构可以看成一个节点，那么上面的Cluster集群中就有三个节点。

### Cluster模式的原理

#### Redis集群TCP端口

每个Redis集群节点都需要开启两个TCP监听端口，一个用于给客户端提供普通的Redis服务，常见为6379，另外一个用于集群间通信服务，一般为在普通端口基础上偏移10000如16379。

第二个端口用于集群总线，即使用二进制协议的节点到节点的通信通道。节点使用集群总线进行**故障检测**，**配置更新**，**故障转移授权**等。客户端永远不应尝试与集群总线端口通信，但始终使用正常的Redis命令端口，但请确保在防火墙中打开两个端口，否则Redis集群节点将无法通信。

#### Redis集群数据分片


# 常见应用场景

todo

# 实战篇

## 使用docker搭建redis主从复制集群

### 0. 目标

本地搭建三个redis实例（一主两备），实现效果：主实例插入数据备实例可以复制同步过去。

### 1. 安装docker，运行docker

docker安装步骤省略，大家可以从官网下载并安装。

检查docker是否运行成功：

```plain
docker info
```
出现回显表示运行成功，可以做下一步操作了。
### 2. 拉取redis镜像文件

执行以下命令默认拉取tag为latest的官方redis镜像

```plain
docker pull redis
```
### 3. 准备好redis配置文件redis.conf

下载地址：[https://raw.githubusercontent.com/antirez/redis/5.0/redis.conf](https://raw.githubusercontent.com/antirez/redis/5.0/redis.conf)

拷贝为3三份，如：redis01.conf, redis02.conf, redis03.conf

打开所有的配置文件，修改如下配置项：

* 注释只监听本地选项，可以远程连接。#bind 127.0.0.1
* 关闭保护模式 protected-mode no
* 打开AOF持久化开关 appendonly yes
### 4. 启动redis实例

```plain
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
### 5. 配置主从复制集群

检查实例运行状态：

```plain
docker ps
```
回显有三个redis实例即为正常。
查询实例1：redis-server-01 运行的内部ip

```plain
docker inspect redis-server-01
```
通过回显可以看到："IPAddress": "172.17.0.4"
我们将实例1规划为主，另外两个实例自然为备了，通过将主的ip和port配置在备的配置文件中即可实现主从复制的效果。

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
### 6. 测试主从复制效果

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
"ray" # 可以查到，表明从实例已经将主实例的数据同步过来了
```
--- 至此redis主从复制实例搭建和测试完毕，小伙伴们学会了吗。
## 使用docker搭建redis主从复制+哨兵模式

<div align="center">  <img src="https://uploader.shimo.im/f/WSboj3FYK3kO96X2.png!thumbnail" width="500"/> </div><br>

搭建三个Sentinel实例+Redis实例

### 0. 哨兵作用

* 监控（Monitoring）：哨兵会不断地检查主节点和从节点是否运作正常。
* 自动故障转移（Automatic failover）：当主节点不能正常工作时，哨兵会开始自动故障转移操作，它会将失效主节点的其中一个从节点升级为新的主节点，并让其他从节点改为复制新的主节点。
* 配置提供者（Configurationprovider）：客户端在初始化时，通过连接哨兵来获得当前 Redis 服务的主节点地址。
* 通知（Notification）：哨兵可以将故障转移的结果发送给客户端。
### 1. 准备好哨兵配置文件sentinel.conf

官网下载源码包：[https://redis.io/download](https://redis.io/download)解压之后sentinel.conf

拷贝为3三份，如：sentinel01.conf, sentinel02.conf, sentinel03.conf

打开所有的配置文件，修改如下配置项：

* 关闭保护模式 protected-mode no
* 配置redis主实例ip和端口 sentinel monitor mymaster 172.17.0.4 6379 1 -配置日志文件  logfile "sentinel.log"
### 2. 启动sentinel哨兵实例

```plain
# 实例1
docker run -p 26381:26379 -v /your/path/sentinel01.conf:/etc/redis/sentinel.conf -v /your/path/sentinel-data01:/data --name sentinel-01 -d redis redis-sentinel /etc/redis/sentinel.conf
# 实例2
docker run -p 26382:26379 -v /your/path/sentinel02.conf:/etc/redis/sentinel.conf -v /your/path/sentinel-data02:/data --name sentinel-02 -d redis redis-sentinel /etc/redis/sentinel.conf
# 实例3
docker run -p 26383:26379 -v /your/path/sentinel03.conf:/etc/redis/sentinel.conf -v /your/path/sentinel-data03:/data --name sentinel-03 -d redis redis-sentinel /etc/redis/sentinel.conf
```
### 3. 测试哨兵模式

连接哨兵实例1，查询当前状态：

```plain
docker exec -it sentinel-01 redis-cli -p 26379
127.0.0.1:26379> info Sentinel
回显如下
# Sentinel
sentinel_masters:1
sentinel_tilt:0
sentinel_running_scripts:0
sentinel_scripts_queue_length:0
sentinel_simulate_failure_flags:0
master0:name=mymaster,status=ok,address=172.17.0.4:6379,slaves=2,sentinels=3
```
根据以上回显可以看出当前redis-server主实例ip为172.17.0.4
现在模拟一下故障，停掉redis-server主实例，预期哨兵集群会从两个备实例选出一个作为主实例，下面开始测试：

```plain
docker stop redis-server-01 # 停主实例
```
连接哨兵实例1查询当前主实例ip是否变换：
```plain
docker exec -it sentinel-01 redis-cli -p 26379
127.0.0.1:26379> info Sentinel
回显如下
# Sentinel
sentinel_masters:1
sentinel_tilt:0
sentinel_running_scripts:0
sentinel_scripts_queue_length:0
sentinel_simulate_failure_flags:0
master0:name=mymaster,status=ok,address=172.17.0.3:6379,slaves=2,sentinels=3
```
根据回显可以看出主ip已经换为172.17.0.3
--- 至此哨兵模式已经测试完毕。

# 公众号

公众号比Github早一到两天更新，如果大家想要实时关注我更新的文章以及分享的干货，可以关注我的公众号。

<div align="center">  <img src="https://uploader.shimo.im/f/zZcm5ufFQNgAN5q4.jpg!thumbnail" width=""/> </div><br>
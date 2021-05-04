> 本文首发我的微信公众号『[爱笑的架构师](https://mp.weixin.qq.com/s?__biz=MzIwODI1OTk1Nw==&mid=2650321014&idx=1&sn=ad594766b3973bbf5156567849db7c48&chksm=8f09ce6cb87e477a87731ad9281d082a3b52ce3a2af705527b6a683cb341cedb71bc96419d13&token=875646549&lang=zh_CN#rd)』，欢迎大家关注。

<!-- TOC -->

- [什么是持久化？](#什么是持久化)
- [Redis为什么要持久化？](#redis为什么要持久化)
- [Redis如何实现持久化？](#redis如何实现持久化)
- [RDB持久化](#rdb持久化)
- [AOF持久化](#aof持久化)
- [RDB和AOF的优缺点](#rdb和aof的优缺点)

<!-- /TOC -->

## 什么是持久化？

持久化（Persistence），即把数据（如内存中的对象）保存到可永久保存的存储设备中（如磁盘）。持久化的主要应用是将内存中的对象存储在数据库中，或者存储在磁盘文件中、XML数据文件中等等。

<div align="center">  <img src="https://cdn.jsdelivr.net/gh/SmileLionCoder/assets@main/202010/20201025211500.png" width=""/> </div><br>

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

<div align="center">  <img src="https://cdn.jsdelivr.net/gh/SmileLionCoder/assets@main/202010/20201025211619.png" width="300"/> </div><br>

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

<div align="center">  <img src="https://cdn.jsdelivr.net/gh/SmileLionCoder/assets@main/202010/20201025211644.png" width="200"/> </div><br>

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

<div align="center">  <img src="https://cdn.jsdelivr.net/gh/SmileLionCoder/assets@main/202010/20201025211707.png" width="300"/> </div><br>

AOF文件重写后为什么会变小？

（1）旧的AOF文件含有无效的命令，如：del key1， hdel key2等。重写只保留最终数据的写入命令。

（2）多条命令可以合并，如lpush list a，lpush list b，lpush list c可以直接转化为lpush list a b c。

**AOF文件数据恢复**

<div align="center">  <img src="https://cdn.jsdelivr.net/gh/SmileLionCoder/assets@main/202010/20201025211835.png" width="300"/> </div><br>

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
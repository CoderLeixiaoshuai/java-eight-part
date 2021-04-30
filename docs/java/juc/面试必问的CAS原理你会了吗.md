> 本文首发我的微信公众号『[爱笑的架构师](https://mp.weixin.qq.com/s?__biz=MzIwODI1OTk1Nw==&mid=2650324759&idx=1&sn=11908655d1388b44a61904a175a3a09a&chksm=8f09c10db87e481b025e620ecf86bd14ce4ab8b979264a12ac1e63de2e10eaae95eff2e3bf32&token=997683041&lang=zh_CN#rd)』，欢迎大家关注。

在并发编程中我们都知道`i++`操作是非线程安全的，这是因为 `i++`操作不是原子操作。

如何保证原子性呢？常用的方法就是`加锁`。在Java语言中可以使用 `Synchronized`和`CAS`实现加锁效果。

`Synchronized`是悲观锁，线程开始执行第一步就是获取锁，一旦获得锁，其他的线程进入后就会阻塞等待锁。如果不好理解，举个生活中的例子：一个人进入厕所后首先把门锁上（获取锁），然后开始上厕所，这个时候有其他人来了只能在外面等（阻塞），就算再急也没用。上完厕所完事后把门打开（解锁），其他人就可以进入了。

![](https://cdn.jsdelivr.net/gh/smileArchitect/assets/202102/20210424231036.png)

`CAS`是乐观锁，线程执行的时候不会加锁，假设没有冲突去完成某项操作，如果因为冲突失败了就重试，最后直到成功为止。

# 什么是 CAS？

CAS（Compare-And-Swap）是`比较并交换`的意思，它是一条 CPU 并发原语，用于判断内存中某个值是否为预期值，如果是则更改为新的值，这个过程是`原子`的。下面用一个小示例解释一下。

CAS机制当中使用了3个基本操作数：内存地址V，旧的预期值A，计算后要修改后的新值B。

（1）初始状态：在内存地址V中存储着变量值为 1。

![](https://cdn.jsdelivr.net/gh/smileArchitect/assets/202102/20210424204102.png)

 （2）线程1想要把内存地址为 V 的变量值增加1。这个时候对线程1来说，旧的预期值A=1，要修改的新值B=2。

![](https://cdn.jsdelivr.net/gh/smileArchitect/assets/202102/20210424230126.png)

（3）在线程1要提交更新之前，线程2捷足先登了，已经把内存地址V中的变量值率先更新成了2。

![](https://cdn.jsdelivr.net/gh/smileArchitect/assets/202102/20210424210449.png)

（4）线程1开始提交更新，首先将预期值A和内存地址V的实际值比较（Compare），发现A不等于V的实际值，提交失败。

![](https://cdn.jsdelivr.net/gh/smileArchitect/assets/202102/20210424205654.png)

（5）线程1重新获取内存地址 V 的当前值，并重新计算想要修改的新值。此时对线程1来说，A=2，B=3。这个重新尝试的过程被称为`自旋`。如果多次失败会有多次自旋。

![](https://cdn.jsdelivr.net/gh/smileArchitect/assets/202102/20210424210002.png)

（6）线程 1 再次提交更新，这一次没有其他线程改变地址 V 的值。线程1进行Compare，发现预期值 A 和内存地址 V的实际值是相等的，进行 Swap 操作，将内存地址 V 的实际值修改为 B。

![](https://cdn.jsdelivr.net/gh/smileArchitect/assets/202102/20210424230152.png)

总结：更新一个变量的时候，只有当变量的预期值 A 和内存地址 V 中的实际值相同时，才会将内存地址 V 对应的值修改为 B，这整个操作就是`CAS`。

# CAS 基本原理

CAS 主要包括两个操作：`Compare`和`Swap`，有人可能要问了：两个操作能保证是原子性吗？可以的。

CAS 是一种`系统原语`，原语属于操作系统用语，原语由若干指令组成，用于完成某个功能的一个过程，并且原语的执行必须是连续的，在执行过程中不允许被中断，也就是说 CAS 是一条 CPU 的原子指令，由操作系统硬件来保证。

> 在 Intel 的 CPU 中，使用 cmpxchg 指令。

回到 Java 语言，JDK 是在 1.5 版本后才引入 CAS 操作，在`sun.misc.Unsafe`这个类中定义了 CAS 相关的方法。

```java
public final native boolean compareAndSwapObject(Object o, long offset, Object expected, Object x);

public final native boolean compareAndSwapInt(Object o, long offset, int expected, int x);

public final native boolean compareAndSwapLong(Object o, long offset, long expected, long x);
```

可以看到方法被声明为`native`，如果对 C++ 比较熟悉可以自行下载 OpenJDK 的源码查看 unsafe.cpp，这里不再展开分析。

# CAS 在 Java 语言中的应用

在 Java 编程中我们通常不会直接使用到 CAS，都是通过 JDK 封装好的并发工具类来间接使用的，这些并发工具类都在`java.util.concurrent`包中。

> J.U.C 是`java.util.concurrent`的简称，也就是大家常说的 Java 并发编程工具包，面试常考，非常非常重要。

目前 CAS 在 JDK 中主要应用在 J.U.C 包下的 Atomic 相关类中。

![](https://cdn.jsdelivr.net/gh/smileArchitect/assets/202102/20210424220317.png)

比如说 AtomicInteger 类就可以解决 i++ 非原子性问题，通过查看源码可以发现主要是靠 volatile 关键字和 CAS 操作来实现，具体原理和源码分析后面的文章会展开分析。

# CAS 的问题

CAS 不是万能的，也有很多问题。

`敲黑板：CAS有哪些问题，这是面试高频考点，需要重点掌握`。

## 典型 ABA 问题

ABA 是 CAS 操作的一个经典问题，假设有一个变量初始值为 A，修改为 B，然后又修改为 A，这个变量实际被修改过了，但是 CAS 操作可能无法感知到。

如果是整形还好，不会影响最终结果，但如果是对象的引用类型包含了多个变量，引用没有变实际上包含的变量已经被修改，这就会造成大问题。

如何解决？思路其实很简单，在变量前加版本号，每次变量更新了就把版本号加一，结果如下：

![](https://cdn.jsdelivr.net/gh/smileArchitect/assets/202102/20210424222404.png)

最终结果都是 A 但是版本号改变了。

从 JDK 1.5 开始提供了`AtomicStampedReference`类，这个类的 `compareAndSe `方法首先检查`当前引用`是否等于`预期引用`，并且`当前标志`是否等于`预期标志`，如果全部相等，则以原子方式将该引用和该标志的值设置为给定的更新值。

## 自旋开销问题

CAS 出现冲突后就会开始`自旋`操作，如果资源竞争非常激烈，自旋长时间不能成功就会给 CPU 带来非常大的开销。

解决方案：可以考虑限制自旋的次数，避免过度消耗 CPU；另外还可以考虑延迟执行。

## 只能保证单个变量的原子性

当对一个共享变量执行操作时，可以使用 CAS 来保证原子性，但是如果要对多个共享变量进行操作时，CAS 是无法保证原子性的，比如需要将 i 和 j 同时加 1：

i++；j++；

这个时候可以使用 synchronized 进行加锁，有没有其他办法呢？有，将多个变量操作合成一个变量操作。从 JDK1.5 开始提供了`AtomicReference` 类来保证引用对象之间的原子性，你可以把多个变量放在一个对象里来进行CAS操作。

# 有态度的总结

CAS 是 Compare And Swap，是一条 CPU 原语，由操作系统保证原子性。

Java语言从 JDK1.5 版本开始引入 CAS ， 并且是 Java 并发编程J.U.C 包的基石，应用非常广泛。

当然 CAS 也不是万能的，也有很多问题：典型 ABA 问题、自旋开销问题、只能保证单个变量的原子性。

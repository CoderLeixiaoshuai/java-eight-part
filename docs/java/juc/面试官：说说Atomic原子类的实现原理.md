> 本文首发我的微信公众号『[爱笑的架构师](https://mp.weixin.qq.com/s?__biz=MzIwODI1OTk1Nw==&mid=2650325167&idx=1&sn=b71f86a9e25deb9a01721142fbc13902&chksm=8f09beb5b87e37a3efe9b41b2392aafdcf4ee126d4d9c4a5ecc7a39ab876f3498e2296a7e01d&token=1052638001&lang=zh_CN&scene=21#wechat_redirect)』，欢迎大家关注。

<!-- TOC -->

- [线程安全真的是线程的安全吗？](#线程安全真的是线程的安全吗)
- [什么是 Atomic？](#什么是-atomic)
- [实现一个计数器](#实现一个计数器)
- [AtomicInteger 源码分析](#atomicinteger-源码分析)
- [AtomicLong 和 LongAdder 谁更牛？](#atomiclong-和-longadder-谁更牛)
- [总结](#总结)

<!-- /TOC -->

当我们谈论『线程安全』的时候，肯定都会想到 Atomic 类。不错，Atomic 相关类都是线程安全的，在讲 Atomic 类之前我想再聊聊『线程安全』这个概念。

# 线程安全真的是线程的安全吗？

初看『线程安全』这几个字，很容易望文生义，这不就是线程的安全吗？其实不是，线程本身没有好坏，没有『安全的线程』和『不安全的线程』之分，俗话说：人之初性本善，线程天生也是纯洁善良的，真正让线程变坏是因为访问的变量的原因，变量对于操作系统来说其实就是内存块，所以绕了这么一大圈，线程安全称为『内存的安全』可能更为贴切。

简而言之，线程访问的内存决定了这个线程是否是安全的。

变量大致可以分为**局部变量**和**共享变量**，局部变量对于 JVM 来说是栈空间，大家都背过八股文，栈是线程私有的是非共享的，那自然也是内存安全的；共享变量对于 JVM 来说一般是存在于堆上，堆上的东西是所有线程共享的，如果不加任何限制自然是不安全的。

因为线程安全这个概念已经深入人心了，所以后面我们还是用线程安全来表达内存安全的含义。

那如何解决这种`不安全`呢？方法有很多，比如：加锁、Atomic 原子类等。

好了，咱们今天先来看看`Atomic类`。

# 什么是 Atomic？

`Java`从`JDK1.5`开始提供`java.util.concurrent.atomic`包，这里包含了多个原子操作类。原子操作类提供了一个简单、高效、安全的方式去更新一个变量。

<img src="https://cdn.jsdelivr.net/gh/CoderLeixiaoshuai/assets/202102/20210602205949-2021-06-02-20-59-50.png" alt="20210602205949-2021-06-02-20-59-50" height="300">

Atomic 包下的原子操作类有很多，可以大致分为四种类型：

- 原子操作基本类型
- 原子操作数组类型
- 原子操作引用类型
- 原子操作更新属性

 Atomic原子操作类在源码中都使用了`Unsafe类`，`Unsafe类`提供了硬件级别的原子操作，可以安全地直接操作内存变量。后面讲解源码时再详细介绍。

# 实现一个计数器

假如在业务代码中需要实现一个计数器的功能，啪地一下，很快我们就写出了以下的代码：

```java
/**
 * Author: leixiaoshuai
 */
public class Counter {
    private int count;

    public void increase() {
        count++;
    }
}
```

`increase`方法对 count 变量进行递增。

当代码提交上库进行`code review`时，啪地一下，很快收到了检视意见（严重级别）：
> 如果在多线程场景下，你的计数器可能有问题。

上大一的时候老师就讲过 `count++` 是非原子性的，它实际上包含了三个操作：读数据，加一，写回数据。

再次修改代码，多线访问`increase方法`会有问题，那就给它加个锁吧，count变量修改了其他线程可能不能即时看到，那就给变量加个 `volatile` 吧。

吭哧吭哧，代码如下：

```java
/**
 * Author: leixiaoshuai
 */
public class LockCounter {
    private volatile int count;

    public synchronized void increase() {
        count++;
    }
}
```

一顿操作猛如虎，再次提交代码后，依然收到了检视意见（建议级别）：
> 加锁会影响效率，可以考虑使用原子操作类。 

原子操作类？「黑人问号脸」，莫不是大佬知道我晚上有约会故意整我，不想合入代码吧。带着将信将疑的态度，打开百度谷歌，原来 AtomicInteger 可以轻松解决这个问题，手忙脚乱一顿复制粘贴代码搞定了，终于可以下班了。

```java
/**
 * Author: leixiaoshuai
 */
public class AtomicCounter {
    private AtomicInteger count = new AtomicInteger(0);

    public void increase() {
        count.incrementAndGet();
    }
}
```

# AtomicInteger 源码分析

调用`AtomicInteger类`的`incrementAndGet方法`不用加锁可以实现安全的递增，这个好神奇，下面带领大家分析一下源码是这么实现的，等不及了等不及了。

打开源码，可以看到定义的incrementAndGet方法：

```java
/**
* 在当前值的基础上自动加 1
*
* @return 更新后的值
*/
public final int incrementAndGet() {
    return unsafe.getAndAddInt(this, valueOffset, 1) + 1;
}
```

通过源码可以看到实际上是调用了 unsafe 的一个方法，unsafe 是什么待会再说。

我们再看看getAndAddInt方法的参数：第一个参数 this 是当前对象的引用；第二个参数valueOffset是用来记录value值在内存中的偏移地址，第三个参数是一个常量 1；

在 AtomicInteger 中定义了一个常量`valueOffset`和一个可变的成员变量 `value`：

```java
private static final Unsafe unsafe = Unsafe.getUnsafe();
private static final long valueOffset;

static {
    try {
        valueOffset = unsafe.objectFieldOffset
            (AtomicInteger.class.getDeclaredField("value"));
    } catch (Exception ex) { throw new Error(ex); }
}

private volatile int value;
```

`value` 变量保存当前对象的值，`valueOffset` 是变量的内存偏移地址，也是通过调用unsafe的方法获取。

```java
public final class Unsafe {
    // ……省略其他方法

    public native long objectFieldOffset(Field f);
}
```

这里再说说 `Unsafe` 这个类，人如其名：不安全的类。打开 Unsafe 类会看到大部分方法都标识了 `native`，也就是说这些都是本地方法，本地方法强依赖于操作系统平台，一般都是采用`C/C++`语言编写，在调用 Unsafe 类的本地方法实际会执行这些方法，熟悉 C/C++的小伙伴可自行下载源码研究。

好了，我们再回到最开始，调用了 Unsafe 类的getAndAddInt方法：

```java
public final class Unsafe {
    // ……省略其他方法

    public final int getAndAddInt(Object o, long offset, int delta) {
        int v;
        do {
            v = getIntVolatile(o, offset); 
            // 循环 CAS 操作
        } while (!compareAndSwapInt(o, offset, v, v + delta));
        return v;
    }

    // 根据内存偏移地址获取当前值
    public native int getIntVolatile(Object o, long offset);

    // CAS 操作
    public final native boolean compareAndSwapInt(Object o, long offset,
                                                  int expected,
                                                  int x);
}
```

通过getIntVolatile方法获取当前 AtomicInteger 对象的value值，这是一个本地方法。

然后调用compareAndSwapInt进行 CAS 原子操作，尝试在当前值的基础上加 1，如果 CAS 失败会循环进行重试。

因此compareAndSwapInt方法是最核心的，详细实现大家可以自行找源码看。这里我们看看方法的参数，一共有四个参数：o 是指当前对象；offset 是指当前对象值的内存偏移地址；expected是期望值；x是修改后的值；

compareAndSwapInt方法的思路是拿到对象 o 和 offset 后会再去取对象实际的值，如果当前值与之前取的期望值是一致的就认为 value 没有被修改过，直接将 value 的值更新为 x，这样就完成了一次 CAS 操作，CAS 操作是通过操作系统保证原子性的。

如果当前值与期望值不一致，说明 value 值被修改过，那么就会重试 CAS 操作直到成功。
<img src="https://cdn.jsdelivr.net/gh/CoderLeixiaoshuai/assets/202102/20210602232044-2021-06-02-23-20-44.png" alt="20210602232044-2021-06-02-23-20-44">

AtomicInteger类中还有很多其他的方法，如：

```java
decrementAndGet()
getAndDecrement()
getAndIncrement()
accumulateAndGet()
// …… 省略
```

这些方法实现原理都是大同小异，希望大家可以举一反三理解其他的方法。

另外还有一些其他的类，如：`AtomicLong`，`AtomicReference`，`AtomicIntegerArray`等，这里也不再赘述，原理都是大同小异。

# AtomicLong 和 LongAdder 谁更牛？

Java 在 `jdk1.8版本` 引入了 `LongAdder` 类，与 `AtomicLong` 一样可以实现加、减、递增、递减等线程安全操作，但是在高并发竞争非常激烈的场景下 `LongAdder` 的效率更胜一筹，后续单独用一篇文章进行介绍。

# 总结

讲了半天，可能有的小伙伴还是比较懵，Atomic 类到底是如何实现线程安全的？

在语言层面上，Atomic 类是没有做任何同步操作的，翻看源代码方法没有任何加锁，其实最大功劳还是在 CAS 身上。CAS 利用操作系统的硬件特性实现了原子性，利用 CPU 多核能力实现了硬件层面的阻塞。

只有 CAS 的原子性保证就一定是线程安全的吗？当然不是的，通过源码发现 value 变量还用了 volatile 修饰了，保证了线程可见性。

那有些小伙伴可能要问了，那是不是加锁就没有用了，非也，虽然基于 CAS 的线程安全机制很好很高效，但是这适合一些粒度比较小的需求才有效，如果遇到非常复杂的业务逻辑还是需要加锁操作的。

大家学会了吗？



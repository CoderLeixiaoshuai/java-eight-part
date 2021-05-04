> 本文首发我的微信公众号『[爱笑的架构师](https://mp.weixin.qq.com/s?__biz=MzIwODI1OTk1Nw==&mid=2650321684&idx=1&sn=c3f63443f7e6fb4f373a30699f51e55f&chksm=8f09cd0eb87e44185016b022fdf24e735684e91e5d1ae390b861098c18e3fb26925053b2fd50&token=1553501157&lang=zh_CN#rd)』，欢迎大家关注。

<!-- TOC -->

- [ThreadLocal的value值存在哪里？](#threadlocal的value值存在哪里)
    - [ThreadLocal类set方法](#threadlocal类set方法)
    - [ThreadLocal类get方法](#threadlocal类get方法)
- [ThreadLocal相关类的关系总结](#threadlocal相关类的关系总结)
- [ThreadLocal内存模型原理](#threadlocal内存模型原理)
- [强引用弱引用的概念](#强引用弱引用的概念)
    - [强引用](#强引用)
    - [弱引用](#弱引用)
    - [软引用](#软引用)
    - [虚引用](#虚引用)
- [内存泄露是不是弱引用的锅？](#内存泄露是不是弱引用的锅)
- [ThreadLocal最佳实践](#threadlocal最佳实践)

<!-- /TOC -->

组内来了一个实习生，看这小伙子春光满面、精神抖擞、头发微少，我心头一喜：绝对是个潜力股。于是我找经理申请亲自来带他，为了帮助小伙子快速成长，我给他分了一个需求，这不需求刚上线几天就出网上问题了😭后台监控服务发现内存一直在缓慢上升，初步怀疑是内存泄露。

把实习生的PR都找出来仔细review，果然发现问题了。由于公司内部代码是保密的，这里简单写一个demo还原场景（忽略代码风格问题）。

```java
public class ThreadPoolDemo {
    private static final ThreadPoolExecutor poolExecutor = new ThreadPoolExecutor(5, 5, 1, TimeUnit.MINUTES, new LinkedBlockingQueue<>());
    public static void main(String[] args) throws InterruptedException {
        for (int i = 0; i < 100; ++i) {
            poolExecutor.execute(new Runnable() {
                @Override
                public void run() {
                    ThreadLocal<BigObject> threadLocal = new ThreadLocal<>();
                    threadLocal.set(new BigObject());
                    // 其他业务代码
                }
            });
            Thread.sleep(1000);
        }
    }
    static class BigObject {
        // 100M
        private byte[] bytes = new byte[100 * 1024 * 1024];
    }
}
```
代码分析：
* 创建一个核心线程数和最大线程数都为10的线程池，保证线程池里一直会有10个线程在运行。
* 使用for循环向线程池中提交了100个任务。
* 定义了一个ThreadLocal类型的变量，Value类型是大对象。
* 每个任务会向threadLocal变量里塞一个大对象，然后执行其他业务逻辑。
* 由于没有调用线程池的shutdown方法，线程池里的线程还是会在运行。

乍一看这代码好像没有什么问题，那为什么会导致服务GC后内存还高居不下呢？

代码中给threadLocal赋值了一个大的对象，但是执行完业务逻辑后没有调用remove方法，最后导致线程池中10个线程的threadLocals变量中包含的大对象没有被释放掉，出现了内存泄露。

大家说说这样的实习生还能留不？

# ThreadLocal的value值存在哪里？

实习生说他以为线程任务结束了threadLocal赋值的对象会被JVM垃圾回收，很疑惑为什么会出现内存泄露。作为师傅我肯定要给他把原理讲透呀。

ThreadLocal类提供set/get方法存储和获取value值，但实际上ThreadLocal类并不存储value值，真正存储是靠ThreadLocalMap这个类，ThreadLocalMap是ThreadLocal的一个静态内部类，它的key是ThreadLocal实例对象，value是任意Object对象。

ThreadLocalMap类的定义

```java
static class ThreadLocalMap {
    // 定义一个table数组，存储多个threadLocal对象及其value值
    private Entry[] table;
    ThreadLocalMap(ThreadLocal<?> firstKey, Object firstValue) {
        table = new Entry[INITIAL_CAPACITY];
        int i = firstKey.threadLocalHashCode & (INITIAL_CAPACITY - 1);
        table[i] = new Entry(firstKey, firstValue);
        size = 1;
        setThreshold(INITIAL_CAPACITY);
    }
    // 定义一个Entry类，key是一个弱引用的ThreadLocal对象
    // value是任意对象
    static class Entry extends WeakReference<ThreadLocal<?>> {
        /** The value associated with this ThreadLocal. */
        Object value;
        Entry(ThreadLocal<?> k, Object v) {
            super(k);
            value = v;
        }
    }
    // 省略其他
}
```
进一步分析ThreadLocal类的代码，看set和get方法如何与ThreadLocalMap静态内部类关联上。

## ThreadLocal类set方法

```java
public class ThreadLocal<T> {
	public void set(T value) {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null)
            map.set(this, value);
        else
            createMap(t, value);
    }

    ThreadLocalMap getMap(Thread t) {
        return t.threadLocals;
    }

    void createMap(Thread t, T firstValue) {
        t.threadLocals = new ThreadLocalMap(this, firstValue);
    }
    // 省略其他方法
}
```
set的逻辑比较简单，就是获取当前线程的ThreadLocalMap，然后往map里添加KV，K是当前ThreadLocal实例，V是我们传入的value。
这里需要注意一下，map的获取是需要从Thread类对象里面取，看一下Thread类的定义。

```java
public class Thread implements Runnable {
    ThreadLocal.ThreadLocalMap threadLocals = null;
    //省略其他
}
```
Thread类维护了一个ThreadLocalMap的变量引用。
## ThreadLocal类get方法

get获取当前线程的对应的私有变量，是之前set或者通过initialValue的值，代码如下：

```java
class ThreadLocal<T> {
    public T get() {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null) {
            ThreadLocalMap.Entry e = map.getEntry(this);
            if (e != null)
                return (T)e.value;
        }
        return setInitialValue();
    }
}
```
代码逻辑分析：
* 获取当前线程的ThreadLocalMap实例；
* 如果不为空，以当前ThreadLocal实例为key获取value；
* 如果ThreadLocalMap为空或者根据当前ThreadLocal实例获取的value为空，则执行setInitialValue()；
# ThreadLocal相关类的关系总结

看了上面的分析是不是对Thread，ThreadLocal，ThreadLocalMap，Entry这几个类之间的关系有点晕了，没关系我专门画了一个UML类图来总结（忽略UML标准语法）。

<img src="https://cdn.jsdelivr.net/gh/CoderLeixiaoshuai/assets/202102/20210504233932-2021-05-04-23-39-33.png" alt="20210504233932-2021-05-04-23-39-33">

* 每个线程是一个Thread实例，其内部维护一个threadLocals的实例成员，其类型是ThreadLocal.ThreadLocalMap。
* 通过实例化ThreadLocal实例，我们可以对当前运行的线程设置一些线程私有的变量，通过调用ThreadLocal的set和get方法存取。
* ThreadLocal本身并不是一个容器，我们存取的value实际上存储在ThreadLocalMap中，ThreadLocal只是作为TheadLocalMap的key。
* 每个线程实例都对应一个TheadLocalMap实例，我们可以在同一个线程里实例化很多个ThreadLocal来存储很多种类型的值，这些ThreadLocal实例分别作为key，对应各自的value，最终存储在Entry table数组中。
* 当调用ThreadLocal的set/get进行赋值/取值操作时，首先获取当前线程的ThreadLocalMap实例，然后就像操作一个普通的map一样，进行put和get。
# ThreadLocal内存模型原理

经过上面的分析我们对ThreadLocal相关的类设计已经非常清楚了，下面通过一张图更加深入理解一下ThreadLocal的内存存储。

<img src="https://cdn.jsdelivr.net/gh/CoderLeixiaoshuai/assets/202102/20210504233947-2021-05-04-23-39-48.png" alt="20210504233947-2021-05-04-23-39-48">

图中左边是栈，右边是堆。线程的一些局部变量和引用使用的内存属于Stack（栈）区，而普通的对象是存储在Heap（堆）区。

* 线程运行时，我们定义的TheadLocal对象被初始化，存储在Heap，同时线程运行的栈区保存了指向该实例的引用，也就是图中的ThreadLocalRef。
* 当ThreadLocal的set/get被调用时，虚拟机会根据当前线程的引用也就是CurrentThreadRef找到其对应在堆区的实例，然后查看其对用的TheadLocalMap实例是否被创建，如果没有，则创建并初始化。
* Map实例化之后，也就拿到了该ThreadLocalMap的句柄，那么就可以将当前ThreadLocal对象作为key，进行存取操作。
* 图中的虚线，表示key对应ThreadLocal实例的引用是个弱引用。
# 强引用弱引用的概念

ThreadLocalMap的key是一个弱引用类型，源代码如下：

```java
static class ThreadLocalMap {
    // 定义一个Entry类，key是一个弱引用的ThreadLocal对象
    // value是任意对象
    static class Entry extends WeakReference<ThreadLocal<?>> {
        /** The value associated with this ThreadLocal. */
        Object value;
        Entry(ThreadLocal<?> k, Object v) {
            super(k);
            value = v;
        }
    }
    // 省略其他
}
```
下面解释一下常见的几种引用概念。

## 强引用

**一直活着**：类似“Object obj=new Object()”这类的引用，只要强引用还存在，垃圾收集器永远不会回收掉被引用的对象实例。

## 弱引用

**回收就会死亡**：被弱引用关联的对象实例只能生存到下一次垃圾收集发生之前。当垃圾收集器工作时，无论当前内存是否足够，都会回收掉只被弱引用关联的对象实例。在JDK 1.2之后，提供了WeakReference类来实现弱引用。

## 软引用

**有一次活的机会**：软引用关联着的对象，在系统将要发生内存溢出异常之前，将会把这些对象实例列进回收范围之中进行第二次回收。如果这次回收还没有足够的内存，才会抛出内存溢出异常。在JDK 1.2之后，提供了SoftReference类来实现软引用。

## 虚引用

**也称为幽灵引用或者幻影引用**，它是最弱的一种引用关系。一个对象实例是否有虚引用的存在，完全不会对其生存时间构成影响，也无法通过虚引用来取得一个对象实例。为一个对象设置虚引用关联的唯一目的就是能在这个对象实例被收集器回收时收到一个系统通知。在JDK 1.2之后，提供了PhantomReference类来实现虚引用。

# 内存泄露是不是弱引用的锅？

从表面上看内存泄漏的根源在于使用了弱引用，但是另一个问题也同样值得思考：为什么ThreadLocalMap使用弱引用而不是强引用？

翻看官网文档的说法：

>To help deal with very large and long-lived usages, the hash table entries use WeakReferences for keys.
>>为了处理非常大和长期的用途，哈希表条目使用weakreference作为键。

分两种情况讨论：

**（1）key 使用强引用**

引用ThreadLocal的对象被回收了，但是ThreadLocalMap还持有ThreadLocal的强引用，如果没有手动删除，ThreadLocal不会被回收，导致Entry内存泄漏。

**（2）key 使用弱引**

引用ThreadLocal的对象被回收了，由于ThreadLocalMap持有ThreadLocal的弱引用，即使没有手动删除，ThreadLocal也会被回收。value在下一次ThreadLocalMap调用set、get、remove的时候会被清除。

比较两种情况，我们可以发现：由于ThreadLocalMap的生命周期跟Thread一样长，如果都没有手动删除对应key，都会导致内存泄漏，但是使用弱引用可以多一层保障：弱引用ThreadLocal被清理后key为null，对应的value在下一次ThreadLocalMap调用set、get、remove的时候可能会被清除。

因此，ThreadLocal内存泄漏的根源是：由于ThreadLocalMap的生命周期跟Thread一样长，如果没有手动删除对应key就会导致内存泄漏，而不是因为弱引用。

# ThreadLocal最佳实践

通过前面几小节我们分析了ThreadLocal的类设计以及内存模型，同时也重点分析了发生内存泄露的条件和特定场景。最后结合项目中的经验给出建议使用ThreadLocal的场景：

* 当需要存储线程私有变量的时候。
* 当需要实现线程安全的变量时。
* 当需要减少线程资源竞争的时候。

综合上面的分析，我们可以理解ThreadLocal内存泄漏的前因后果，那么怎么避免内存泄漏呢？

答案就是：每次使用完ThreadLocal，建议调用它的remove()方法，清除数据。

另外需要强调的是并不是所有使用ThreadLocal的地方，都要在最后remove（），因为他们的生命周期可能是需要和项目的生存周期一样长的，所以要进行恰当的选择，以免出现业务逻辑错误！



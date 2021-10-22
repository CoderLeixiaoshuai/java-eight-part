在日常编码中，Java 并发编程可是少不了，试试下面这些并发编程工具类：

![image-20210912233756980](https://cdn.jsdelivr.net/gh/CoderLeixiaoshuai/assets/202105/20210912233757.png)



今天先带领大家一起重温学习 CountDownLatch 这个牛叉的工具类。

# 认识 CountDownLatch

`CountDownLatch`是一个同步工具类，用来协调多个线程之间的同步，或者说起到线程之间通信的作用（非互斥）。



CountDownLatch 能够使一个线程在等待另外一些线程完成各自工作之后，再继续执行。使用一个计数器进行实现。计数器初始值为线程的数量。当每一个线程完成自己任务后，计数器的值就会减一。当计数器的值为0时，表示所有的线程都已经完成一些任务，然后在CountDownLatch上等待的线程就可以恢复执行接下来的任务。



![CountDownLatch 流程图 (1)](https://cdn.jsdelivr.net/gh/CoderLeixiaoshuai/assets/202105/20210912233121.png)



# CountDownLatch 的使用

CountDownLatch类使用起来非常简单。

Class 位于：`java.util.concurrent.CountDownLatch`



下面简单介绍它的构造方法和常用方法。

## 构造方法

CountDownLatch只提供了一个构造方法：

```java
// count 为初始计数值
public CountDownLatch(int count) {
  // ……
}
```

## 常用方法

```java
//常用方法1：调用await()方法的线程会被挂起，它会等待直到count值为0才继续执行
public void await() throws InterruptedException {
  // ……
}   

// 常用方法2：和await()类似，只不过等待超时后count值还没变为0的话就会继续执行
public boolean await(long timeout, TimeUnit unit) throws InterruptedException { 
  // ……
}

// 常用方法3：将count值减1
public void countDown() {
  // ……
}  
```



# CountDownLatch 的应用场景

我们考虑一个场景：用户购买一个商品下单成功后，我们会给用户发送各种消息提示用户『购买成功』，比如发送邮件、微信消息、短信等。所有的消息都发送成功后，我们在后台记录一条消息表示成功。



当然我们可以使用单线程去完成，逐个完成每个操作，如下图所示：

![CountDownLatch 下单流程 (1)](https://cdn.jsdelivr.net/gh/CoderLeixiaoshuai/assets/202105/20210912205856.png)

但是这样效率就会非常低。如何解决单线程效率低的问题？当然是通过多线程啦。

使用多线程也会遇到一个问题，子线程消息还没发送完，主线程可能就已经打出『所有的消息都已经发送完毕啦』，这在逻辑上肯定是不对的。我们期望所有子线程发完消息主线程才会打印消息，怎么实现呢？CountDownLatch就可以解决这一类问题。

![CountDownLatch 下单流程2](https://cdn.jsdelivr.net/gh/CoderLeixiaoshuai/assets/202105/20210912205750.png)



我们使用代码实现上面的需求。

```java
import java.util.concurrent.*;

public class OrderServiceDemo {

    public static void main(String[] args) throws InterruptedException {
        System.out.println("main thread: Success to place an order");

        int count = 3;
        CountDownLatch countDownLatch = new CountDownLatch(count);

        Executor executor = Executors.newFixedThreadPool(count);
        executor.execute(new MessageTask("email", countDownLatch));
        executor.execute(new MessageTask("wechat", countDownLatch));
        executor.execute(new MessageTask("sms", countDownLatch));

        // 主线程阻塞，等待所有子线程发完消息
        countDownLatch.await();
        // 所有子线程已经发完消息，计数器为0，主线程恢复
        System.out.println("main thread: all message has been sent");
    }

    static class MessageTask implements Runnable {
        private String messageName;
        private CountDownLatch countDownLatch;

        public MessageTask(String messageName, CountDownLatch countDownLatch) {
            this.messageName = messageName;
            this.countDownLatch = countDownLatch;
        }

        @Override
        public void run() {
            try {
                // 线程发送消息
                System.out.println("Send " + messageName);
                try {
                    TimeUnit.SECONDS.sleep(1);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            } finally {
                // 发完消息计数器减 1
                countDownLatch.countDown();
            }
        }
    }
}
```

程序运行结果：

```text
main thread: Success to place an order
Send email
Send wechat
Send sms
main thread: all message has been sent
```

从运行结果可以看到主线程是在所有的子线程发送完消息后才打印，这符合我们的预期。

# CountDownLatch 的限制

CountDownLatch是一次性的，计算器的值只能在构造方法中初始化一次，之后没有任何机制再次对其设置值，当CountDownLatch使用完毕后，它不能再次被使用。
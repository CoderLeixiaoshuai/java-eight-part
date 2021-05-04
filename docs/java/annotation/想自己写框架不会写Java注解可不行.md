> 本文首发我的微信公众号『[爱笑的架构师](https://mp.weixin.qq.com/s/JqrJGwyU0oKdWYtHe_W31w)』，欢迎大家关注。

<!-- MarkdownTOC -->

- [用注解一时爽，一直用一直爽](#用注解一时爽一直用一直爽)
- [原来注解不神秘](#原来注解不神秘)
- [造火箭啦，自己动手写一个注解](#造火箭啦自己动手写一个注解)
	- [第一步定义一个注解](#第一步定义一个注解)
	- [第二步实现注解的业务逻辑](#第二步实现注解的业务逻辑)
	- [第三步在业务代码中尽情的使用注解](#第三步在业务代码中尽情的使用注解)
- [公众号](#公众号)

<!-- /MarkdownTOC -->


<div align="center">  <img src="https://cdn.jsdelivr.net/gh/SmileLionCoder/assets@main/202010/20201023232517.png" width=""/> </div><br>

# 用注解一时爽，一直用一直爽

Java后端开发进入spring全家桶时代后，开发一个微服务提供简单的增删改查接口跟玩泥巴似的非常简单，一顿操作猛如虎，回头一看代码加了一堆注解：@Controller @Autowired @Value，面向注解编程变成了大家不可缺少的操作。

想象一下如果没有注解Java程序员可以要哭瞎😭

既然注解（annotation）这么重要，用的这么爽，那注解的实现原理你知道么？我猜你只会用注解不会自己写注解（手动滑稽）。

好了，下面的内容带大家从零开始写一个注解，揭开注解神秘的面纱。

# 原来注解不神秘

注解用大白话来说就是一个标记或者说是特殊的注释，如果没有解析这些标记的操作那它啥也不是。

注解的格式如同类或者方法一样有自己特殊的语法，这个语法下文会详细介绍。

那如何去解析注解呢？这就要用到Java强大的反射功能了。反射大家应该都用过，可以通过类对象获取到这个类的各种信息比如成员变量、方法等，那注解标记能不能通过反射获取呢？当然可以了。

所以注解的原理其实很简单，本质上是通过反射功能动态获取注解标记，然后按照不同的注解执行不同的操作，比如@Autowired可以注入一个对象给变量赋值。

看到这里是不是很躁动啊，来吧自己也撸一个注解。

# 造火箭啦，自己动手写一个注解

便于大家理解，这里先引入一个场景：在线教育火了，经理让我写一个模块实现学生信息管理功能，考虑到分布式并发问题，经理让我务必加上分布式锁。

经理问我几天能搞定？我说至少3天。如是脑补了以下代码：

```java
/**
 * 更新学生信息
 * @param student 学生对象
 * @return true 更新成功，false 更新失败
 */
public boolean updateStudentInfo(Student student) {
    // 尝试获取分布式锁
    String lockKey = "student:" + student.getId();
    if (RedisTool.tryLock(lockKey, 10,
            TimeUnit.SECONDS, 5)) {
        try {
            // 这里写业务逻辑
        } finally {
            RedisTool.releaseLock(lockKey);
        }
    }
    // 获取锁失败
    return false;
}
```

经理走后我在思考，我能不能只花一天时间写完，剩下两天时间用来写博客划水呢？突然灵感来了，我可以把重复的代码逻辑抽出来用注解实现不就节省代码了，哈哈，赶紧写。

使用注解之后整个方法清爽了很多，HR小姐姐都夸我写的好呢。
```java
@EnableRedisLock(lockKey = "student", expireTime = 10, timeUnit = TimeUnit.SECONDS, retryTimes = 5)
public boolean updateStudentInfo(Student student) {
    // 这里写业务逻辑
    // studentDao.update(student);
    return true;
}
```

代码已经写完上库了，现在我在划水写博客呢。是不是很简洁很优雅很牛逼，怎么做到的呢，主要分为三步：1打开冰箱门，2把大象放进去，3把冰箱门关好。好了，扯远了，大家接着往下看。

## 第一步定义一个注解

<div align="center">  <img src="https://cdn.jsdelivr.net/gh/SmileLionCoder/assets@main/202010/20201023233716.png" width="70%"/> </div><br>

一个注解可以简单拆解为三个部分：

第一部分：注解体

注解的定义有点类似于接口（interface），只不过前面一个加了一个@符号，这个千万不能省。

第二部分：注解变量

注解变量的语法有点类似于接口里面定义的方法，变量名后面带一对括号，不同的是注解变量后面可以有默认值。另外返回值只能是Java基本类型、String类型或者枚举类，不可以是对象类型。

第三部分：元注解

元注解（meta-annotation）说白了就是给注解加注解的注解，是不是有点晕了，这种注解是JDK提前内置好的，可以直接拿来用的。不太懂也没有关系反正数量也不多，总共就4个，我们背下来吧：@Target @Retention @Documented @Inherited

* Target注解

用来描述注解的使用范围，即被修饰的注解可以用在什么地方 。

注解可以用于修饰 packages、types（类、接口、枚举、注解类）、类成员（方法、构造方法、成员变量、枚举值）、方法参数和本地变量（如循环变量、catch参数），在定义注解类时使用了@Target 能够更加清晰的知道它能够被用来修饰哪些对象，具体的取值范围定义在ElementType.java 枚举类中。

比如上面我们写的Redis锁的注解就只能用于方法上了。

* Retention注解

用来描述注解保留的时间范围，即注解的生命周期。在 RetentionPolicy 枚举类中定义了三个周期：

```java
public enum RetentionPolicy {
    SOURCE, // 源文件保留
    CLASS,  // 编译期保留，默认值
    RUNTIME // 运行期保留，可通过反射去获取注解信息
}
```
像我们熟知的@Override注解就只能保留在源文件中，代码编译后注解就消失了。
比如上面我们写的Redis锁的注解就保留到了运行期，运行的时候可以通过反射获取信息。

* Documented注解

用来描述在使用 javadoc 工具为类生成帮助文档时是否要保留其注解信息，很简单不多解释了。

* Inherited注解

被Inherited注解修饰的注解具有继承性，如果父类使用了被@Inherited修饰的注解，则其子类将自动继承该注解。

好了，这一步我们已经将注解定义好了，但是这个注解如何工作呢？接着看。

## 第二步实现注解的业务逻辑

在第一步中我们发现定义的注解（@EnableRedisLock）中没有业务逻辑，只有一些变量，别忘了我们的注解是要使能Redis分布式锁的功能，那这个注解到底是怎么实现加锁和释放锁的功能呢？这个就需要我们借助反射的强大功能了。
```java
@Aspect
public class RedisLockAspect {
    @Around(value = "@annotation(com.smilelioncoder.EnableRedisLock)")
    public void handleRedisLock(ProceedingJoinPoint joinPoint)
            throws Throwable {
        // 通过反射获取到注解对象，可见反射非常重要的
        EnableRedisLock redisLock = ((MethodSignature) joinPoint.getSignature())
                .getMethod()
                .getAnnotation(EnableRedisLock.class);

        // 获取注解对象的变量值
        String lockKey = redisLock.lockKey();
        long expireTime = redisLock.expireTime();
        TimeUnit timeUnit = redisLock.timeUnit();
        int retryTimes = redisLock.retryTimes();

        // 获取锁
        if (tryLock(lockKey, expireTime, timeUnit, retryTimes)) {
            try {
                // 获取锁成功继续执行业务逻辑
                joinPoint.proceed();
            } finally {
                releseLock();
            }
        }
    }
}
```

这里借助了切面的功能，将EnableRedisLock注解作为一个切点，只要方法上标注了这个注解就会自动执行这里的代码逻辑。

通过反射机制拿到注解对象后就可以执行加锁解锁的常用逻辑啦。Redis实现分布式锁相信大家已经很熟悉了，这里就不在啰嗦了。

## 第三步在业务代码中尽情的使用注解

```java
@EnableRedisLock(lockKey = "student", expireTime = 10, timeUnit = TimeUnit.SECONDS, retryTimes = 5)
public void method1(Student student) {
    // 这里写业务逻辑
}
```
在需要加锁的方法上直接加上注解就可以啦，怎么样是不是很简单呀，赶紧在你的项目中运用起来吧。
好了，自己写一个注解的内容就介绍到这里了，学会了吗？

# 公众号
公众号比Github早一到两天更新，如果大家想要实时关注我更新的文章以及分享的干货，可以关注我的公众号。

<div align="center">  <img src="https://cdn.jsdelivr.net/gh/SmileLionCoder/assets@main/wechat-01.jpg" width=""/> </div><br>

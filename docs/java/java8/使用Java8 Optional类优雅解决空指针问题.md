> 本文首发我的微信公众号『[爱笑的架构师](https://mp.weixin.qq.com/s?__biz=MzIwODI1OTk1Nw==&mid=2650321295&idx=1&sn=2fdb1d4c7e44177a7b08393114e55f16&chksm=8f09cf95b87e4683e521502b33319f957a038b5ecc095171de9d287b337411f2ffb2bf1e01d5&token=997683041&lang=zh_CN#rd)』，欢迎大家关注。

<!-- MarkdownTOC -->

- [1. 不受待见的空指针异常](#1-不受待见的空指针异常)
- [2. 糟糕的代码](#2-糟糕的代码)
- [3. 解决空指针的"银弹"](#3-解决空指针的银弹)
- [4. Optional使用入门](#4-optional使用入门)
- [5. 使用Optional重构代码](#5-使用optional重构代码)
- [总结](#总结)
- [公众号](#公众号)

<!-- /MarkdownTOC -->

>Java8 由Oracle在2014年发布，是继Java5之后最具革命性的版本。
>Java8吸收其他语言的精髓带来了函数式编程，lambda表达式，Stream流等一系列新特性，学会了这些新特性，可以让你实现高效编码优雅编码。
# 1. 不受待见的空指针异常

有个小故事：null引用最早是由英国科学家Tony Hoare提出的，多年后Hoare为自己的这个想法感到后悔莫及，并认为这是"价值百万的重大失误"。可见空指针是多么不受待见。

NullPointerException是Java开发中最常遇见的异常，遇到这种异常我们通常的解决方法是在调用的地方加一个if判空。

if判空越多会造成过多的代码分支，后续代码维护也就越来越复杂。

# 2. 糟糕的代码

比如看下面这个例子，使用过多的if判空。

Person对象里定义了House对象，House对象里定义了Address对象：

```java
public class Person {
    private String name;
    private int age;
    private House house;
    public House getHouse() {
        return house;
    }
}
class House {
    private long price;
    private Address address;
    public Address getAddress() {
        return address;
    }
}
class Address {
    private String country;
    private String city;
    public String getCity() {
        return city;
    }
}
```
现在获取这个人买房的城市，那么通常会这样写：
```java
public String getCity() {
    String city = new Person().getHouse().getAddress().getCity();
    return city;
}
```
但是这样写容易出现空指针的问题，比如这个人没有房，House对象为null。接着你会改造这段代码，加上很多判断条件：
```java
public String getCity2(Person person) {
    if (person != null) {
        House house = person.getHouse();
        if (house != null) {
            Address address = house.getAddress();
            if (address != null) {
                String city = address.getCity();
                return city;
            }
        }
    }
    return "unknown";
}
```
为了避免空指针异常，每一层都加上判断，但是这样会造成代码嵌套太深，不易维护。
你可能想到如何改造上面的代码，比如加上提前判空退出：

```java
public String getCity3(Person person) {
    String city = "unknown";
    if (person == null) {
      return city; 
    }
    House house = person.getHouse();
    if (house == null) {
        return city;
    }
    Address address = house.getAddress();
    if (address == null) {
        return city;
    }
    return address.getCity();
}
```
但是这样简单的代码已经加入了三个退出条件，非常不利于后面代码维护。那怎样才能将代码写的优雅一点呢，下面引入今天的主角"Optional"。
# 3. 解决空指针的"银弹"

从Java8开始引入了一个新类 java.util.Optional，这是一个对象的容器，意味着可能包含或者没有包含一个非空的值。下面重点看一下Optional的常用方法：

```java
public final class Optional<T> {
    // 通过指定非空值创建Optional对象
    // 如果指定的值为null，会抛空指针异常
    public static <T> Optional<T> of(T value) {
        return new Optional<>(value);
    }
    // 通过指定可能为空的值创建Optional对象
    public static <T> Optional<T> ofNullable(T value) {
        return value == null ? empty() : of(value);
    }
    // 返回值，不存在抛异常
    public T get() {
        if (value == null) {
            throw new NoSuchElementException("No value present");
        }
        return value;
    }
    // 如果值存在，根据consumer实现类消费该值
    public void ifPresent(Consumer<? super T> consumer) {
        if (value != null)
            consumer.accept(value);
    }
    // 如果值存在则返回，如果值为空则返回指定的默认值
    public T orElse(T other) {
        return value != null ? value : other;
    }
    // map flatmap等方法与Stream使用方法类似，这里不再赘述，读者可以参考之前的Stream系列。
}
```
以上就是Optional类常用的方法，使用起来非常简单。
# 4. Optional使用入门

**（1）创建Optional实例**

* 创建空的Optional对象。可以通过静态工厂方法Optional.Empty() 创建一个空的对象，例如：
```java
Optional<Person> optionalPerson = Optional.Empty();
```
* 指定非空值创建Optional对象。
```java
Person person = new Person();
Optional<Person> optionalPerson = Optional.of(person);
```
* 指定可能为空的值创建Optional对象。
```java
Person person = null; // 可能为空
Optional<Person> optionalPerson = Optional.of(person);
```
**（2）常用方法**
**ifPresent**

如果值存在，则调用consumer实例消费该值，否则什么都不执行。举个栗子：

```java
String str = "hello java8";
// output: hello java8
Optional.ofNullable(str).ifPresent(System.out::println);
String str2 = null;
// output: nothing
Optional.ofNullable(str2).ifPresent(System.out::println);
```
**filter, map, flatMap**
在三个方法在前面讲Stream的时候已经详细讲解过，读者可以翻看之前写的文章，这里不再赘述。

**orElse**

如果value为空，则返回默认值，举个栗子：

```java
public void test(String city) {
    String defaultCity = Optional.ofNullable(city).orElse("unknown");
}
```
**orElseGet**
如果value为空，则调用Supplier实例返回一个默认值。举个例子：

```java
public void test2(String city) {
    // 如果city为空，则调用generateDefaultCity方法
    String defaultCity = Optional.of(city).orElseGet(this::generateDefaultCity);
}
private String generateDefaultCity() {
    return "beijing";
}
```
**orElseThrow**
如果value为空，则抛出自定义异常。举个栗子：

```java
public void test3(String city) {
    // 如果city为空，则抛出空指针异常。
    String defaultCity = Optional.of(city).orElseThrow(NullPointerException::new);
}
```
# 5. 使用Optional重构代码

**再看一遍重构之前的代码，使用了三个if使代码嵌套层次变得很深。**

```java
// before refactor
public String getCity2(Person person) {
    if (person != null) {
        House house = person.getHouse();
        if (house != null) {
            Address address = house.getAddress();
            if (address != null) {
                String city = address.getCity();
                return city;
            }
        }
    }
    return "unknown";
}
```
**使用Optional重构**
```java
public String getCityUsingOptional(Person person) {
    String city = Optional.ofNullable(person)
            .map(Person::getHouse)
            .map(House::getAddress)
            .map(Address::getCity).orElse("Unknown city");
    return city;
}
```
只使用了一行代码就获取到city值，不用再去不断的判断是否为空，这样写代码是不是很优雅呀。
# 总结
使用optional类可以很优雅的解决项目中空指针的问题，但是optional也不是万能的哦，小伙伴们要适度使用。赶紧用Optional重构之前写的项目吧~**

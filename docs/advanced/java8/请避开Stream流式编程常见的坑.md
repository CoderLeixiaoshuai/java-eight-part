<!-- MarkdownTOC -->

- [1. Stream是什么？](#1-stream是什么)
- [2. Stream的特点](#2-stream的特点)
- [3. 创建Stream实例的方法](#3-创建stream实例的方法)
- [4. Stream常用操作](#4-stream常用操作)
- [5. 实战：使用Stream重构老代码](#5-实战：使用stream重构老代码)
- [6. 使用Stream常见的误区](#6-使用stream常见的误区)
- [公众号](#公众号)

<!-- /MarkdownTOC -->

>Java8 由Oracle在2014年发布，是继Java5之后最具革命性的版本了。
>Java8吸收其他语言的精髓带来了函数式编程，lambda表达式，Stream流等一系列新特性，学会了这些新特性，可以让你实现高效编码优雅编码。

# 1. Stream是什么？

Stream是Java8新增的一个接口，允许以声明性方式处理数据集合。Stream不是一个集合类型不保存数据，可以把它看作是遍历数据集合的高级迭代器（Iterator）。

Stream操作可以像Builder一样逐步叠加，形成一条流水线。流水线一般由数据源+零或者多个中间操作+一个终端操作所构成。中间操作可以将流转换成另外一个流，比如使用filter过滤元素，使用map映射提取值。

Stream与lambda表达式密不可分，本文默认你已经掌握了lambda基础知识。

# 2. Stream的特点

* 只能遍历（消费）一次。Stream实例只能遍历一次，终端操作后一次遍历就结束，再次遍历需要重新生成实例，这一点类似于Iterator迭代器。
* 保护数据源。对Stream中任何元素的修改都不会导致数据源被修改，比如过滤删除流中的一个元素，再次遍历该数据源依然可以获取该元素。
* 懒。filter, map 操作串联起来形成一系列中间运算，如果没有一个终端操作（如collect）这些中间运算永远也不会被执行。
# 3. 创建Stream实例的方法

（1）使用指定值创建Stream实例

```java
// of为Stream的静态方法
Stream<String> strStream = Stream.of("hello", "java8", "stream");
// 或者使用基本类型流
IntStream intStream = IntStream.of(1, 2, 3);
```
（2）使用集合创建Stream实例（常用方式）

```java
// 使用guava库，初始化一个不可变的list对象
ImmutableList<Integer> integers = ImmutableList.of(1, 2, 3);
// List接口继承Collection接口，java8在Collection接口中添加了stream方法
Stream<Integer> stream = integers.stream();
```
（3）使用数组创建Stream实例

```java
// 初始化一个数组
Integer[] array = {1, 2, 3};
// 使用Arrays的静态方法stream
Stream<Integer> stream = Arrays.stream(array);
```
（4）使用生成器创建Stream实例

```java
// 随机生成100个整数
Random random = new Random();
// 加上limit否则就是无限流了
Stream<Integer> stream = Stream.generate(random::nextInt).limit(100);
```
（5）使用迭代器创建Stream实例

```java
// 生成100个奇数，加上limit否则就是无限流了
Stream<Integer> stream = Stream.iterate(1, n -> n + 2).limit(100);
stream.forEach(System.out::println);
```
（6）使用IO接口创建Stream实例

```java
// 获取指定路径下文件信息，list方法返回Stream类型
Stream<Path> pathStream = Files.list(Paths.get("/"));
```
# 4. Stream常用操作

Stream接口中定义了很多操作，大致可以分为两大类，一类是中间操作，另一类是终端操作；

<div align="center">  <img src="https://cdn.jsdelivr.net/gh/SmileLionCoder/assets@main/202010/20201018221358.png" width="500"/> </div><br>


**（1）中间操作**

中间操作会返回另外一个流，多个中间操作可以连接起来形成一个查询。

中间操作有惰性，如果流上没有一个终端操作，那么中间操作是不会做任何处理的。

下面介绍常用的中间操作：

**map操作**

map是将输入流中每一个元素映射为另一个元素形成输出流。

```java
// 初始化一个不可变字符串
List<String> words = ImmutableList.of("hello", "java8", "stream");
// 计算列表中每个单词的长度
List<Integer> list = words.stream()
        .map(String::length)
        .collect(Collectors.toList());
// output: 5 5 6
list.forEach(System.out::println);
```
**flatMap操作**

```java
List<String[]> list1 = words.stream()
        .map(word -> word.split("-"))
        .collect(Collectors.toList());
        
// output: [Ljava.lang.String;@59f95c5d, 
//             [Ljava.lang.String;@5ccd43c2
list1.forEach(System.out::println);
```
纳里？你预期是List<String>, 返回却是List<String[]>,  这是因为split方法返回的是String[]

这个时候你可以想到要将数组转成stream, 于是有了第二个版本

```java
Stream<Stream<String>> arrStream = words.stream()
        .map(word -> word.split("-"))
        .map(Arrays::stream);
        
// output: java.util.stream.ReferencePipeline$Head@2c13da15, 
// java.util.stream.ReferencePipeline$Head@77556fd
arrStream.forEach(System.out::println);
```
还是不对，这个问题使用flatMap扁平流可以解决，flatMap将流中每个元素取出来转成另外一个输出流

```java
Stream<String> strStream = words.stream()
        .map(word -> word.split("-"))
        .flatMap(Arrays::stream)
        .distinct();
// output: hello java8 stream
strStream.forEach(System.out::println);
```
**filter操作**

filter接收Predicate对象，按条件过滤，符合条件的元素生成另外一个流。

```java
// 过滤出单词长度大于5的单词，并打印出来
List<String> words = ImmutableList.of("hello", "java8", "hello", "stream");
words.stream()
        .filter(word -> word.length() > 5)
        .collect(Collectors.toList())
        .forEach(System.out::println);
// output: stream
```

**（2）终端操作**

终端操作将stream流转成具体的返回值，比如List，Integer等。常见的终端操作有：foreach, min, max, count等。

foreach很常见了，下面举一个max的例子。

```java
// 找出最大的值
List<Integer> integers = Arrays.asList(6, 20, 19);
integers.stream()
        .max(Integer::compareTo)
        .ifPresent(System.out::println);
// output: 20
```
# 5. 实战：使用Stream重构老代码

假如有一个需求：过滤出年龄大于20岁并且分数大于95的学生。

使用for循环写法：

```java
private List<Student> getStudents() {
    Student s1 = new Student("xiaoli", 18, 95);
    Student s2 = new Student("xiaoming", 21, 100);
    Student s3 = new Student("xiaohua", 19, 98);
    List<Student> studentList = Lists.newArrayList();
    studentList.add(s1);
    studentList.add(s2);
    studentList.add(s3);
    return studentList;
}
public void refactorBefore() {
    List<Student> studentList = getStudents();
    // 使用临时list
    List<Student> resultList = Lists.newArrayList();
    for (Student s : studentList) {
        if (s.getAge() > 20 && s.getScore() > 95) {
            resultList.add(s);
        }
    }
    // output: Student{name=xiaoming, age=21, score=100}
    resultList.forEach(System.out::println);
}
```
使用for循环会初始化一个临时list用来存放最终的结果，整体看起来不够优雅和简洁。

使用lambda和stream重构后：

```java
public void refactorAfter() {
    List<Student> studentLists = getStudents();
    // output: Student{name=xiaoming, age=21, score=100}
   studentLists.stream().filter(this::filterStudents).forEach(System.out::println);
}
private boolean filterStudents(Student student) {
    // 过滤出年龄大于20岁并且分数大于95的学生
    return student.getAge() > 20 && student.getScore() > 95;
}
```
使用filter和方法引用使代码清晰明了，也不用声明一个临时list，非常方便。

# 6. 使用Stream常见的误区

（1）误区一：重复消费stream对象

stream对象一旦被消费，不能再次重复消费。

```java
List<String> strings = Arrays.asList("hello", "java8", "stream");
Stream<String> stream = strings.stream();
stream.forEach(System.out::println); // ok
stream.forEach(System.out::println); // IllegalStateException
```
上述代码执行后报错：

java.lang.IllegalStateException: stream has already been operated upon or closed

（2）误区二：修改数据源

在流操作的过程中尝试添加新的string对象，结果报错：

```java
List<String> strings = Arrays.asList("hello", "java8", "stream");
// expect: HELLO JAVA8 STREAM WORLD, but throw UnsupportedOperationException
strings.stream()
        .map(s -> {
            strings.add("world");
            return s.toUpperCase();
        }).forEach(System.out::println);
```
注意：一定不要在操作流的过程中修改数据源。

# 总结
java8 流式编程在一定程度上可以使代码变得优美，不过也要避开常见的坑，如：不要重复消费对象、不要修改数据源。

# 公众号
公众号比Github早一到两天更新，如果大家想要实时关注我更新的文章以及分享的干货，可以关注我的公众号。

<div align="center">  <img src="https://cdn.jsdelivr.net/gh/SmileLionCoder/assets@main/wechat-01.jpg" width=""/> </div><br>
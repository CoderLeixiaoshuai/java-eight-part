<!-- TOC -->

- [函数式接口](#函数式接口)
- [1. Predicate](#1-predicate)
    - [（1）定义](#1定义)
    - [（2）使用方法](#2使用方法)
    - [（3）函数描述符](#3函数描述符)
- [2. Consumer](#2-consumer)
    - [（1）定义](#1定义-1)
    - [（2）使用方法](#2使用方法-1)
    - [（3）函数描述符](#3函数描述符-1)
- [3. Supplier](#3-supplier)
    - [（1）定义](#1定义-2)
    - [（2）使用方法](#2使用方法-2)
    - [（3）函数描述符](#3函数描述符-2)
- [4. Function](#4-function)
    - [（1）定义](#1定义-3)
    - [（2）使用方法](#2使用方法-3)
    - [（3）函数描述符](#3函数描述符-3)
- [5. UnaryOperator](#5-unaryoperator)
    - [（1）定义](#1定义-4)
    - [（2）使用方法](#2使用方法-4)
    - [（3）函数描述符](#3函数描述符-4)
- [6. BinaryOperator](#6-binaryoperator)
    - [（1）定义](#1定义-5)
    - [（2）使用方法](#2使用方法-5)
    - [（3）函数描述符](#3函数描述符-5)
- [7. 总结](#7-总结)

<!-- /TOC -->

> Java8 由Oracle在2014年发布，是继Java5之后最具革命性的版本。
> Java8吸收其他语言的精髓带来了函数式编程，lambda表达式，Stream流等一系列新特性，学会了这些新特性，可以让你实现高效编码优雅编码。

# 函数式接口
函数式接口是指只定义了**一个抽象方法**的接口，不包括default默认方法。

函数式接口的抽象方法的签名称为**函数描述符**，通过函数描述符可以很好得到Lambda表达式的签名。

常见的函数式接口有：Runnable, Callable, Comparator等。除此之外，Java8设计者还新增了一些比较抽象的函数式接口，比如：Predicate, Consumer, Supplier, Function, UnaryOperator, BinaryOperator等, 这些函数式接口定义在java.util.function包中。

接下来详细介绍function包中定义的抽象接口：

# 1. Predicate

## （1）定义

Predicate是谓词的意思，用来判断泛型T对象是否符合条件，如果符合返回true，否则返回false。


查看jdk源码，定义了一个抽象方法test：
```java
@FunctionalInterface
public interface Predicate<T> {

    /**
     * Evaluates this predicate on the given argument.
     *
     * @param t the input argument
     * @return {@code true} if the input argument matches the predicate,
     * otherwise {@code false}
     */
    boolean test(T t);
}
```

## （2）使用方法

Predicate是一个接口所以不能直接实例化，可以使用匿名类或者Lambda表达式实例化。以Lambda为例：
```java
// 接收string对象，判断是否为空，返回boolean
Predicate predicate = (String str) -> str.isEmpty();
```

下面以一个校验参数的实例说明：
```java
@Test
public void testPredicate() {
    String input = "hello java8";
    if (validate(input, (str) -> !str.isEmpty() && str.length() > 5)) {
        // 校验输入
        System.out.println("valid input");
    }
}

// 第二个参数接收一个Predicate实例
private <T> boolean validate(T input, Predicate<T> predicate) {
    return predicate.test(input);
}
```

## （3）函数描述符
Predicate： T -> boolean

接受泛型T对象返回boolean。

注意：java.util.function包中还针对基本类型封装了类似IntPredicate, LongPredicate等接口，这无非是表明只接受Int或Long类型的输入，后面Consumer等函数式接口与这个类似，本文不再赘述。


# 2. Consumer

## （1）定义

Consumer是消费者的意思，用来接收一个泛型T对象，执行相关操作，最后返回void。


查看jdk源码，定义了一个抽象方法accept：
```java
@FunctionalInterface
public interface Consumer<T> {

    /**
     * Performs this operation on the given argument.
     *
     * @param t the input argument
     */
    void accept(T t);
}
```

## （2）使用方法

使用Lambda表达式实例化Consumer接口：
```java
Consumer<String> consumer = (str) -> System.out.println(str);
```

下面以打印字符串的实例讲解Consumer的用法：
```java
@Test
public void testConsumer() {
    String input = "hello java8";
    // 打印输入
    consume(input, (str) -> System.out.println(str));
}

private <T> void consume(T input, Consumer<T> consumer) {
    consumer.accept(input);
}
```

## （3）函数描述符
Consumer： T -> void

接受泛型T对象返回void。

# 3. Supplier

## （1）定义

Supplier是提供者的意思，用来生成泛型T对象。

查看jdk源码，定义了一个抽象方法get：
```java
@FunctionalInterface
public interface Supplier<T> {

    /**
     * Gets a result.
     *
     * @return a result
     */
    T get();
}
```

## （2）使用方法

使用Lambda表达式实例化Supplier接口：
```java
Supplier supplier = () -> "Hello Java8 supplier";
```

下面以获取当前时间的实例讲解Supplier的用法：
```java
@Test
public void testSupplier() {
    // 获取当前时间
    Long currentTime = supply(() -> System.currentTimeMillis());
}

private <T> T supply(Supplier<T> supplier) {
    return supplier.get();
}
```

## （3）函数描述符
Supplier： void -> T

接受void返回泛型T对象。

# 4. Function

## （1）定义

Function是函数的意思，用来定义一个抽象函数，接收泛型T对象，返回泛型R对象。

查看jdk源码，定义了一个抽象方法apply：
```java
@FunctionalInterface
public interface Function<T, R> {

    /**
     * Applies this function to the given argument.
     *
     * @param t the function argument
     * @return the function result
     */
    R apply(T t);
}
```

## （2）使用方法

使用Lambda表达式实例化Function接口：
```java
Function<String, Integer> function = (str) -> str.length();
```

下面以判断输入是否以指定字符串开头的实例讲解Function的用法：
```java
@Test
public void testFunction() {
    String input = "hello java8";
    if(func(input, (String str) -> str.startsWith("hello"))) {
        System.out.println("start with hello");
    }

}

private <T, R> R func(T t, Function<T, R> function) {
    return function.apply(t);
}
```

## （3）函数描述符
Function： T -> R

接受泛型T对象，返回泛型R对象。

# 5. UnaryOperator

## （1）定义

UnaryOperator是一元操作符的意思，接收一个泛型T对象参数，返回相同T类型对象。

查看jdk源码，UnaryOperator继承自Function接口，定义了一个identity方法：
```java
@FunctionalInterface
public interface UnaryOperator<T> extends Function<T, T> {

    /**
     * Returns a unary operator that always returns its input argument.
     *
     * @param <T> the type of the input and output of the operator
     * @return a unary operator that always returns its input argument
     */
    static <T> UnaryOperator<T> identity() {
        return t -> t;
    }
}
```

## （2）使用方法

使用Lambda表达式实例化UnaryOperator接口：
```java
UnaryOperator<Integer> unaryOperator = (Integer i) -> i * i;
```

下面以整数递增的实例讲解UnaryOperator的用法：
```java
@Test
public void testUnaryOperator() {
    UnaryOperator<Integer> unaryOperator = (Integer i) -> i * i;
    
    int input = 0;
    int result = unaryOperate(input, (Integer i) -> i + 1);
    // output: 1
    System.out.println(result);
}

private <T> T unaryOperate(T t, UnaryOperator<T> unaryOperator) {
    return unaryOperator.apply(t);
}
```

## （3）函数描述符
UnaryOperator： T -> T

接受泛型T对象，返回泛型T对象。

# 6. BinaryOperator

## （1）定义

BinaryOperator是二元操作符的意思，接收两个相同泛型参数类型T，返回R类型对象。

查看jdk源码，BinaryOperator继承自BiFunction接口，继承了BiFunction的apply方法：
```java
@FunctionalInterface
public interface BinaryOperator<T> extends BiFunction<T,T,T> {
    /**
     * Applies this function to the given arguments.
     *
     * @param t the first function argument
     * @param u the second function argument
     * @return the function result
     */
    R apply(T t, U u);
}
```

## （2）使用方法

使用Lambda表达式实例化BinaryOperator接口：
```java
BinaryOperator<String> binaryOperator = (String str1, String str2) -> str1 + str2;
```

下面以整数求和实例讲解BinaryOperator的用法：
```java
@Test
public void testBinaryOperator() {
    int a1 = 10;
    int a2 = 20;
    
    int sum = binaryOperate(a1, a2, (Integer i1, Integer i2) -> i1 + i2);
    // output: 30
    System.out.println(sum);
}

private <T> T binaryOperate(T t1, T t2, BinaryOperator<T> binaryOperator) {
    return binaryOperator.apply(t1, t2);
}
```

## （3）函数描述符
BinaryOperator： (T, T) -> T

接受两个泛型T对象，返回一个泛型T对象。

# 7. 总结

java.util.function包中定义了很多函数式抽象接口，只要记住它们的函数描述符就可以很方便的知道他们的使用方法。

- Predicate： T -> boolean
- Consumer： T -> void
- Supplier： void -> T
- Function： T -> R
- UnaryOperator： T -> T
- BinaryOperator： (T, T) -> T

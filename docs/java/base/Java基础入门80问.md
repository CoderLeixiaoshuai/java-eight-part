> 本文首发我的微信公众号『[爱笑的架构师](#公众号)』，欢迎大家关注。

<!-- TOC -->

- [1.一个".java"源文件中是否可以包括多个类（不是内部类）？有什么限制？](#1一个java源文件中是否可以包括多个类不是内部类有什么限制)
- [2.Java有没有goto?](#2java有没有goto)
- [3.说说&和&&的区别](#3说说和的区别)
- [4.在JAVA中如何跳出当前的多重嵌套循环？](#4在java中如何跳出当前的多重嵌套循环)
- [5.switch语句能否作用在byte上，能否作用在long上，能否作用在String上?](#5switch语句能否作用在byte上能否作用在long上能否作用在string上)
- [6.short s1 = 1; s1 = s1 + 1;有什么错? short s1 = 1; s1 += 1;有什么错?](#6short-s1--1-s1--s1--1有什么错-short-s1--1-s1--1有什么错)
- [7.char型变量中能不能存贮一个中文汉字?为什么?](#7char型变量中能不能存贮一个中文汉字为什么)
- [8.用最有效率的方法算出2乘以8等于几?](#8用最有效率的方法算出2乘以8等于几)
- [9.请设计一个一百亿的计算器](#9请设计一个一百亿的计算器)
- [10.使用final关键字修饰一个变量时，是引用不能变，还是引用的对象不能变？](#10使用final关键字修饰一个变量时是引用不能变还是引用的对象不能变)
- [11."=="和equals方法究竟有什么区别？](#11和equals方法究竟有什么区别)
- [12.静态变量和实例变量的区别？](#12静态变量和实例变量的区别)
- [13.是否可以从一个static方法内部发出对非static方法的调用？](#13是否可以从一个static方法内部发出对非static方法的调用)
- [14.Integer与int的区别](#14integer与int的区别)
- [15.Math.round(11.5)等于多少? Math.round(-11.5)等于多少?](#15mathround115等于多少-mathround-115等于多少)
- [16.请说出作用域public，private，protected，以及不写时的区别](#16请说出作用域publicprivateprotected以及不写时的区别)
- [17.Overload和Override的区别。Overloaded的方法是否可以改变返回值的类型?](#17overload和override的区别overloaded的方法是否可以改变返回值的类型)
- [18.构造器Constructor是否可被override?](#18构造器constructor是否可被override)
- [19.接口是否可继承接口? 抽象类是否可实现(implements)接口? 抽象类是否可继承具体类(concrete class)? 抽象类中是否可以有静态的main方法？](#19接口是否可继承接口-抽象类是否可实现implements接口-抽象类是否可继承具体类concrete-class-抽象类中是否可以有静态的main方法)
- [20.写clone()方法时，通常都有一行代码，是什么？](#20写clone方法时通常都有一行代码是什么)
- [21.面向对象的特征有哪些方面](#21面向对象的特征有哪些方面)
- [22.java中实现多态的机制是什么？](#22java中实现多态的机制是什么)
- [23.abstract class和interface有什么区别?](#23abstract-class和interface有什么区别)
- [24.abstract的method是否可同时是static,是否可同时是native，是否可同时是synchronized?](#24abstract的method是否可同时是static是否可同时是native是否可同时是synchronized)
- [25.什么是内部类？Static Nested Class 和 Inner Class的不同。](#25什么是内部类static-nested-class-和-inner-class的不同)
- [26.内部类可以引用它的包含类的成员吗？有没有什么限制？](#26内部类可以引用它的包含类的成员吗有没有什么限制)
- [27.Anonymous Inner Class (匿名内部类) 是否可以extends(继承)其它类，是否可以implements(实现)interface(接口)?](#27anonymous-inner-class-匿名内部类-是否可以extends继承其它类是否可以implements实现interface接口)
- [28.super.getClass()方法调用](#28supergetclass方法调用)
- [29.String是最基本的数据类型吗?](#29string是最基本的数据类型吗)
- [30.String s = "Hello";s = s + " world!";这两行代码执行后，原始的String对象中的内容到底变了没有？](#30string-s--hellos--s---world这两行代码执行后原始的string对象中的内容到底变了没有)
- [31.是否可以继承String类?](#31是否可以继承string类)
- [32.String s = new String("xyz");创建了几个String Object? 二者之间有什么区别？](#32string-s--new-stringxyz创建了几个string-object-二者之间有什么区别)
- [33.String 和StringBuffer的区别](#33string-和stringbuffer的区别)
- [34.如何把一段逗号分割的字符串转换成一个数组?](#34如何把一段逗号分割的字符串转换成一个数组)
- [35.数组有没有length()这个方法? String有没有length()这个方法？](#35数组有没有length这个方法-string有没有length这个方法)
- [36.下面这条语句一共创建了多少个对象? String s="a"+"b"+"c"+"d";](#36下面这条语句一共创建了多少个对象-string-sabcd)
- [37.try {}里有一个return语句，那么紧跟在这个try后的finally {}里的code会不会被执行，什么时候被执行，在return前还是后?](#37try-里有一个return语句那么紧跟在这个try后的finally-里的code会不会被执行什么时候被执行在return前还是后)
- [38.下面的程序代码输出的结果是多少？](#38下面的程序代码输出的结果是多少)
- [39.final, finally, finalize的区别。](#39final-finally-finalize的区别)
- [40.运行时异常与一般异常有何异同？](#40运行时异常与一般异常有何异同)
- [41.error和exception有什么区别?](#41error和exception有什么区别)
- [42.Java中的异常处理机制的简单原理和应用。](#42java中的异常处理机制的简单原理和应用)
- [43.请写出你最常见到的5个runtime exception。](#43请写出你最常见到的5个runtime-exception)
- [44.java中有几种方法可以实现一个线程？用什么关键字修饰同步方法? stop()和suspend()方法为何不推荐使用？](#44java中有几种方法可以实现一个线程用什么关键字修饰同步方法-stop和suspend方法为何不推荐使用)
- [45.sleep() 和 wait() 有什么区别?](#45sleep-和-wait-有什么区别)
- [46.同步和异步有何异同，在什么情况下分别使用他们？举例说明。](#46同步和异步有何异同在什么情况下分别使用他们举例说明)
- [47.多线程有几种实现方法?同步有几种实现方法?](#47多线程有几种实现方法同步有几种实现方法)
- [48.启动一个线程是用run()还是start()？](#48启动一个线程是用run还是start)
- [49.当一个线程进入一个对象的一个synchronized方法后，其它线程是否可进入此对象的其它方法?](#49当一个线程进入一个对象的一个synchronized方法后其它线程是否可进入此对象的其它方法)
- [50.线程的基本概念、线程的基本状态以及状态之间的关系。](#50线程的基本概念线程的基本状态以及状态之间的关系)
- [51.简述synchronized和java.util.concurrent.locks.Lock的异同 ？](#51简述synchronized和javautilconcurrentlockslock的异同-)
- [52.设计4个线程，其中两个线程每次对j增加1，另外两个线程对j每次减少1。写出程序。](#52设计4个线程其中两个线程每次对j增加1另外两个线程对j每次减少1写出程序)
- [53.子线程循环10次，接着主线程循环100，接着又回到子线程循环10次，接着再回到主线程又循环100，如此循环50次，请写出程序。](#53子线程循环10次接着主线程循环100接着又回到子线程循环10次接着再回到主线程又循环100如此循环50次请写出程序)
- [54.Collection框架中实现比较要实现什么接口](#54collection框架中实现比较要实现什么接口)
- [55.ArrayList和Vector的区别](#55arraylist和vector的区别)
- [56.HashMap和Hashtable的区别](#56hashmap和hashtable的区别)
- [57.List 和 Map 区别?](#57list-和-map-区别)
- [58.List, Set, Map是否继承自Collection接口?](#58list-set-map是否继承自collection接口)
- [59.List、Map、Set三个接口，存取元素时，各有什么特点？](#59listmapset三个接口存取元素时各有什么特点)
- [60.说出ArrayList,Vector, LinkedList的存储性能和特性](#60说出arraylistvector-linkedlist的存储性能和特性)
- [61.去掉一个Vector集合中重复的元素](#61去掉一个vector集合中重复的元素)
- [62.Collection 和 Collections的区别。](#62collection-和-collections的区别)
- [63.Set里的元素是不能重复的，那么用什么方法来区分重复与否呢? 是用==还是equals()? 它们有何区别?](#63set里的元素是不能重复的那么用什么方法来区分重复与否呢-是用还是equals-它们有何区别)
- [64.你所知道的集合类都有哪些？主要方法？](#64你所知道的集合类都有哪些主要方法)
- [65.两个对象值相同(x.equals(y) == true)，但却可有不同的hash code，这句话对不对?](#65两个对象值相同xequalsy--true但却可有不同的hash-code这句话对不对)
- [66.TreeSet里面放对象，如果同时放入了父类和子类的实例对象，那比较时使用的是父类的compareTo方法，还是使用的子类的compareTo方法，还是抛异常！](#66treeset里面放对象如果同时放入了父类和子类的实例对象那比较时使用的是父类的compareto方法还是使用的子类的compareto方法还是抛异常)
- [67.说出一些常用的类，包，接口，请各举5个。](#67说出一些常用的类包接口请各举5个)
- [68.java中有几种类型的流？JDK为每种类型的流提供了一些抽象类以供继承，请说出他们分别是哪些类？](#68java中有几种类型的流jdk为每种类型的流提供了一些抽象类以供继承请说出他们分别是哪些类)
- [69.字节流与字符流的区别](#69字节流与字符流的区别)
- [70.什么是java序列化，如何实现java序列化？或者请解释Serializable接口的作用。](#70什么是java序列化如何实现java序列化或者请解释serializable接口的作用)
- [71.描述一下JVM加载class文件的原理机制?](#71描述一下jvm加载class文件的原理机制)
- [72.heap和stack有什么区别。](#72heap和stack有什么区别)
- [73.GC是什么? 为什么要有GC?](#73gc是什么-为什么要有gc)
- [74.垃圾回收的优点和原理。并考虑2种回收机制。](#74垃圾回收的优点和原理并考虑2种回收机制)
- [75.垃圾回收器的基本原理是什么？垃圾回收器可以马上回收内存吗？有什么办法主动通知虚拟机进行垃圾回收？](#75垃圾回收器的基本原理是什么垃圾回收器可以马上回收内存吗有什么办法主动通知虚拟机进行垃圾回收)
- [76.什么时候用assert。](#76什么时候用assert)
- [77.java中会存在内存泄漏吗，请简单描述。](#77java中会存在内存泄漏吗请简单描述)
- [78.能不能自己写个类，也叫java.lang.String？](#78能不能自己写个类也叫javalangstring)
- [79.获得一个类的类对象有哪些方式？](#79获得一个类的类对象有哪些方式)
- [80.Java代码查错](#80java代码查错)
- [公众号](#公众号)

<!-- /TOC -->

# 1.一个".java"源文件中是否可以包括多个类（不是内部类）？有什么限制？

可以有多个类，但只能有一个public的类，并且public的类名必须与文件名相一致。

# 2.Java有没有goto?

没有，但是 goto 是 java 中的保留字。

# 3.说说&和&&的区别

&和&&都可以用作逻辑与的运算符，表示逻辑与（and），当运算符两边的表达式的结果都为true时，整个运算结果才为true，否则，只要有一方为false，则结果为false。

&&还具有短路的功能，即如果第一个表达式为false，则不再计算第二个表达式，例如，对于if(str != null && !str.equals(“”))表达式，当str为null时，后面的表达式不会执行，所以不会出现NullPointerException如果将&&改为&，则会抛出NullPointerException异常。If(x==33 & ++y>0) y会增长，If(x==33 && ++y>0)不会增长

&还可以用作位运算符，当&操作符两边的表达式不是boolean类型时，&表示按位与操作，我们通常使用0x0f来与一个整数进行&运算，来获取该整数的最低4个bit位，例如，0x31 & 0x0f的结果为0x01。

# 4.在JAVA中如何跳出当前的多重嵌套循环？

在Java中，要想跳出多重循环，可以在外面的循环语句前定义一个标号，然后在里层循环体的代码中使用带有标号的break 语句，即可跳出外层循环。例如，

```java
    ok:
    for (int i = 0; i < 10; i++) {
        for (int j = 0; j < 10; j++) {
            System.out.println("i=" + i + ",j=" + j);
            if (j == 5) break ok;
        }
    }
```
另外，可以不使用标号这种方式，而是让外层的循环条件表达式的结果可以受到里层循环体代码的控制，例如，要在二维数组中查找到某个数字。

```java
    int[][] arr = {{1, 2, 3}, {4, 5, 6, 7}, {9}};
    boolean found = false;
    for (int i = 0; i < arr.length && !found; i++) {
        for (int j = 0; j < arr[i].length; j++) {
            System.out.println("i = " + i + ", j = " + j);
            if (arr[i][j] == 5) {
                found = true;
                break;
            }
        }
    }
```

敲黑板：建议使用第二种，第一种已经被业界淘汰了。

# 5.switch语句能否作用在byte上，能否作用在long上，能否作用在String上?

在switch（expr1）中，expr1只能是一个整数表达式或者枚举常量（更大字体），整数表达式可以是int基本类型或Integer包装类型，由于，byte,short,char都可以隐含转换为int，所以，这些类型以及这些类型的包装类型也是可以的。

switch 不支持 long 类型；从 java1.7开始 switch 开始支持 String，这是 Java 的语法糖。

# 6.short s1 = 1; s1 = s1 + 1;有什么错? short s1 = 1; s1 += 1;有什么错?

对于short s1 = 1; s1 = s1 + 1; 由于s1+1运算时会自动提升表达式的类型，所以结果是int型，再赋值给short类型s1时，编译器将报告需要强制转换类型的错误。

对于short s1 = 1; s1 += 1;由于 += 是java语言规定的运算符，java编译器会对它进行特殊处理，因此可以正确编译。

# 7.char型变量中能不能存贮一个中文汉字?为什么?

char型变量是用来存储Unicode编码的字符的，unicode编码字符集中包含了汉字，所以，char型变量中当然可以存储汉字啦。不过，如果某个特殊的汉字没有被包含在unicode编码字符集中，那么，这个char型变量中就不能存储这个特殊汉字。补充说明：unicode编码占用两个字节，所以，char类型的变量也是占用两个字节。



# 8.用最有效率的方法算出2乘以8等于几?

2 << 3。因为将一个数左移n位，就相当于乘以了2的n次方，那么，一个数乘以8只要将其左移3位即可，而位运算cpu直接支持的，效率最高，所以，2乘以8等於几的最效率的方法是2 << 3。

# 9.请设计一个一百亿的计算器

首先要明白这道题目的考查点是什么，一是要对计算机原理的底层细节要清楚，要知道加减法的位运算原理和计算机中的算术运算会发生越界的情况；二是要具备一定的面向对象的设计思想。

首先，计算机中用固定数量的几个字节来存储的数值，所以计算机中能够表示的数值是有一定的范围的，先以byte 类型的整数为例，它用1个字节进行存储，表示的最大数值范围为-128到+127。-1在内存中对应的二进制数据为11111111，如果两个-1相加，不考虑Java运算时的类型提升，运算后会产生进位，二进制结果为1,11111110，由于进位后超过了byte类型的存储空间，所以进位部分被舍弃，即最终的结果为11111110，也就是-2，这正好利用溢位的方式实现了负数的运算。-128在内存中对应的二进制数据为10000000，如果两个-128相加，不考虑Java运算时的类型提升，运算后会产生进位，二进制结果为1,00000000，由于进位后超过了byte类型的存储空间，所以进位部分被舍弃，即最终的结果为00000000，也就是0，这样的结果显然不符合期望，这说明计算机中的算术运算是会发生越界情况的，两个数值的运算结果不能超过计算机中的该类型的数值范围。由于Java中涉及表达式运算时的类型自动提升，无法用byte类型来做演示这种问题和现象的实验，可以用下面一个使用整数做实验的例子程序体验一下：

```java
    int a = Integer.MAX_VALUE;
    int b = Integer.MAX_VALUE;
    int sum = a + b;
    System.out.println(“a=”+a+”,b=”+b+”,sum=”+sum);
```
先不考虑long类型，由于int的正数范围为2的31次方，表示的最大数值约等于2*1000*1000*1000，也就是20亿的大小，所以，要实现一个一百亿的计算器，我们得自己设计一个类可以用于表示很大的整数，并且提供了与另外一个整数进行加减乘除的功能，大概功能如下：

1）这个类内部有两个成员变量，一个表示符号，另一个用字节数组表示数值的二进制数；

2）有一个构造方法，把一个包含有多位数值的字符串转换到内部的符号和字节数组中；

3）提供加减乘除的功能；

```java
public class BigInteger{
		int sign;
		byte[] val;
		public Biginteger(String val)	{
			sign = ;
			val = ;
		}
		public BigInteger add(BigInteger other)	{
			
		}
		public BigInteger subtract(BigInteger other)	{
			
		}
		public BigInteger multiply(BigInteger other){
			
		}
		public BigInteger divide(BigInteger other){
			
		}
}
```
备注：要想写出这个类的完整代码，是非常复杂的，如果有兴趣的话，可以参看jdk中自带的java.math.BigInteger类的源码。面试的人也知道谁都不可能在短时间内写出这个类的完整代码的，他要的是你是否有这方面的概念和意识，他最重要的还是考查你的能力，所以，不要因为自己无法写出完整的最终结果就放弃答这道题，能做的就是你比别人写得多，证明你比别人强，有这方面的思想意识就可以了，毕竟别人可能连题目的意思都看不懂，什么都没写，要敢于答这道题，即使只答了一部分，那也与那些什么都不懂的人区别出来，拉开了距离，算是矮子中的高个，机会当然就得到了。另外，答案中的框架代码也很重要，体现了一些面向对象设计的功底，特别是其中的方法命名很专业，用的英文单词很精准，这也是能力、经验、专业性、英语水平等多个方面的体现，会给人留下很好的印象，在编程能力和其他方面条件差不多的情况下，英语好除了可以获得更多机会外，薪水可以高出一千元。

# 10.使用final关键字修饰一个变量时，是引用不能变，还是引用的对象不能变？

使用final关键字修饰一个变量时，是指引用变量不能变，引用变量所指向的对象中的内容还是可以改变的。例如，对于如下语句：

```java
final StringBuffer a=new StringBuffer("immutable");
```
执行如下语句将报告编译期错误：
```java
a=new StringBuffer("");
```
但是，执行如下语句则可以通过编译：
```java
a.append(" broken!");
```
有人在定义方法的参数时，可能想采用如下形式来阻止方法内部修改传进来的参数对象：

```java
public void method(final StringBuffer param){}
```
实际上，这是办不到的，在该方法内部仍然可以增加如下代码来修改参数对象：
```java
param.append("a");
```
# 11."=="和equals方法究竟有什么区别？

==操作符专门用来比较两个变量的值是否相等，也就是用于比较变量所对应的内存中所存储的数值是否相同，要比较两个基本类型的数据或两个引用变量是否相等，只能用==操作符。

如果一个变量指向的数据是对象类型的，那么，这时候涉及了两块内存，对象本身占用一块内存（堆内存），变量也占用一块内存，例如Objet obj = new Object();变量obj是一个内存，new Object()是另一个内存，此时，变量obj所对应的内存中存储的数值就是对象占用的那块内存的首地址。对于指向对象类型的变量，如果要比较两个变量是否指向同一个对象，即要看这两个变量所对应的内存中的数值是否相等，这时候就需要用==操作符进行比较。

equals方法是用于比较两个独立对象的内容是否相同，就好比去比较两个人的长相是否相同，它比较的两个对象是独立的。例如，对于下面的代码：

```java
String a=new String("foo");
String b=new String("foo");
```
两条new语句创建了两个对象，然后用a,b这两个变量分别指向了其中一个对象，这是两个不同的对象，它们的首地址是不同的，即a和b中存储的数值是不相同的，所以，表达式a==b将返回false，而这两个对象中的内容是相同的，所以，表达式a.equals(b)将返回true。

在实际开发中，我们经常要比较传递进行来的字符串内容是否等，例如，String input = input.equals(“quit”)，许多人稍不注意就使用==进行比较了，这是错误的，随便从网上找几个项目实战的教学视频看看，里面就有大量这样的错误。记住，字符串的比较基本上都是使用equals方法。

如果一个类没有自己定义equals方法，那么它将继承Object类的equals方法，Object类的equals方法的实现代码如下：

```java
boolean equals(Object o){
return this==o;
}
```
这说明，如果一个类没有自己定义equals方法，它默认的equals方法（从Object 类继承的）就是使用==操作符，也是在比较两个变量指向的对象是否是同一对象，这时候使用equals和使用==会得到同样的结果，如果比较的是两个独立的对象则总返回false。如果你编写的类希望能够比较该类创建的两个实例对象的内容是否相同，那么你必须覆盖equals方法，由你自己写代码来决定在什么情况即可认为两个对象的内容是相同的。

# 12.静态变量和实例变量的区别？

在语法定义上的区别：静态变量前要加static关键字，而实例变量前则不加。

在程序运行时的区别：实例变量属于某个对象的属性，必须创建了实例对象，其中的实例变量才会被分配空间，才能使用这个实例变量。静态变量不属于某个实例对象，而是属于类，所以也称为类变量，只要程序加载了类的字节码，不用创建任何实例对象，静态变量就会被分配空间，静态变量就可以被使用了。总之，实例变量必须创建对象后才可以通过这个对象来使用，静态变量则可以直接使用类名来引用。

例如，对于下面的程序，无论创建多少个实例对象，永远都只分配了一个staticVar变量，并且每创建一个实例对象，这个staticVar就会加1；但是，每创建一个实例对象，就会分配一个instanceVar，即可能分配多个instanceVar，并且每个instanceVar的值都只自加了1次。

```java
public class VariantTest {
    public static int staticVar = 0;
    public int instanceVar = 0;

    public VariantTest() {
        staticVar++;
        instanceVar++;
        System.out.println("staticVar=" + staticVar + ",instanceVar=" + instanceVar);
    }
}
```
# 13.是否可以从一个static方法内部发出对非static方法的调用？

不可以。

因为非static方法是要与对象关联在一起的，必须创建一个对象后，才可以在该对象上进行方法调用，而static方法调用时不需要创建对象，可以直接调用。也就是说，当一个static方法被调用时，可能还没有创建任何实例对象，如果从一个static方法中发出对非static方法的调用，那个非static方法是关联到哪个对象上的呢？这个逻辑无法成立，所以，一个static方法内部发出对非static方法的调用。

# 14.Integer与int的区别

int是java提供的8种原始数据类型之一。Java为每个原始类型提供了封装类，Integer是java为int提供的封装类。int的默认值为0，而Integer的默认值为null，即Integer可以区分出未赋值和值为0的区别，int则无法表达出未赋值的情况，例如，要想表达出没有参加考试和考试成绩为0的区别，则只能使用Integer。在JSP开发中，Integer的默认为null，所以用el表达式在文本框中显示时，值为空白字符串，而int默认的默认值为0，所以用el表达式在文本框中显示时，结果为0，所以，int不适合作为web层的表单数据的类型。

在Hibernate中，如果将OID定义为Integer类型，那么Hibernate就可以根据其值是否为null而判断一个对象是否是临时的，如果将OID定义为了int类型，还需要在hbm映射文件中设置其unsaved-value属性为0。

另外，Integer提供了多个与整数相关的操作方法，例如，将一个字符串转换成整数，Integer中还定义了表示整数的最大值和最小值的常量。

# 15.Math.round(11.5)等于多少? Math.round(-11.5)等于多少?

Math类中提供了三个与取整有关的方法：ceil、floor、round，这些方法的作用与它们的英文名称的含义相对应，例如，ceil的英文意义是天花板，该方法就表示向上取整，Math.ceil(11.3)的结果为12,Math.ceil(-11.3)的结果是-11；floor的英文意义是地板，该方法就表示向下取整，Math.floor(11.6)的结果为11,Math.floor(-11.6)的结果是-12；最难掌握的是round方法，它表示“四舍五入”，算法为Math.floor(x+0.5)，即将原来的数字加上0.5后再向下取整，所以，Math.round(11.5)的结果为12，Math.round(-11.5)的结果为-11。

# 16.请说出作用域public，private，protected，以及不写时的区别

这四个作用域的可见范围如下表所示。

说明：如果在修饰的元素上面没有写任何访问修饰符，则表示friendly。

| 作用域     | 当前类 | 同一package | 子孙类 | 其他package |
|:----------|:------|:-----------|:------|:-----------|
| public    | √     | √          | √    | √          |
| protected | √     | √          | √    | ×          |
| friendly  | √     | √          | ×     | ×          |
| private   | √     | ×         | ×     | ×          |

备注：只要记住了有4种访问权限，4个访问范围，然后将全选和范围在水平和垂直方向上分别按排从小到大或从大到小的顺序排列，就很容易画出上面的图了。

# 17.Overload和Override的区别。Overloaded的方法是否可以改变返回值的类型?

Overload是重载的意思，Override是覆盖的意思，也就是重写。

重载Overload表示同一个类中可以有多个名称相同的方法，但这些方法的参数列表各不相同（即参数个数或类型不同）。

重写Override表示子类中的方法可以与父类中的某个方法的名称和参数完全相同，通过子类创建的实例对象调用这个方法时，将调用子类中的定义方法，这相当于把父类中定义的那个完全相同的方法给覆盖了，这也是面向对象编程的多态性的一种表现。子类覆盖父类的方法时，只能比父类抛出更少的异常，或者是抛出父类抛出的异常的子异常，因为子类可以解决父类的一些问题，不能比父类有更多的问题。子类方法的访问权限只能比父类的更大，不能更小。如果父类的方法是private类型，那么，子类则不存在覆盖的限制，相当于子类中增加了一个全新的方法。

至于Overloaded的方法是否可以改变返回值的类型这个问题，要看你倒底想问什么呢？这个题目很模糊。如果几个Overloaded的方法的参数列表不一样，它们的返回者类型当然也可以不一样。但我估计你想问的问题是：如果两个方法的参数列表完全一样，是否可以让它们的返回值不同来实现重载Overload。这是不行的，我们可以用反证法来说明这个问题，因为我们有时候调用一个方法时也可以不定义返回结果变量，即不要关心其返回结果，例如，我们调用map.remove(key)方法时，虽然remove方法有返回值，但是我们通常都不会定义接收返回结果的变量，这时候假设该类中有两个名称和参数列表完全相同的方法，仅仅是返回类型不同，java就无法确定编程者倒底是想调用哪个方法了，因为它无法通过返回结果类型来判断。

override可以翻译为覆盖，从字面就可以知道，它是覆盖了一个方法并且对其重写，以求达到不同的作用。对我们来说最熟悉的覆盖就是对接口方法的实现，在接口中一般只是对方法进行了声明，而我们在实现时，就需要实现接口声明的所有方法。除了这个典型的用法以外，我们在继承中也可能会在子类覆盖父类中的方法。在覆盖要注意以下的几点：

1）覆盖的方法的标志必须要和被覆盖的方法的标志完全匹配，才能达到覆盖的效果；

2）覆盖的方法的返回值必须和被覆盖的方法的返回一致；

3）覆盖的方法所抛出的异常必须和被覆盖方法的所抛出的异常一致，或者是其子类；

4）被覆盖的方法不能为private，否则在其子类中只是新定义了一个方法，并没有对其进行覆盖。

overload对我们来说可能比较熟悉，可以翻译为重载，它是指我们可以定义一些名称相同的方法，通过定义不同的输入参数来区分这些方法，然后再调用时，JVM就会根据不同的参数样式，来选择合适的方法执行。在使用重载要注意以下的几点：

1）在使用重载时只能通过不同的参数样式。例如，不同的参数类型，不同的参数个数，不同的参数顺序（当然，同一方法内的几个参数类型必须不一样，例如可以是fun(int,float)，但是不能为fun(int,int)）；

2）不能通过访问权限、返回类型、抛出的异常进行重载；

3）方法的异常类型和数目不会对重载造成影响；

4）对于继承来说，如果某一方法在父类中是访问权限是priavte，那么就不能在子类对其进行重载，如果定义的话，也只是定义了一个新方法，而不会达到重载的效果。

# 18.构造器Constructor是否可被override?

构造器Constructor不能被继承，因此不能重写Override，但可以被重载Overload。

# 19.接口是否可继承接口? 抽象类是否可实现(implements)接口? 抽象类是否可继承具体类(concrete class)? 抽象类中是否可以有静态的main方法？

接口可以继承接口。抽象类可以实现(implements)接口，抽象类可以继承具体类。抽象类中可以有静态的main方法。

备注：只要明白了接口和抽象类的本质和作用，这些问题都很好回答，想想看，如果自己作为是java语言的设计者，是否会提供这样的支持，如果不提供的话，有什么理由吗？如果没有道理不提供，那答案就是肯定的了。

只有记住抽象类与普通类的唯一区别就是不能创建实例对象和允许有abstract方法。

# 20.写clone()方法时，通常都有一行代码，是什么？

clone 有缺省行为，super.clone();因为首先要把父类中的成员复制到位，然后才是复制自己的成员。

# 21.面向对象的特征有哪些方面

计算机软件系统是现实生活中的业务在计算机中的映射，而现实生活中的业务其实就是一个个对象协作的过程。面向对象编程就是按现实业务一样的方式将程序代码按一个个对象进行组织和编写，让计算机系统能够识别和理解用对象方式组织和编写的程序代码，这样就可以把现实生活中的业务对象映射到计算机系统中。

面向对象的编程语言有封装、继承 、抽象、多态等4个主要的特征。

1）封装：

封装是保证软件部件具有优良的模块性的基础，封装的目标就是要实现软件部件的“高内聚、低耦合”，防止程序相互依赖性而带来的变动影响。在面向对象的编程语言中，对象是封装的最基本单位，面向对象的封装比传统语言的封装更为清晰、更为有力。面向对象的封装就是把描述一个对象的属性和行为的代码封装在一个“模块”中，也就是一个类中，属性用变量定义，行为用方法进行定义，方法可以直接访问同一个对象中的属性。通常情况下，只要记住让变量和访问这个变量的方法放在一起，将一个类中的成员变量全部定义成私有的，只有这个类自己的方法才可以访问到这些成员变量，这就基本上实现对象的封装，就很容易找出要分配到这个类上的方法了，就基本上算是会面向对象的编程了。把握一个原则：把对同一事物进行操作的方法和相关的方法放在同一个类中，把方法和它操作的数据放在同一个类中。

例如，人要在黑板上画圆，这一共涉及三个对象：人、黑板、圆，画圆的方法要分配给哪个对象呢？由于画圆需要使用到圆心和半径，圆心和半径显然是圆的属性，如果将它们在类中定义成了私有的成员变量，那么，画圆的方法必须分配给圆，它才能访问到圆心和半径这两个属性，人以后只是调用圆的画圆方法、表示给圆发给消息而已，画圆这个方法不应该分配在人这个对象上，这就是面向对象的封装性，即将对象封装成一个高度自治和相对封闭的个体，对象状态（属性）由这个对象自己的行为（方法）来读取和改变。一个更便于理解的例子就是，司机将火车刹住了，刹车的动作是分配给司机，还是分配给火车，显然，应该分配给火车，因为司机自身是不可能有那么大的力气将一个火车给停下来的，只有火车自己才能完成这一动作，火车需要调用内部的离合器和刹车片等多个器件协作才能完成刹车这个动作，司机刹车的过程只是给火车发了一个消息，通知火车要执行刹车动作而已。

2）继承：

在定义和实现一个类的时候，可以在一个已经存在的类的基础之上来进行，把这个已经存在的类所定义的内容作为自己的内容，并可以加入若干新的内容，或修改原来的方法使之更适合特殊的需要，这就是继承。继承是子类自动共享父类数据和方法的机制，这是类之间的一种关系，提高了软件的可重用性和可扩展性。

3）抽象：

抽象就是找出一些事物的相似和共性之处，然后将这些事物归为一个类，这个类只考虑这些事物的相似和共性之处，并且会忽略与当前主题和目标无关的那些方面，将注意力集中在与当前目标有关的方面。例如，看到一只蚂蚁和大象，你能够想象出它们的相同之处，那就是抽象。抽象包括行为抽象和状态抽象两个方面。例如，定义一个Person类，如下：

class Person{

String name;

int age;

}

人本来是很复杂的事物，有很多方面，但因为当前系统只需要了解人的姓名和年龄，所以上面定义的类中只包含姓名和年龄这两个属性，这就是一种抽像，使用抽象可以避免考虑一些与目标无关的细节。我对抽象的理解就是不要用显微镜去看一个事物的所有方面，这样涉及的内容就太多了，而是要善于划分问题的边界，当前系统需要什么，就只考虑什么。

4）多态：

多态是指程序中定义的引用变量所指向的具体类型和通过该引用变量发出的方法调用在编程时并不确定，而是在程序运行期间才确定，即一个引用变量倒底会指向哪个类的实例对象，该引用变量发出的方法调用到底是哪个类中实现的方法，必须在由程序运行期间才能决定。因为在程序运行时才确定具体的类，这样，不用修改源程序代码，就可以让引用变量绑定到各种不同的类实现上，从而导致该引用调用的具体方法随之改变，即不修改程序代码就可以改变程序运行时所绑定的具体代码，让程序可以选择多个运行状态，这就是多态性。多态性增强了软件的灵活性和扩展性。例如，下面代码中的UserDao是一个接口，它定义引用变量userDao指向的实例对象由daofactory.getDao()在执行的时候返回，有时候指向的是UserJdbcDao这个实现，有时候指向的是UserHibernateDao这个实现，这样，不用修改源代码，就可以改变userDao指向的具体类实现，从而导致userDao.insertUser()方法调用的具体代码也随之改变，即有时候调用的是UserJdbcDao的insertUser方法，有时候调用的是UserHibernateDao的insertUser方法：

UserDao userDao = daofactory.getDao();

userDao.insertUser(user);

比喻：人吃饭，你看到的是左手，还是右手？

# 22.java中实现多态的机制是什么？

靠的是父类或接口定义的引用变量可以指向子类或具体实现类的实例对象，而程序调用的方法在运行期才动态绑定，就是引用变量所指向的具体实例对象的方法，也就是内存里正在运行的那个对象的方法，而不是引用变量的类型中定义的方法。

# 23.abstract class和interface有什么区别?

含有abstract修饰符的class即为抽象类，abstract 类不能创建的实例对象。含有abstract方法的类必须定义为abstract class，abstract class类中的方法不必是抽象的。abstract class类中定义抽象方法必须在具体(Concrete)子类中实现，所以，不能有抽象构造方法或抽象静态方法。如果的子类没有实现抽象父类中的所有抽象方法，那么子类也必须定义为abstract类型。

接口（interface）可以说成是抽象类的一种特例，接口中的所有方法都必须是抽象的。接口中的方法定义默认为public abstract类型，接口中的成员变量类型默认为public static final。

下面比较一下两者的语法区别：

1）抽象类可以有构造方法，接口中不能有构造方法。

2）抽象类中可以有普通成员变量，接口中没有普通成员变量

3）抽象类中可以包含非抽象的普通方法，接口中的所有方法必须都是抽象的，不能有非抽象的普通方法。

4） 抽象类中的抽象方法的访问类型可以是public，protected和（默认类型,虽然

eclipse下不报错，但应该也不行），但接口中的抽象方法只能是public类型的，并且默认即为public abstract类型。

5）抽象类中可以包含静态方法，接口中不能包含静态方法

6）抽象类和接口中都可以包含静态成员变量，抽象类中的静态成员变量的访问类型可以任意，但接口中定义的变量只能是public static final类型，并且默认即为public static final类型。

7）一个类可以实现多个接口，但只能继承一个抽象类。



下面接着再说说两者在应用上的区别：

接口更多的是在系统架构设计方法发挥作用，主要用于定义模块之间的通信契约。而抽象类在代码实现方面发挥作用，可以实现代码的重用，例如，模板方法设计模式是抽象类的一个典型应用，假设某个项目的所有Servlet类都要用相同的方式进行权限判断、记录访问日志和处理异常，那么就可以定义一个抽象的基类，让所有的Servlet都继承这个抽象基类，在抽象基类的service方法中完成权限判断、记录访问日志和处理异常的代码，在各个子类中只是完成各自的业务逻辑代码，伪代码如下：

```java
public abstract class BaseServlet extends HttpServlet {

    public final void service(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
        // 记录访问日志, 进行权限判断
        if (具有权限) {
            try {
                doService(request, response);
            } catch (Exception e) {
                // todo：记录异常信息
            }
        }
    }

    protected abstract void doService(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
        //注意访问权限定义成protected，显得既专业，又严谨，因为它是专门给子类用的
    }
}
```

```java
public class MyServlet1 extends BaseServlet {
    protected void doService(HttpServletRequest request, HttpServletResponse response) throws IOExcetion, ServletException {
        // 本Servlet只处理的具体业务逻辑代码
    }
}
```
父类方法中间的某段代码不确定，留给子类干，就用模板方法设计模式。
备注：这道题的思路是先从总体解释抽象类和接口的基本概念，然后再比较两者的语法细节，最后再说两者的应用区别。比较两者语法细节区别的条理是：先从一个类中的构造方法、普通成员变量和方法（包括抽象方法），静态变量和方法，继承性等6个方面逐一去比较回答，接着从第三者继承的角度的回答，特别是最后用了一个典型的例子来展现自己深厚的技术功底。

# 24.abstract的method是否可同时是static,是否可同时是native，是否可同时是synchronized?

abstract的method 不可以是static的，因为抽象的方法是要被子类实现的，而static与子类扯不上关系！

native方法表示该方法要用另外一种依赖平台的编程语言实现的，不存在着被子类实现的问题，所以，它也不能是抽象的，不能与abstract混用。例如，FileOutputSteam类要硬件打交道，底层的实现用的是操作系统相关的api实现，例如，在windows用c语言实现的，所以，查看jdk 的源代码，可以发现FileOutputStream的open方法的定义如下：

private native void open(String name) throws FileNotFoundException;

如果我们要用java调用别人写的c语言函数，我们是无法直接调用的，我们需要按照java的要求写一个c语言的函数，又我们的这个c语言函数去调用别人的c语言函数。由于我们的c语言函数是按java的要求来写的，我们这个c语言函数就可以与java对接上，java那边的对接方式就是定义出与我们这个c函数相对应的方法，java中对应的方法不需要写具体的代码，但需要在前面声明native。

关于synchronized与abstract合用的问题，我觉得也不行，因为在我几年的学习和开发中，从来没见到过这种情况，并且我觉得synchronized应该是作用在一个具体的方法上才有意义。而且，方法上的synchronized同步所使用的同步锁对象是this，而抽象方法上无法确定this是什么。

# 25.什么是内部类？Static Nested Class 和 Inner Class的不同。

内部类就是在一个类的内部定义的类，内部类中不能定义静态成员（静态成员不是对象的特性，只是为了找一个容身之处，所以需要放到一个类中而已，这么一点小事，你还要把它放到类内部的一个类中，过分了啊！提供内部类，不是为让你干这种事情，无聊，不让你干。我想可能是既然静态成员类似c语言的全局变量，而内部类通常是用于创建内部对象用的，所以，把“全局变量”放在内部类中就是毫无意义的事情，既然是毫无意义的事情，就应该被禁止），内部类可以直接访问外部类中的成员变量，内部类可以定义在外部类的方法外面，也可以定义在外部类的方法体中，如下所示：

```java
public class Outer {
    int out_x = 0;

    public void method() {
        //在方法体内部定义的内部类
        class Inner2 {
            public void method() {
                out_x = 3;
            }
        }
        Inner2 inner2 = new Inner2();
    }

    //在方法体外面定义的内部类
    public class Inner1 {

    }
}
```
在方法体外面定义的内部类的访问类型可以是public,protecte,默认的，private等4种类型，这就好像类中定义的成员变量有4种访问类型一样，它们决定这个内部类的定义对其他类是否可见；对于这种情况，我们也可以在外面创建内部类的实例对象，创建内部类的实例对象时，一定要先创建外部类的实例对象，然后用这个外部类的实例对象去创建内部类的实例对象，代码如下：

```java
Outer outer = new Outer();
Outer.Inner1 inner1 = outer.new Innner1();
```
在方法内部定义的内部类前面不能有访问类型修饰符，就好像方法中定义的局部变量一样，但这种内部类的前面可以使用final或abstract修饰符。这种内部类对其他类是不可见的其他类无法引用这种内部类，但是这种内部类创建的实例对象可以传递给其他类访问。这种内部类必须是先定义，后使用，即内部类的定义代码必须出现在使用该类之前，这与方法中的局部变量必须先定义后使用的道理也是一样的。这种内部类可以访问方法体中的局部变量，但是，该局部变量前必须加final修饰符。

对于这些细节，只要在eclipse写代码试试，根据开发工具提示的各类错误信息就可以马上了解到。

在方法体内部还可以采用如下语法来创建一种匿名内部类，即定义某一接口或类的子类的同时，还创建了该子类的实例对象，无需为该子类定义名称：

```java
public class Outer {
    public void start() {
        new Thread(new Runnable() {
            public void run() {
            }
        }).start();
    }
}
```
最后，在方法外部定义的内部类前面可以加上static关键字，从而成为Static Nested Class，它不再具有内部类的特性，所有，从狭义上讲，它不是内部类。Static Nested Class与普通类在运行时的行为和功能上没有什么区别，只是在编程引用时的语法上有一些差别，它可以定义成public、protected、默认的、private等多种类型，而普通类只能定义成public和默认的这两种类型。在外面引用Static Nested Class类的名称为“外部类名.内部类名”。在外面不需要创建外部类的实例对象，就可以直接创建Static Nested Class，例如，假设Inner是定义在Outer类中的Static Nested Class，那么可以使用如下语句创建Inner类：

```java
Outer.Inner inner = new Outer.Inner();
```
由于static Nested Class不依赖于外部类的实例对象，所以，static Nested Class能访问外部类的非static成员变量。当在外部类中访问Static Nested Class时，可以直接使用Static Nested Class的名字，而不需要加上外部类的名字了，在Static Nested Class中也可以直接引用外部类的static的成员变量，不需要加上外部类的名字。

在静态方法中定义的内部类也是Static Nested Class，这时候不能在类前面加static关键字，静态方法中的Static Nested Class与普通方法中的内部类的应用方式很相似，它除了可以直接访问外部类中的static的成员变量，还可以访问静态方法中的局部变量，但是，该局部变量前必须加final修饰符。

备注：首先根据印象说出自己对内部类的总体方面的特点。例如，在两个地方可以定义，可以访问外部类的成员变量，不能定义静态成员，这是大的特点。然后再说一些细节方面的知识，例如，几种定义方式的语法区别，静态内部类，以及匿名内部类。

# 26.内部类可以引用它的包含类的成员吗？有没有什么限制？

完全可以。如果不是静态内部类，那没有什么限制！

如果把静态嵌套类当作内部类的一种特例，那在这种情况下不可以访问外部类的普通成员变量，而只能访问外部类中的静态成员，例如，下面的代码：

```java
class Outer {
    static int x;

    static class Inner {
        void test() {
            syso(x);
        }
    }
}
```
答题时，也要能察言观色，揣摩提问者的心思，显然面试官想知道的是静态内部类不能访问外部类的成员，但如果一上来就顶牛，这不好，要先顺着人家，让人家满意，然后再说特殊情况，让人家吃惊。

# 27.Anonymous Inner Class (匿名内部类) 是否可以extends(继承)其它类，是否可以implements(实现)interface(接口)?

可以继承其他类或实现其他接口。不仅是可以，而是必须!

# 28.super.getClass()方法调用

下面程序的输出结果是多少？

```java
import java.util.Date;

public class Test extends Date {
	public static void main(String[] args) {
		new Test().test();
	}
	
	public void test() {
		System.out.println(super.getClass().getName());
	}
}
```
很奇怪，结果是Test。

在test方法中，直接调用getClass().getName()方法，返回的是Test类名。由于getClass()在Object类中定义成了final，子类不能覆盖该方法，所以，在test方法中调用getClass().getName()方法，其实就是在调用从父类继承的getClass()方法，等效于调用super.getClass().getName()方法，所以，super.getClass().getName()方法返回的也应该是Test。

如果想得到父类的名称，应该用如下代码：

```java
getClass().getSuperClass().getName();
```
# 29.String是最基本的数据类型吗?

基本数据类型包括byte、int、char、long、float、double、boolean和short。

java.lang.String类是final类型的，因此不可以继承这个类、不能修改这个类。为了提高效率节省空间，我们应该用StringBuffer类

# 30.String s = "Hello";s = s + " world!";这两行代码执行后，原始的String对象中的内容到底变了没有？

没有。

因为String被设计成不可变(immutable)类，所以它的所有对象都是不可变对象。在这段代码中，s原先指向一个String对象，内容是 "Hello"，然后我们对s进行了+操作，那么s所指向的那个对象是否发生了改变呢？答案是没有。这时，s不指向原来那个对象了，而指向了另一个 String对象，内容为"Hello world!"，原来那个对象还存在于内存之中，只是s这个引用变量不再指向它了。

通过上面的说明，我们很容易导出另一个结论，如果经常对字符串进行各种各样的修改，或者说，不可预见的修改，那么使用String来代表字符串的话会引起很大的内存开销。因为 String对象建立之后不能再改变，所以对于每一个不同的字符串，都需要一个String对象来表示。这时，应该考虑使用StringBuffer类，它允许修改，而不是每个不同的字符串都要生成一个新的对象。并且，这两种类的对象转换十分容易。

同时，我们还可以知道，如果要使用内容相同的字符串，不必每次都new一个String。例如我们要在构造器中对一个名叫s的String引用变量进行初始化，把它设置为初始值，应当这样做：

```java
public class Demo {
private String s;
...
public Demo {
	s = "Initial Value";
}
...
}
```
而非
```java
s = new String("Initial Value");
```
后者每次都会调用构造器，生成新对象，性能低下且内存开销大，并且没有意义，因为String对象不可改变，所以对于内容相同的字符串，只要一个String对象来表示就可以了。也就说，多次调用上面的构造器创建多个对象，他们的String类型属性s都指向同一个对象。

上面的结论还基于这样一个事实：对于字符串常量，如果内容相同，Java认为它们代表同一个String对象。而用关键字new调用构造器，总是会创建一个新的对象，无论内容是否相同。

至于为什么要把String类设计成不可变类，是它的用途决定的。其实不只String，很多Java标准类库中的类都是不可变的。在开发一个系统的时候，我们有时候也需要设计不可变类，来传递一组相关的值，这也是面向对象思想的体现。不可变类有一些优点，比如因为它的对象是只读的，所以多线程并发访问也不会有任何问题。当然也有一些缺点，比如每个不同的状态都要一个对象来代表，可能造成性能上的问题。所以Java标准类库还提供了一个可变版本，即StringBuffer。

# 31.是否可以继承String类?

String类是final类故不可以继承。

# 32.String s = new String("xyz");创建了几个String Object? 二者之间有什么区别？

两个或一个，”xyz”对应一个对象，这个对象放在字符串常量缓冲区，常量”xyz”不管出现多少遍，都是缓冲区中的那一个。New String每写一遍，就创建一个新的对象，它一句那个常量”xyz”对象的内容来创建出一个新String对象。如果以前就用过’xyz’，这句代表就不会创建”xyz”自己了，直接从缓冲区拿。

# 33.String 和StringBuffer的区别

JAVA平台提供了两个类：String和StringBuffer，它们可以储存和操作字符串，即包含多个字符的字符数据。这个String类提供了数值不可改变的字符串。而这个StringBuffer类提供的字符串进行修改。当你知道字符数据要改变的时候你就可以使用StringBuffer。典型地，你可以使用StringBuffers来动态构造字符数据。另外，String实现了equals方法，new String(“abc”).equals(new String(“abc”)的结果为true,而StringBuffer没有实现equals方法，所以，new StringBuffer(“abc”).equals(new StringBuffer(“abc”)的结果为false。

接着要举一个具体的例子来说明，我们要把1到100的所有数字拼起来，组成一个串。

```java
StringBuffer sbf = new StringBuffer();
for(int i=0;i<100;i++)
{
	sbf.append(i);
}
```
上面的代码效率很高，因为只创建了一个StringBuffer对象，而下面的代码效率很低，因为创建了101个对象。

```java
String str = new String();
for(int i=0;i<100;i++)
{
	str = str + i;
}
```
在讲两者区别时，应把循环的次数搞成10000，然后用endTime-beginTime来比较两者执行的时间差异，最后还要讲讲StringBuilder与StringBuffer的区别。

String覆盖了equals方法和hashCode方法，而StringBuffer没有覆盖equals方法和hashCode方法，所以，将StringBuffer对象存储进Java集合类中时会出现问题。

# 34.如何把一段逗号分割的字符串转换成一个数组?

1）用正则表达式，代码大概为：

```java
String [] result = orgStr.split(",");
```
2）用 StringTokenizer ,代码为：
```java
StringTokenizer tokener = StringTokenizer(orgStr,",");
String [] result = new String[tokener .countTokens()];
Int i=0;
while(tokener.hasNext(){result[i++]=toker.nextToken();}
```
# 35.数组有没有length()这个方法? String有没有length()这个方法？

数组没有length()这个方法，有length的属性。String有有length()这个方法。

# 36.下面这条语句一共创建了多少个对象? String s="a"+"b"+"c"+"d";

对于如下代码：

```java
String s1 = "a";
String s2 = s1 + "b";
String s3 = "a" + "b";
System.out.println(s2 == "ab");
System.out.println(s3 == "ab");
```
第一条语句打印的结果为false，第二条语句打印的结果为true，这说明javac编译可以对字符串常量直接相加的表达式进行优化，不必要等到运行期去进行加法运算处理，而是在编译时去掉其中的加号，直接将其编译成一个这些常量相连的结果。

题目中的第一行代码被编译器在编译时优化后，相当于直接定义了一个”abcd”的字符串，所以，上面的代码应该只创建了一个String对象。写如下两行代码，

```java
String s = "a" + "b" + "c" + "d";
System.out.println(s == "abcd");
```
最终打印的结果应该为true。

# 37.try {}里有一个return语句，那么紧跟在这个try后的finally {}里的code会不会被执行，什么时候被执行，在return前还是后?

也许你的答案是在return之前，但往更细地说，我的答案是在return中间执行，请看下面程序代码的运行结果：

```java
public class Test {

    public static void main(String[] args) {
        System.out.println(Test.test());
    }

    static int test() {
        int x = 1;
        try {
            return x;
        } finally {
            ++x;
        }
    }
}
```
---------执行结果 ---------

1

运行结果是1，为什么呢？主函数调用子函数并得到结果的过程，好比主函数准备一个空罐子，当子函数要返回结果时，先把结果放在罐子里，然后再将程序逻辑返回到主函数。所谓返回，就是子函数说，我不运行了，你主函数继续运行吧，这没什么结果可言，结果是在说这话之前放进罐子里的。

# 38.下面的程序代码输出的结果是多少？

```java
public class SmallT {
    public static void main(String args[]) {
        SmallT t = new SmallT();
        int b = t.get();
        System.out.println(b);
    }

    public int get() {
        try {
            return 1;
        } finally {
            return 2;
        }
    }
}
```
返回的结果是2。

从下面例子的运行结果中可以发现，try中的return语句调用的函数先于finally中调用的函数执行，也就是说return语句先执行，finally语句后执行，所以，返回的结果是2。Return并不是让函数马上返回，而是return语句执行后，将把返回结果放置进函数栈中，此时函数并不是马上返回，它要执行finally语句后才真正开始返回。

在讲解答案时可以用下面的程序来帮助分析：

```java
public class Test {
    /**
     * @param args add by leixiaoshuai 爱笑的架构师
     */
    public static void main(String[] args) {
        System.out.println(new Test().test());
    }

    int test() {
        try {
            return func1();
        } finally {
            return func2();
        }
    }

    int func1() {
        System.out.println("func1");
        return 1;
    }

    int func2() {
        System.out.println("func2");
        return 2;
    }
}
```
-----------执行结果-----------------
func1

func2

2

结论：finally中的代码比return 和break语句后执行。

# 39.final, finally, finalize的区别。

final 用于声明属性，方法和类，分别表示属性不可变，方法不可覆盖，类不可继承。

内部类要访问局部变量，局部变量必须定义成final类型。

finally是异常处理语句结构的一部分，表示总是执行。

finalize是Object类的一个方法，在垃圾收集器执行的时候会调用被回收对象的此方法，可以覆盖此方法提供垃圾收集时的其他资源回收，例如关闭文件等。JVM不保证此方法总被调用

# 40.运行时异常与一般异常有何异同？

异常表示程序运行过程中可能出现的非正常状态，运行时异常表示虚拟机的通常操作中可能遇到的异常，是一种常见运行错误。java编译器要求方法必须声明抛出可能发生的非运行时异常，但是并不要求必须声明抛出未被捕获的运行时异常。

# 41.error和exception有什么区别?

error 表示恢复不是不可能但很困难的情况下的一种严重问题。比如说内存溢出。不可能指望程序能处理这样的情况。 exception 表示一种设计或实现问题。也就是说，它表示如果程序运行正常，从不会发生的情况。

# 42.Java中的异常处理机制的简单原理和应用。

异常是指java程序运行时（非编译）所发生的非正常情况或错误，与现实生活中的事件很相似，现实生活中的事件可以包含事件发生的时间、地点、人物、情节等信息，可以用一个对象来表示，Java使用面向对象的方式来处理异常，它把程序中发生的每个异常也都分别封装到一个对象来表示的，该对象中包含有异常的信息。

Java对异常进行了分类，不同类型的异常分别用不同的Java类表示，所有异常的根类为java.lang.Throwable，Throwable下面又派生了两个子类：Error和Exception，Error 表示应用程序本身无法克服和恢复的一种严重问题，程序只有死的份了，例如，说内存溢出和线程死锁等系统问题。Exception表示程序还能够克服和恢复的问题，其中又分为系统异常和普通异常，系统异常是软件本身缺陷所导致的问题，也就是软件开发人员考虑不周所导致的问题，软件使用者无法克服和恢复这种问题，但在这种问题下还可以让软件系统继续运行或者让软件死掉，例如数组脚本越界（ArrayIndexOutOfBoundsException），空指针异常（NullPointerException）、类转换异常（ClassCastException）；普通异常是运行环境的变化或异常所导致的问题，是用户能够克服的问题，例如，网络断线，硬盘空间不够，发生这样的异常后，程序不应该死掉。

java为系统异常和普通异常提供了不同的解决方案，编译器强制普通异常必须try..catch处理或用throws声明继续抛给上层调用方法处理，所以普通异常也称为checked异常，而系统异常可以处理也可以不处理，所以，编译器不强制用try..catch处理或用throws声明，所以系统异常也称为unchecked异常。

提示答题者：就按照三个级别去思考：虚拟机必须宕机的错误，程序可以死掉也可以不死掉的错误，程序不应该死掉的错误；

# 43.请写出你最常见到的5个runtime exception。

这道题主要考你的代码量到底多大，如果你长期写代码的，应该经常都看到过一些系统方面的异常，你不一定真要回答出5个具体的系统异常，但你要能够说出什么是系统异常，以及几个系统异常就可以了，当然，这些异常完全用其英文名称来写是最好的，如果实在写不出，那就用中文吧，有总比没有强！

所谓系统异常，就是…..，它们都是RuntimeException的子类，在jdk doc中查RuntimeException类，就可以看到其所有的子类列表，也就是看到了所有的系统异常。我比较有印象的系统异常有：NullPointerException、ArrayIndexOutOfBoundsException、ClassCastException。

# 44.java中有几种方法可以实现一个线程？用什么关键字修饰同步方法? stop()和suspend()方法为何不推荐使用？

java5以前，有如下两种：

第一种：new Thread(){}.start();这表示调用Thread子类对象的run方法，new Thread(){}表示一个Thread的匿名子类的实例对象，子类加上run方法后的代码如下：

```java
new Thread(){
	public void run(){
	}
}.start();
```
第二种：

new Thread(new Runnable(){}).start();这表示调用Thread对象接受的Runnable对象的run方法，new Runnable(){}表示一个Runnable的匿名子类的实例对象,runnable的子类加上run方法后的代码如下：

```java
new Thread(new Runnable(){
			public void run(){
			}	
		}
	).start();
```
从java5开始，还有如下一些线程池创建多线程的方式：

```java
ExecutorService pool = Executors.newFixedThreadPool(3)
for(int i=0;i<10;i++)
{
    pool.execute(new Runable(){public void run(){}});
}
Executors.newCachedThreadPool().execute(new Runable(){public void run(){}});
Executors.newSingleThreadExecutor().execute(new Runable(){public void run(){}});
```
有两种实现方法，分别使用new Thread()和new Thread(runnable)形式，第一种直接调用thread的run方法，所以，我们往往使用Thread子类，即new SubThread()。第二种调用runnable的run方法。

有两种实现方法，分别是继承Thread类与实现Runnable接口，用synchronized关键字修饰同步方法。反对使用stop()，是因为它不安全。它会解除由线程获取的所有锁定，而且如果对象处于一种不连贯状态，那么其他线程能在那种状态下检查和修改它们。结果很难检查出真正的问题所在。suspend()方法容易发生死锁。调用suspend()的时候，目标线程会停下来，但却仍然持有在这之前获得的锁定。此时，其他任何线程都不能访问锁定的资源，除非被"挂起"的线程恢复运行。对任何线程来说，如果它们想恢复目标线程，同时又试图使用任何一个锁定的资源，就会造成死锁。所以不应该使用suspend()，而应在自己的Thread类中置入一个标志，指出线程应该活动还是挂起。若标志指出线程应该挂起，便用wait()命其进入等待状态。若标志指出线程应当恢复，则用一个notify()重新启动线程。

# 45.sleep() 和 wait() 有什么区别?

sleep是线程类（Thread）的方法，导致此线程暂停执行指定时间，给执行机会给其他线程，但是监控状态依然保持，到时后会自动恢复。调用sleep不会释放对象锁。 wait是Object类的方法，对此对象调用wait方法导致本线程放弃对象锁，进入等待此对象的等待锁定池，只有针对此对象发出notify方法（或notifyAll）后本线程才进入对象锁定池准备获得对象锁进入运行状态。

sleep就是正在执行的线程主动让出cpu，cpu去执行其他线程，在sleep指定的时间过后，cpu才会回到这个线程上继续往下执行，如果当前线程进入了同步锁，sleep方法并不会释放锁，即使当前线程使用sleep方法让出了cpu，但其他被同步锁挡住了的线程也无法得到执行。wait是指在一个已经进入了同步锁的线程内，让自己暂时让出同步锁，以便其他正在等待此锁的线程可以得到同步锁并运行，只有其他线程调用了notify方法（notify并不释放锁，只是告诉调用过wait方法的线程可以去参与获得锁的竞争了，但不是马上得到锁，因为锁还在别人手里，别人还没释放。如果notify方法后面的代码还有很多，需要这些代码执行完后才会释放锁，可以在notfiy方法后增加一个等待和一些代码，看看效果），调用wait方法的线程就会解除wait状态和程序可以再次得到锁后继续向下运行。对于wait的讲解一定要配合例子代码来说明，才显得自己真明白。

```java
public class MultiThread {
    public static void main(String[] args) {
        new Thread(new Thread1()).start();
        try {
            Thread.sleep(10);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        new Thread(new Thread2()).start();
    }

    private static class Thread1 implements Runnable {
        @Override
        public void run() {
            //由于这里的Thread1和下面的Thread2内部run方法要用同一对象作为监视器，我们这里不能用this，因为在Thread2里面的this和这个Thread1的this不是同一个对象。我们用MultiThread.class这个字节码对象，当前虚拟机里引用这个变量时，指向的都是同一个对象。
            synchronized (MultiThread.class) {
                System.out.println("enter thread1...");
                System.out.println("thread1 is waiting");
                try {
                    //释放锁有两种方式，第一种方式是程序自然离开监视器的范围，也就是离开了synchronized关键字管辖的代码范围，另一种方式就是在synchronized关键字管辖的代码内部调用监视器对象的wait方法。这里，使用wait方法释放锁。
                    MultiThread.class.wait();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println("thread1 is going on...");
                System.out.println("thread1 is being over!");
            }
        }
    }

    private static class Thread2 implements Runnable {
        @Override
        public void run() {
            synchronized (MultiThread.class) {
                System.out.println("enter thread2...");
                System.out.println("thread2 notify other thread can release wait status..");
                //由于notify方法并不释放锁， 即使thread2调用下面的sleep方法休息了10毫秒，但thread1仍然不会执行，因为thread2没有释放锁，所以Thread1无法得不到锁。
                MultiThread.class.notify();
                System.out.println("thread2 is sleeping ten millisecond...");
                try {
                    Thread.sleep(10);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println("thread2 is going on...");
                System.out.println("thread2 is being over!");
            }
        }
    }
}
```
# 46.同步和异步有何异同，在什么情况下分别使用他们？举例说明。

如果数据将在线程间共享。例如正在写的数据以后可能被另一个线程读到，或者正在读的数据可能已经被另一个线程写过了，那么这些数据就是共享数据，必须进行同步存取。

当应用程序在对象上调用了一个需要花费很长时间来执行的方法，并且不希望让程序等待方法的返回时，就应该使用异步编程，在很多情况下采用异步途径往往更有效率。

# 47.多线程有几种实现方法?同步有几种实现方法?

多线程有两种实现方法，分别是继承Thread类与实现Runnable接口。

同步的实现方面有两种，分别是synchronized,wait与notify。

wait():使一个线程处于等待状态，并且释放所持有的对象的lock。

sleep():使一个正在运行的线程处于睡眠状态，是一个静态方法，调用此方法要捕捉InterruptedException异常。

notify():唤醒一个处于等待状态的线程，注意的是在调用此方法的时候，并不能确切的唤醒某一个等待状态的线程，而是由JVM确定唤醒哪个线程，而且不是按优先级。

Allnotity():唤醒所有处入等待状态的线程，注意并不是给所有唤醒线程一个对象的锁，而是让它们竞争。

# 48.启动一个线程是用run()还是start()？

启动一个线程是调用start()方法，使线程就绪状态，以后可以被调度为运行状态，一个线程必须关联一些具体的执行代码，run()方法是该线程所关联的执行代码。

# 49.当一个线程进入一个对象的一个synchronized方法后，其它线程是否可进入此对象的其它方法?

分几种情况：

1）其他方法前是否加了synchronized关键字，如果没加，则能。

2）如果这个方法内部调用了wait，则可以进入其他synchronized方法。

3）如果其他个方法都加了synchronized关键字，并且内部没有调用wait，则不能。

4）如果其他方法是static，它用的同步锁是当前类的字节码，与非静态的方法不能同步，因为非静态的方法用的是this。

# 50.线程的基本概念、线程的基本状态以及状态之间的关系。

一个程序中可以有多条执行线索同时执行，一个线程就是程序中的一条执行线索，每个线程上都关联有要执行的代码，即可以有多段程序代码同时运行，每个程序至少都有一个线程，即main方法执行的那个线程。如果只是一个cpu，它怎么能够同时执行多段程序呢？这是从宏观上来看的，cpu一会执行a线索，一会执行b线索，切换时间很快，给人的感觉是a,b在同时执行，好比大家在同一个办公室上网，只有一条链接到外部网线，其实，这条网线一会为a传数据，一会为b传数据，由于切换时间很短暂，所以，大家感觉都在同时上网。

状态：就绪，运行，synchronize阻塞，wait和sleep挂起，结束。wait必须在synchronized内部调用。

调用线程的start方法后线程进入就绪状态，线程调度系统将就绪状态的线程转为运行状态，遇到synchronized语句时，由运行状态转为阻塞，当synchronized获得锁后，由阻塞转为运行，在这种情况可以调用wait方法转为挂起状态，当线程关联的代码执行完后，线程变为结束状态。

# 51.简述synchronized和java.util.concurrent.locks.Lock的异同 ？

主要相同点：Lock能完成synchronized所实现的所有功能。

主要不同点：Lock有比synchronized更精确的线程语义和更好的性能。synchronized会自动释放锁，而Lock一定要求程序员手工释放，并且必须在finally从句中释放。Lock还有更强大的功能，例如，它的tryLock方法可以非阻塞方式去拿锁。



举例说明（对下面的题用lock进行了改写）：

```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class ThreadTest {
    private int j;
    private Lock lock = new ReentrantLock();

    public static void main(String[] args) {
        ThreadTest tt = new ThreadTest();
        for (int i = 0; i < 2; i++) {
            new Thread(tt.new Adder()).start();
            new Thread(tt.new Subtractor()).start();
        }
    }

    private class Subtractor implements Runnable {
        @Override
        public void run() {
            while (true) {
				/*synchronized (ThreadTest.this) {			
					System.out.println("j--=" + j--);
					//这里抛异常了，锁能释放吗？
				}*/
                lock.lock();
                try {
                    System.out.println("j--=" + j--);
                } finally {
                    lock.unlock();
                }
            }
        }

    }

    private class Adder implements Runnable {
        @Override
        public void run() {
            while (true) {
				/*synchronized (ThreadTest.this) {
				System.out.println("j++=" + j++);	
				}*/
                lock.lock();
                try {
                    System.out.println("j++=" + j++);
                } finally {
                    lock.unlock();
                }
            }
        }

    }
}
```
# 52.设计4个线程，其中两个线程每次对j增加1，另外两个线程对j每次减少1。写出程序。

以下程序使用内部类实现线程，对j增减的时候没有考虑顺序问题。

```java
public class ThreadTest1
{
private int j;
public static void main(String[] args){
    ThreadTest1 tt=new ThreadTest1();
    Inc inc=tt.new Inc();
    Dec dec=tt.new Dec();
    for(int i=0;i<2;i++){
        Thread t=new Thread(inc);
        t.start();
        t=new Thread(dec);
        t.start();
        }
    }
private synchronized void inc(){
    j++;
    System.out.println(Thread.currentThread().getName()+"-inc:"+j);
    }
private synchronized void dec(){
    j--;
    System.out.println(Thread.currentThread().getName()+"-dec:"+j);
    }
class Inc implements Runnable{
    public void run(){
        for(int i=0;i<100;i++){
        inc();
        }
    }
}
class Dec implements Runnable{
    public void run(){
        for(int i=0;i<100;i++){
        dec();
        }
    }
}
}
```
----------随手再写的一个-------------

```java
class A
{
JManger j =new JManager();
main()
{
	new A().call();
}
void call()
{
	for(int i=0;i<2;i++)
	{
		new Thread(
			new Runnable(){ public void run(){while(true){j.accumulate();}}}
		).start();
		new Thread(new Runnable(){ public void run(){while(true){j.sub();}}}).start();
	}
}
}
class JManager
{
	private int j = 0;
	
	public synchronized void subtract()
	{
		j--;
	}
	
	public synchronized void accumulate()
	{
		j++;
	}
	
}
```
# 53.子线程循环10次，接着主线程循环100，接着又回到子线程循环10次，接着再回到主线程又循环100，如此循环50次，请写出程序。

最终的程序代码如下：

```java
public class ThreadTest {

	public static void main(String[] args) {
		new ThreadTest().init();
	}
	public void init()
	{
		final Business business = new Business();
		new Thread(
				new Runnable()
				{
					public void run() {
						for(int i=0;i<50;i++)
						{
							business.SubThread(i);
						}						
					}
					
				}
		
		).start();
		for(int i=0;i<50;i++)
		{
			business.MainThread(i);
		}		
	}
	
	private class Business
	{
		boolean bShouldSub = true;//这里相当于定义了控制该谁执行的一个信号灯
		public synchronized void MainThread(int i)
		{
			if(bShouldSub)
				try {
					this.wait();
				} catch (InterruptedException e) {
					e.printStackTrace();
				}		
				
			for(int j=0;j<5;j++)
			{
				System.out.println(Thread.currentThread().getName() + ":i=" + i +",j=" + j);
			}
			bShouldSub = true;
			this.notify();
		
		}
		
		public synchronized void SubThread(int i)
		{
			if(!bShouldSub)
				try {
					this.wait();
				} catch (InterruptedException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}	
				
			for(int j=0;j<10;j++)
			{
				System.out.println(Thread.currentThread().getName() + ":i=" + i +",j=" + j);
			}
			bShouldSub = false;				
			this.notify();			
		}
	}
}
```
备注：不可能一上来就写出上面的完整代码，最初写出来的代码如下，问题在于两个线程的代码要参照同一个变量，即这两个线程的代码要共享数据，所以，把这两个线程的执行代码搬到同一个类中去：

```java
package com.huawei.interview.lym;
public class ThreadTest {
	private static boolean bShouldMain = false;
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		/*new Thread(){
		public void run()
		{
			for(int i=0;i<50;i++)
			{
				for(int j=0;j<10;j++)
				{
					System.out.println("i=" + i + ",j=" + j);
				}
			}				
		}
		
	}.start();*/		
				
		//final String str = new String("");
		new Thread(
				new Runnable()
				{
					public void run()
					{
						for(int i=0;i<50;i++)
						{
							synchronized (ThreadTest.class) {
								if(bShouldMain)
								{
									try {
										ThreadTest.class.wait();}
									catch (InterruptedException e) {
										e.printStackTrace();
									}
								}
								for(int j=0;j<10;j++)
								{
									System.out.println(
											Thread.currentThread().getName() +
											"i=" + i + ",j=" + j);
								}
								bShouldMain = true;
								ThreadTest.class.notify();
							}							
						}						
					}
				}
		).start();
		
		for(int i=0;i<50;i++)
		{
			synchronized (ThreadTest.class) {
				if(!bShouldMain)
				{
					try {
						ThreadTest.class.wait();}
					catch (InterruptedException e) {
						e.printStackTrace();
					}
				}				
				for(int j=0;j<5;j++)
				{
					System.out.println(
							Thread.currentThread().getName() + 						
							"i=" + i + ",j=" + j);
				}
				bShouldMain = false;
				ThreadTest.class.notify();				
			}			
		}
	}
}
```
下面使用jdk5中的并发库来实现的：
```java
import java.util.concurrent.Executors;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;
import java.util.concurrent.locks.Condition;
public class ThreadTest
{
	private static Lock lock = new ReentrantLock();
	private static Condition subThreadCondition = lock.newCondition();
	private static boolean bBhouldSubThread = false;
	public static void main(String [] args)
	{
		ExecutorService threadPool = Executors.newFixedThreadPool(3);
		threadPool.execute(new Runnable(){
			public void run()
			{
				for(int i=0;i<50;i++)
				{
					lock.lock();					
					try
					{					
						if(!bBhouldSubThread)
							subThreadCondition.await();
						for(int j=0;j<10;j++)
						{
							System.out.println(Thread.currentThread().getName() + ",j=" + j);
						}
						bBhouldSubThread = false;
						subThreadCondition.signal();
					}catch(Exception e)
					{						
					}
					finally
					{
						lock.unlock();
					}
				}			
			}
			
		});
		threadPool.shutdown();
		for(int i=0;i<50;i++)
		{
				lock.lock();					
				try
				{	
					if(bBhouldSubThread)
							subThreadCondition.await();								
					for(int j=0;j<10;j++)
					{
						System.out.println(Thread.currentThread().getName() + ",j=" + j);
					}
					bBhouldSubThread = true;
					subThreadCondition.signal();					
				}catch(Exception e)
				{						
				}
				finally
				{
					lock.unlock();
				}					
		}
	}
}
```
# 54.Collection框架中实现比较要实现什么接口

comparable/comparator

# 55.ArrayList和Vector的区别

这两个类都实现了List接口（List接口继承了Collection接口），他们都是有序集合，即存储在这两个集合中的元素的位置都是有顺序的，相当于一种动态的数组，我们以后可以按位置索引号取出某个元素，，并且其中的数据是允许重复的，这是HashSet之类的集合的最大不同处，HashSet之类的集合不可以按索引号去检索其中的元素，也不允许有重复的元素（本来题目问的与hashset没有任何关系，但为了说清楚ArrayList与Vector的功能，我们使用对比方式，更有利于说明问题）。

接着才说ArrayList与Vector的区别，这主要包括两个方面：.

（1）同步性：

Vector是线程安全的，也就是说是它的方法之间是线程同步的，而ArrayList是线程序不安全的，它的方法之间是线程不同步的。如果只有一个线程会访问到集合，那最好是使用ArrayList，因为它不考虑线程安全，效率会高些；如果有多个线程会访问到集合，那最好是使用Vector，因为不需要我们自己再去考虑和编写线程安全的代码。

备注：对于Vector&ArrayList、Hashtable&HashMap，要记住线程安全的问题，记住Vector与Hashtable是旧的，是java一诞生就提供了的，它们是线程安全的，ArrayList与HashMap是java2时才提供的，它们是线程不安全的。所以，我们讲课时先讲老的。

（2）数据增长：

ArrayList与Vector都有一个初始的容量大小，当存储进它们里面的元素的个数超过了容量时，就需要增加ArrayList与Vector的存储空间，每次要增加存储空间时，不是只增加一个存储单元，而是增加多个存储单元，每次增加的存储单元的个数在内存空间利用与程序效率之间要取得一定的平衡。Vector默认增长为原来两倍，而ArrayList的增长策略在文档中没有明确规定（从源代码看到的是增长为原来的1.5倍）。ArrayList与Vector都可以设置初始的空间大小，Vector还可以设置增长的空间大小，而ArrayList没有提供设置增长空间的方法。

总结：即Vector增长原来的一倍，ArrayList增加原来的0.5倍。

# 56.HashMap和Hashtable的区别

HashMap是Hashtable的轻量级实现（非线程安全的实现），他们都完成了Map接口，主要区别在于HashMap允许空（null）键值（key）,由于非线程安全，在只有一个线程访问的情况下，效率要高于Hashtable。

HashMap允许将null作为一个entry的key或者value，而Hashtable不允许。HashMap把Hashtable的contains方法去掉了，改成containsvalue和containsKey。因为contains方法容易让人引起误解。

Hashtable继承自Dictionary类，而HashMap是Java1.2引进的Map interface的一个实现。

最大的不同是，Hashtable的方法是Synchronize的，而HashMap不是，在多个线程访问Hashtable时，不需要自己为它的方法实现同步，而HashMap 就必须为之提供外同步。

Hashtable和HashMap采用的hash/rehash算法都大概一样，所以性能不会有很大的差异。

就HashMap与HashTable主要从三方面来说：

1）历史原因:Hashtable是基于陈旧的Dictionary类的，HashMap是Java 1.2引进的Map接口的一个实现；

2）同步性:Hashtable是线程安全的，也就是说是同步的，而HashMap是线程序不安全的，不是同步的；

3）值：只有HashMap可以让你将空值作为一个表的条目的key或value

# 57.List 和 Map 区别?

一个是存储单列数据的集合，另一个是存储键和值这样的双列数据的集合，List中存储的数据是有顺序，并且允许重复；Map中存储的数据是没有顺序的，其键是不能重复的，它的值是可以有重复的。

# 58.List, Set, Map是否继承自Collection接口?

List，Set是，Map不是

# 59.List、Map、Set三个接口，存取元素时，各有什么特点？

这样的题属于随意发挥题：这样的题比较考水平，两个方面的水平：一是要真正明白这些内容，二是要有较强的总结和表述能力。如果你明白，但表述不清楚，在别人那里则等同于不明白。

首先，List与Set具有相似性，它们都是单列元素的集合，所以，它们有一个功共同的父接口，叫Collection。Set里面不允许有重复的元素，所谓重复，即不能有两个相等（注意，不是仅仅是相同）的对象 ，即假设Set集合中有了一个A对象，现在我要向Set集合再存入一个B对象，但B对象与A对象equals相等，则B对象存储不进去，所以，Set集合的add方法有一个boolean的返回值，当集合中没有某个元素，此时add方法可成功加入该元素时，则返回true，当集合含有与某个元素equals相等的元素时，此时add方法无法加入该元素，返回结果为false。Set取元素时，没法说取第几个，只能以Iterator接口取得所有的元素，再逐一遍历各个元素。

List表示有先后顺序的集合， 注意，不是那种按年龄、按大小、按价格之类的排序。当我们多次调用add(Obj e)方法时，每次加入的对象就像火车站买票有排队顺序一样，按先来后到的顺序排序。有时候，也可以插队，即调用add(int index,Obj e)方法，就可以指定当前对象在集合中的存放位置。一个对象可以被反复存储进List中，每调用一次add方法，这个对象就被插入进集合中一次，其实，并不是把这个对象本身存储进了集合中，而是在集合中用一个索引变量指向这个对象，当这个对象被add多次时，即相当于集合中有多个索引指向了这个对象，如图x所示。List除了可以以Iterator接口取得所有的元素，再逐一遍历各个元素之外，还可以调用get(index i)来明确说明取第几个。

Map与List和Set不同，它是双列的集合，其中有put方法，定义如下：put(obj key,obj value)，每次存储时，要存储一对key/value，不能存储重复的key，这个重复的规则也是按equals比较相等。取则可以根据key获得相应的value，即get(Object key)返回值为key 所对应的value。另外，也可以获得所有的key的结合，还可以获得所有的value的结合，还可以获得key和value组合成的Map.Entry对象的集合。

List 以特定次序来持有元素，可有重复元素。Set 无法拥有重复元素,内部排序。Map 保存key-value值，value可多值。

HashSet按照hashcode值的某种运算方式进行存储，而不是直接按hashCode值的大小进行存储。例如，"abc" ---> 78，"def" ---> 62，"xyz" ---> 65在hashSet中的存储顺序不是62，65，78。LinkedHashSet按插入的顺序存储，那被存储对象的hashcode方法还有什么作用呢？hashset集合比较两个对象是否相等，首先看hashcode方法是否相等，然后看equals方法是否相等。new 两个Student插入到HashSet中，看HashSet的size，实现hashcode和equals方法后再看size。

同一个对象可以在Vector中加入多次。往集合里面加元素，相当于集合里用一根绳子连接到了目标对象。往HashSet中却加不了多次的。

# 60.说出ArrayList,Vector, LinkedList的存储性能和特性

ArrayList和Vector都是使用数组方式存储数据，此数组元素数大于实际存储的数据以便增加和插入元素，它们都允许直接按序号索引元素，但是插入元素要涉及数组元素移动等内存操作，所以索引数据快而插入数据慢，Vector由于使用了synchronized方法（线程安全），通常性能上较ArrayList差，而LinkedList使用双向链表实现存储，按序号索引数据需要进行前向或后向遍历，但是插入数据时只需要记录本项的前后项即可，所以插入速度较快。

LinkedList也是线程不安全的，LinkedList提供了一些方法，使得LinkedList可以被当作堆栈和队列来使用。

# 61.去掉一个Vector集合中重复的元素

```java
Vector newVector = new Vector();
For (int i=0;i<vector.size();i++)
{
Object obj = vector.get(i);
	if(!newVector.contains(obj);
		newVector.add(obj);
}
```
还有一种简单的方式
```java
HashSet set = new HashSet(vector);
```
# 62.Collection 和 Collections的区别。

Collection是集合类的上级接口，继承与他的接口主要有Set 和List。

Collections是针对集合类的一个帮助类，他提供一系列静态方法实现对各种集合的搜索、排序、线程安全化等操作。

# 63.Set里的元素是不能重复的，那么用什么方法来区分重复与否呢? 是用==还是equals()? 它们有何区别?

Set里的元素是不能重复的，元素重复与否是使用equals()方法进行判断的。

equals()和==方法决定引用值是否指向同一对象equals()在类中被覆盖，为的是当两个分离的对象的内容和类型相配的话，返回真值。

# 64.你所知道的集合类都有哪些？主要方法？

最常用的集合类是 List 和 Map。 List 的具体实现包括 ArrayList 和 Vector，它们是可变大小的列表，比较适合构建、存储和操作任何类型对象的元素列表。 List 适用于按数值索引访问元素的情形。

Map 提供了一个更通用的元素存储方法。 Map 集合类用于存储元素对（称作"键"和"值"），其中每个键映射到一个值。

记的不是方法名，而是思想，知道它们都有增删改查的方法，。因为只要在eclispe下按点操作符，很自然的这些方法就出来了。记住的一些思想就是List类会有get(int index)这样的方法，因为它可以按顺序取元素，而set类中没有get(int index)这样的方法。List和set都可以迭代出所有元素，迭代时先要得到一个iterator对象，所以，set和list类都有一个iterator方法，用于返回那个iterator对象。map可以返回三个集合，一个是返回所有的key的集合，另外一个返回的是所有value的集合，再一个返回的key和value组合成的EntrySet对象的集合，map也有get方法，参数是key，返回值是key对应的value。

# 65.两个对象值相同(x.equals(y) == true)，但却可有不同的hash code，这句话对不对?

对。

如果对象要保存在HashSet或HashMap中，它们的equals相等，那么，它们的hashcode值就必须相等。

如果不是要保存在HashSet或HashMap，则与hashcode没有什么关系了，这时候hashcode不等是可以的，例如arrayList存储的对象就不用实现hashcode，当然，我们没有理由不实现，通常都会去实现的。

# 66.TreeSet里面放对象，如果同时放入了父类和子类的实例对象，那比较时使用的是父类的compareTo方法，还是使用的子类的compareTo方法，还是抛异常！

当前的add方法放入的是哪个对象，就调用哪个对象的compareTo方法，至于这个compareTo方法怎么做，就看当前这个对象的类中是如何编写这个方法的。

代码：

```java
public class Parent implements Comparable {
	private int age = 0;
	public Parent(int age){
		this.age = age;
	}
	public int compareTo(Object o) {
		// TODO Auto-generated method stub
		System.out.println("method of parent");
		Parent o1 = (Parent)o;
		return age>o1.age?1:age<o1.age?-1:0;
	}
}
public class Child extends Parent {
	public Child(){
		super(3);
	}
	public int compareTo(Object o) {
			// TODO Auto-generated method stub
			System.out.println("method of child");
//			Child o1 = (Child)o;
			return 1;
	}
}
public class TreeSetTest {
	/**
	 * @param args
	 */
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		TreeSet set = new TreeSet();
		set.add(new Parent(3));
		set.add(new Child());
		set.add(new Parent(4));
		System.out.println(set.size());
	}
}
```
# 67.说出一些常用的类，包，接口，请各举5个。

要让人家感觉你对java ee开发很熟，所以，不能仅仅只列core java中的那些东西，要多列你在做ssh项目中涉及的那些东西，就写你最近写的那些程序中涉及的那些类。

常用的类：BufferedReader BufferedWriter FileReader FileWirter String Integer

java.util.Date，System，Class，List,HashMap

常用的包：java.lang java.io java.util java.sql ,javax.servlet,org.apache.strtuts.action,org.hibernate

常用的接口：Remote List Map Document NodeList,Servlet,HttpServletRequest,HttpServletResponse,Transaction(Hibernate)、Session(Hibernate),HttpSession

# 68.java中有几种类型的流？JDK为每种类型的流提供了一些抽象类以供继承，请说出他们分别是哪些类？

字节流，字符流。

字节流继承于InputStream OutputStream，字符流继承于InputStreamReader OutputStreamWriter。在java.io包中还有许多其他的流，主要是为了提高性能和使用方便。

# 69.字节流与字符流的区别

要把一片二进制数据数据逐一输出到某个设备中，或者从某个设备中逐一读取一片二进制数据，不管输入输出设备是什么，我们要用统一的方式来完成这些操作，用一种抽象的方式进行描述，这个抽象描述方式起名为IO流，对应的抽象类为OutputStream和InputStream ，不同的实现类就代表不同的输入和输出设备，它们都是针对字节进行操作的。

在应用中，经常要完全是字符的一段文本输出去或读进来，用字节流可以吗？计算机中的一切最终都是二进制的字节形式存在。对于“中国”这些字符，首先要得到其对应的字节，然后将字节写入到输出流。读取时，首先读到的是字节，可是我们要把它显示为字符，我们需要将字节转换成字符。由于这样的需求很广泛，人家专门提供了字符流的包装类。

底层设备永远只接受字节数据，有时候要写字符串到底层设备，需要将字符串转成字节再进行写入。字符流是字节流的包装，字符流则是直接接受字符串，它内部将串转成字节，再写入底层设备，这为我们向IO设别写入或读取字符串提供了一点点方便。

字符向字节转换时，要注意编码的问题，因为字符串转成字节数组，其实是转成该字符的某种编码的字节形式，读取也是反之的道理。

```java
字节流与字符流关系的代码案例：
import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.InputStreamReader;
import java.io.PrintWriter;
public class IOTest {
	public static void main(String[] args) throws Exception {
		String str = "中国人";
		/*FileOutputStream fos = new FileOutputStream("1.txt");
		
		fos.write(str.getBytes("UTF-8"));
		fos.close();*/
		
		/*FileWriter fw = new FileWriter("1.txt");
		fw.write(str);
		fw.close();*/
		PrintWriter pw = new PrintWriter("1.txt","utf-8");
		pw.write(str);
		pw.close();
		
		/*FileReader fr = new FileReader("1.txt");
		char[] buf = new char[1024];
		int len = fr.read(buf);
		String myStr = new String(buf,0,len);
		System.out.println(myStr);*/
		/*FileInputStream fr = new FileInputStream("1.txt");
		byte[] buf = new byte[1024];
		int len = fr.read(buf);
		String myStr = new String(buf,0,len,"UTF-8");
		System.out.println(myStr);*/
		BufferedReader br = new BufferedReader(
				new InputStreamReader(
					new FileInputStream("1.txt"),"UTF-8"	
					)
				);
		String myStr = br.readLine();
		br.close();
		System.out.println(myStr);
	}
}
```
# 70.什么是java序列化，如何实现java序列化？或者请解释Serializable接口的作用。

我们有时候将一个java对象变成字节流的形式传出去或者从一个字节流中恢复成一个java对象，例如，要将java对象存储到硬盘或者传送给网络上的其他计算机，这个过程我们可以自己写代码去把一个java对象变成某个格式的字节流再传输，但是，jre本身就提供了这种支持，我们可以调用OutputStream的writeObject方法来做，如果要让java 帮我们做，要被传输的对象必须实现serializable接口，这样，javac编译时就会进行特殊处理，编译的类才可以被writeObject方法操作，这就是所谓的序列化。需要被序列化的类必须实现Serializable接口，该接口是一个mini接口，其中没有需要实现的方法，implements Serializable只是为了标注该对象是可被序列化的。

例如，在web开发中，如果对象被保存在了Session中，tomcat在重启时要把Session对象序列化到硬盘，这个对象就必须实现Serializable接口。如果对象要经过分布式系统进行网络传输或通过rmi等远程调用，这就需要在网络上传输对象，被传输的对象就必须实现Serializable接口。

# 71.描述一下JVM加载class文件的原理机制?

JVM中类的装载是由ClassLoader和它的子类来实现的，Java ClassLoader 是一个重要的Java运行时系统组件。它负责在运行时查找和装入类文件的类。

# 72.heap和stack有什么区别。

Java的内存分为两类，一类是栈内存，一类是堆内存。栈内存是指程序进入一个方法时，会为这个方法单独分配一块私属存储空间，用于存储这个方法内部的局部变量，当这个方法结束时，分配给这个方法的栈会释放，这个栈中的变量也将随之释放。

堆是与栈作用不同的内存，一般用于存放不放在当前方法栈中的那些数据，例如，使用new创建的对象都放在堆里，所以，它不会随方法的结束而消失。方法中的局部变量使用final修饰后，放在堆中，而不是栈中。

# 73.GC是什么? 为什么要有GC?

GC是垃圾收集的意思（Gabage Collection）,内存处理是编程人员容易出现问题的地方，忘记或者错误的内存回收会导致程序或系统的不稳定甚至崩溃，Java提供的GC功能可以自动监测对象是否超过作用域从而达到自动回收内存的目的，Java语言没有提供释放已分配内存的显示操作方法。

# 74.垃圾回收的优点和原理。并考虑2种回收机制。

Java语言中一个显著的特点就是引入了垃圾回收机制，使c++程序员最头疼的内存管理的问题迎刃而解，它使得Java程序员在编写程序的时候不再需要考虑内存管理。由于有个垃圾回收机制，Java中的对象不再有"作用域"的概念，只有对象的引用才有"作用域"。垃圾回收可以有效的防止内存泄露，有效的使用可以使用的内存。垃圾回收器通常是作为一个单独的低级别的线程运行，不可预知的情况下对内存堆中已经死亡的或者长时间没有使用的对象进行清楚和回收，程序员不能实时的调用垃圾回收器对某个对象或所有对象进行垃圾回收。回收机制有分代复制垃圾回收和标记垃圾回收，增量垃圾回收。

# 75.垃圾回收器的基本原理是什么？垃圾回收器可以马上回收内存吗？有什么办法主动通知虚拟机进行垃圾回收？

对于GC来说，当程序员创建对象时，GC就开始监控这个对象的地址、大小以及使用情况。通常，GC采用有向图的方式记录和管理堆(heap)中的所有对象。通过这种方式确定哪些对象是“可达的”，哪些对象是“不可达的”。当GC确定一些对象为“不可达”时，GC就有责任回收这些内存空间。可以。程序员可以手动执行System.gc()，通知GC运行，但是Java语言规范并不保证GC一定会执行。



# 76.什么时候用assert。

assertion(断言)在软件开发中是一种常用的调试方式，很多开发语言中都支持这种机制。在实现中，assertion就是在程序中的一条语句，它对一个boolean表达式进行检查，一个正确程序必须保证这个boolean表达式的值为true；如果该值为false，说明程序已经处于不正确的状态下，assert将给出警告或退出。一般来说，assertion用于保证程序最基本、关键的正确性。

assertion检查通常在开发和测试时开启。为了提高性能，在软件发布后，assertion检查通常是关闭的。

```java
package com.huawei.interview;
public class AssertTest {
	/**
	 * @param args
	 */
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		int i = 0;
		for(i=0;i<5;i++)
		{
			System.out.println(i);
		}
		//假设程序不小心多了一句--i;
		--i;
		assert i==5;		
	}
}
```
# 77.java中会存在内存泄漏吗，请简单描述。

所谓内存泄露就是指一个不再被程序使用的对象或变量一直被占据在内存中。java中有垃圾回收机制，它可以保证一对象不再被引用的时候，即对象编程了孤儿的时候，对象将自动被垃圾回收器从内存中清除掉。由于Java 使用有向图的方式进行垃圾回收管理，可以消除引用循环的问题，例如有两个对象，相互引用，只要它们和根进程不可达的，那么GC也是可以回收它们的，例如下面的代码可以看到这种情况的内存回收：

```java
package com.huawei.interview;
import java.io.IOException;
public class GarbageTest {
	/**
	 * @param args
	 * @throws IOException
	 */
	public static void main(String[] args) throws IOException {
		// TODO Auto-generated method stub
		try {
			gcTest();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		System.out.println("has exited gcTest!");
		System.in.read();
		System.in.read();		
		System.out.println("out begin gc!");		
		for(int i=0;i<100;i++)
		{
			System.gc();
			System.in.read();	
			System.in.read();	
		}
	}
	private static void gcTest() throws IOException {
		System.in.read();
		System.in.read();		
		Person p1 = new Person();
		System.in.read();
		System.in.read();		
		Person p2 = new Person();
		p1.setMate(p2);
		p2.setMate(p1);
		System.out.println("before exit gctest!");
		System.in.read();
		System.in.read();		
		System.gc();
		System.out.println("exit gctest!");
	}
	private static class Person
	{
		byte[] data = new byte[20000000];
		Person mate = null;
		public void setMate(Person other)
		{
			mate = other;
		}
	}
}
```
Java中的内存泄露的情况：长生命周期的对象持有短生命周期对象的引用就很可能发生内存泄露，尽管短生命周期对象已经不再需要，但是因为长生命周期对象持有它的引用而导致不能被回收，这就是Java中内存泄露的发生场景，通俗地说，就是程序员可能创建了一个对象，以后一直不再使用这个对象，这个对象却一直被引用，即这个对象无用但是却无法被垃圾回收器回收的，这就是Java中可能出现内存泄露的情况，例如，缓存系统，我们加载了一个对象放在缓存中(例如放在一个全局map对象中)，然后一直不再使用它，这个对象一直被缓存引用，但却不再被使用。

检查Java中的内存泄露，一定要让程序将各种分支情况都完整执行到程序结束，然后看某个对象是否被使用过，如果没有，则才能判定这个对象属于内存泄露。

如果一个外部类的实例对象的方法返回了一个内部类的实例对象，这个内部类对象被长期引用了，即使那个外部类实例对象不再被使用，但由于内部类持久外部类的实例对象，这个外部类对象将不会被垃圾回收，这也会造成内存泄露。

主要特点就是清空堆栈中的某个元素，并不是彻底把它从数组中拿掉，而是把存储的总数减少，在拿掉某个元素时，顺便也让它从数组中消失，将那个元素所在的位置的值设置为null即可：

```java
public class Stack {
    private Object[] elements=new Object[10];
    private int size = 0;
    public void push(Object e){
    ensureCapacity();
        elements[size++] = e;
    }
    public Object pop(){
        if( size == 0)
        throw new EmptyStackException();
            return elements[--size];
        }
    private void ensureCapacity(){
        if(elements.length == size){
            Object[] oldElements = elements;
            elements = new Object[2 * elements.length+1];
            System.arraycopy(oldElements,0, elements, 0, size);     
        }
    }
}
```
上面的原理应该很简单，假如堆栈加了10个元素，然后全部弹出来，虽然堆栈是空的，没有我们要的东西，但是这是个对象是无法回收的，这个才符合了内存泄露的两个条件：无用，无法回收。
但是就是存在这样的东西也不一定会导致什么样的后果，如果这个堆栈用的比较少，也就浪费了几个K内存而已，反正我们的内存都上G了，哪里会有什么影响，再说这个东西很快就会被回收的，有什么关系。

例：

```java
public class Bad{
    public static Stack s=Stack();
    static{
        s.push(new Object());
        s.pop(); //这里有一个对象发生内存泄露
        s.push(new Object()); //上面的对象可以被回收了，等于是自愈了
    }
}
```
因为是static，就一直存在到程序退出，但是我们也可以看到它有自愈功能，就是说如果你的Stack最多有100个对象，那么最多也就只有100个对象无法被回收其实这个应该很容易理解，Stack内部持有100个引用，最坏的情况就是他们都是无用的，因为我们一旦放新的进取，以前的引用自然消失！

内存泄露的另外一种情况：当一个对象被存储进HashSet集合中以后，就不能修改这个对象中的那些参与计算哈希值的字段了，否则，对象修改后的哈希值与最初存储进HashSet集合中时的哈希值就不同了，在这种情况下，即使在contains方法使用该对象的当前引用作为的参数去HashSet集合中检索对象，也将返回找不到对象的结果，这也会导致无法从HashSet集合中单独删除当前对象，造成内存泄露。

# 78.能不能自己写个类，也叫java.lang.String？

可以，但在应用的时候，需要用自己的类加载器去加载，否则，系统的类加载器永远只是去加载jre.jar包中的那个java.lang.String。由于在tomcat的web应用程序中，都是由webapp自己的类加载器先自己加载WEB-INF/classess目录中的类，然后才委托上级的类加载器加载，如果我们在tomcat的web应用程序中写一个java.lang.String，这时候Servlet程序加载的就是我们自己写的java.lang.String，但是这么干就会出很多潜在的问题，原来所有用了java.lang.String类的都将出现问题。

虽然java提供了endorsed技术，可以覆盖jdk中的某些类，具体做法是….。但是，能够被覆盖的类是有限制范围，反正不包括java.lang这样的包中的类。例如，运行下面的程序：

```java
package java.lang;
public class String {
	/**
	 * @param args
	 */
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		System.out.println("string");
	}
}
```
报告的错误如下：
java.lang.NoSuchMethodError: main

Exception in thread "main"

这是因为加载了jre自带的java.lang.String，而该类中没有main方法。

# 79.获得一个类的类对象有哪些方式？

答：

方法1：类型.class，例如：String.class

方法2：对象.getClass()，例如：”hello”.getClass()

方法3：Class.forName()，例如：Class.forName(“java.lang.String”)

# 80.Java代码查错

1）

```java
abstract class Name {
    private String name;
    public abstract boolean isStupidName(String name) {}
}
```
这有何错误?
答案: 错。abstract method必须以分号结尾，且不带花括号。

2）

```java
public class Something {
    void doSomething () {
        private String s = "";
        int l = s.length();
    }
}
```
有错吗?
答案: 错。局部变量前不能放置任何访问修饰符 (private，public，和protected)。final可以用来修饰局部变量(final如同abstract和strictfp，都是非访问修饰符，strictfp只能修饰class和method而非variable)。

3）

```java
abstract class Something {
    private abstract String doSomething ();
}
```
这好像没什么错吧?
答案: 错。abstract的methods不能以private修饰。abstract的methods就是让子类implement(实现)具体细节的，怎么可以用private把abstract method封锁起来呢? (同理，abstract method前不能加final)。

4）

```java
public class Something {
    public int addOne(final int x) {
        return ++x;
    }
}
```
这个比较明显。
答案: 错。int x被修饰成final，意味着x不能在addOne method中被修改。

5）

```java
public class Something {
    public static void main(String[] args) {
        Other o = new Other();
        new Something().addOne(o);
    }
    public void addOne(final Other o) {
        o.i++;
    }
}
class Other {
    public int i;
}
```
和上面的很相似，都是关于final的问题，这有错吗?
答案: 正确。在addOne method中，参数o被修饰成final。如果在addOne method里我们修改了o的reference(比如: o = new Other();)，那么如同上例这题也是错的。但这里修改的是o的member vairable(成员变量)，而o的reference并没有改变。

6）

```java
class Something {
    int i;
    public void doSomething() {
        System.out.println("i = " + i);
    }
}
```
有什么错呢? 看不出来啊。
答案: 正确。输出的是"i = 0"。int i属於instant variable (实例变量，或叫成员变量)。instant variable有default value。int的default value是0。

7）

```java
class Something {
    final int i;
    public void doSomething() {
        System.out.println("i = " + i);
    }
}
```
和上面一题只有一个地方不同，就是多了一个final。这难道就错了吗?
答案: 错。final int i是个final的instant variable (实例变量，或叫成员变量)。final的instant variable没有default value，必须在constructor (构造器)结束之前被赋予一个明确的值。可以修改为“final int i = 0;”。

8）

```java
public class Something {
    public static void main(String[] args) {
        Something s = new Something();
        System.out.println("s.doSomething() returns " + doSomething());
    }
    public String doSomething() {
        return "Do something ...";
    }
}
```
看上去很完美。
答案: 错。看上去在main里call doSomething没有什么问题，毕竟两个methods都在同一个class里。但仔细看，main是static的。static method不能直接call non-static methods。可改成"System.out.println("s.doSomething() returns " + s.doSomething());"。同理，static method不能访问non-static instant variable。

9）

此处，Something类的文件名叫OtherThing.java

```java
class Something {
    private static void main(String[] something_to_do) {   
        System.out.println("Do something ...");
    }
}
```
这个好像很明显。
答案: 正确。从来没有人说过Java的Class名字必须和其文件名相同。但public class的名字必须和文件名相同。

10）

```java
interface A{
    int x = 0;
}
class B{
    int x =1;
}
class C extends B implements A {
    public void pX(){
        System.out.println(x);
    }
    public static void main(String[] args) {
        new C().pX();
    }
}
```
答案：错误。在编译时会发生错误(错误描述不同的JVM有不同的信息，意思就是未明确的x调用，两个x都匹配（就象在同时import java.util和java.sql两个包时直接声明Date一样）。对于父类的变量,可以用super.x来明确，而接口的属性默认隐含为 public static final.所以可以通过A.x来明确。
11）

```java
interface Playable {
    void play();
}
interface Bounceable {
    void play();
}
interface Rollable extends Playable, Bounceable {
    Ball ball = new Ball("PingPang");
}
class Ball implements Rollable {
    private String name;
    public String getName() {
        return name;
    }
    public Ball(String name) {
        this.name = name;   
    }
    public void play() {
        ball = new Ball("Football");
        System.out.println(ball.getName());
    }
}
```
这个错误不容易发现。
答案: 错。

"interface Rollable extends Playable, Bounceable"没有问题。interface可继承多个interfaces，所以这里没错。问题出在interface Rollable里的"Ball ball = new Ball("PingPang");"。任何在interface里声明的interface variable (接口变量，也可称成员变量)，默认为public static final。也就是说"Ball ball = new Ball("PingPang");"实际上是"public static final Ball ball = new Ball("PingPang");"。在Ball类的Play()方法中，"ball = new Ball("Football");"改变了ball的reference，而这里的ball来自Rollable interface，Rollable interface里的ball是public static final的，final的object是不能被改变reference的。

因此编译器将在"ball = new Ball("Football");"这里显示有错。

# 公众号
**`Github` 上所有的文章我都会首发在微信公众号『爱笑的架构师』，大家可以关注一下，定时推送技术干货~**

<div align="center">
    <img src="https://cdn.jsdelivr.net/gh/smileArchitect/assets@main/202012/20201205221844.png"></img>
</div>


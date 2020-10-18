<!-- MarkdownTOC -->

- [烂代码登场](#烂代码登场)
- [开始重构烂代码](#开始重构烂代码)
    - [**第一步：定义函数式接口**](#第一步：定义函数式接口)
    - [**第二步：定义模板方法**](#第二步：定义模板方法)
    - [**第三步：传递lambda表达式**](#第三步：传递lambda表达式)
- [总结](#总结)
- [公众号](#公众号)

<!-- /MarkdownTOC -->

>Java8 由Oracle在2014年发布，是继Java5之后最具革命性的版本。
>Java8吸收其他语言的精髓带来了函数式编程，lambda表达式，Stream流等一系列新特性，学会了这些新特性，可以让你实现高效编码优雅编码。
# 烂代码登场

首先引入一个实际的例子，我们常常会写一个dao类来操作数据库，比如查询记录，插入记录等。

下面的代码中实现了查询和插入功能（引入Mybatis三方件）：

```java
public class StudentDao {
    /**
     * 根据学生id查询记录
     * @param id 学生id
     * @return 返回学生对象
     */
    public Student queryOne(int id) {
        SqlSessionFactory sqlSessionFactory = getSqlSessionFactory();
        SqlSession session = null;
        try {
            session = sqlSessionFactory.openSession();
            // 根据id查询指定的student对象
            return session.selectOne("com.coderspace.mapper.student.queryOne", id);
        } finally {
            if (session != null) {
                session.close();
            }
        }
    }
    /**
     * 插入一条学生记录
     * @param student 待插入对象
     * @return true if success, else return false
     */
    public boolean insert(Student student) {
        SqlSessionFactory sqlSessionFactory = getSqlSessionFactory();
        SqlSession session = null;
        try {
            session = sqlSessionFactory.openSession();
            // 向数据库插入student对象
            int rows = session.insert("com.coderspace.mapper.student.insert", student);
            return rows > 0;
        } finally {
            if (session != null) {
                session.close();
            }
        }
    }
  }
```
<div align="center">  <img src="https://uploader.shimo.im/f/MN7BiaxSuGpyBMFe.gif" width=""/> </div><br>

睁大眼睛观察上面的代码可以发现，这两个方法有很多重复的代码。

除了下面这两行，其他的代码都是一样的，都是先获取session，然后执行核心操作，最后关闭session。

```java
// 方法1中核心代码
return session.selectOne("com.coderspace.mapper.student.queryOne", id);
```
```java
// 方法2中核心代码
int rows = session.insert("com.coderspace.mapper.student.insert", student);
```
作为一个有追求的程序员，不，应该叫代码艺术家，是不是应该考虑重构一下。

获取session和关闭session这段代码围绕着具体的核心操作代码，我们可以称这段代码为模板代码。

假如又来了一个需求，需要实现删除student方法，那么你肯定会copy上面的获取session和关闭session代码，这样做有太多重复的代码，作为一名优秀的工程师肯定不会容忍这种事情的发生。

# 开始重构烂代码

怎么重构呢？现在请出我们的主角登场：**环绕执行模式使行为参数化**。

名字是不是很高大上，啥叫行为参数化？上面例子中我们已经观察到了，除了核心操作代码其他代码都是一模一样，那我们是不是可以**将核心操作代码作为入参传入模板方法中**，根据不同的行为分别执行。

变量对象很容易作为参数传入，行为可以作为参数传入吗？

答案是：当然可以，可以采用lambda表达式传入。

下面开始重构之前的例子，主要可以分为三步：

（1）定义函数式接口；

（2）定义模板方法；

（3）传递lambda表达式

所有的环绕执行模式都可以套用上面这三步公式。

## **第一步：定义函数式接口**

```java
@FunctionalInterface
public interface DbOperation<R> {
    /**
     * 通用操作数据库接口
     * @param session 数据库连接session
     * @param mapperId 关联mapper文件id操作
     * @param params 操作参数
     * @return 返回值，R泛型
     */
    R operate(SqlSession session, String mapperId, Object params);
}
```
<div align="center">  <img src="https://uploader.shimo.im/f/gTcrLM9D9DjJVsqJ.gif" width=""/> </div><br>

定义了一个operate抽象方法，接收三个参数，返回泛型R。

## **第二步：定义模板方法**

DbOperation是一个函数式接口，作为入参传入：

```java
public class CommonDao<R> {
    
    public R proccess(DbOperation<R> dbOperation, String mappperId, Object params) {
        SqlSessionFactory sqlSessionFactory = getSqlSessionFactory();
        SqlSession session = null;
        try {
            session = sqlSessionFactory.openSession();
            // 核心操作
            return dbOperation.operate(session, mappperId, params);
        } finally {
            if (session != null) {
                session.close();
            }
        }
    }
  }
```
<div align="center">  <img src="https://uploader.shimo.im/f/UttKDAdGSmrEt8Iv.gif" width=""/> </div><br>

## **第三步：传递lambda表达式**

```java
// 根据id查询学生
String mapperId = "com.coderspace.mapper.student.queryOne";
int studentNo = 123;
CommonDao<Student> commonDao = new CommonDao<>();
// 使用lambda传递具体的行为
Student studentObj = commonDao.proccess(
        (session, mappperId, params) -> session.selectOne(mappperId, params),
        mapperId, studentNo);
// 插入学生记录
String mapperId2 = "com.coderspace.mapper.student.insert";
Student student = new Student("coderspace", 1, 100);
CommonDao<Boolean> commonDao2 = new CommonDao<>();
// 使用lambda传递具体的行为
Boolean successInsert = commonDao2.proccess(
        (session, mappperId, params) -> session.selectOne(mappperId, params),
        mapperId2, student);
```
<div align="center">  <img src="https://uploader.shimo.im/f/YUqcPAZ0GfF0Di5G.gif" width=""/> </div><br>

实现了上面三步，假如要实现删除方法，CommonDao里面一行代码都不用改，只用在调用方传入不同的参数即可实现。

# 总结

环绕执行模式在项目实战中大有用途，如果你发现几行易变的代码外面围绕着一堆固定的代码，这个时候你应该考虑使用lambda环绕执行模式了。

环绕执行模式固有套路请跟我一起大声读三遍：

第一步：定义函数式接口

第二步：定义模板方法

第三步：传递lambda表达式

絮叨：

是不是太太太方便了，要是被经理看到了肯定又要给涨薪，NO，拒绝！


# 公众号
公众号比Github早一到两天更新，如果大家想要实时关注我更新的文章以及分享的干货，可以关注我的公众号。

<div align="center">  <img src="https://uploader.shimo.im/f/zZcm5ufFQNgAN5q4.jpg!thumbnail" width=""/> </div><br>
---
title: Java 异常机制
date: 2022-09-20 19:06:02
categories:
- Java
tags:
- Exception
---

# Java 异常机制

## 异常

异常是指程序在运行过程中发生的，由于外部问题导致的程序运行异常事件，异常的发生往往会中断程序的运行。
在 Java 中，异常本身也是一个对象，程序发生异常就会产生一个异常对象。

### Java 的异常结构

![structure-of-java-exception.png](https://s2.loli.net/2023/03/20/2f4ZoQ6M8xOERGn.png)



* `Throwable` 所有 Error 和 Exception 的超类。它包含了其线程创建时线程执行堆栈的快照，提供了 printStackTrace() 等接口用于获取堆栈跟踪数据等信息。
* `Error` 描述了 Java 运行时系统的内部错误和资源耗尽错误。**你的应用程序中不应该抛出这种类型的错误**。
* `Exception` 描述了程序本身可以捕获并且可以处理的异常，主要分为 RuntimeException 和其他异常。**它们应该在函数声明中被提及并被显示处理**。

> RuntimeException 由编程错误导致的异常。
> 其他异常：由像 I/O 错误这类的问题导致的异常
>
> 《Java 核心技术卷 Ⅰ：基础知识（原书第11版）》

#### Checked Exception & Unchecked Exception

Unchecked Exception（非检查型异常/未检异常）：图中绿色部分，编译器不会检查它，会由 JVM 自动抛出并自动捕获。可以看到， Error 类及其派生子类 以及 RuntimeException 类及其派生子类都属于 Unchecked Exception 。也是我们开发中经常碰到的情况。

Checked Exception（检查型异常/受检异常）：图中红色部分，编译器会检查是否为检查型异常提供了异常处理器。也就是说我们必须为此类异常进行捕获处理。

#### RuntimeException

属于 UncheckedException ，即此类异常 Java 编译器不会检查它，此类异常出现的绝大多数情况是代码本身有问题，应该从代码逻辑上解决并改进。

## 异常处理机制

由上文可知，只有 Exception 及其派生子类需要进行处理，RuntimeException 编译器不会进行检查，JVM 会自动抛出并捕获，而非 RuntimeException 则必须进行手动捕获。当然 RuntimeException 我们也可以手动捕获。下面介绍一下 Java 中提供了哪些方式帮助开发者处理异常的。

### 发现异常： throws 和 throw

要处理异常首先需要发现异常。在 Java 中，我们可以通过 `throws` 关键字来声明异常，通过 **throw** 关键字来抛出异常。这两种方式都是在告诉开发者需要注意，有异常需要处理。

#### throws 和 throw 的区别

throw： 用在方法体中，用于抛出异常。类似这样：

```java
public void checkedParam() {
    throw new RuntimeException();
}
```

throws ：用于声明该方法可能抛出的异常。

用在方法签名中，类似这样：

```java
void findFile() throws IOException
```

如果是用在实现方法上，一般和 throws 关键字一同使用，如下：

```java
public void findFile() throws IOException {
    throw new IOException
}
```

捕获异常： try catch finally

try catch finally 都是 Java 中的关键字，主要用来捕获程序运行期间发生的异常，可以对已知异常的捕获处理，也可以对未知异常处理，来保证程序的正常运行。使用方法类似这样：

```java
public void findFile() {
    try {
        getFile();
    } catch (RuntimeException e){
        // do something if catch RuntimeException
    } finally {
        // always do in here
    }
}
```

> 注意：这三个关键字主要有这三种组合方式： try - catch，try - catch - finally ，try -finally。
>
> catch 语句可以有一个或多个或者没有，finally 至多有一个，try 必须要有。


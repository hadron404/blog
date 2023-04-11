---
title: Java 并发编程
date: 2023-04-11 20:37:01
categories:
- concurrency-programming
tags:
- java
---

## Thread class

Java doc 注释
> A thread is a thread of execution in a program.<br>
> The Java Virtual Machine allows an application to have multiple threads of execution running concurrently.<br> 
> Every thread has a priority.

`java.lang.Thread` 类是 Java 提供的最核心的创建线程的类。

### 线程的状态
1. **初始(NEW)**：新创建了一个线程对象，但还没有调用start()方法。
2. **运行(RUNNABLE)**：Java线程中将就绪（ready）和运行中（running）两种状态笼统的称为“运行”。线程对象创建后，其他线程(比如main线程）调用了该对象的start()方法。该状态的线程位于可运行线程池中，等待被线程调度选中，获取CPU的使用权，此时处于就绪状态（ready）。就绪状态的线程在获得CPU时间片后变为运行中状态（running）。
3. **阻塞(BLOCKED)**：表示线程阻塞于锁。
4. **等待(WAITING)**：进入该状态的线程需要等待其他线程做出一些特定动作（通知或中断）。
5. **超时等待(TIMED_WAITING)**：该状态不同于WAITING，它可以在指定的时间后自行返回。
6. **终止(TERMINATED)**：表示该线程已经执行完毕。

### 创建执行线程的方法

* 定义实现 `java.lang.Runnable` 接口（推荐使用），同时通过 `java.lang.Thread` 的构造函数进行实例化
* 定义一个 `java.lang.Thread` 类的子类，同时重写 `run` 方法。

其中特别说明的是通过 Runnable 接口的方式，其中一种是实现 Runnable 接口，适用于执行任务无返回结果的场景，
一种是实现 Callable 接口，结合 Future 实现，适合于执行任务有返回结果的场景。两个接口都是函数式接口，实现时
可以通过 Lambda 函数或方法引用来简化代码。


## Executor Framework

### ThreadPoolExecutor class

#### 执行流程

1. 判断线程池中核心线程数是否达到 `corePoolSize`，未达到，创建核心线程执行任务
2. 若已达 `corePoolSize`，则判断 `workQueue` 是否已满，未满，则将新任务添加进 `workQueue`
3. 若 `wokrQueue` 已满，则判断线程池中线程数是否已达到 `maximumPoolSize` ，若未达到，则创建一个非核心线程执行任务
4. 若已达 `maximumPoolSize`，则执行线程池的拒绝策略

#### 核心参数

1. `corePoolSize`: 核心线程数（常驻线程）
2. `maximumPoolSize`：线程池中运行的最大线程数（包括核心线程和非核心线程）
3. `keepAliveTime` 线程池中空闲线程（仅适用于非核心线程）所能存活的最长时间
4. `unit` 存活时间单位，与 `keepAliveTime` 搭配使用
5. `workQueue` 存放任务的工作阻塞队列
6. 线程拒绝策略


#### 线程池的拒绝策略

* **AbortPolicy**：中止策略，线程池会抛出异常并中止执行此任务（默认）
* **CallerRunsPolicy**：把任务交给添加此任务的（main）线程来执行；
* **DiscardPolicy**：忽略此任务，忽略最新的一个任务；
* **DiscardOldestPolicy**：忽略最早的任务，最先加入队列的任务。


### Executors class
Java 封装了一系列常用的线程池方法。

## CompletableFuture class

## Atomic
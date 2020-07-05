---
title: Java多线程和锁
categories: Java
tags:
  - Java
  - 锁
  - 多线程
abbrlink: c0c98174
date: 2020-06-26 19:49:15
---

## 线程🔌

一个java程序实际上是一个JVM进程。JVM进程用一个主线程来执行main()方法，在main()方法内部我们又可以启动多个线程。此外，JVM还有负责垃圾回收的其他工作线程等。

💡多线程经常需要读写共享数据，并且需要同步。

#### 多线程实现方式

| extends Thread |

| implements Runnable |

在main()方法中，调用的run()是一个普通的java方法，不会启动新的线程。调用的start()方法是native方法，是C实现的，会启动新线程。

#### 程序计数器为什么是私有的？

程序计数器主要有下面两个作用：

1.  字节码解释器通过改变程序计数器来依次读取指令，从而实现代码的流程控制，如：顺序执行、选择、循环、异常处理。
2.  在多线程的情况下，程序计数器用于记录当前线程执行的位置，从而当线程被切换回来的时候能够知道该线程上次运行到哪儿了。

需要注意的是，如果执行的是 native 方法，那么程序计数器记录的是 undefined 地址，只有执行的是 Java 代码时程序计数器记录的才是下一条指令的地址。

所以，程序计数器私有主要是为了**线程切换后能恢复到正确的执行位置**

#### 什么是上下文切换？

当前任务在执行完 CPU 时间片切换到另一个任务之前会先保存自己的状态，以便下次再切换回这个任务时，可以再加载这个任务的状态。**任务从保存到再加载的过程就是一次上下文切换**。

#### Java线程的状态有以下几种

- New：新创建的线程，尚未执行；
- Runnable：运行中的线程，正在执行`run()`方法的Java代码；
- Blocked：运行中的线程，因为某些操作被阻塞而挂起；
- Waiting：运行中的线程，因为某些操作在等待中；
- Timed Waiting：运行中的线程，因为执行`sleep()`方法正在计时等待；
- Terminated：线程已终止，因为`run()`方法执行完毕。

当线程启动后，它可以在`Runnable`、`Blocked`、`Waiting`和`Timed Waiting`这几个状态之间切换，直到最后变成`Terminated`状态，线程终止。

通过对另一个线程对象调用`join()`方法可以等待其执行结束。

#### 中断线程的方式

t.interrupt(); // 中断t线程，通过检测isInterrupted()判断是否已中断。  
t.running = false; // 标志位置为false。

#### 💡`volatile`关键字的目的是告诉虚拟机

- 每次访问变量时，总是获取主内存的最新值；
- 每次修改变量后，立刻回写到主内存。

`volatile`关键字解决的是可见性问题：当一个线程修改了某个共享变量的值，其他线程能够立刻看到修改后的值。

#### 守护线程

- 守护线程是为其他线程服务的线程；
- 所有非守护线程都执行完毕后，虚拟机退出；
- 守护线程不能持有需要关闭的资源（如打开文件等）。

## 锁🔒

#### 可重入锁

JVM允许同一个线程重复获取同一个锁，这种能被同一个线程反复获取的锁，就叫做`可重入锁`（所以需要判断是第几次锁）

#### 死锁

在获取多个锁的时候，不同线程获取多个不同对象的锁可能导致死锁。此时，两个线程各自持有不同的锁，然后各自试图获取对方手里的锁，造成了双方无限等待下去，这就是`死锁`。  
死锁发生后，没有任何机制能解除死锁，只能`强制结束JVM进程`  
怎么避免？→ 线程获取锁的顺序要一致

#### ReentrantLock

synchronized可以配合wait和notify实现线程在条件不满足时等待，条件满足时唤醒，  
ReentrantLock可以配合Condition实现。

- ReentrantLock获取锁更安全
- 必须先获取到锁，再进入try {...}代码块，最后使用finally保证释放锁
- 可以使用tryLock()尝试获取锁

`Condition`提供的`await()`、`signal()`、`signalAll()`原理和 `synchronized`锁对象的`wait()`、`notify()`、`notifyAll()`是一致的，并且其行为也是一样的：

- `await()`会释放当前锁，进入等待状态
- `signal()`会唤醒某个等待线程
- `signalAll()`会唤醒所有等待线程
- 唤醒线程从`await()`返回后需要重新获得锁

## 同步🎹

`wait和notify用于多线程协调运行`

- 在`synchronized`内部可以调用wait()使线程进入等待状态；
- 必须在已获得的锁对象上调用wait()方法；
- 在synchronized内部可以调用notify()或notifyAll()唤醒其他等待线程；
- 必须在已获得的锁对象上调用notify()或notifyAll()方法；
- 已唤醒的线程还需要重新获得锁后才能继续执行。

## 线程池

#### 常用的线程池

创建这些线程池的方法都被封装到`Executors`这个类中

- FixedThreadPool：线程数固定的线程池；
- CachedThreadPool：线程数根据任务动态调整的线程池；
- SingleThreadExecutor：仅单线程执行的线程池。
- `ScheduledThreadPool`可以定期调度多个任务。

```
// 创建固定大小的线程池
ExecutorService executor = Executors.newFixedThreadPool(3);
```

- 线程池内部维护一组线程，可以高效执行大量小任务；
- `Executors`提供了静态方法创建不同类型的`ExecutorService`；
- 必须调用`shutdown()`关闭`ExecutorService`
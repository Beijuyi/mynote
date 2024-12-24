#java #多线程

> [!参考文档]
> [基本功 | 一文讲清多线程和多线程同步 - 美团技术团队](https://tech.meituan.com/2024/07/19/multi-threading-and-multi-thread-synchronization.html)
> https://mynamelancelot.github.io/java/concurrent.html
> https://www.tutorialspoint.com/java/java_multithreading.htm
> https://www.javaguides.net/p/java-multithreading-utorial.html


# 基本概念

## 程序|线程|进程|上下文

**程序**：用某种编程语言(java、python等)编写，能够完成一定任务或者功能的代码集合,是指令和数据的有序集合，是**一段静态代码**。

**进程**: **应用程序在内存中分配的空间，也就是正在运行的程序**，各个进程之间互不干扰。同时进程保存着程序每一个时刻运行的状态。

**上下文切换**:  （有时也称做进程切换或任务切换）是指 CPU 从一个进程（或线程）切换到另一个进程（或线程）。上下文是指**某一时间点 CPU 寄存器和程序计数器的内容。**

CPU采用**时间片轮转**的方式运行进程：CPU为每个进程分配一个时间段，称作它的时间片。如果在时间片结束时进程还在运行，则**暂停**这个进程的运行，并且CPU分配给另一个进程（这个过程叫做上下文切换）。如果进程在时间片结束前阻塞或结束，则CPU立即进行切换，不用等待时间片用完。
上下文切换通常是计算密集型的，意味着此操作会**消耗大量的 CPU 时间，故线程也不是越多越好**。如何减少系统中上下文切换次数，是提升多线程性能的一个重点课题。

**进程暂停** : 进程暂停时, 计算机会保存当前进程的状态（进程标识，进程使用的资源等），在下一次切换回来时根据之前保存的状态进行恢复，接着继续执行。

**进程让操作系统的并发性成为了可能，而线程让进程的内部并发成为了可能。**

**线程**是一个执行上下文，它包含诸多状态数据：每个线程有自己的执行流、调用栈、错误码、信号掩码、私有数据。Linux内核用任务（Task）表示一个执行流。

进程是一个独立的运行环境，而线程是在进程中执行的一个任务。
**进程是操作系统进行资源分配的基本单位，而线程是操作系统进行调度的基本单位**，即 CPU 分配时间的单位。

**寄存器**: cpu内部的少量的速度很快的闪存，通常存储和访问计算过程的中间值，提高计算机程序的运行速度。
**程序计数器**: 一个专用的寄存器，用于**表明指令序列中 CPU 正在执行的位置**，存的值为正在执行的指令的位置或者下一个将要被执行的指令的位置，具体实现依赖于特定的系统。
CPU通过为每个线程分配CPU时间片来实现多线程机制。
# 线程的状态

操作系统层面:
![](Pasted%20image%2020241220124051.png)

Java 层面:
![](Pasted%20image%2020241220124131.png)

# JDK创建线程的方式

- 继承Thread类
```java
public class ThreadTest {
  public static void main(String[] args) {
    Thread thread = new ThreadDemo();
    thread.start();
  }
}

class ThreadDemo extends Thread {
  @Override
  public void run() {
    System.out.println("do something");
  }
}
```

- 实现Runnable接口
```java
public class ThreadTest {
  public static void main(String[] args) {
    Thread thread = new Thread(new RunnableDemo());
    thread.start();
  }
}

class RunnableDemo implements Runnable {
  @Override
  public void run() {
    System.out.println("do something");
  }
}
```

- 实现Callable接口携带返回值
```java
public class ThreadTest {
  public static void main(String[] args) throws Exception {
    FutureTask<String> futureTask = new FutureTask<>(new CallableDemo());
    Thread thread = new Thread(futureTask);
    thread.start();     //此时Callable接口已经被调用
    String result = futureTask.get();
    System.out.println(result);
  }
}

class CallableDemo implements Callable<String> {
  @Override
  public String call() throws Exception {
    return "do something";
  }
}
```

- 定时器
```java
public class ThreadTest {
  public static void main(String[] args) {
    Timer timer = new Timer();
    timer.schedule(new TimerTaskDemo(),1000);
  }
}

class TimerTaskDemo extends TimerTask {
  @Override
  public void run() {
    System.out.println("do something");
  }
}
```

- 线程池
```java
public class ThreadTest {
  public static void main(String[] args) {
    ExecutorService executor = Executors.newFixedThreadPool(5);
    executor.execute(new ExecutorDemo());
    executor.shutdown();	//线程池销毁，正在执行和在队列等待的线程不会销毁
  }
}

class ExecutorDemo implements Runnable {
  @Override
  public void run() {
    System.out.println("do something");
  }
}
```

# 线程带来的风险

## 线程安全性问题

**出现线程安全性问题的条件**
1. 在多线程的环境下
2. 必须有共享资源
3. 对共享资源进行非原子性操作

**解决线程安全性问题的方法**
- 针对多个线程操作同一共享资源——不共享资源（ThreadLocal、不共享、操作无状态化、不可变）
- 针对多个线程进行非原子性操作——将非原子性操作改成**原子性操作**（使用加锁机制来保证可见性和有序性以及原子性、使用JDK自带的原子性操作的类、JUC提供的相应的并发工具类）

### 活跃性问题

**死锁问题**
指两个或两个以上的进程（或线程）在执行过程中，因争夺资源而造成的一种互相等待的现象（至少两个资源），若无外力作用，它们都将无法推进下去。此时称系统处于死锁状态或系统产生了死锁，这些永远在互相等待的进程称为**死锁进程**。
比喻解释：有两根筷子，线程A和线程B都抢到了一个，互不相让.

**活锁问题**
活锁出现在两个线程互相改变对方的结束条件，最后两个线程谁也无法结束.
比喻解释：线程A和B 在同一个家庭（总共有10w），一个每月攒1w（攒够20w不攒了，出去玩），一个每月花1w（花光去打工)
代码示例
```java
public class LiveLock {
  static volatile int count = 10;

  // 无法解锁，一个想加到20，一个想减到0
  public static void main(String[] args) {
    Thread t1 = new Thread(() -> {
      // 期望减到 0 退出循环
      while (count > 0) {
        try {
          TimeUnit.MILLISECONDS.sleep(500);
        } catch (InterruptedException e) {
          throw new RuntimeException(e);
        }

        count--;
        System.out.println("t1 count: " + count);
      }
    }, "t1");
    Thread t2 = new Thread(() -> {
      // 期望超过 20 退出循环
      while (count < 20) {
        try {
          TimeUnit.MILLISECONDS.sleep(500);
        } catch (InterruptedException e) {
          throw new RuntimeException(e);
        }

        count++;
        System.out.println("t2 count: " + count);
      }
    }, "t2");

    t1.start();
    t2.start();

    try {
      t1.join();
      t2.join();
    } catch (InterruptedException e) {
      throw new RuntimeException(e);
    }

    System.out.println("main end");
  }
}
```

**饥饿问题**
某个线程可能永远抢不到锁, 抢不到资源.
是指如果线程A占用了资源R，线程B又请求封锁R，于是B等待。线程C也请求资源R，当线程A释放了R上的封锁后，系统首先批准了线程C的请求，线程B仍然等待。然后线程D又请求封锁R，当线程C释放了R上的封锁之后，系统又批准了线程D的请求……，线程B可能永远等待.

### 性能问题

1. 线程的生命周期开销非常高。在线程切换时存在CPU上下文切换开销，内存同步也存在着开销。
2. 消耗过多的CPU资源。如果可运行的线程数量多于可用处理器的数量，那么有线程将会被闲置。大量空闲的线程会占用许多内存，给垃圾回收器带来压力，而且大量的线程在竞争CPU资源时还将产生其他性能的开销。
3. 降低稳定性

# synchronized的原理和使用

synchronized是一个：`非公平`、`悲观`、`独享`、`互斥`、`可重入` 锁。自JDK6优化以后，基于JVM可以根据竞争激烈程度，从 `偏向锁` –» `轻量级锁` –» `重量级锁` 升级。

## synchronized的用法
- 修饰方法
修饰普通方法，相当于锁当前对象，调用者，即指 this对象
```java
public synchronized void method() {
  System.out.println("do something");
}
```
修饰静态方法，相当于锁当前类对象，也指 `className.class`
```java
public static synchronized void method() {
  System.out.println("do something");
}
```
- 修饰代码块【可以缩小锁的范围，提升性能】
锁普通对象和锁this
```java
public void method() {
  synchronized(this) {
    System.out.println("do something");
  }
}
```
锁定类对象`className.class`
```java
public void method() {
  synchronized(SynchronizedTest.class) {
    System.out.println("do something");
  }
}
```

## synchronized3种级别锁原理

使用了对象的[MarkWord](https://mynamelancelot.github.io/java/JVM.html#%E5%AF%B9%E8%B1%A1%E7%9A%84%E7%BB%93%E6%9E%84)
1. MarkWord总共有四种状态:无锁状态、偏向锁、轻量级锁和重量级锁。
2. 随着锁的竞争：偏向锁–>轻量级锁–>重量级锁，只能升级
3. 不同锁状态的MarkWord结构不同

 **MarkWord 数据一览**
![](Pasted%20image%2020241220130405.png)

### 偏向锁的原理

**偏向锁状态的消息头结构**
- 消息头锁标志位01
- 是否是偏向锁【0或1】
- 偏向线程ID

**一个对象创建时**
- 如果开启了偏向锁（默认开启），那么对象创建后，markword 值为 0x05 即最后 3 位为 101，这时它的，thread、epoch、age 都为 0
- 偏向锁是默认是延迟的，不会在程序启动时立即生效，如果想避免延迟，可以加 VM 参数`-XX:BiasedLockingStartupDelay=0`来禁用延迟
- 如果没有开启偏向锁，那么对象创建后，markword 值为 0x01 即最后 3 位为 001，这时它的 hashcode、age 都为 0，第一次用到 hashcode 时才会赋值（那么使用hash方法会使偏向锁失效）

**偏向锁衍化过程**
- 如果3个条件都满足【锁标志位–01】，【是否是偏向锁–1】，【偏向线程ID–当前线程】则直接获取到锁，不需要任何CAS操作，而且执行完同步代码块后，并不会修改这3个条件
- 如果一个新的线程尝试获取锁发现【锁标志位–01】，【是否是偏向锁 –1】，【偏向线程ID–不是自己】，则会触发一个check判断偏向锁指向的线程ID锁是否已经使用完了
    - 如果使用完了，则不存在竞争，CAS把偏向线程ID指向自己，这个对象锁就归自己所有了
    - 如果还在使用，竞争成立，则挂起当前线程，并达到原偏向线程释放锁之后将对象锁升级为轻量级锁

**偏向锁的好处**
- 老线程重复使用锁，无需任何CAS操作
- 新线程获取偏向锁，但是没有竞争，只需要在满足条件的时候CAS偏向线程ID即可
- 完美支持重入功能，而且没有任何CAS操作
  
### 轻量级【自旋】锁

**轻量级锁状态的消息头结构**
- 消息头锁标志位00
- 指向栈中锁记录的指针

**轻量级锁衍化过程**
- 创建锁记录（Lock Record）对象，每个线程都的栈帧都会包含一个锁记录的结构，内部可以存储锁定对象的Mark Word![](Pasted%20image%2020241220131038.png)

- 让锁记录中 Object reference 指向锁对象，并尝试用 cas 替换 Object 的 Mark Word，将 Mark Word 的值存入锁记录
![](Pasted%20image%2020241220131119.png)
如果 cas 失败，有两种情况:
- 其它线程已经持有了该 Object 的轻量级锁，这时表明有竞争，进入锁膨胀过程
- 自己执行了 synchronized 锁重入，那么再添加一条 Lock Record 作为重入的计数，如果退出则计数减一
![](Pasted%20image%2020241220131145.png)
**轻量级锁衍化过程细节描述**
- 检查消息头的状态：是否处于无锁状态（偏向锁状态结束，等安全点后释放锁了，就是无锁状态），不是无锁状态挂起当前线程等待锁释放
- 所有争夺的线程都会拷贝一份消息头到各自的线程栈的`lock record`中，叫做`displace Mark Word`，并且记录自己的唯一线程标识符
- CAS把公有的消息头，变成指向自己线程标识符，这个时候消息头的数据结构发生改变，变成线程引用
- CAS成功的线程会执行同步操作，等到需要释放锁时把`displace Mark Word`写回到公有消息头里面，释放锁
- 重入的时候，无需要任何的操作，只需要在自己的`displace Mark Word`中标记一下
- CAS争夺锁失败的线程会发生自旋，自旋一定次数后还是失败的话，会修改消息头的状态为重量级锁，并且自身进入阻塞状态，等待拥有锁的线程执行结束。

**轻量级锁的优势**
​ 在获取锁的耗时不长的时候（比如锁的执行时间短、或者争抢的线程不多可以很快获得锁），通过一定次数的自旋，避免了重量级锁的线程阻塞和切换，提升了响应速度也兼顾了CPU的性能。

### 重量级锁
![](Pasted%20image%2020241220131225.png)
**重量级锁状态的消息头结构**
- 消息头锁标志位10
- 指向互斥量（重量级锁）的指针

**重量级锁衍化过程**
- 所有的竞争线程首先通过CAS拼接到`Contention List`这个队列里面
- 当锁的所有者释放的时候，会把一些线程推入到`EntryList`当中，然后`EntryList`开始CAS竞争，竞争成功就拿到锁，其它线程开始阻塞，等待下一次机会
- 当有锁线程调用obj.wait方法时，它会释放锁进入`WaitSet`，当调用`notify`方法，会从`waitSet`中随机选一个线程，`notifyAll`就是全部进行操作，让他们进入`EntryList`

**重量级锁特点**
- 处于ContentionList、EntryList、WaitSet中的线程都处于阻塞状态，该阻塞是由操作系统来完成的
- Synchronized是非公平锁。Synchronized在线程进入ContentionList时，等待的线程会先尝试自旋获取锁，如果获取不到就进入ContentionList，这明显对于已经进入队列的线程是不公平的。

# 早期线程通信机制

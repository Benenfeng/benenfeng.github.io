---
title: synchronized 几种用法
date: 2019-08-24 16:29:45
tags: java
---

## synchronized 几种用法

synchronized 是 Java 内建的同步机制 （关键字），当一个线程已经获取当前锁时，其他试图获取的线程只能等待或者阻塞在那里。这里介绍synchronized的几种用法：

* 1. 代码块加锁
* 2. 普通方法加锁
* 3. 静态方法加锁


#### synchronized 修饰代码块

demo：

```java
package com.benen.syn;

public class SynchronizedCodeBlock implements  Runnable {

  private int increment = 0;

  public void add() {
    for (int i = 0; i < 5; i++) {
      synchronized (this) {
        try {
          System.out.println(Thread.currentThread().getName() + " number: " + ++increment);
          Thread.sleep(100);
        } catch (InterruptedException e) {
          e.printStackTrace();
        }
      }
    }
  }

  @Override
  public void run() {
    add();
  }

  public static void main(String[] args) {
    SynchronizedCodeBlock block = new SynchronizedCodeBlock();
    Thread t1 = new Thread(block, "thread 1");
    Thread t2 = new Thread(block, "thread 2");
    Thread t3 = new Thread(block, "thread 3");
    t1.start();
    t2.start();
    t3.start();
  }
}
```

结果:
```java
thread 1 number: 1
thread 1 number: 2
thread 1 number: 3
thread 1 number: 4
thread 3 number: 5
thread 3 number: 6
thread 3 number: 7
thread 3 number: 8
thread 3 number: 9
thread 2 number: 10
thread 2 number: 11
thread 2 number: 12
thread 2 number: 13
thread 2 number: 14
thread 1 number: 15
```

明确对象加锁的方式

```java
public void method(Object obj) {
  synchronized(obj) {
        //TODO
  }
}
```

#### synchronized 修饰普通方法

synchronized 修饰普通方法就是在方法前面加synchronized, demo 如下：

```java
package com.benen.syn;

public class SynchronizedGeneralMethod implements Runnable {

  private int increment = 0;

  public synchronized void add() {
    try {
      System.out.println(Thread.currentThread().getName() + " number: " + ++increment);
      Thread.sleep(100);
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
  }

  @Override
  public void run() {
    for (int i = 0; i < 5; i++) {
      add();
    }
  }

  public static void main(String[] args) {
    SynchronizedGeneralMethod block = new SynchronizedGeneralMethod();
    Thread t1 = new Thread(block, "thread 1");
    Thread t2 = new Thread(block, "thread 2");
    Thread t3 = new Thread(block, "thread 3");
    t1.start();
    t2.start();
    t3.start();
  }
}

```

结果：
```java
thread 2 number: 1
thread 2 number: 2
thread 2 number: 3
thread 2 number: 4
thread 3 number: 5
thread 3 number: 6
thread 3 number: 7
thread 3 number: 8
thread 3 number: 9
thread 1 number: 10
thread 1 number: 11
thread 1 number: 12
thread 1 number: 13
thread 1 number: 14
thread 2 number: 15
```


#### synchronized 修饰静态方法

synchronized 修饰静态方法就是在方法前面加synchronized, demo 如下：

```java
package com.benen.syn;

public class SynchronizedStaticMethod implements Runnable {

  private static int increment = 0;

  public static synchronized void add() {
    try {
      System.out.println(Thread.currentThread().getName() + " number: " + ++increment);
      Thread.sleep(100);
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
  }

  @Override
  public void run() {
    for (int i = 0; i < 5; i++) {
      add();
    }
  }

  public static void main(String[] args) {
    SynchronizedStaticMethod block = new SynchronizedStaticMethod();
    Thread t1 = new Thread(block, "thread 1");
    Thread t2 = new Thread(block, "thread 2");
    Thread t3 = new Thread(block, "thread 3");
    t1.start();
    t2.start();
    t3.start();
  }
}

```
结果：
```java
thread 1 number: 1
thread 1 number: 2
thread 1 number: 3
thread 1 number: 4
thread 1 number: 5
thread 3 number: 6
thread 2 number: 7
thread 2 number: 8
thread 2 number: 9
thread 2 number: 10
thread 2 number: 11
thread 3 number: 12
thread 3 number: 13
thread 3 number: 14
thread 3 number: 15
```

#### Conclusion
* **对象锁：** synchronized 关键字加在普通方法或者对象上，如果作用的的对象不是静态的，则取得锁的是对象 。
* **类锁：** synchronized 关键字作用在静态方法，则取得锁的是类，该类的所有对象竞争同一把锁。

若有错误或者不当之处，还请大家批评指正！

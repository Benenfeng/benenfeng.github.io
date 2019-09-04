---
title: 简述 Java 回调
date: 2019-09-03 22:14:27
tags: java
---

## 简述 Java 回调

回调的思想就是： 类A的方法a()调用类B的b()方法 类B的b()方法执行完毕主动调用类A的callback()方法。
考虑上面的描述太抽象，我们用一个具体的例子来讲述回调。

例子： 部门经理想搞一个部门活动，但是不知道什么时间比较合适，就安排自己的秘书去调查安排，秘书调查完以后通知经理具体的时间搞活动大家都方便。

在上面这个例子中，经理就是A类，让秘书找时间就是方法a(), 秘书就是B类，秘书找具体的时间就是方法b(), 秘书找到具体的时间通知经理就是callback()方法。

我们给出代码示例可能就更好懂一点：

**回调接口**

```java

package com.benen.callback;

import java.util.Date;

public interface CallBack {  
    void book(Date date);
}
```

**经理类实现回调接口：**

```java
package com.benen.callback;

import java.util.Date;

public class Manager implements CallBack {

  private Secretary secretary;

  public Manager(Secretary secretary) {
    this.secretary = secretary;
  }

  @Override
  public void book(Date date) {
    System.out.println("the time of the team building is " + date);
  }

  public void plan() {
    System.out.println("Manager plans to hold team building");
    secretary.search(this);
  }
}

```

**秘书类：**

```java
package com.benen.callback;

import java.util.Date;

public class Secretary {

  public void search(CallBack callBack) {

    System.out.println("Secretary starts to search ..");
    try {
      Thread.sleep(2000);
      System.out.println("Secretary is doing ...");
      callBack.book(new Date());
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
    System.out.println("finished ...");
  }
}

```

**测试类：**

```java

package com.benen.callback;

public class Test {
  public static void main(String[] args) {
    Secretary secretary = new Secretary();
    Manager manager = new Manager(secretary);
    manager.plan();
  }
}
```

**Output结果：**
```java
Manager plans to hold team building
Secretary starts to search ..
Secretary is doing ...
the time of the team building is Tue Sep 03 22:32:57 CST 2019
finished ...

```

**总结：**

* 定义回调接口类 CallBack
* 类A 实现回调接口，并且包含类B的对象
* 类B 方法b(CallBack c)
* A调用B对象的b(CallBack c)方法，并将自身做参数传给b(CallBack c)方法， 最后在b(CallBack c)中调用回调方法。

以上就是java回调用法的大致流程，若有错误或者不当之处，还请大家批评指正！

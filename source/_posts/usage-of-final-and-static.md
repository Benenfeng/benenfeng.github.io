---
title: Java 中 static 和 final 关键字的用法
date: 2019-08-31 17:41:45
tags: java
---


## Java 中 static 和 final 关键字的用法

### staitc 关键字的用法

从static字面意思就是“静态的”， java 中静态是相对普通成员（非static)。 

#### **static 关键字主要两个作用：**

- 1. 为某特定数据类型或者对象分配单一的存储空间，于创造多少对象无关；
- 2. 将方法或者属性与类关联绑定，而不是与类对应的对象关联一起，就是在不实例化对象的情况下也可以直接调用类属性和类方法。

static 主要用法包含成员属性、成员方法、代码块和内部类：

##### 1. static 成员变量
   java 中没有全局变量的概念，用static 修饰的变量可以起到全局变量的效果。静态变量属于类，在内存中只有一份，所有该类的对象实例都指向这个内存地址。只要类被加载，静态变量就会被分配内存空间，就可以使用了。使用实例如下：

```java

package com.benen.keyword;

public class TestStatic {

  public static int staticCount = 0;

  public int nonStaticCount = 0;

  public static void main(String[] args) {

    TestStatic testStatic1 = new TestStatic();
    System.out.println("testStatic1 nonStaticCount: " +  testStatic1.nonStaticCount++);
    System.out.println("TestStatic staticCount: " + TestStatic.staticCount++);
    TestStatic testStatic2 = new TestStatic();
    System.out.println("testStatic2 nonStaticCount: " +  testStatic2.nonStaticCount);
    System.out.println("TestStatic staticCount: " + TestStatic.staticCount);
  }
}

output：

testStatic1 nonStaticCount: 0
TestStatic staticCount: 0
testStatic2 nonStaticCount: 0
TestStatic staticCount: 1

```

从上例可知，静态的变量只有一个属于类。需要注意的是局部变量不能用static 修饰。

##### 2. static 成员方法
  
  static 方法是类的方法，不需要创建对象实例就可以被调用，和static变量类似。

```java
package com.benen.keyword;

public class TestStatic {


  public static void print(String input) {
    System.out.println(input);
  }

  public static void main(String[] args) {

    TestStatic.print("This is a static method!");
  }

}

```

##### 3. static 代码块

static 代码块在类中独立于成员属性和成员方法的代码块。加载类是执行静态代码块，如果有多个静态代码块，则依次执行。静态代码块只会执行依次，就是在类加载的时候。

```java

package com.benen.keyword;

public class TestStatic {

  static {
    System.out.println("this is a static block!");
  }

  public TestStatic() {
    System.out.println("This is a constructor!");
  }

  public static void main(String[] args) {

    System.out.println("----------");
    new TestStatic();
    new TestStatic();
  }

}

output:

this is a static block!
----------
This is a constructor!
This is a constructor!

```

##### 4. static 内部类

静态内部类就是被声明static的内部类，不依赖外部类实例对象对象，静态内部类只能访问外部类的静态属性和静态方法，示例如下：

```java

package com.benen.keyword;

public class TestStatic {

  public static int outer = 1;

  public static void outerMethod() {
    System.out.println("out static method.");
  }

  static class StaticInner {

    void innerMethod() {
      System.out.println("outer: " + outer);
      outerMethod();
    }
  }

  public static void main(String[] args) {

    TestStatic.StaticInner inner = new TestStatic.StaticInner();
    inner.innerMethod();
  }

}

output:

outer: 1
out static method.
```

### final 关键字的用法

* final 修饰的类叫最终类，该类不能被继承。final 不能修饰抽象类，因为被final修饰的抽象类不能继承，而抽象类不能被实例化。
* final 修饰的方法不能被重写。
* final 修饰的变量叫常量，常量必须初始化，初始化之后值就不能被修改。


若有错误或者不当之处，还请大家批评指正！


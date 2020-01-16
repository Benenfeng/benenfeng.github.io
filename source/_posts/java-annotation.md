---
title: 简述 Java 注解
date: 2019-09-15 10:41:28
tags: java
---

## 简述 Java 注解

#### 什么是注解

Java 注解可以在编译、类加载和运行时被读取和执行一些处理。通过Java注解可以在不改变原有代码和逻辑的情况下进行一些其他的补充操作。Java 注解可以理解一种特殊意义的注释，然后通过其它代码的解析，补充原代码的操作，如果没有其它代码的解析，注解不会影响原代码的功能。

通常定义注解为：

```java
public @interface MyAnnotation {

}
```


#### 什么是元注解

元注解就是用来修饰注解的注解, 是一种基础注解，通常用在定义注解上面，就是注解的注解，例如：

```java

@Target(ElementType.METHOD)
public @interface MyAnnotation {

}

```
Java 中的元注解有 **@Target**、**@Retention**、**@Documented**、**@Inherited**、**@Repeatable**。

* @Target：指示注释类型所适用的程序元素的种类
* @Retention：注解的保留期即生命周期
* @Documented：将注解元素包含在 JavaDoc 文档中
* @Inherited：允许子类继承超类的注解
* @Repeatable： 可重复的注解

##### @Target

@Target：指示注释类型所适用的程序元素的种类, 例如方法、类或者属性上面。@Target 有如下几种取值：

* ElementType.ANNOTATION_TYPE 作用在注解上面
* ElementType.CONSTRUCTOR 对构造方法进行注解
* ElementType.FIELD 对属性进行注解
* ElementType.LOCAL_VARIABLE 对局部变量进行注解
* ElementType.METHOD 对方法进行注解
* ElementType.PACKAGE 对一个包进行注解
* ElementType.PARAMETER 对一个方法内的参数进行注解
* ElementType.TYPE 给类、接口、枚举注解

##### @Retention

@Retention：注解的保留期即生命周期。 @Retention 有如下几种取值：

* RetentionPolicy.SOURCE：只保留在源码阶段，当前注解编译期可见
* RetentionPolicy.CLASS：会写入 class 文件 但是类加载的时候会被丢弃，虚拟机不需要保存
* RetentionPolicy.RUNTIME：运行时有效，会在虚拟机中保留，通过放射可以获取，非常常用

##### @Documented：

它的作用就是将注解元素包含在 JavaDoc 文档中。

##### @Inherited

@Inherited 说的不是注解本身的继承，而且被注解注解过的类的继承问题，如果超类被@Father 注解过，
而且@Father 被@Inherited 注解过，如果子类继承了超类，并且子类没有被其它注解注解过，那么子类继承了超类的@Father 注解。

##### @Repeatable

@Repeatable 字面意思是可重复的，例如生活中，一个人可能扮演多个角色，既是父亲又是丈夫，也可能一个人会多种技能，既会写代码又会开滴滴，同时还会做饭。
例如一本书既包含前端知识，又包含后端知识， 等等例子。

```java

@Target(ElementType.TYPE)
@Target(RetentionPolicy.RUNTIME)
public @interface Persons {
  Person[] value();
    
}

@Repeatable(Persons.class)
public @interface Person {
  String role() default "";
}

@Person(role="Father")
@Person(role="Husband")
public class Man {
    String name = "Li San"
}


```

#### 注解的属性

注解的属性是注解的成员变量，如上例中的 role 就是@Person 注解的属性， 可以通过上面的赋值方式进行赋值 `@Person(role=”Father)`，
当然如果有多个属性，可以用逗号隔开,加入@Person 里面还有一个属性是skill，就可以这样赋值`@Person(role="Father", skill="cook")`

Java 注解中支持的属性有8种基本类型(int short boolen...)、Sting、 Class、 Enum、 Annotation 和 这些类型的数组。不支持Integer这种类。

#### 内置的注解

下面我们介绍几种常用的内置注解

**@Deprecated：** @Deprecated用来描述在当前系统中已经被废弃不推荐使用的类或方法等。

**@Override：**  主要是用来校验当前被标注的方法是否为重写方法，只作用于ElementType.METHOD。

**@SuppressWarnings：** 过滤掉编译时编译器产生的警告信息。

**@FunctionalInterface：** 函数式接口的注解，例如

```java
@FunctionalInterface
public class Lambda() {
    void fun(int i);
}
```

#### 自定义注解与解析

这里面我们定义一个@BookTopic 的注解，用来给Book类进行说明。**@BookTopic** 如下：


```java

package com.benen.annotation;

import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;

@Retention(RetentionPolicy.RUNTIME)
public @interface BookTopic {

  String[] value() default "";
}

```

**Book**类和测试如下：

```java
package com.benen.annotation;

import java.lang.annotation.Annotation;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

public class Book {

  @BookTopic(value = {"Java", "Spring", "SpringBoot"})
  @Deprecated
  public void bookInfo(String bookName, int bookPrice) {
    System.out.println("书名： " + bookName + " 价格： " + bookPrice);
  }

  public static void main(String[] args) throws NoSuchMethodException, InvocationTargetException, IllegalAccessException {
    Book book = new Book();
    Class<Book> c = Book.class;
    Method mBookInfo = c.getMethod("bookInfo", String.class, int.class);
    iterator(mBookInfo);
    mBookInfo.invoke(book, new Object[]{"Spring Boot 编程思想", 60});
    printAllAnnotation(mBookInfo);
  }


  public static void iterator(Method method) {
    if(method.isAnnotationPresent(BookTopic.class)){
      BookTopic myAnnotation = method.getAnnotation(BookTopic.class);
      String[] values = myAnnotation.value();
      System.out.printf("这本书关于: ");
      for (String str:values)
        System.out.printf(str + " ");
      System.out.println();
    }
  }

  public static void printAllAnnotation(Method method) {
    System.out.println(method.getName() + "方法包含的注解有：");
    Annotation[] annotations = method.getAnnotations();
    for(Annotation annotation : annotations){
      System.out.println(annotation);
    }
  }

}
```

**Output:**
```java
这本书关于: Java Spring SpringBoot 
书名： Spring Boot 编程思想 价格： 60
bookInfo方法包含的注解有：
@com.benen.annotation.BookTopic(value=[Java, Spring, SpringBoot])
@java.lang.Deprecated()
```

#### 总结

Java 注解是对原代码的一种补充，可以编写出更灵活的代码， 通常注解主要给编译器及工具类型的软件用的。
由于注解的提取需要借助于 Java 的反射机制，而反射机制又比较慢和脆弱，所以需要谨慎使用Java 注解。

#### 参考文献
* https://www.runoob.com/w3cnote/java-annotation.html
---
title: 简述 Java 反射
date: 2019-09-07 18:39:57
tags: java
---

![image](/images/sky1.jpg)

## 简述 Java 反射

反射即在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能调用它的任意一个方法和属性。这种动态获取信息及动态调用对象方法的功能叫Java的反射机制。

### 反射机制常用的功能：

* 在运行中分析类的能力和属性
* 在运行中构造任意类的对象
* 在运行时调用一个对象的方法和属性
* 修改构造函数、方法和属性的可见性
* 用于生成动态代理


### 反射机制相关的 Java 类


* Class类：代表类的实体，在运行的Java应用程序中表示类和接口。 

* Field类：代表类的成员属性。

* Method类：代表类的方法。

* Constructor类：代表类的构造方法。

* Array类：提供了动态创建数组，以及访问数组的元素的静态方法。

#### Class 类

在程序运行期间，Java 运行时系统为所有对象维护一个被称为运行时的标识，这个信息跟踪每个对象所属的类，可以通过专门的 Java 类访问这些信息，保存这些信息的类就是 Class。
Java 中，每个 class 都有一个对应的 Class 对象， 用于表示这个类的类型信息。

Class 类相关的常用的方法：

* getName()：获得类的完整名字。 

* getFields()：获得类的public类型的属性。

* getDeclaredFields()：获得类的所有属性。

* getMethods()：获得类的public类型的方法。

* getDeclaredMethods()：获得类的所有方法。

* getMethod(String name, Class[] parameterTypes)：获得类的特定方法，name参数指定方法的名字，parameterTypes参数指定方法的参数类型。

* getConstrutors()：获得类的public类型的构造方法。

* getConstrutor(Class[] parameterTypes)：获得类的特定构造方法，parameterTypes参数指定构造方法的参数类型。



#### Field 类

Field 代表类的成员属性， 常用的方法如下：

* equals(Object obj) 属性与obj相等则返回true
* get(Object obj) 获得obj中对应的属性值
* set(Object obj, Object value)	设置obj中对应属性值

#### Method 类

Method 代表类的方法，常用方法如下：

* invoke(Object obj, Object... args)	传递object对象及参数调用该对象对应的方法

#### Constructor 类

Constructor 代表类的构造方法， 常用方法如下：

* newInstance()：通过类的不带参数的构造方法创建这个类的一个对象。

### 反射的使用


我们先定义一个 **Person 类**， 然后通过反射操作这个类来演示这个反射的使用。

##### Person Class

```java

package com.benen.reflect;

public class Person {

    private int age;

    private String name;

    private String sex;

    public Person() {
        this.age = 12;
        this.name = "Bruce";
        this.sex = "man";
    }

    private Person(int age, String name, String sex) {
        this.age = age;
        this.name = name;
        this.sex = sex;
    }

    public int getAge() {
        return age;
    }

    public String getName() {
        return name;
    }

    public String getSex() {
        return sex;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public void generalMethod(String in) {
        System.out.println("reflectMethod, input is :" + in);
    }

    @Override
    public String toString() {
        return "Person{" +
            "age=" + age +
            ", name='" + name + '\'' +
            ", sex='" + sex + '\'' +
            '}';
    }
}
```

##### 反射demo类 ReflectDemo：

```java
package com.benen.reflect;

import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.Method;

public class ReflectDemo {

  public static final String PERSON = "com.benen.reflect.Person";

  /**
   * test creating object.
   */
  public static void reflectNewInstance() {

    try {
      Class personClass = Class.forName(ReflectDemo.PERSON);
      Object obj = personClass.newInstance();
      Person person = (Person) obj;
      person.setAge(15);
      person.setName("Evan");
      person.setSex("man");
      System.out.println("reflect new instance person：" + person.toString());
    } catch (Exception e) {
      e.printStackTrace();
    }
  }

  /**
   * test private constructor.
   */
  public static void reflectPrivateConstructor() {
    try {
      Class<?> personClass = Class.forName(ReflectDemo.PERSON);
      Constructor<?> declaredConstructorBook = personClass.getDeclaredConstructor(int.class, String.class, String.class);
      declaredConstructorBook.setAccessible(true);
      Object obj = declaredConstructorBook.newInstance(27, "Danny","man");
      Person person = (Person) obj;
      System.out.println("reflectPrivateConstructor person = " + person.toString());
    } catch (Exception ex) {
      ex.printStackTrace();
    }
  }

  /**
   * test method.
   */
  public static void reflectMethod() {
    try {
      Class<?> personClass = Class.forName(ReflectDemo.PERSON);
      Method personMethod = personClass.getMethod("generalMethod", String.class);
      personMethod.invoke((Person)personClass.newInstance(), "Test Method Reflect");
    } catch (Exception e) {
      e.printStackTrace();
    }
  }

  /**
   * get field.
   */
  public static void reflectField() {
    try {
      Class<?> personClass = Class.forName(ReflectDemo.PERSON);
      Field field = personClass.getDeclaredField("name");
      field.setAccessible(true);
      String name = (String) field.get(personClass.newInstance());
      System.out.println("reflectPrivateField name: " + name);
    } catch (Exception e) {
      e.printStackTrace();
    }
  }

  public static void main(String[] args) {
    //test
    try {
      ReflectDemo.reflectNewInstance();

      ReflectDemo.reflectPrivateConstructor();

      ReflectDemo.reflectField();

      ReflectDemo.reflectMethod();
    } catch (Exception e) {
      e.printStackTrace();
    }
  }

}

```

##### 结果输出如下：

```java
reflect new instance person：Person{age=15, name='Evan', sex='man'}
reflectPrivateConstructor person = Person{age=27, name='Danny', sex='man'}
reflectPrivateField name: Bruce
reflectMethod, input is :Test Method Reflect
```


### 总结

反射机制可以在运行时查看域和方法，可以编写出更灵活更通用的代码。但是反射也是很脆弱的，编译器很难发现程序中的问题，只有在运行时才能发现错误并抛出异常。

以上就是java反射用法的基本理解，若有错误或者不当之处，还请大家批评指正！

### 参考文献

* Java 核心技术卷I
* [Java高级特性——反射](https://www.jianshu.com/p/9be58ee20dee)ƒ∂
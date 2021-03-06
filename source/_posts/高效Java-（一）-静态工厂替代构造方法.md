---

title: 高效Java（一） 静态工厂替代构造方法
date: 2020-04-06 21:43:14
tags:
categories:
---

> 本系列内容总结自Effctive-java-3 https://sjsdfg.github.io/

***静态工厂方法与设计模式中的工厂方法模式不同***

普通的创建对象

```java
Date date = new Date();
```

**new究竟做了什么？**

当我们使用new来构造一个新的类示例时，其实是告诉JVM我们需要一个新的示例。JVM就会在内存中开辟一个新的空间，然后调用构造函数来初始化成员变量，最终把引用返回给调用方。

**静态工厂示例**：将基本类型`boolean`转换为 `Boolean` 对象引用：

```java
public static Boolean valueOf(boolean b) {
    return b ? Boolean.TRUE : Boolean.FALSE;
}
```

```java
Boolean flag = Boolean.valueOf(true);
```



### 静态工厂的优点

- **与构造方法不同，它们是有名字的**

  一个类只能有一个给定签名的构造方法。而静态工厂方法有名字，所以在类中需要具有相同签名的多个构造方法的情况下，可以使用静态工厂替换构造方法

- **与构造方法不同，它们不需要每次调用都创建一个新的对象**

  单例的写法大多都是用静态工厂方法来实现的

- **与构造方法不同，可以返回其返回类型的任何子类型对象**

  ```java
  Class Person {
      public static Person getInstance(){
          return new Person();
          // 这里可以改为 return new Player() / Cooker()
      }
  }
  Class Player extends Person{
  }
  Class Cooker extends Person{
  }
  ```

- **返回对象的类可以根据输入的参数不同而不同**

- **在编写包含改方法的类时，返回的对象的类不需要存在**

### 缺点

- **没有公共或保护构造方法的类不能被子类化**
- **不方便查找**
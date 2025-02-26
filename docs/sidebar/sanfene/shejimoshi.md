---
title: 设计模式面试题，1道设计模式八股文（1万字1张手绘图），面渣逆袭必看👍
shortTitle: 面渣逆袭-设计模式
description: 下载次数超 1 万次，1 万字 1 张手绘图，详解 1 道 设计模式 面试高频题（让天下没有难背的八股），面渣背会这些 设计模式 八股文，这次吊打面试官，我觉得稳了（手动 dog）。
author: 沉默王二
category:
  - 面渣逆袭
tag:
  - 面渣逆袭
head:
  - - meta
    - name: keywords
      content: 设计模式面试题,设计模式,八股文,面试题
---

## 01、什么是责任链模式？

>推荐阅读：[refactoringguru.cn：责任链模式](https://refactoringguru.cn/design-patterns/chain-of-responsibility)

责任链模式（Chain of Responsibility Pattern）是一种行为设计模式，它使多个对象都有机会处理请求，从而避免了请求的发送者和接收者之间的耦合关系。

请求会沿着一条链传递，直到有一个对象处理它为止。这种模式常用于处理不同类型的请求以及在不确定具体接收者的情况下将请求传递给多个对象中的一个。

![天未：图解 23 种设计模式](https://cdn.tobebetterjavaer.com/stutymore/shejimoshi-20240309104732.png)

### 基本概念

责任链模式主要包括以下几个角色：

- **Handler（抽象处理者）**：定义了一个处理请求的接口或抽象类，其中通常会包含一个指向链中下一个处理者的引用。
- **ConcreteHandler（具体处理者）**：实现抽象处理者的处理方法，如果它能处理请求，则处理；否则将请求转发给链中的下一个处理者。
- **Client（客户端）**：创建处理链，并向链的第一个处理者对象提交请求。

### 工作流程

1. 客户端将请求发送给链上的第一个处理者对象。
2. 处理者接收到请求后，决定自己是否有能力进行处理。
   - 如果可以处理，就处理请求。
   - 如果不能处理，就将请求转发给链上的下一个处理者。
3. 过程重复，直到链上的某个处理者能处理该请求或者链上没有更多的处理者。

### 应用场景

责任链模式适用于以下场景：

- 有多个对象可以处理同一请求，但具体由哪个对象处理则在运行时动态决定。
- 在不明确指定接收者的情况下，向多个对象中的一个提交请求。
- 需要动态组织和管理处理者时。

### 优缺点

**优点**：

- 降低耦合度：它将请求的发送者和接收者解耦。
- 增加了给对象指派职责的灵活性：可以在运行时动态改变链中的成员或调整它们的次序。
- 可以方便地增加新的处理类，在不影响现有代码的情况下扩展功能。

**缺点**：

- 请求可能不会被处理：如果没有任何处理者处理请求，它可能会达到链的末端并被丢弃。
- 性能问题：一个请求可能会在链上进行较长的遍历，影响性能。
- 调试困难：特别是在链较长时，调试可能会比较麻烦。

### 实现示例

假设有一个日志系统，根据日志的严重性级别（错误、警告、信息）将日志消息发送给不同的处理器处理。

```java
abstract class Logger {
    public static int INFO = 1;
    public static int DEBUG = 2;
    public static int ERROR = 3;

    protected int level;

    // 责任链中的下一个元素
    protected Logger nextLogger;

    public void setNextLogger(Logger nextLogger) {
        this.nextLogger = nextLogger;
    }

    public void logMessage(int level, String message) {
        if (this.level <= level) {
            write(message);
        }
        if (nextLogger != null) {
            nextLogger.logMessage(level, message);
        }
    }

    abstract protected void write(String message);
}

class ConsoleLogger extends Logger {
    public ConsoleLogger(int level) {
        this.level = level;
    }

    @Override
    protected void write(String message) {
        System.out.println("Standard Console::Logger: " + message);
    }
}

class ErrorLogger extends Logger {
    public ErrorLogger(int level) {
        this.level = level;
    }

    @Override
    protected void write(String message) {
        System.out.println("Error Console::Logger: " + message);
    }
}

class FileLogger extends Logger {
    public FileLogger(int level) {
        this.level = level;
    }

    @Override
    protected void write(String message) {
        System.out.println("File::Logger: " + message);
    }
}

public class ChainPatternDemo {
    private static Logger getChainOfLoggers() {
        Logger errorLogger = new ErrorLogger(Logger.ERROR);
        Logger fileLogger = new FileLogger(Logger.DEBUG);
        Logger consoleLogger = new ConsoleLogger(Logger.INFO);

        errorLogger.setNextLogger(fileLogger);
        fileLogger.setNextLogger(consoleLogger);

        return errorLogger;
    }

    public static void main(String[] args) {
        Logger loggerChain = getChainOfLoggers();

        loggerChain.logMessage(Logger.INFO, "INFO 级别");
        loggerChain.logMessage(Logger.DEBUG, " Debug 级别");
        loggerChain.logMessage(Logger.ERROR, "Error 级别");
    }
}
```

在这个示例中，创建了一个日志处理链。不同级别的日志将被相应级别的处理器处理。责任链模式让日志系统的扩展和维护变得更加灵活。

输出结果：

```
Standard Console::Logger: INFO 级别
File::Logger:  Debug 级别
Standard Console::Logger:  Debug 级别
Error Console::Logger: Error 级别
File::Logger: Error 级别
Standard Console::Logger: Error 级别
```

> 1. [Java 面试指南（付费）](https://javabetter.cn/zhishixingqiu/mianshi.html)收录的华为 OD 原题：请说说责任链模式。

## 02、什么是工厂模式？

>推荐阅读：[refactoringguru.cn：工厂模式](https://refactoringguru.cn/design-patterns/factory-method)

工厂模式（Factory Pattern）属于创建型设计模式，主要用于创建对象，而不暴露创建对象的逻辑给客户端。

其在父类中提供一个创建对象的方法， 允许子类决定实例化对象的类型。

举例来说，卡车 Truck 和轮船 Ship 都必须实现运输工具 Transport 接口，该接口声明了一个名为 deliver 的方法。

卡车都实现了 deliver 方法，但是卡车的 deliver 是在陆地上运输，而轮船的 deliver 是在海上运输。

![refactoringguru.cn：工厂模式](https://cdn.tobebetterjavaer.com/stutymore/shejimoshi-20240314083451.png)

调用工厂方法的代码（客户端代码）无需了解不同子类之间的差别，只管调用接口的 deliver 方法即可。

### 工厂模式的主要类型

①、**简单工厂模式**（Simple Factory）：它引入了创建者的概念，将实例化的代码从应用程序的业务逻辑中分离出来。简单工厂模式包括一个工厂类，它提供一个方法用于创建对象。

```java
class SimpleFactory {
    public static Transport createTransport(String type) {
        if ("truck".equalsIgnoreCase(type)) {
            return new Truck();
        } else if ("ship".equalsIgnoreCase(type)) {
            return new Ship();
        }
        return null;
    }

    public static void main(String[] args) {
        Transport truck = SimpleFactory.createTransport("truck");
        truck.deliver();

        Transport ship = SimpleFactory.createTransport("ship");
        ship.deliver();
    }
}
```

②、**工厂方法模式**（Factory Method）：定义一个创建对象的接口，但由子类决定要实例化的类是哪一个。工厂方法让类的实例化推迟到子类进行。

```java
interface Transport {
    void deliver();
}

class Truck implements Transport {
    @Override
    public void deliver() {
        System.out.println("在陆地上运输");
    }
}

class Ship implements Transport {
    @Override
    public void deliver() {
        System.out.println("在海上运输");
    }
}

interface TransportFactory {
    Transport createTransport();
}

class TruckFactory implements TransportFactory {
    @Override
    public Transport createTransport() {
        return new Truck();
    }
}

class ShipFactory implements TransportFactory {
    @Override
    public Transport createTransport() {
        return new Ship();
    }
}

public class FactoryMethodPatternDemo {
    public static void main(String[] args) {
        TransportFactory truckFactory = new TruckFactory();
        Transport truck = truckFactory.createTransport();
        truck.deliver();

        TransportFactory shipFactory = new ShipFactory();
        Transport ship = shipFactory.createTransport();
        ship.deliver();
    }
}
```

### 应用场景

1. **数据库访问层（DAL）组件**：工厂方法模式适用于数据库访问层，其中需要根据不同的数据库（如MySQL、PostgreSQL、Oracle）创建不同的数据库连接。工厂方法可以隐藏这些实例化逻辑，只提供一个统一的接口来获取数据库连接。
2. **日志记录**：当应用程序需要实现多种日志记录方式（如向文件记录、数据库记录或远程服务记录）时，可以使用工厂模式来设计一个灵活的日志系统，根据配置或环境动态决定具体使用哪种日志记录方式。


> 1. [Java 面试指南（付费）](https://javabetter.cn/zhishixingqiu/mianshi.html)收录的华为一面原题：说下工厂模式，场景

## 03、什么是单例模式？

>推荐阅读：[refactoringguru.cn：单例模式](https://refactoringguru.cn/design-patterns/singleton)

单例模式（Singleton Pattern）是一种创建型设计模式，它确保一个类只有一个实例，并提供一个全局访问点来获取该实例。单例模式主要用于控制对某些共享资源的访问，例如配置管理器、连接池、线程池、日志对象等。

![refactoringguru.cn：单例模式](https://cdn.tobebetterjavaer.com/stutymore/shejimoshi-20240314085956.png)

### 实现单例模式的关键点：

1. **私有构造方法**：确保外部代码不能通过构造器创建类的实例。
2. **私有静态实例变量**：持有类的唯一实例。
3. **公有静态方法**：提供全局访问点以获取实例，如果实例不存在，则在内部创建。

### 常见的单例模式实现：

#### 01、饿汉式

饿汉式单例（Eager Initialization）在类加载时就急切地创建实例，不管你后续用不用得到，这也是饿汉式的来源，简单但不支持延迟加载实例。

```java
public class Singleton {
    private static final Singleton instance = new Singleton();

    private Singleton() {}

    public static Singleton getInstance() {
        return instance;
    }
}
```

#### 02、懒汉式

懒汉式单例（Lazy Initialization）在实际使用时才创建实例，“确实懒”（😂）。这种实现方式需要考虑线程安全问题，因此一般会带上 [synchronized 关键字](https://javabetter.cn/thread/synchronized-1.html)。

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {}

    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

#### 03、双重检查锁定

双重检查锁定（Double-Checked Locking）结合了懒汉式的延迟加载和线程安全，同时又减少了同步的开销，主要是用 synchronized 同步代码块来替代同步方法。

```java
public class Singleton {
    private static volatile Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

当 instance 创建后，再次调用 getInstance 方法时，不会进入同步代码块，从而提高了性能。

在 instance 前加上 [volatile 关键字](https://javabetter.cn/thread/volatile.html)，可以防止指令重排，因为 `instance = new Singleton()` 并不是一个原子操作，可能会被重排序，导致其他线程获取到未初始化完成的实例。

#### 04、静态内部类

利用 Java 的[静态内部类](https://javabetter.cn/oo/static.html)（Static Nested Class）和[类加载机制](https://javabetter.cn/jvm/class-load.html)来实现线程安全的延迟初始化。

```java
public class Singleton {
    private Singleton() {}

    private static class SingletonHolder {
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return SingletonHolder.INSTANCE;
    }
}
```

当第一次加载 Singleton 类时并不会初始化 SingletonHolder，只有在第一次调用 getInstance 方法时才会导致 SingletonHolder 被加载，从而实例化 instance。

#### 05、枚举

使用[枚举（Enum）](https://javabetter.cn/basic-extra-meal/enum.html)实现单例是最简单的方式，也能防止反射攻击和序列化问题。

```java
public enum Singleton {
    INSTANCE;
    // 可以添加实例方法
}
```

> 1. [Java 面试指南（付费）](https://javabetter.cn/zhishixingqiu/mianshi.html)收录的华为一面原题：说下单例模式，有几种

---

*没有什么使我停留——除了目的，纵然岸旁有玫瑰、有绿荫、有宁静的港湾，我是不系之舟*。

**系列内容**：

- [面渣逆袭 Java SE 篇👍](https://javabetter.cn/sidebar/sanfene/javase.html)
- [面渣逆袭 Java 集合框架篇👍](https://javabetter.cn/sidebar/sanfene/javathread.html)
- [面渣逆袭 Java 并发编程篇👍](https://javabetter.cn/sidebar/sanfene/collection.html)
- [面渣逆袭 JVM 篇👍](https://javabetter.cn/sidebar/sanfene/jvm.html)
- [面渣逆袭 Spring 篇👍](https://javabetter.cn/sidebar/sanfene/spring.html)
- [面渣逆袭 Redis 篇👍](https://javabetter.cn/sidebar/sanfene/redis.html)
- [面渣逆袭 MyBatis 篇👍](https://javabetter.cn/sidebar/sanfene/mybatis.html)
- [面渣逆袭 MySQL 篇👍](https://javabetter.cn/sidebar/sanfene/mysql.html)
- [面渣逆袭操作系统篇👍](https://javabetter.cn/sidebar/sanfene/os.html)
- [面渣逆袭计算机网络篇👍](https://javabetter.cn/sidebar/sanfene/network.html)
- [面渣逆袭RocketMQ篇👍](https://javabetter.cn/sidebar/sanfene/rocketmq.html)
- [面渣逆袭分布式篇👍](https://javabetter.cn/sidebar/sanfene/fenbushi.html)
- [面渣逆袭微服务篇👍](https://javabetter.cn/sidebar/sanfene/weifuwu.html)
- [面渣逆袭设计模式篇 👍](https://javabetter.cn/sidebar/sanfene/shejimoshi.html)

----

GitHub 上标星 10000+ 的开源知识库《[二哥的 Java 进阶之路](https://github.com/itwanger/toBeBetterJavaer)》第一版 PDF 终于来了！包括Java基础语法、数组&字符串、OOP、集合框架、Java IO、异常处理、Java 新特性、网络编程、NIO、并发编程、JVM等等，共计 32 万余字，500+张手绘图，可以说是通俗易懂、风趣幽默……详情戳：[太赞了，GitHub 上标星 10000+ 的 Java 教程](https://javabetter.cn/overview/)


微信搜 **沉默王二** 或扫描下方二维码关注二哥的原创公众号沉默王二，回复 **222** 即可免费领取。

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/gongzhonghao.png)

> 图文详解 RocketMQ 面试高频题，这次吊打面试官，我觉得稳了（手动 dog）。整理：沉默王二，戳[转载链接](https://mp.weixin.qq.com/s/N6wq52pBGh8xkS-5uRcO2g)，作者：三分恶，戳[原文链接](https://mp.weixin.qq.com/s/IvBt3tB_IWZgPjKv5WGS4A)。
# 设计模式

“设计模式”一词最早起源于 GoF 的[《设计模式：可复用面向对象软件的基础》](https://book.douban.com/subject/1052241/) 一书，
此书采用 C++ 作为示例，时至今日，设计模式更多活跃于 Java 领域，故此节的所有示例均采用 Java 书写。

GoF 的《设计模式：可复用面向对象软件的基础》一书中共提出了 **23** 种设计模式，分为 **3** 类：
**创建型模式**、**结构型模式**、**行为型模式**。

## 创建型模式

- 单例（或称单件，Singleton）
- 原型
- 工厂方法
- 抽象工厂
- 建造者

## 结构型模式

- 适配器
- 桥接
- 组合
- 装饰器
- 外观
- 享元
- 代理

## 行为型模式

- 备忘录
- 策略
- 访问者
- 命令
- 模板方法
- 责任链
- 解释器
- 迭代器
- 中介者
- 观察者
- 状态

## 单例模式

**单例模式**确保一个类只有一个实例，并提供一个全局访问点。

```java
public class Singleton {
    private static Singleton uniqueInstance;
    private Singleton() {}
    public static synchronized Singleton getInstance() {
        if (uniqueInstance == numm) {
            uniqueInstance = new Singleton();
        }
        return uniqueInstance;
    }
}

Singleton.getInstance();
```

## 策略模式

**策略模式**定义了算法族，分别封装起来，让它们之间可以互相替换，此模式让算法的变化独立于使用算法的客户。

```java
```

## 工厂模式

**工厂模式**又分为**抽象工厂模式**和**工厂方法模式**，抽象工厂模式提供一个接口，用于创建相关或依赖对象的家族，而不需要明确指定具体类；工厂方法模式定义了一个创建对象的接口，但由子类决定要实例化的类是哪一个。工厂方法让类把实例化推迟到子类。

```java
```

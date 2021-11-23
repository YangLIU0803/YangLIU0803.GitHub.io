---
layout: article
title: Spring AOP
key: 10002
tags: Spring
category: blog
date: 2021-11-22 14:50:00 +08:00
modify_date: 2021-11-23 11:40:00 +08:00
picture_frame: shadow
---

组里的项目用到了Spring框架，好奇注解@RoleControl，于是了解了一下AOP（面向切面编程）。

**还在更新中。**
<!--more-->

## 1 AOP是什么

AOP（Aspect-Oriented Programming，面向切面编程）是一种编程范式，是面向对象编程的补充，它提供了另外一种思路来实现应用系统的公共服务。AOP采用“横切”技术，解剖已封装的对象，将这种公共服务封装到一个可重用的模块中，这模块称之为“Aspect”，即“切面”。“切面”可降低系统代码冗余，降低模块间的耦合度，提升系统的可维护性。

上面的根本看不懂，人话：软件开发中有些散落在应用中多处的功能（称之为横切关注点），概念上与应用的业务逻辑分离，但往往需要嵌入到业务逻辑中。比如，很多家庭都需要用电，户主单独去计算用电量很麻烦、容易出错，于是电网安装电表来监控用电量。横切关注点就像是电表。
> 重用通用功能最常见的面向对象的技术是**继承**或者**委托**。但整个应用中，继承会导致一个脆弱的对象体系；委托可能需要对对象进行复杂的调用。
面向切面编程需要定义通用功能，但可以通过声明的方式定义这个功能以何种方式在何处使用，无需修改受影响的类。

切面的好处：
1. 通用功能集中在一个地方，而不是分散在多处代码里。
2. Service更简洁，只需关注自己的核心代码，非核心代码被转移到切面中。

## 2 AOP术语、使用场景

- advice 通知：（what、when）
> - @Before @After @Around @AfterReturning @AfterThrowing
- join point 连接点：(能够插入切面代码的点)
- pointcut 切点：（where）
- aspect 切面：通知和切点的结合
- Introduction 引入：允许向现有的类添加新方法或者属性
- Weaving 织入：把切面应用到目标对象并创建新的代理对象的过程.
> - 编译期：切面在目标类编译时被织入。这种方式需要特殊的编译器。AspectJ的织入编译器就是以这种方式织入切面的。
> - 类加载期：切面在目标类加载到JVM时被织入。这种方式需要特殊的类加载器（ClassLoader），它可以在目标类被引入应用之前增强该目标类的字节码。AspectJ 5的加载时织入（load-timeweaving，LTW）就支持以这种方式织入切面。
> - 运行期：切面在应用运行的某个时刻被织入。一般情况下，在织入切面时，AOP容器会为目标对象动态地创建一个代理对象。Spring AOP就是以这种方式织入切面的。

- AOP proxy：由 AOP 框架创建的一个对象，用于实现切面合同（ advice 方法执行等）。在 Spring Framework 中，AOP 代理是 JDK 动态代理或 CGLIB 代理。
--------
- 日志
- Service权限管理
- 数据库事务管理
- 缓存

## 3 使用方法

### 3.1 编写切点
首先定义一个接口来作为切点：
  public interface Performance {
      void perform();
  }
  
假设我们想编写Performance的perform()方法触发的通知。下面的表达式能够设置当perform()方法执行时触发通知的调用。execution()指示器选择Performance的perform()方法。方法表达式以号开始，表明了不关心方法返回值的类型。然后指定了全限定类名和方法名。对于方法参数列表，使用两个点号（..）表明切点要选择任意的perform()方法，无论该方法的入参是什么。

  execution(* com.wtj.springlearn.aop.Performance.perform(..))

### 3.2 在切点中选择bean
### 3.3 使用注解创建切面
通过@Aspect进行标注，表示该Audience不仅是一个POJO还是一个切面。类中的方法表示了切面的具体行为。
  @Aspect
  public class RoleControlAspect{
    @Around(value = "@annotation(com.huawei.it.isc.logistics.bms.infrastructure.interceptor.RoleControl)")
  }
### 注入
### 自动代理



## 4 Spring对AOP的支持


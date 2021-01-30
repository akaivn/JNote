# JVM

<p align = "center">
    <a href = “https://docs.oracle.com/javase/8/docs/api/index.html” target = "_blank"><img src= "https://img.shields.io/badge/Java-1.8-red?logo=java" alt=""/></a>
</p>

## Introduction

### 前言

在学习本知识点之前，我们来看一看此节所面向的人群

-   拥有一定开发经验的Java平台开发人员
-   软件设计师、架构师
-   系统调优人员
-   有一定的Java编程基础并希望进一步理解Java的程序员
-   虚拟机爱好者，JVM实践者

只有拥有了以上前提，才推荐学习此节

我们也可以借此回顾一下什么是 `JRE` 和 `JDK`

JRE：https://baike.baidu.com/item/JRE/2902404?fr=aladdin

JDK：https://baike.baidu.com/item/jdk/1011?fr=aladdin

#### 问题引出

我们先来看几个你可能在项目中遇到的问题

-   运行着的线上系统突然卡死，系统无法访问，甚至直接 **OOM** 
-   想解决线上JVM GC问题，但却无从下手
-   新项目上线，对各种JVM参数设置一脸茫然，直接默认吧，然后就GG了
-   每次面试之前都要重新背一遍JVM的一些原理概念性的东西，然而面试官却经常问你在实际项目中如何调优JVM参数，如何解决GC、OOM等问题，一脸懵逼

**大部分Java开发人员，除会在项目中使用到与Java平台相关的各种高精尖技术，对于Java技术的核心Java虚拟机了解甚少**

如果我们把核心类库的API 比做数学公式的话，那么Java虚拟机的知识就好比公式的推导过程。因此学习JVM是非常有必要的

![image-20210126194824317](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/26/194825-343286.png)

计算机系统体系对我们来说越来越远，在不了解底层实现方式的前提下，通过高级语言很容易编写程序代码。但事实上计算机并不认识高级语言

### 简介

#### What

JVM：全称为 (**Java Virtual Machine**)表示为 Java虚拟机，包含了执行，解释，编译的所有步骤，是Java能够一次编写处处运行的首要原因，它忽略了跨平台的平台条件，使得已经编写好的代码在新的平台上无需二次编译，直接就可以执行上次已经编译好的 .class (字节码)文件

更多详情可参照官网解释

百度：https://baike.baidu.com/item/JVM/2902369?fr=aladdin

Google：https://zh.wikipedia.org/wiki/Java%E8%99%9A%E6%8B%9F%E6%9C%BA

#### Why

-   面试的需要（BATJ、TMD、PKQ等面试都爱问)

-   中高级程序员必备技能

-   项目管理、调优的需要

-   追求极客的精神
    -   比如:垃圾回收算法、JIT、底层原理

![image-20210126195239349](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/26/195240-819960.png)

垃圾收集机制为我们打理了很多繁琐的工作，大大提高了开发的效率，但是，垃圾收集也不是万能的，懂得JVM内部的内存结构、工作机制，是设计高扩展性应用和诊断运行时问题的基础,也是Java工程师进阶的必备能力

Java不同于c、c++、c#的最大原因就是Java拥有自动的回收和管理内存的机制，因此阿里巴巴Java开发手册也名言道：任何Java程序都不允许显示的调用 `System.GC()` 来手动回收内存





## RAM And GC





## ByteCode And ClassLoader





## PerformenceMonitor And Optimize



## Interview
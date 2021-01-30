# JVM

<p align = "center">
    <a href = “https://docs.oracle.com/javase/8/docs/api/index.html” target = "_blank"><img src= "https://img.shields.io/badge/JDK-1.8-red?logo=java" alt=""/></a>
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

### 跨平台

Java是一门跨平台的语言，而Java所依托的虚拟机JVM是 `一个跨语言的平台`，如下图

![image-20210129110407755](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/29/110409-322095.png)

如图中所示，任何语言都可以把自己的**API、一系列的高级语言**最终都编译为遵循JVM虚拟机规范的 `字节码文件` ，在下文我们同称为 **JVM字节码文件**，依托JVM即可以执行任何高级语言的API

JVM不只是可以单单的运行 Java字节码

### 虚拟机

先今，有三大非常有名的JVM虚拟机，分别为 `Hotspot`、`JRockit`、`IBM J9`

目前我们使用的最多的还是 Hotspot VM + JRockit 的整合(同属Oracle)，过多关于Hotspot VM的介绍可自行百度

查看当前安装的VM，可执行

```shell
$ java -version

java version "1.8.0_271"
Java(TM) SE Runtime Environment (build 1.8.0_271-b09)
Java HotSpot(TM) 64-Bit Server VM (build 25.271-b09, mixed mode)
```

### 整体结构

![image-20210129111816585](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/29/111817-451089.png)

### 执行流程

![image-20210129112105198](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/30/101039-435987.png)

这里需要注意的是：在JVM中，JIT负责编译和缓存需要循环执行的字节码，翻译字节码和JIT编译器同时存在，分工不同，提升效率

### 编译器架构

Java编译器输入的指令流基本上是一种基于**栈的指令集**架构，另外一种指令集架构则是基于**寄存器的指令集**架构。

#### 特点和区别

具体来说:这两种架构之间的区别:

- 基于栈式架构的特点
  - 设计和实现更简单，适用于资源受限的系统
  - 避开了寄存器的分配难题:使用零地址指令方式分配
  - 指令流中的指令大部分是零地址指令，其执行过程依赖于操作栈。指令集更小，编译器容易实现
  - 不需要硬件支持，可移植性更好，更好实现跨平台·基于寄存器架构的特点
- 基于寄存器的指令集架构的特点
  - 典型的应用是x86的二进制指令集:比如传统的Pc以及Android的Davlik虚拟机
  - 指令集架构则完全依赖硬件，可移植性差
  - 性能优秀和执行更高效
  - 花费更少的指令去完成一项操作
  - 在大部分情况下，基于寄存器架构的指令集往往都以一地址指令、二地址指令和三地址指令为主，而基于栈式架构的指令集却是以零地址指令为主

#### 栈实例

接下来我们来使用一个实例看看编译的步骤

```java
public class StackStruTest {
    public static void main(String[] args) {
        int i = 3;
        int j = 4;
        int k = i + j ;
    }
}
```

以上代码运行后的栈保存在.class字节码中，进入到字节码文件目录内，使用如下命令反编译字节码

```shell
$ javap -v StackStruTest.class
```

反编译后得到如下栈执行流程

```shell
 Code:
      stack=2, locals=4, args_size=1
         0: iconst_3 # 把第一个常量放到索引1的位置
         1: istore_1
         2: iconst_4 # 把第二个常量放到索引2的位置
         3: istore_2
         4: iload_1 # 分别加载过来
         5: iload_2
         6: iadd # Add
         7: istore_3 # 相加的结果存入索引3
         8: return
```

### JVM声明周期

JVM的声明周期为三个阶段分别为：启动、执行、退出

#### 启动

Java虚拟机的启动是通过引导类加载器(**bootstrap class loader**)创建一个初始类(**initial class**)来完成的，这个类是由虚拟机的具体实现指定的

#### 执行

- 一个运行中的Java虚拟机有着一个清晰的任务:执行Java程序。

- 程序开始执行时他才运行，程序结束时他就停止。
- **执行一个所谓的Java程序的时候，真真正正在执行的是一个叫做Java虚拟机的进程**

#### 退出

有如下的几种情况会导致JVM退出:

- 程序正常执行结束
- 程序在执行过程中遇到了异常或错误而异常终止
- 由于操作系统出现错误而导致Java虚拟机进程终止
- 某线程调用Runtime类或system类的exit方法，或 Runtime类的halt方法，并且Java安全管理器也允许这次exit或halt操作
- 除此之外，JNI (Java Native Interface)规范描述了用JNIInvocation API来加载或卸载Java虚拟机时，Java虚拟机的退出情况

## RAM And GC





## ByteCode And ClassLoader





## PerformenceMonitor And Optimize



## Interview
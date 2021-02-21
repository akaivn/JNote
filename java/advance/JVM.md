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

### 内存结构概述

#### 类加载简图

![image-20210221160816625](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202102/21/160817-496526.png)



#### 类加载详细图

![classloader](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202102/21/160944-621831.png)

### 类的加载及类的加载过程

#### 类加载器的作用

![image-20210221161631796](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202102/21/161632-34956.png)

- 类加载器子系统负责从文件系统或者网络中加载class文件，class文件在文件开头有特定的文件标识
- ClassLoader只负责class文件的加载，至于它是否可以运行，则由ExecutionEngine（执行引擎）决定
- 加载的类信息存放于一块称为方法区的内存空间。除了类的信息外，方法区中还会存放运行时常量池信息，可能还包括字符串字面量和数字常量（这部分常量信息是Class文件中常量池部分的内存映射)

![image-20210221162514006](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202102/21/162514-217060.png)

通过Car class获得调用该类的类加载器 ClassLoader,之后调用getClass()方法，在堆中创建该类的实例

**类加载的过程图**

![image-20210221162802691](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202102/21/162802-527241.png)

#### 类加载阶段

##### 第一阶段 Loading

![image-20210221162932313](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202102/21/162933-87990.png)

1．通过一个类的全限定名获取定义此类的二进制字节流

2．将这个字节流所代表的静态存储结构转化为方法区的运行时数据结构

3．**在内存中生成一个代表这个类的java.lang.class对象**，作为方法区这个类的各种数据的访问入口

> 加载 .class 文件的方式

- 从本地系统中直接加载
- 通过网络获取，典型场景:web Applet
- 从zip压缩包中读取，成为日后jar、war格式 
- 运行时计算生成，使用最多的是:动态代理技术
- 由其他文件生成，典型场景:JSP应用
- 从专有数据库中提取.class文件，比较少见
- 从加密文件中获取，典型的防class文件被反编译的保护措施

##### 第二阶段 Linking

类加载器进入第二阶段为 **链接** 该阶段主要为以下三个步骤

> 验证(Verify)

目的在于确保class文件的字节流中包含信息符合当前虚拟机要求，保证被加载类的正确性，不会危害虚拟机自身安全

主要包括四种验证，文件格式验证，元数据验证，字节码验证，符号引用验证

> 准备(Prepare)

为类变量分配内存并且设置该类变量的默认初始值，即零值

例如：

```java
public class Hello{
    private static int a = 1;// 在这里类加载的时候的值为 0 ，到类加载达到初始化阶段时，才会为其分配具体值
}
```

这里不包含用final修饰的static，因为final在编译的时候就会分配了，准备阶段会显式初始化

这里不会为实例变量分配初始化，类变量会分配在方法区中，而实例变量是会随着对象一起分配到Java堆中

> 解析(Resolve)

将常量池内的符号引用转换为直接引用的过程

事实上，解析操作往往会伴随着JVM在执行完初始化之后再执行

符号引用就是一组符号来描述所引用的目标。符号引用的字面量形式明确定义在《java虚拟机规范》的class文件格式中。直接引用就是直接指向目标的指针、相对偏移量或一个间接定位到目标的句柄

解析动作主要针对类或接口、字段、类方法、接口方法、方法类型等。对应常量池中的CONSTANT_Class_info、CONSTANT_Fieldref_info、CONSTANT Methodref_info等

符号例图：

![image-20210221164633688](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202102/21/164634-318975.png)

##### 第三阶段 Initialization

第三阶段为初始化

**初始化阶段就是执行类构造器方法<clinit>()的过程**

**此方法不需定义，是javac编译器自动收集类中的所有类变量的赋值动作和静态代码块中的语句合并而来**

我们可以安装IDEA插件 **jclasslib** 来查看具体的详细信息

![image-20210221170543069](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202102/21/170543-45362.png)

当没有定义 static 变量或方法时，是没有<clinit>出现的

![image-20210221170719654](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202102/21/170719-780849.png)

**构造器方法中指令按语句在源文件中出现的顺序执行**

例如：

有以下代码

```java
public class Hello {
    private static int a = 1;

    static {
        a = 4;
    }

    public static void main(String[] args) {
        System.out.println(Hello.a);
    }
}
```

对应的jclasslib中的<clinit>方法为：

![image-20210221171045449](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202102/21/171046-269324.png)

因为顺序的不同，static代码块在其静态变量定义之后出现，因此变量a的值会被重新覆盖为4

再例如：

有如下代码

```java
public class Hello {

    static {
        num = 4;
    }
    private static int num = 1;

    public static void main(String[] args) {
        System.out.println(Hello.num);
    }
}
```

jclasslib图为

![image-20210221171417874](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202102/21/171420-96860.png)

为什么num被定义到具体变量之前还能被执行而不报错？

我们前面说过在类的加载的第二阶段 Linking 中，类的静态变量其实已经被在类加载过程中赋值为0，也就是说类加载开始后num的值为0，而后经历静态代码块中的后值为4，之后再重新赋值为1

假如我们在静态代码块中对num操作会发生什么？

![image-20210221171746963](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202102/21/171747-737611.png)

这个错误叫做：`超前引用`，虽然类在加载的时候是会提前赋值为0的，但是此时我们并没有真正的声明变量，因此在此处调用时就会报错

**<clinit>()不同于类的构造器。(关联:构造器是虚拟机视角下的<init>()。若该类具有父类，JVM会保证子类的<clinit>()执行前，父类的<clinit>()已经执行完毕。**

例：有如下代码

```java
public class World {
    static class Father{
        public static String str = "hello world!";
        static {
            str = "again to value";
        }
    }

    static class Son extends Father{
        public static String sonStr = str;
    }

    public static void main(String[] args) {
        System.out.println(Son.sonStr);
    }
}
```

对应的jclasslib图为：

![image-20210221172251263](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202102/21/172251-467013.png)

加载main方法时，引用了Son类，Son继承了Father类，因此先加载的是Father类，其后是Son类，Son类的值被Father中的static静态代码块重新发赋了值，因此，main方法中打印的其实是被重新赋值的新值

**虚拟机必须保证一个类的<clinit> ()方法在多线程下被同步加锁**

`虚拟机会保证多线程的情况下，类也只会被加载一次`

例：现有代码如下

```java
public class ThreadTest {
    public static void main(String[] args) {
        Runnable runnable = () -> {
            System.out.println(Thread.currentThread().getName()+"开始了");
            ByClassLoad byClassLoad = new ByClassLoad();// new
            System.out.println(Thread.currentThread().getName()+"结束了");
        };
        Thread thread = new Thread(runnable,"线程1");
        Thread thread1 = new Thread(runnable,"线程2");
        thread.start();
        thread1.start();
    }
}

class ByClassLoad {
    static {
        if (true) {
            System.out.println(Thread.currentThread().getName() + "加载了我。。。");
        }
    }
}
```

当运行此代码时，虚拟机会保证在加载ByClassLoad类时，只会加载一次，执行结束后，代码有可能是这样的：

```java
线程1开始了
线程2开始了
线程1加载了我。。。
线程1结束了
线程2结束了
```

或

```java
线程2开始了
线程1开始了
线程2加载了我。。。
线程2结束了
线程1结束了
```

#### 类加载器分类

JVM支持两种类型的类加载器，分别为**引导类加载器（BootstrapClassLoader）和自定义类加载器User-Defined ClassLoader)**

从概念上来讲，自定义类加载器一般指的是程序中由开发人员自定义的一类类加载器，但是Java虚拟机规范却没有这么定义，而是**将所有派生于抽象类ClassLoader的类加载器都划分为自定义类加载器**

无论类加载器的类型如何划分，在程序中我们最常见的类加载器始终只有3个，如下所示:



![image-20210221175343278](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202102/21/175344-427304.png)

他们并不是相互继承的关系，无法准确的形容他们，他们更像是 **包含** 的关系：例如 /a/b/c.txt 我们并不能说成c.txt继承自/b，/b继承自/a，这种说法不准确

我们来从实例代码中获得相应的类加载器

```java
public class ClassLoaderTest {
    public static void main(String[] args) {

        // 系统类加载器
        // sun.misc.Launcher$AppClassLoader@18b4aac2
        ClassLoader systemClassLoader = ClassLoader.getSystemClassLoader();
        System.out.println(systemClassLoader);
        
        // 扩展类加载器
        // sun.misc.Launcher$ExtClassLoader@1b6d3586
        ClassLoader extensionClassLoader = systemClassLoader.getParent();
        System.out.println(extensionClassLoader);

        // 视图获取顶级的 BootStrapClassLoader
        // null
        ClassLoader bootStrapClassLoader = extensionClassLoader.getParent();
        System.out.println(bootStrapClassLoader);

        // 当前类的类加载器 （获得到的是系统类加载器，对象都一样）
        // sun.misc.Launcher$AppClassLoader@18b4aac2
        System.out.println(ClassLoaderTest.class.getClassLoader());

        // Java常用对象类加载器（说明，String对象用的是顶级的类加载器）
        // null
        System.out.println(String.class.getClassLoader());
    }
}
```

**我们无法获取最顶级的BootstrapClassLoader，第一是因为不予许我们随意访问，第二是因为它是底层的类加载器，使用c、c++编写**

##### Bootstrap ClassLoader

启动类加载器

- 启动类加载器（引导类加载器，Bootstrap classLoader)

- 它用来加载Java的核心库（JAVA_HOME/jre/lib/rt.jar、resources.jar或sun.boot.class.path路径下的内容），用于提供JVM自身需要的类
- 并不继承自java.lang.classLoader，没有父加载器
- 加载扩展类和应用程序类加载器，并指定为他们的父类加载器
- 出于安全考虑，Bootstrap启动类加载器只加载包名为java、javax、sun等开头的类

##### Extension ClassLoader

扩展类加载器

- **Java语言编写**，由sun.misc.Launcher$ExtClassLoader实现
- **派生于classLoader类**
- 父类加载器为启动类加载器
- 从java.ext.dirs系统属性所指定的目录中加载类库，或从JDK的安装目录的jre/lib/ext子目录（扩展目录)下加载类库。**如果用户创建的JAR放在此目录下，也会自动由扩展类加载器加载**

##### App ClassLoader

系统类加载器

- java语言编写，由sun.misc.Launcher$AppclassLoader实现
- 派生于classLoader类
- 父类加载器为扩展类加载器
- 它负责加载环境变量classpath或系统属性java.class.path指定路径下的类库
- 该类加载是程序中默认的类加载器，一般来说Java应用的类都是由它来完成加载
- 通过classLoader#getsystemclassLoader ()方法可以获取到该类加载器

#### 用户自定义类加载器

在Java的日常应用程序开发中，类的加载几乎是由上述3种类加载器相互配合执行的，在必要时，我们还可以自定义类加载器，来定制类的加载方式

为什么要自定义类加载器？

- 隔离加载类
- 修改类加载的方式
- 扩展加载源
- 防止源码泄漏

**用户自定义类加载器实现步骤:**

- 开发人员可以通过继承抽象类java.lang.classLoader类的方式，实现自己的类加载器，以满足一些特殊的需求
- 在JDK1.2之前,在自定义类加载器时，总会去继承classLoader类并重写loadclass ()方法，从而实现自定义的类加载类，但是在JDK1.2之后已不再建议用户去覆盖loadclass ()方法，而是建议把自定义的类加载逻辑写在findclass ()方法中
- 在编写自定义类加载器时，如果没有太过于复杂的需求，可以直接继承URLClassLoader类，这样就可以避免自己去编写findclass ()方法及其获取字节码流的方式，使自定义类加载器编写更加简洁

#### ClassLoader

ClassLoader类，它是一个抽象类，其后所有的类加载器都继承自classLoader(不包括启动类加载器)

部分Api说明

![image-20210221183523016](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202102/21/183526-575168.png)

类加载器关系图：

![image-20210221183552850](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202102/21/183553-633203.png)

几种获得ClassLoader对象的方式

```java
public static void main(String[] args) {
    // 获得当前类的classloader
    ClassLoader classLoader = ClassLoaderTest.class.getClassLoader();
    System.out.println(classLoader);
    // 获取当前非线程上下文的classloader
    ClassLoader contextClassLoader = Thread.currentThread().getContextClassLoader();
    System.out.println(contextClassLoader);
    // 获取系统的classloader
    ClassLoader systemClassLoader = ClassLoader.getSystemClassLoader();
    System.out.println(systemClassLoader);
}
```

#### 双亲委派



## ByteCode And ClassLoader





## PerformenceMonitor And Optimize



## Interview
# JVM

<p align = "center">
    <a href = “https://docs.oracle.com/javase/8/docs/api/index.html” target = "_blank"><img src= "https://img.shields.io/badge/JDK-1.8-red?logo=java" alt=""/></a>
</p>


## 简介

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

![image-20210126194824317](https://gitee.com/QianKa/image-bucket/raw/master/typora/202101/26/194825-343286.png)

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

![image-20210126195239349](https://gitee.com/QianKa/image-bucket/raw/master/typora/202101/26/195240-819960.png)

垃圾收集机制为我们打理了很多繁琐的工作，大大提高了开发的效率，但是，垃圾收集也不是万能的，懂得JVM内部的内存结构、工作机制，是设计高扩展性应用和诊断运行时问题的基础,也是Java工程师进阶的必备能力

Java不同于c、c++、c#的最大原因就是Java拥有自动的回收和管理内存的机制，因此阿里巴巴Java开发手册也名言道：任何Java程序都不允许显示的调用 `System.GC()` 来手动回收内存

### 跨平台

Java是一门跨平台的语言，而Java所依托的虚拟机JVM是 `一个跨语言的平台`，如下图

![image-20210129110407755](https://gitee.com/QianKa/image-bucket/raw/master/typora/202101/29/110409-322095.png)

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

![image-20210129111816585](https://gitee.com/QianKa/image-bucket/raw/master/typora/202101/29/111817-451089.png)

### 执行流程

![image-20210129112105198](https://gitee.com/QianKa/image-bucket/raw/master/typora/202101/30/101039-435987.png)

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

## 内存与垃圾回收

### 类加载器

#### 内存结构概述

##### 类加载简图

![image-20210221160816625](https://gitee.com/QianKa/image-bucket/raw/master/typora/202102/21/160817-496526.png)



##### 类加载详细图

![classloader](https://gitee.com/QianKa/image-bucket/raw/master/typora/202102/21/160944-621831.png)

#### 类的加载及类的加载过程

##### 类加载器的作用

![image-20210221161631796](https://gitee.com/QianKa/image-bucket/raw/master/typora/202102/21/161632-34956.png)

- 类加载器子系统负责从文件系统或者网络中加载class文件，class文件在文件开头有特定的文件标识
- ClassLoader只负责class文件的加载，至于它是否可以运行，则由ExecutionEngine（执行引擎）决定
- 加载的类信息存放于一块称为方法区的内存空间。除了类的信息外，方法区中还会存放运行时常量池信息，可能还包括字符串字面量和数字常量（这部分常量信息是Class文件中常量池部分的内存映射)

![image-20210221162514006](https://gitee.com/QianKa/image-bucket/raw/master/typora/202102/21/162514-217060.png)

通过Car class获得调用该类的类加载器 ClassLoader,之后调用getClass()方法，在堆中创建该类的实例

**类加载的过程图**

![image-20210221162802691](https://gitee.com/QianKa/image-bucket/raw/master/typora/202102/21/162802-527241.png)

##### 类加载阶段

###### 第一阶段 Loading

![image-20210221162932313](https://gitee.com/QianKa/image-bucket/raw/master/typora/202102/21/162933-87990.png)

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

###### 第二阶段 Linking

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

![image-20210221164633688](https://gitee.com/QianKa/image-bucket/raw/master/typora/202102/21/164634-318975.png)

###### 第三阶段 Initialization

第三阶段为初始化

**初始化阶段就是执行类构造器方法<clinit>()的过程**

**此方法不需定义，是javac编译器自动收集类中的所有类变量的赋值动作和静态代码块中的语句合并而来**

我们可以安装IDEA插件 **jclasslib** 来查看具体的详细信息

![image-20210221170543069](https://gitee.com/QianKa/image-bucket/raw/master/typora/202102/21/170543-45362.png)

当没有定义 static 变量或方法时，是没有<clinit>出现的

![image-20210221170719654](https://gitee.com/QianKa/image-bucket/raw/master/typora/202102/21/170719-780849.png)

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

![image-20210221171045449](https://gitee.com/QianKa/image-bucket/raw/master/typora/202102/21/171046-269324.png)

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

![image-20210221171417874](https://gitee.com/QianKa/image-bucket/raw/master/typora/202102/21/171420-96860.png)

为什么num被定义到具体变量之前还能被执行而不报错？

我们前面说过在类的加载的第二阶段 Linking 中，类的静态变量其实已经被在类加载过程中赋值为0，也就是说类加载开始后num的值为0，而后经历静态代码块中的后值为4，之后再重新赋值为1

假如我们在静态代码块中对num操作会发生什么？

![image-20210221171746963](https://gitee.com/QianKa/image-bucket/raw/master/typora/202102/21/171747-737611.png)

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

![image-20210221172251263](https://gitee.com/QianKa/image-bucket/raw/master/typora/202102/21/172251-467013.png)

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

##### 类加载器分类

JVM支持两种类型的类加载器，分别为**引导类加载器（BootstrapClassLoader）和自定义类加载器User-Defined ClassLoader)**

从概念上来讲，自定义类加载器一般指的是程序中由开发人员自定义的一类类加载器，但是Java虚拟机规范却没有这么定义，而是**将所有派生于抽象类ClassLoader的类加载器都划分为自定义类加载器**

无论类加载器的类型如何划分，在程序中我们最常见的类加载器始终只有3个，如下所示:



![image-20210221175343278](https://gitee.com/QianKa/image-bucket/raw/master/typora/202102/21/175344-427304.png)

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

###### Bootstrap ClassLoader

启动类加载器

- 启动类加载器（引导类加载器，Bootstrap classLoader)

- 它用来加载Java的核心库（JAVA_HOME/jre/lib/rt.jar、resources.jar或sun.boot.class.path路径下的内容），用于提供JVM自身需要的类
- 并不继承自java.lang.classLoader，没有父加载器
- 加载扩展类和应用程序类加载器，并指定为他们的父类加载器
- 出于安全考虑，Bootstrap启动类加载器只加载包名为java、javax、sun等开头的类

###### Extension ClassLoader

扩展类加载器

- **Java语言编写**，由sun.misc.Launcher$ExtClassLoader实现
- **派生于classLoader类**
- 父类加载器为启动类加载器
- 从java.ext.dirs系统属性所指定的目录中加载类库，或从JDK的安装目录的jre/lib/ext子目录（扩展目录)下加载类库。**如果用户创建的JAR放在此目录下，也会自动由扩展类加载器加载**

###### App ClassLoader

系统类加载器

- java语言编写，由sun.misc.Launcher$AppclassLoader实现
- 派生于classLoader类
- 父类加载器为扩展类加载器
- 它负责加载环境变量classpath或系统属性java.class.path指定路径下的类库
- 该类加载是程序中默认的类加载器，一般来说Java应用的类都是由它来完成加载
- 通过classLoader#getsystemclassLoader ()方法可以获取到该类加载器

##### 用户自定义类加载器

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

##### ClassLoader

ClassLoader类，它是一个抽象类，其后所有的类加载器都继承自classLoader(不包括启动类加载器)

部分Api说明

![image-20210221183523016](https://gitee.com/QianKa/image-bucket/raw/master/typora/202102/21/183526-575168.png)

类加载器关系图：

![image-20210221183552850](https://gitee.com/QianKa/image-bucket/raw/master/typora/202102/21/183553-633203.png)

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

##### 双亲委派

> 什么是 `双亲委派` 

Java虚拟机对class文件采用的是按需加载的方式，也就是说当需要使用该类时才会将它的class文件加载到内存生成class对象。而且加载某个类的class文件时，Java虚拟机采用的是双亲委涨模式，即`把请求交由父类处理`，它是一种任务委派模式

> 工作原理

如果一个类加载器收到了类加载请求，它并不会自己先去加载，而是把这个请求委托给父类的加载器去执行

如果父类加载器还存在其父类加载器，则进一步向上委托，依次递归，请求最终将到达顶层的启动类加载器

如果父类加载器可以完成类加载任务，就成功返回，**倘若父类加载器无法完成此加载任务，子加载器才会尝试自己去加载**，这就是双亲委派模式

![image-20210227143114239](https://gitee.com/QianKa/image-bucket/raw/master/typora/202102/27/143115-380021.png)

我们来举一个栗子：

在idea的 **java** 包下新建 全限定类型为 `java.lang.String` 类

有如下代码：

```java
public class String {
    
    static {
        System.out.println("自定义String对象");
    }
    
}
```

在另一个类中我们尝试加载它：

```java
public class Customer {

    public static void main(String[] args) {
        String str = new String();
        System.out.println(str);
    }
}
```

如果加载成功，应该会出现 static 代码块中的打印语句，但是根据双亲委派机制，首先：又应用类加载器向上委托，交给扩展类加载器，扩展类加载器再次向上委托，交给启动类加载器，启动类加载器一看在自己加载的范围内有String 类，所有，就会加载原系统的String对象，而不会加载自定义的String类

接下来我们还想尝试，使用自定义String类来运行main方法，看能不能执行？

```java
public class String {

    static {
        System.out.println("自定义String对象!");
    }

    public static void main(String[] args) {
        System.out.println("原系统String对象是没有main方法的！");
    }

}
```

运行后为：

![image-20210227144314372](https://gitee.com/QianKa/image-bucket/raw/master/typora/202102/27/144314-325916.png)

**由此便证明了根本不是由我们的系统类加载器来加载的自定义String类**

例二：

我们在java.lang包下去加一个定义的类 ShkStart 看是否能够正常执行

```java
public class ShkStart {
    public static void main(String[] args) {}
}
```

执行结果如下：

![image-20210227145541390](https://gitee.com/QianKa/image-bucket/raw/master/typora/202102/27/145543-247820.png)

我们自定义类时，java.lang包是由引导类加载器加载的，我们在这个包下新建自定义类引导类加载器是无法识别的，因此就会报错，这也就是为什么我们自定义的包名不能是java已有的由非应用类加载器加载的包名

> 双亲委派有什么优势？

1. 避免类的重复加载
2. 保护程序安全，防止核心API被随意篡改
   - 自定义类：自定义 `java.lang.String` 没有被加载。
   - 自定义类：`java.lang.ShkStart`（报错：阻止创建 `java.lang` 开头的类）

> 沙箱安全机制概念

什么是沙箱？

docker有沙箱机制，360的安全运行有沙箱机制，什么是沙箱呢？

沙箱是指单独的独立出一份空间，在里面运行程序或做其他操作出现问题时都无法影响和干扰外部的环境，沙箱是独立的

1. 自定义String类时：在加载自定义String类的时候会率先使用引导类加载器加载，而引导类加载器在加载的过程中会先加载`jdk`自带的文件（rt.jar包中`java.lang.String.class`），报错信息说没有main方法，就是因为加载的是rt.jar包中的String类
2. 这样可以保证对java核心源代码的保护

> 如何判断两个Class对象完全相同

在JVM中表示两个class对象是否为同一个类存在两个必要条件：

1. 类的全限定类名必须一致
2. **加载这个类的ClassLoader（指ClassLoader实例对象）必须相同** 换句话说，在JVM中，即使这两个类对象（class对象）来源同一个Class文件，被同一个虚拟机所加载，但只要加载它们的ClassLoader实例对象不同，那么这两个类对象也是不相等的

> 对类加载器的引用

1. JVM必须知道一个类型是由启动加载器加载的还是由用户类加载器加载的
2. **如果一个类型是由用户类加载器加载的，那么JVM会将这个类加载器的一个引用作为类型信息的一部分保存在方法区中**
3. 当解析一个类型到另一个类型的引用的时候，JVM需要保证这两个类型的类加载器是相同的（后面讲）

### 运行时数据区

#### 运行时数据区结构

> 此章把运行时数据区里比较少的地方讲一下。虚拟机栈，堆，方法区这些地方后续再讲

本节所讲的是运行时数据区，它是在类完成加载后执行的阶段

![image-20210227152635256](https://gitee.com/QianKa/image-bucket/raw/master/typora/202102/27/152636-769687.png)

当我们通过前面的：类的加载 --> 验证 --> 准备 --> 解析 --> 初始化，这几个阶段完成后，就会用到执行引擎对我们的类进行使用，同时执行引擎将会使用到我们运行时数据区

![img](https://cdn.jsdelivr.net/gh/youthlql/lql_img/JVM/chapter_003/0002.png)

##### 运行时数据区与内存

也就是准备执行引擎执行时的一些数据

1. 内存是非常重要的系统资源，是硬盘和CPU的中间仓库及桥梁，承载着操作系统和应用程序的实时运行。JVM内存布局规定了Java在运行过程中内存申请、分配、管理的策略，保证了JVM的高效稳定运行。**不同的JVM对于内存的划分方式和管理机制存在着部分差异**。结合JVM虚拟机规范，来探讨一下经典的JVM内存布局。
2. 我们通过磁盘或者网络IO得到的数据，都需要先加载到内存中，然后CPU从内存中获取数据进行读取，也就是说内存充当了CPU和磁盘之间的桥梁

![img](https://cdn.jsdelivr.net/gh/youthlql/lql_img/JVM/chapter_003/0004.jpg)

上图的元数据区是JDK8之后的规范，原来这里叫做 `Method Area` 方法区，同时JDK8之后，出现的 `Code Cache` （代码缓存）也属于**非堆空间**

##### 线程的内存空间

1. Java虚拟机定义了若干种程序运行期间会使用到的运行时数据区：其中有一些会随着虚拟机启动而创建，随着虚拟机退出而销毁。另外一些则是与线程一一对应的，这些与线程对应的数据区域会随着线程开始和结束而创建和销毁
2. 灰色的为单独线程私有的，红色的为多个线程共享的。即：
   - 线程独有：独立包括程序计数器、栈、本地方法栈
   - 线程间共享：堆、堆外内存（永久代或元空间、代码缓存）

![img](https://cdn.jsdelivr.net/gh/youthlql/lql_img/JVM/chapter_003/0005.png)

##### Runtime

**每个JVM只有一个Runtime实例**。即为运行时环境，相当于内存结构的中间的那个框框：运行时环境

#### 线程

##### JVM 线程

1. 线程是一个程序里的运行单元。JVM允许一个应用有多个线程并行的执行
2. 在Hotspot JVM里，每个线程都与操作系统的本地线程直接映射
   - 当一个Java线程准备好执行以后，此时一个操作系统的本地线程也同时创建。Java线程执行终止后，本地线程也会回收
3. 操作系统负责将线程安排调度到任何一个可用的CPU上。一旦本地线程初始化成功，它就会调用Java线程中的run()方法

##### JVM系统线程

- 如果你使用`jconsole`或者是任何一个调试工具，都能看到在后台有许多线程在运行。这些后台线程不包括调用`public static void main(String[])`的main线程以及所有这个main线程自己创建的线程
- 这些主要的后台系统线程在Hotspot JVM里主要是以下几个：

1. **虚拟机线程**：这种线程的操作是需要JVM达到安全点才会出现。这些操作必须在不同的线程中发生的原因是他们都需要JVM达到安全点，这样堆才不会变化。这种线程的执行类型括"stop-the-world"的垃圾收集，线程栈收集，线程挂起以及偏向锁撤销
2. **周期任务线程**：这种线程是时间周期事件的体现（比如中断），他们一般用于周期性操作的调度执行
3. **GC线程**：这种线程对在JVM里不同种类的垃圾收集行为提供了支持
4. **编译线程**：这种线程在运行时会将字节码编译成到本地代码
5. **信号调度线程**：这种线程接收信号并发送给JVM，在它内部通过调用适当的方法进行处理

#### PC寄存器


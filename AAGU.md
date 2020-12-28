# Google

Guava 是一组来自 Google 的核心 Java 库，包括新的集合类型（如 multimap 和 multiset）、不可变集合、图形库以及用于并发、I/O、散列、缓存、原语、字符串等的实用程序！被广泛应用于 Google 的大多数 Java 项目中，也被许多其他公司广泛使用。

导入Guava依赖

```xml
<!-- https://mvnrepository.com/artifact/com.google.guava/guava -->
<dependency>
    <groupId>com.google.guava</groupId>
    <artifactId>guava</artifactId>
    <version>29.0-jre</version>
</dependency>
```

一个不可变的 ImmutableXxx 集合可以通过以下几种方式创建：

​	使用 copyOf方法，如 ImmutableSet.copyOf(set)使用 of 方法, 如 ImmutableSet.of("a", "b", "c") 或 ImmutableMap.of("a", 1, "b", 2)使用 Builder 方法,

​	Guava 为 java jdk 每种标准集合类型提供了简单易用的不可变版本，包括 Guava 自己的集合变体，为 java 提供的不可变版本都是继承 java jdk 的接口而来，所以操作上基本无异。下面接口的实现类也都有对应的不可变版本

| 接口                   | JDK 或者 Guava | 不可变版本                  |
| :--------------------- | :------------- | :-------------------------- |
| Collection             | JDK            | ImmutableCollection         |
| List                   | JDK            | ImmutableList               |
| Set                    | JDK            | ImmutableSet                |
| SortedSet/NavigableSet | JDK            | ImmutableSortedSet          |
| Map                    | JDK            | ImmutableMap                |
| SortedMap              | JDK            | ImmutableSortedMap          |
| Multiset               | Guava          | ImmutableMultiset           |
| SortedMultiset         | Guava          | mmutableSortedMultiset      |
| Multimap               | Guava          | ImmutableMultimap           |
| ListMultimap           | Guava          | ImmutableListMultimap       |
| SetMultimap            | Guava          | ImmutableSetMultimap        |
| BiMap                  | Guava          | ImmutableBiMap              |
| ClassToInstanceMap     | Guava          | ImmutableClassToInstanceMap |
| Table                  | Guava          | ImmutableTable              |

详情见：[Guava](https://blog.csdn.net/wangmx1993328/article/details/103533060#Guava%20%E6%A6%82%E8%BF%B0)



![img](https://user-gold-cdn.xitu.io/2018/8/31/1658bcc0e49550ca?imageslim)

# Apach

导入Apache commons-I/O 

```xml
<!-- https://mvnrepository.com/artifact/commons-io/commons-io -->
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.7</version>
</dependency>
```

导入Apache commons-BeanUtils 

```xml
<!-- https://mvnrepository.com/artifact/commons-beanutils/commons-beanutils -->
<dependency>
    <groupId>commons-beanutils</groupId>
    <artifactId>commons-beanutils</artifactId>
    <version>1.9.4</version>
</dependency>
```

导入Apache commons-Lang

```xml
<!-- https://mvnrepository.com/artifact/org.apache.commons/commons-lang3 -->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-lang3</artifactId>
    <version>3.11</version>
</dependency>
```

导入Apache commons-Collections

```xml
<!-- https://mvnrepository.com/artifact/org.apache.commons/commons-collections4 -->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-collections4</artifactId>
    <version>4.4</version>
</dependency>
```

导入Apache commons-codec

```xml
<!-- https://mvnrepository.com/artifact/commons-codec/commons-codec -->
<dependency>
    <groupId>commons-codec</groupId>
    <artifactId>commons-codec</artifactId>
    <version>1.14</version>
</dependency>
```




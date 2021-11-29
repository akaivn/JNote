# Dubbo

## 1、分布式概念和基础

### 分布式系统概念

分布式系统相当于若干计算机的集合，它们能够整体提供服务，==而对于用户来说相当于单个服务。==（它的理念跟 Nginx+Tomcat 的理念相似）

### 为什么要有分布式系统

随着互联网网站规模的扩大，传统的单个服务器加单个计算机已经无法满足处理多个请求，如淘宝、京东等大型电商网站。此时就考虑将项目功能模块拆分成一个个单独的模块，再将他们部署到不同的计算机的服务器上，对外是一个整体。

### 分布式系统引出的问题

分布式系统的出现引出了模块之间相互引用相互依赖的复杂性，此时我们急需一个能够治理和操作多个模块的协调工作的工具，Dubbo就应运而生。

### 应用架构的演变

![image-20200826202101306](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/26/202102-586476.png)

拆开上图来分别叙说

#### 1、All in One

所有的功能单体集中到一台服务器中， 不方便维护和扩展 

![image-20200826203437940](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/26/203439-908020.png)

#### 2、MVC，垂直架构

我们基于上述集中的功能和模块 做简体拆分，把他们分成一个个单独的模块分别部署到不同的服务器中去，他们的功能(WEB+JDBC+Controller+Model)都独立在自己的模块中。

缺点：

-   应用之间不可能完全独立，必定还会有各种引用

-   页面的修改可能也会引起此服务器上的项目进行重新打包和部署

![image-20200826203603058](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/26/203604-283693.png)

#### 3、RPC，远程过程调用

再基于上述架构做调整，我们将 核心业务再做分离，形成一个个的中心服务，谁用谁就调。

此时RPC问题暴露出来，我们需要一个能够解决RPC问题的服务组件或工具

![image-20200826203735618](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/26/203737-33128.png)



![image-20200826203903557](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/26/203905-467164.png)

#### 4、流动计算架构

我们将上述架构分析发现，服务器载有的不同的模块 有的服务器接收比较大的请求量，而有的服务器接收很小的请求量，此时就会造成资源的浪费。能不能==实时监控和管理资源的调度问题==？

流动式架构由此而生

![image-20200826203925108](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/26/203927-723245.png)



### RPC

上述提到了这个概念名词，什么是RPC呢？

==RPC【Remote Procedure Call】是指远程过程调用，是一种进程间通信方式，他是一种技术的思想，而不是规范==。它允许程序调用另一个地址空间（通常是共享网络的另一台机器上）的过程或函数，而不用程序员显式编码这个远程调用的细节。即程序员无论是调用本地的还是远程的函数，本质上编写的调用代码基本相同。

#### RPC的原理图

![image-20200826204030618](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/26/204031-468401.png)



![image-20200826204041331](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/26/204041-224012.png)

==RPC两个核心模块：通讯，序列化。==

## 2、Dubbo

Dubbo是阿里巴巴公司开源的一个高性能优秀的服务框架，使得应用可通过高性能的 RPC 实现服务的输出和输入功能，可以和spring框架无缝集成。它是一款高性能、轻量级的开源Java RPC框架，它提供了三大核心能力：==面向接口的远程方法调用，智能容错和负载均衡，以及服务自动注册和发现==。

2018.2月 移交给Apache开源

### 设计架构

![image-20200826204552178](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/26/204553-245731.png)

### 核心概念

**服务提供者（Provider）**：暴露服务的服务提供方，服务提供者在启动时，向注册中心注册自己提供的服务

**服务消费者 (Consumer)** ：调用远程服务的服务消费方，服务消费者在启动时，向注册中心订阅自己所需的服务，服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用

**注册中心（Registry）**: 注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者

**监控中心（Monitor）**：服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心

### 调用关系说明

-   服务容器负责启动，加载，运行服务提供者
-   服务提供者在启动时，向注册中心注册自己提供的服务
-   服务消费者在启动时，向注册中心订阅自己所需的服务
-   注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者
-   服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用
-   服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心

### 安装

在这之前我们需要安装注册中心（Registry），Dubbo只做调度和管理（Manager），我们需要一个容器（注册中心）来放置我们的服务，注册中心我们选用==Zookeeper==

我们最好将一系列安装放到Linux中去，它会带给我们更高的性能和安全性

当然我们也会提供Windows的安装方法

#### Windows安装

##### Zookeeper

###### 1、下载zookeeper

网址 [https://mirror.bit.edu.cn/apache/zookeeper/zookeeper-3.6.1/apache-zookeeper-3.6.1-bin.tar.gz](https://mirror.bit.edu.cn/apache/zookeeper/zookeeper-3.6.1/apache-zookeeper-3.6.1-bin.tar.gz)

###### 2、解压zookeeper

解压运行zkServer.cmd ，初次运行会报错，没有zoo.cfg配置文件

###### 3、修改zoo.cfg配置文件

将conf下的zoo_sample.cfg复制一份改名为zoo.cfg即可。

注意几个重要位置：

-   dataDir=./  临时数据存储的目录（可写相对路径）

-   clientPort=2181  zookeeper的端口号

-   修改完成后再次启动zookeeper

###### 4、使用zkCli.cmd测试

-   ls /：列出zookeeper根下保存的所有节点

-   create –e /kate 123：创建一个kate 节点，值为123

-   get /kate ：获取/kate 节点的值

##### Dubbo-admin

###### 1、下载

https://github.com/apache/incubator-dubbo-ops

![image-20200826211627459](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/26/211628-244904.png)

###### 2、进入目录，修改dubbo-admin配置

修改 src\main\resources\application.properties 指定zookeeper地址

![image-20200826211744644](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/26/211747-102977.png)

###### 3、打包dubbo-admin

```sh
mvn clean package -Dmaven.test.skip=true 
```

###### 4、运行dubbo-admin

```sh
java -jar dubbo-admin-0.0.1-SNAPSHOT.jar
```

**注意：【有可能控制台看着启动了，但是网页打不开，需要在控制台按下ctrl+c即可】**

默认使用root/root 登陆

![image-20200826211902745](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/26/211906-773510.png)

#### Linux安装

Zookeeper 的安装依赖于JDK环境，因此我们需要提前安装JDK

因已学Docker，所以放弃使用原生Linux安装，转而使用Docker，Docker安装Zookeeper，Springboot笔记有介绍，这里还是说一下原生安装

另附JDK安装教程

##### JDK8

###### 1、下载JDK

http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

###### 2、上传到服务器并解压

```shell
tar -zxvf
```

###### 3、设置环境变量

```shell
# 进入此文件夹
/usr/local/java/jdk1.8.0_171
# 执行此命令
vim /etc/profile
# 文件末尾加入下面配置
export JAVA_HOME=/usr/local/java/jdk1.8.0_171
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
# 4、使环境变量生效&测试JDK
java -version
java
javac
```

##### Zookeeper

###### 1、下载

 https://archive.apache.org/dist/zookeeper/zookeeper-3.4.11/    

wget https://archive.apache.org/dist/zookeeper/zookeeper-3.4.11/zookeeper-3.4.11.tar.gz     

###### 2、解压

###### 3、移动到指定位置并改名为zookeeper

![image-20200826213328601](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/26/213329-758338.png)

![image-20200826213337998](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/26/213338-895608.png)

###### 4、开机启动zookeeper

```shell
#!/bin/bash
#chkconfig:2345 20 90
#description:zookeeper
#processname:zookeeper
ZK_PATH=/usr/local/zookeeper
export JAVA_HOME=/usr/local/java/jdk1.8.0_171
case $1 in
         start) sh  $ZK_PATH/bin/zkServer.sh start;;
         stop)  sh  $ZK_PATH/bin/zkServer.sh stop;;
         status) sh  $ZK_PATH/bin/zkServer.sh status;;
         restart) sh $ZK_PATH/bin/zkServer.sh restart;;
         *)  echo "require start|stop|status|restart"  ;;
esac
```

把脚本注册为Service

```shell
chkconfig --add zookeeper
chkconfig --list
```

![image-20200826213526022](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/27/195415-676298.png)

增加权限

![image-20200826213544870](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/27/195227-305955.png)



###### 5、配置Zookeeper

##### 初始化zookeeper配置文件

拷贝/usr/local/zookeeper/conf/zoo_sample.cfg  

到同一个目录下改个名字叫zoo.cfg

![image-20200826213623896](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/26/213624-284268.png)

启动zookeeper

![image-20200826213637410](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/27/195230-357477.png)

##### Dubbo-admin

###### 1、安装Tomcat8（旧版dubbo-admin是war，新版是jar不需要安装Tomcat）

此处安装Tomcat略

###### 安装Dubbo-admin

dubbo本身并不是一个服务软件。它其实就是一个jar包能够帮你的java程序连接到zookeeper，并利用zookeeper消费、提供服务。所以你不用在Linux上启动什么dubbo服务。

但是为了让用户更好的管理监控众多的dubbo服务，官方提供了一个可视化的监控程序，不过这个监控即使不装也不影响使用

**此处同Windows安装一样，就此略**

此时发现Docker是一个多么好的东西。。。为了学习使用，我们选择在Windows上进行安装，后期移步到Docker上

### 起步

#### 环境搭建

创建两个maven工程，一个做服务消费者，一个做服务提供者

创建一个用户工程，提供用户信息 创建一个订单工程，消费用户信息

为了演示方便，均使用maven-java工程

![image-20200827201918356](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/27/201919-144057.png)

**创建的用户工程模块因为需要被订单工程模块所消费，所以我们需要将bean包下的类和用户service复制到订单工程模块中，假如以后有很多的bean和service我们也需要这样做？==因此Dubbo建议我们将这些公共的接口和bean，以及异常都放到一个公共的工程模块中去，通过maven工程的引入来做调用，而自己的模块就专门负责来实现，并且由Dubbo完成调度和管理==**

我们做一下抽离工作

![image-20200827202907596](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/27/202908-962572.png)

每个模块只做自己的实现，需要哪个功能时，从公共的工程中引用即可！

maven工程导入如下

```xml
<!--导入maven工程-->
<dependencies>
    <dependency>
        <groupId>org.example</groupId>
        <artifactId>public-interface-j</artifactId>
        <version>1.0-SNAPSHOT</version>
    </dependency>
</dependencies>
```

#### Dubbo改造开始

##### 配置服务提供者

使用 Dubbo的改造我们分为如下几步

**1、导入Dubbo和Zookeeper的依赖**

```xml
<!-- https://mvnrepository.com/artifact/com.alibaba/dubbo -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>dubbo</artifactId>
    <version>2.6.2</version>
</dependency>

<!-- 由于我们使用zookeeper作为注册中心，所以需要操作zookeeper  dubbo 2.6以前的版本引入zkclient操作zookeeper dubbo 2.6及以后的版本引入curator操作zookeeper下面两个zk客户端根据dubbo版本2选1即可-->

<!--基于此工程我们选择导入 curator-->
<!-- https://mvnrepository.com/artifact/org.apache.curator/curator-framework -->
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-framework</artifactId>
    <version>4.2.0</version>/2
</dependency>
```

**2、编写Spring配置文件**

配置服务提供者

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://code.alibabatech.com/schema/dubbo http://code.alibabatech.com/schema/dubbo/dubbo.xsd">


    <!--1、配置提供方的应用信息，注册到注册中心去，将服务发布-->
    <dubbo:application name="user-info"></dubbo:application>

    <!-- 2、指定注册中心的位置 -->
    <dubbo:registry protocol="zookeeper" address="127.0.0.1:2181"></dubbo:registry>

    <!-- 3、指定通信规则（通信协议？通信端口） -->
    <dubbo:protocol name="dubbo" port="20080"></dubbo:protocol>

    <!-- 4、暴露服务   ref：指向服务的真正的实现对象 -->
    <dubbo:service interface="kate.service.UserService" ref="userService"></dubbo:service>
    <!--配置实现的bean用于引入-->
    <bean id="userService" class="cn.kate.service.impl.UserServiceImpl"></bean>

</beans>
```

**3、编写启动类**

```java
public static void main(String[] args) throws IOException {
    ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("provider.xml");
    ioc.start();

    // 因为是java应用，spring程序读取完配置运行后会停止，因此我们使用以下代码来阻塞程序的终止
    // 按任意键退出
    System.in.read();
}
```

==注：Zookeeper一定要提前已经启动好的！==

启动dubbo-admin后 观察效果

**密码：root，账号：root**

**默认端口为：locahost:7001**

服务提供者

![image-20200827211802577](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/27/211804-780825.png)

提供的具体服务

![image-20200827211655307](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/27/211657-302818.png)

在哪个端口提供的

![image-20200827211731869](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/27/211732-28209.png)

##### 配置服务消费者

配置服务消费者的大致步骤跟提供者相差不大这里简略说明一下

**1、引入Dubbo和Zookeeper依赖**

**2、编写配置Spring配置文件**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://code.alibabatech.com/schema/dubbo http://code.alibabatech.com/schema/dubbo/dubbo.xsd">
	<!--配置包扫描-->
    <context:component-scan base-package="cn.kate.service"/>
    
    <!-- 1、消费方应用名，用于计算依赖关系，不是匹配条件，不要与提供方一样 -->
    <dubbo:application name="order-info"></dubbo:application>

    <!--2、指定注册中心的地址-->
    <dubbo:registry address="zookeeper://127.0.0.1:2181"></dubbo:registry>

    <!--3、指定要引入的接口 也就是服务提供者放在注册中心内的服务接口名，Dubbo会将它引入到Spring IOC中-->
    <dubbo:reference id="userService" interface="kate.service.UserService"></dubbo:reference>
</beans>
```

**3、编写启动类测试**

```java
/**
 * @author kate
 * @Date 2020/8/27 21:33
 */
public class MainApplication {
    public static void main(String[] args) throws IOException {
        ClassPathXmlApplicationContext classPathXmlApplicationContext = new ClassPathXmlApplicationContext("consumer.xml");
        OrderService orderService = classPathXmlApplicationContext.getBean(OrderService.class);
        orderService.initOrder("1");

        System.out.println("调用结束。。。");

        // 阻塞程序
        System.in.read();
    }
}
```

==注：dubbo.jar 和 zookeeper 必须已经在后台启动，且服务提供者必须在后台已启动==

**4、查看dubbo-admin控制台结果**

IDEA 控制台显示

![image-20200827213724876](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/27/213726-861663.png)

**dubbo-admin 控制台显示**

应用数＋1

![image-20200827213810234](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/27/213811-835115.png)

消费成功

![image-20200827213843538](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/27/213844-786886.png)

角色展示

![image-20200827213914252](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/27/213914-611892.png)

至此，一个简单的分布式工程环境就搭建好了！

##### 安装Monitor监控中心

Dubbo-admin是一个图形化的服务管理页面

而Monitor是一个图形化的简易的监控中心，它里面记录了服务的调用信息

**1、解压 incubator-dubbo-ops-master.zip**

**2、进入dubbo-monitor-simple\src\main\resources\conf**

**3、修改配置文件 dubbo.properties**

**4、打包dubbo-monitor-simple**

```shell
mvn clean package -Dmavne.test.skip = true
```

==一定要返回到有pom.xml文件的地方再进行打包，不然maven会报一个找不到 pom文件的异常==

**5、打包完成后即可进入 target文件夹 找到 dubbo-monitor-simple-2.0.0-assembly.tar.gz 进行解压缩**

**6、进入dubbo-monitor-simple-2.0.0\assembly.bin\start.bat 启动**

前提是你已经启动了zookeeper

**7、启动页面如下**

![image-20200828210520915](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/28/210521-379457.png)

注：

-   ==如果启动出现jetty错误，端口可能被占用，修改端口后需重新打包==

-   ==如果出现servlet-api缺失，要pom中引入，再次打包==

**8、访问**

端口地址即为你修改的jetty端口，默认为8080，访问界面如下

![image-20200828210743503](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/28/210744-885344.png)

接下来我们将监控中心配置到项目工程中去

在服务提供者和消费者的xml文件中加入以下配置

```xml
<!--配置监控中心-->
<!--此 protocol配置表示monitor启用自动发现注册中心并监控-->
<dubbo:monitor protocol="registry"></dubbo:monitor>

<!--我们也可以使用直连的方式-->
<!--<dubbo:monitor address="zookeeper://127.0.0.1:2181"></dubbo:monitor>-->
```

Simple Monitor 挂掉不会影响到 Consumer 和 Provider 之间的调用，所以用于生产环境不会有风险。

Simple Monitor 采用磁盘存储统计信息，请注意安装机器的磁盘限制，如果要集群，建议用mount共享磁盘。

将项目重新启动并观察监控中心

![image-20200828211526860](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/29/095408-645565.png)

此时代表我们的监控中心整合完毕！

### 整合Springboot

#### 1、导入官方的starter依赖

```xml
<!-- https://mvnrepository.com/artifact/com.alibaba.boot/dubbo-spring-boot-starter -->
<dependency>
    <groupId>com.alibaba.boot</groupId>
    <artifactId>dubbo-spring-boot-starter</artifactId>
    <version>0.2.0</version>
</dependency>
```

==以上导入的是alibaba的starter，我们也可以导入apache的，详情见springboot高级整合dubbo分布式==

#### 2、properties配置

将原来的xml配置文件的配置转移到springboot中的properties配置文件中

##### 服务提供者

```properties
# 配置全局应用的名称
dubbo.application.name=boot-provider
# 指定注册中心的协议
dubbo.registry.protocol=zookeeper
# 指定注册中心的地址
dubbo.registry.address=127.0.0.1:2181
# 指定通信的规则(协议) 和端口
dubbo.protocol.name=dubbo
dubbo.protocol.port=20880
# 配置监控中心monitor 使用自动发现配置
dubbo.monitor.protocol=registry
```

##### 服务消费者

```properties
# 配置全局唯一应用名
dubbo.application.name=boot-consumer
# 配置注册中心的地址
dubbo.registry.address=zookeeper://127.0.0.1:2181
# 配置监控中心
dubbo.monitor.protocol=registry
```

#### 3、建立通信

##### 服务提供者

```java
@Component // 加入到ioc容器中
@Service // 标注这是dubbo要注册到注册中心去的服务类
public class UserServiceImpl implements UserService {
    @Override
    public List<UserAddress> getUserAddressList(String userId) {
        System.out.println("UserServiceImpl.....old...");
        // TODO Auto-generated method stub
        UserAddress address1 = new UserAddress(1, "北京市昌平区宏福科技园综合楼3层", "1", "李老师", "010-56253825", "Y");
        UserAddress address2 = new UserAddress(2, "深圳市宝安区西部硅谷大厦B座3层（深圳分校）", "1", "王老师", "010-56253825", "N");
        return Arrays.asList(address1, address2);
    }
}
```

##### 服务消费者

```java
@Service
public class OrderServiceImpl implements OrderService {

	@Reference // 标注这个类是通过远程引用调用
	private UserService userService;
	@Override
	public List<UserAddress> initOrder(String userId) {
		//1、查询用户的收货地址
		List<UserAddress> addressList = userService.getUserAddressList(userId);
		return addressList;
	}

}
```

注意：如果想使Dubbo相关注解生效，必须配置包扫描

你可以使用properties配置

```properties
dubbo.scan.base-packages=xx.xx
```

或者使用注解

```java
@SpringBootApplication
@EnableDubbo
public class ConsumerApplication {
    public static void main(String[] args) {
        SpringApplication.run(ConsumerApplication.class, args);
    }
}
```

**监控结果显示**

![image-20200828221510204](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/28/221511-670541.png)

图标方式

![image-20200828221526272](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/28/221526-322069.png)

### 配置

#### 加载流程

以上我们配置的Dubbo使用的是xml文件，或者使用的是properties配置文件，dubbo官方提供一个写死的配置文件名==dubbo.properties==，此配置文件主要来存放一些公共的配置。以下是这些配置文件加载的顺序

![image-20200829101338188](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/29/101339-128115.png)

#### 启动时检查

Dubbo 缺省会在启动时检查依赖的服务是否可用，不可用时会抛出异常，阻止 Spring 初始化完成，以便上线时，能及早发现问题，默认 `check="true"`。

服务消费者启动时会检查注册中心中是否存在服务提供者信息，如果没有的话，它会拿到一个null，抛出一个异常

当spring启动懒加载或者通过延时加载 提供者时，消费者此时即可配置为启动时不检查，总是会返回引用，当服务恢复时，能自动连上

**示例**

##### 通过spring配置文件配置

```xml
<!--关闭某一个服务的启动时检查(没有提供者时报错)-->
<dubbo:reference interface="com.foo.BarService" check="false" />
<!--关闭所有的启动时检查(没有提供者时报错)-->
<dubbo:consumer check="false" />
<!--关闭注册中心启动时检查 (注册订阅失败时报错)-->
<dubbo:registry check="false" />
```

##### 通过dubbo.properties

```properties
dubbo.reference.com.foo.BarService.check=false
dubbo.reference.check=false
dubbo.consumer.check=false
dubbo.registry.check=false
```

##### 通过虚拟机参数

```shell
java -Ddubbo.reference.com.foo.BarService.check=false
java -Ddubbo.reference.check=false
java -Ddubbo.consumer.check=false 
java -Ddubbo.registry.check=false
```

当配置了 `check=false` 时，什么时候发起对注册中心的调用，什么时候再去检查注册中心中是否有对应的服务提供者

#### 超时

当后台程序需要时间来加载时，dubbo为了考虑响应的时间设置超时属性

我们可以通过以下配置来配置超时时间

##### xml

###### 消费者

```xml
<!--表示采用全局(消费者)的超时配置-->
<dubbo:consumer timeout="2000"></dubbo:consumer>
<!--表示采用接口级的超时配置-->
<dubbo:reference id="x" interface="y" timeout="1000"></dubbo:reference>
<!--表示采用方法级的超时配置-->
<dubbo:reference id="x" interface="y" timeout="1000">
    <dubbo:method name="getUserAddressList"/>
</dubbo:reference>
```

###### 提供者

```xml
<!--表示采用全局(提供者)的超时配置-->
<dubbo:provider timeout="3000"></dubbo:provider>
<!--表示采用接口级的超时配置-->
<dubbo:service interface="y" ref="x" timeout="4000"></dubbo:service>
<!--表示采用方法级的超时配置-->
<dubbo:service interface="x" ref="y" timeout="4000">
    <dubbo:method name="getUserAddressList"></dubbo:method>
</dubbo:service>
```

##### 注解

###### 消费者

```java
@Reference(timeout = 1000) 
```

###### 提供者

```java
@Service(timeout = 1000)
```

**注**：单位为 `ms`，层级关系为：`方法` ---> `接口`  ---> `全局` ，优先级 从高到低，默认不配置超时的情况下 为1000ms

**总结起来就两句话：**

-   ==方法级优先，接口级次之，全局配置再次之。==
-   ==如果级别一样，则消费方优先，提供方次之。==

**图解为：**

https://dubbo.apache.org/docs/zh-cn/user/sources/images/dubbo-config-override.jpg

**官方建议：**

建议由服务提供方设置超时，因为一个方法需要执行多长时间，服务提供方更清楚，如果一个消费方同时引用多个服务，就不需要关心每个服务的超时设置

#### 重试次数

重试次数的配置大多数都是跟超时 在一起使用的，当服务器超时时，会启动重试功能，而且他还有告知其他服务接着重试的功能

当你只有一台服务器(provider)启动，超时之后它会重新请求，当有多态服务器的时候它自己如果请求超时，就让别的服务来试

**配置如下**：

依照官方的文档，建议我们将超时时长设置到提供者中

```xml
<dubbo:service interface="cn.kate.service.UserService" ref="y" timeout="1000"></dubbo:service>
<bean id = "y" class = "cn.kate.service.impl.UserServiceImpl"></bean>
```

按照现实逻辑来看，重试的次数应该在消费者这里

```xml
<dubbo:reference id="userService" interface="cn.kate.service.UserService" retries="3"></dubbo:reference>
```

**注**：重试的次数设置不包含第一次，幂等函数可以多次重试，非幂等函数不应重试

**名词解释**

幂等：无论重试执行多少次，每次的结果都应该相同 【查询、删除、修改】

非幂等：每次执行都会发生不一样的结果【添加】

#### 多版本

一些已经发布的功能模块当我们想要在线上使用不同的版本时，一些地方想要使用旧版本，一些地方想要使用新版本，此时就可以使用dubbo提供的灰度发布功能，配置如下：

**1、我们模拟一下 将原本旧的模块 `UserServiceImpl`复制一份**

**2、配置provider.xml**

```xml
 <!-- 4、暴露服务   ref：指向服务的真正的实现对象 -->
    <dubbo:service interface="kate.service.UserService" ref="userService" version="1.0.0"></dubbo:service>
    <!--配置实现的bean用于引入-->
    <bean id="userService" class="cn.kate.service.impl.UserServiceImpl"></bean>

    <!--新版本-->
    <dubbo:service interface="kate.service.UserService" ref="userService02" version="2.0.0"></dubbo:service>
    <bean id="userService02" class="cn.kate.service.impl.UserServiceImpl2"></bean>
```

**3、服务发布**

**4、配置consumer.xml指定版本消费**

```xml
<dubbo:reference id="userService" interface="kate.service.UserService" version="2.0.0"></dubbo:reference>
```

#### 本地存根

远程服务后，客户端通常只剩下接口，而实现全在服务器端，但提供方有些时候想在客户端也执行部分逻辑，比如：做 ThreadLocal 缓存，提前验证参数，调用失败后伪造容错数据等等，此时就需要在 API 中带上 Stub，客户端生成 Proxy 实例，会把 Proxy 通过构造函数传给 Stub [[1\]](https://dubbo.apache.org/zh-cn/docs/user/demos/local-stub.html#fn1)，然后把 Stub 暴露给用户，Stub 可以决定要不要去调 Proxy。

如下图解释：

![/user-guide/images/stub.jpg](https://dubbo.apache.org/docs/zh-cn/user/sources/images/stub.jpg)

我们创建一个本地存根对象

通常线上的情况下我们会把它放在API工程中

```java
/**
 * @author kate
 * @Date 2020/8/29 13:34
 */
public class LocalStubUserService implements UserService {

    private final UserService userService;

    // dobbo 会给我们自动传入远程的代理对象
    public LocalStubUserService(UserService userService) {
        this.userService = userService;
    }

    @Override
    public List<UserAddress> getUserAddressList(String userId) {
        System.err.println("xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx");
        // 此代码在客户端执行，我们可以做一些业务逻辑的实现
        if(!userId.equals("1")){
            return userService.getUserAddressList(userId);
        }
        return null;
    }
}
```

配置xml文件

provider.xml

```xml
<dubbo:service interface="com.foo.BarService" stub="com.foo.BarServiceStub" />
```

或

```xml
<dubbo:service interface="com.foo.BarService" stub="true" />
```

#### Springboot纯注解

将每一个组件单独创建到应用中，最后在主配置类上启用包扫描即可，示例如下：

```java
package cn.kate.config;

import cn.kate.service.impl.UserServiceImpl;
import com.alibaba.dubbo.config.*;
import kate.service.LocalStubUserService;
import kate.service.UserService;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.util.Arrays;

/**
 * @author kate
 * @Date 2020/8/29 18:48
 */
@Configuration
public class DboConfig {

    // 什么样的标签名就对应了什么样的类
    // <dubbo:application name="user-info"></dubbo:application>
    @Bean
    public ApplicationConfig applicationConfig(){
        ApplicationConfig applicationConfig = new ApplicationConfig();
        applicationConfig.setName("user-info");
        return applicationConfig;
    }

    // <dubbo:registry protocol="zookeeper" address="127.0.0.1:2181"></dubbo:registry>
    @Bean
    public RegistryConfig registryConfig(){
        RegistryConfig registryConfig = new RegistryConfig();
        registryConfig.setProtocol("zookeeper");
        registryConfig.setAddress("127.0.0.1:2181");
        return registryConfig;
    }

    // <dubbo:protocol name="dubbo" port="20082"></dubbo:protocol>
    @Bean
    public ProtocolConfig protocolConfig(){
        ProtocolConfig protocolConfig = new ProtocolConfig();
        protocolConfig.setName("dubbo");
        protocolConfig.setPort(20080);
        return protocolConfig;
    }
    // <dubbo:service interface="kate.service.UserService" ref="userService" version="1.0.0" stub="kate.service.LocalStubUserService"></dubbo:service>
    @Bean
    public ServiceConfig<UserService> serviceServiceConfig(UserService userService){
        ServiceConfig<UserService> userServiceServiceConfig = new ServiceConfig<>();
        userServiceServiceConfig.setRef(userService);
        userServiceServiceConfig.setInterface(UserService.class);
        userServiceServiceConfig.setVersion("1.0.0");
        userServiceServiceConfig.setStub("true");
        // 配置方法
        MethodConfig methodConfig = new MethodConfig();
        methodConfig.setTimeout(2000);

        userServiceServiceConfig.setMethods(Arrays.asList(methodConfig));
        return userServiceServiceConfig;
    }
}
```

最后的启动类上要加上扫描dubbo相关注解的注解

```java
@SpringBootApplication
@EnableDubbo(scanBasePackages = "cn.kate") // or @DubboComponentScan("cn.kate")
public class ProviderApplication {
    public static void main(String[] args) {
        SpringApplication.run(ProviderApplication.class, args);
    }
}
```

### 高可用

#### Zookeeper宕机与Dubbo直连

当Zookeeper宕机的时候，Dubbo依旧能够读取已经读取过的那些服务，这是因为`本地缓存`的原因

详情见下方说明

-   监控中心宕掉不影响使用，只是丢失部分采样数据

-   数据库宕掉后，注册中心仍能通过缓存提供服务列表查询，但不能注册新服务
-   注册中心对等集群，任意一台宕掉后，将自动切换到另一台
-   **注册中心全部宕掉后，服务提供者和服务消费者仍能通过本地缓存通讯**
-   服务提供者无状态，任意一台宕掉后，不影响使用
-   服务提供者全部宕掉后，服务消费者应用将无法使用，并无限次重连等待服务提供者恢复

此时当Zookeeper宕机后，我们仍然能够通过Dubbo直连的方式来获得已提供的服务

**注解**

```java
@Reference(url = "127.0.0.1:20080")
```

**java**

```java
ReferenceConfig<XxxService> reference = new ReferenceConfig<XxxService>(); // 此实例很重，封装了与注册中心的连接以及与提供者的连接，请自行缓存，否则可能造成内存和连接泄漏
// 如果点对点直连，可以用reference.setUrl()指定目标地址，设置url后将绕过注册中心，
// 其中，协议对应provider.setProtocol()的值，端口对应provider.setPort()的值，
// 路径对应service.setPath()的值，如果未设置path，缺省path为接口名
reference.setUrl("dubbo://10.20.130.230:20880/com.xxx.XxxService"); 
```

#### 负载均衡机制

在集群负载均衡时，Dubbo 提供了多种均衡策略，缺省为 random 随机调用

>   随机分配(Random LoadBalance)

应用根据权重被分配到各个服务

![image-20200830091519261](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/30/091520-826272.png)

问题：在一个截面上碰撞的概率高，但调用量越大分布越均匀，而且按概率使用权重后也比较均匀，有利于动态调整提供者权重

>轮询(RoundRobin LoadBalance)

按公约后的权重设置轮循比率

![image-20200830091928493](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/30/091929-947803.png)

问题：存在慢的提供者累积请求的问题，比如：第二台机器很慢，但没挂，当请求调到第二台时就卡在那，久而久之，所有请求都卡在调到第二台上

>   最少活跃调用数(LeastActive LoadBalance)

相同活跃数的随机，活跃数指调用前后计数差

![image-20200830092122974](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/30/092123-760790.png)

问题：使慢的提供者收到更少请求，因为越慢的提供者的调用前后计数差会越大

>   一致性 Hash(ConsistentHash LoadBalance)

相同参数的请求总是发到同一提供者

![image-20200830092234126](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/30/092234-962368.png)

问题：当某一台提供者挂时，原本发往该提供者的请求，基于虚拟节点，平摊到其它提供者，不会引起剧烈变动。算法参见：http://en.wikipedia.org/wiki/Consistent_hashing

-   缺省只对第一个参数 Hash，如果要修改，请配置

```xml
 <dubbo:parameter key="hash.arguments" value="0,1" />
```

-   缺省用 160 份虚拟节点，如果要修改，请配置

```xml
<dubbo:parameter key="hash.nodes" value="320" />
```

均衡机制相关API如下：

![image-20200830092649897](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/30/092650-585632.png)

如何配置Dubbo的负载均衡？

**服务提供者**

```xml
<dubbo:service interface="..." loadbalance="roundrobin" />
```

**提供者方法级别配置**

```xml
<dubbo:service interface="...">
    <dubbo:method name="..." loadbalance="roundrobin"/>
</dubbo:service>
```

**服务消费者**

```xml
<dubbo:reference interface="..." loadbalance="roundrobin" />
```

**消费者方法级别配置**

```xml
<dubbo:reference interface="...">
    <dubbo:method name="..." loadbalance="roundrobin"/>
</dubbo:reference>
```

更多properties，注解，API等配置方式，参照xml来配即可！

关于负载均衡的权重配置，还可在dubbo-admin控制台中进行升降权重

#### 服务降级

当服务器压力剧增的情况下，根据实际业务情况及流量，对一些服务和页面有策略的不处理或换种简单的方式处理，从而释放服务器资源以保证核心交易正常运作或高效运作

```java
RegistryFactory registryFactory = ExtensionLoader.getExtensionLoader(RegistryFactory.class).getAdaptiveExtension();
Registry registry = registryFactory.getRegistry(URL.valueOf("zookeeper://10.20.153.10:2181"));
registry.register(URL.valueOf("override://0.0.0.0/com.foo.BarService?category=configurators&dynamic=false&application=foo&mock=force:return+null"));
```

-   `mock=force:return+null` 表示消费方对该服务的方法调用都直接返回 null 值，不发起远程调用。用来屏蔽不重要服务不可用时对调用方的影响
-   还可以改为 `mock=fail:return+null` 表示消费方对该服务的方法调用在失败后，再返回 null 值，不抛异常。用来容忍不重要服务不稳定时对调用方的影响

关于服务降级，还可在dubbo-admin控制台中进行

#### 集群容错

##### 什么是容错？

指当系统在运行时有错误被激活的情况下仍能保证不间断提供服务的方法

==在集群调用失败时，Dubbo 提供了多种容错方案，缺省为 failover 重试==

##### 容错模式

>   Failover Cluster  

**不配置任何容错策略时，默认为此模式**

失败自动切换，当出现失败，重试其它服务器 [[1\]](https://dubbo.apache.org/zh-cn/docs/user/demos/fault-tolerent-strategy.html#fn1)。通常用于读操作，但重试会带来更长延迟。可通过 `retries="2"` 来设置重试次数(不含第一次)

配置重试的次数参照上述  **配置 ---> 重试次数**

>   FailFast Cluster

快速失败，只发起一次调用，失败立即报错。通常用于非幂等性的写操作，比如新增记录

>   Failsafe Cluster

安全失败，出现异常时，直接忽略。通常用于写入审计日志等操作

>   Failback Cluster

失败自动恢复，后台记录失败请求，定时重发。通常用于消息通知操作

>   Forking Cluster

并行调用多个服务器，只要一个成功即返回。通常用于实时性要求较高的读操作，但需要浪费更多服务资源。可通过 `forks="2"` 来设置最大并行数

>Broadcast Cluster

广播调用所有提供者，逐个调用，任意一台报错则报错 [[2\]](https://dubbo.apache.org/zh-cn/docs/user/demos/fault-tolerent-strategy.html#fn2)。通常用于通知所有提供者更新缓存或日志等本地资源信息

##### 集群配置

示例：

```xml
<dubbo:service cluster="failsafe" />
```

或

```xml
<dubbo:reference cluster="failsafe" />
```

##### 整合Hystrix

Hystrix 旨在通过控制那些访问远程系统、服务和第三方库的节点，从而对延迟和故障提供**更强大的容错能力**。Hystrix具备拥有回退机制和断路器功能的线程和信号隔离，请求缓存和请求打包，以及监控和配置等功能

**我们通过整合Hystrix来获得功能更完善的容错服务**

###### 1、导入依赖

根据springboot中央管理器控制版本

或使用最新的 spring-cloud-starter-netflix-hystrix

```xml
<!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-netflix-hystrix -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
    <version>2.2.5.RELEASE</version>
</dependency>
```

启动类加上注解 `@EnableHystrix` 表示启用Hystrix

###### 2、provider

在Dubbo的Provider上增加@HystrixCommand配置，这样子调用就会经过Hystrix代理

```java
@HystrixCommand // 表示启用Hystrix代理
@Override
public List<UserAddress> getUserAddressList(String userId) throws InterruptedException {
    System.out.println("UserServiceImpl.....old...");

    // 加一个有可能执行的异常模拟程序报错
    if(new Random().nextInt(2)==1){
        System.err.println("111111111111111111111111111111");
        throw new InterruptedException("程序执行异常，已中断！！！");
    }
    // TODO Auto-generated method stub
    UserAddress address1 = new UserAddress(1, "北京市昌平区宏福科技园综合楼3层", "1", "李老师", "010-56253825", "Y");
    UserAddress address2 = new UserAddress(2, "深圳市宝安区西部硅谷大厦B座3层（深圳分校）", "1", "王老师", "010-56253825", "N");

    return Arrays.asList(address1, address2);
}
```

###### 3、Consumer

对于Consumer端，则可以增加一层method调用，并在method上配置@HystrixCommand。当调用出错时，会走到fallbackMethod = "reliable"的调用里

```java
@Reference // 标注这个类是通过远程引用调用
private UserService userService;

@HystrixCommand(fallbackMethod = "helloHystrix") // 表示这是一个熔断方法 fallbackmethod表示出错后回调的方法
@Override
public List<UserAddress> initOrder(String userId) throws InterruptedException {
    //1、查询用户的收货地址
    List<UserAddress> addressList = userService.getUserAddressList(userId);
    return addressList;
}

// 出错后回调这个方法进行容错
public List<UserAddress> helloHystrix(String userId){
    return Arrays.asList(new UserAddress(1,"test","test","test","test","Y"));
}
```

注：**@EnableHystrix 提供者和消费者启动类上都要写**

### 原理

#### RPC与Netty

##### RPC

>   RPC必须具备的四个程序组件

**客户端（Client）、客户端代理对象（Client Stub）、服务端（Server）、服务端代理对象（Server Stub）**

>   一次完整的RPC调用过程（同步调用）主要分为以下几步

-   **客户端那一方以调用本地方式调用服务**
-   Client Stub接收到调用后负责将方法、参数等组装成能够进行网络传输的消息体
-   Client Stub找到服务地址，并将消息发送到服务端
-   Server stub收到消息后进行解码
-   Server stub根据解码结果调用Server端本地的服务
-   Server端的本地服务执行并将结果返回给Server stub
-   Server stub将返回结果打包成消息并发送至消费方
-   Client stub接收到消息，并进行解码
-   **服务消费方得到最终结果**

==RPC框架的目标就是要2~8这些步骤都封装起来，这些细节对用户来说是透明的，不可见的==

##### Netty

我们知道了Dubbo是采用RPC进行的远程调用，那他们服务器与服务之间是如何进行通信的呢？

Dubbo服务于服务间的通信采用的是Netty

在 说Netty之前，我们先了解一下什么是异步非阻塞IO和阻塞IO

###### 什么是NIO 和 BIO

>   BIO （Blocking IO）：阻塞IO

当服务请求过来时，程序为每一个请求创建一个线程，每一个线程对应每一个请求，未执行完毕时，线程中途不能退出，这就是阻塞式IO

![image-20200831195400686](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/31/195401-714246.png)

我们知道这样的话，一个线程只能处理一个资源请求，会造成性能上的浪费，效率也不高，此时NIO无疑是很好的选择

>   NIO（Non-Blocking IO）：非阻塞式 IO

NIO 中通过引入几个重要的组件来实现非阻塞

Channel：通道

Selector：选择器也可以翻译为 **多路复用器**

还是 当有服务请求过来时，程序会为请求创建一个通道，多个通道集中统一由Selector 来监听，而一个Selector只由一个线程控制。我们通过Selector了解到通道中的请求发生某些事件时，使用Seletor控制请求到达指定的位置处再执行

通过下图，可以了解到一个比较清晰的NIO机制

![image-20200831200522136](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/31/200525-409392.png)

它的几个事件分别为：

-   Connect（连接就绪）
-   Accept（接受就绪）
-   Read（读就绪）
-   Write（写就绪）

###### 什么是Netty

Netty是一个异步事件驱动（NIO）的网络应用程序框架， 用于快速开发可维护的高性能协议服务器和客户端。它极大地简化并简化了TCP和UDP套接字服务器等网络编程

>   Netty的原理图

![image-20200831200807019](https://gitee.com/QianKa/image-bucket/raw/master/typora/202008/31/200807-860552.png)

#### Dubbo原理

##### 框架设计

![image-20200902191757801](https://gitee.com/QianKa/image-bucket/raw/master/typora/202009/02/191801-69883.png)

-   config 配置层：对外配置接口，以 ServiceConfig, ReferenceConfig 为中心，可以直接初始化配置类，也可以通过 spring 解析配置生成配置类
-   proxy 服务代理层：服务接口透明代理，生成服务的客户端 Stub 和服务器端 Skeleton, 以 ServiceProxy 为中心，扩展接口为 ProxyFactory
-   registry 注册中心层：封装服务地址的注册与发现，以服务 URL 为中心，扩展接口为 RegistryFactory, Registry, RegistryService
-   cluster 路由层：封装多个提供者的路由及负载均衡，并桥接注册中心，以 Invoker 为中心，扩展接口为 Cluster, Directory, Router, LoadBalance
-   monitor 监控层：RPC 调用次数和调用时间监控，以 Statistics 为中心，扩展接口为 MonitorFactory, Monitor, MonitorService
-   protocol 远程调用层：封装 RPC 调用，以 Invocation, Result 为中心，扩展接口为 Protocol, Invoker, Exporter
-   exchange 信息交换层：封装请求响应模式，同步转异步，以 Request, Response 为中心，扩展接口为 Exchanger, ExchangeChannel, ExchangeClient, ExchangeServer
-   transport 网络传输层：抽象 mina 和 netty 为统一接口，以 Message 为中心，扩展接口为 Channel, Transporter, Client, Server, Codec
-   serialize 数据序列化层：可复用的一些工具，扩展接口为 Serialization, ObjectInput, ObjectOutput, ThreadPool

##### 启动解析、加载配置

>   标签解析

![image-20200902192655070](https://gitee.com/QianKa/image-bucket/raw/master/typora/202009/02/192713-873987.png)

程序启动后，依托Spring加载，会由Spring初始化器  `BeanDefinitionParser` 来解析配置文件，首先来到  `DubboNamespaceHandler` 来创建解析器 再由 `DubboBeanDefinitionParser`  里面有一个parse 解析方法来解析，每次读取配置文件的标签来创建和装配不同的Dubbo对象（这些对象是如何被创建的呢？他们是由一个构造器创创建，每次根据读取的标签的值和原本定死的字符串比较，如果相同就创建它们的对象详情见下图 ==来自构造器的前一步的初始化方法(init)== [1]），

![1](https://gitee.com/QianKa/image-bucket/raw/master/typora/202009/02/194657-616081.png)

##### 服务暴露

等什么时候用Dubbo很熟练再回过头来看，内容太复杂！

P27

##### 服务引用

P28

##### 服务调用

P29

**更多配置和详情信息可见官网：**[Apache Dubbo](http://dubbo.apache.org/zh-cn/)
# SpringCloud

![](https://img.shields.io/badge/springboot-v2.3.4-blue?logo=spring)![](https://img.shields.io/badge/springcloud-v Hoxton.SR8-green?logo=spring)![](https://img.shields.io/badge/Zookeeper-v3.6.1-red?logo=zookeeper)![](https://img.shields.io/badge/Consul-v1.9.2-pink?logo=consul)![](https://img.shields.io/badge/Zipkin-v2.12.9-purple?logo=zipkin)![](https://img.shields.io/badge/Nacos server-v1.1.4-green?logo=nacos)![](https://img.shields.io/badge/Sentinel-v sentinel dashboard-yellow?logo=sentinel)![](https://img.shields.io/badge/Seata-v seata server-orange?logo=seata)

## 简介

Spring Cloud是一系列框架的有序集合。它利用[Spring Boot](https://baike.baidu.com/item/Spring Boot/20249767)的开发便利性巧妙地简化了分布式系统基础设施的开发，如服务发现注册、配置中心、消息总线、负载均衡、断路器、数据监控等，都可以用Spring Boot的开发风格做到一键启动和部署。Spring Cloud并没有重复制造轮子，它只是将各家公司开发的比较成熟、经得起实际考验的服务框架组合起来，通过Spring Boot风格进行再封装屏蔽掉了复杂的配置和实现原理，最终给开发者留出了一套简单易懂、易部署和易维护的分布式系统开发工具包

顾名思义：它是由一系列框架的组合，是分布式系统架构一站式解决方案，现SpringCloud已停更，由于互联网的发展，停更的它被Alibaba重新整合，又在其基础上发展出SpringCloud Alibaba

更多详情请移步：[SpringCloud介绍](https://blog.csdn.net/csdnnews/article/details/105304531)

更多SpringCloud介绍请移步百度：[SpringCloud简介]([https://baike.baidu.com/item/spring%20cloud/20269825?fr=aladdin](https://baike.baidu.com/item/spring cloud/20269825?fr=aladdin))

SpringCloud GitHub地址：[springcloud](https://github.com/spring-projects/spring-boot/releases/)

### SpringCloud组件图

![image-20200903200613969](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/03/200614-416148.png)

---

### 组件停更说明

-   1,Eureka停用,可以使用zk作为服务注册中心

-   服务调用,Ribbon准备停更,代替为LoadBalance
-   Feign改为OpenFeign
-   Hystrix停更,改为resilence4j 或者阿里巴巴的 sentienl
-   Zuul改为gateway
-   服务配置Config改为  Nacos
-   服务总线Bus改为Nacos

替换组件图如下：

![image-20200903201421390](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/03/201422-361317.png)

***

### 版本配合

Springboot与SpringCloud 如果搭配不好，极有可能造成jar包冲突，我们可能大部分时间使用的是IDEA的Spring Initializer 来快速构造Spring工程，但相关的版本问题，还是要有一定的了解

>   SpringCloud

SpringCloud版本迭代顺序以==英文大写字母开头==，例如Axx、Bxx、Cxx  .... ,截止到目前(2020.9.3)，SpringCloud最新版本为：**Hoxton SR8**

>   Springboot

Springboot的版本以 1.x.x ，2.x.x 开头，前面的数字一旦发生变化，就代表版本迭代的幅度大，后面的数字依次递之

下图为SpringCloud官网给出的版本配合

![Springboot+SpringCloud](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/03/194704-920618.png)

最终版本确定（官网推荐）：

-   Springboot 2.3.3.RELEASE
-   SpringCloud **Hoxton.SR8**

## 起步

### 环境搭建

#### 创建Maven父工程POM

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>cn.kate</groupId>
  <artifactId>springcloud</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>pom</packaging>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <junit.version>4.12</junit.version>
    <log4j.version>1.2.17</log4j.version>
    <lombok.version>1.18.10</lombok.version>
    <mysql.version>8.0.18</mysql.version>
    <druid.version>1.1.20</druid.version>
    <mybatis.spring.boot.version>2.1.1</mybatis.spring.boot.version>
  </properties>

  <dependencyManagement>
    <dependencies>
      <!-- spring boot 2.3.3-->
      <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-dependencies</artifactId>
        <version>2.3.3.RELEASE</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
      <!-- spring cloud Hoxton.SR8-->
      <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-dependencies</artifactId>
        <version>Hoxton.SR8</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
      <!-- spring cloud alibaba 2.1.0.RELEASE-->
      <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-alibaba-dependencies</artifactId>
        <version>2.2.1.RELEASE</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
      <!-- mysql-->
      <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>${mysql.version}</version>
      </dependency>
      <!-- druid-->
      <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>${druid.version}</version>
      </dependency>
      <!-- mybatis-->
      <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>${mybatis.spring.boot.version}</version>
      </dependency>
      <!-- lombok-->
      <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>${lombok.version}</version>
        <optional>true</optional>
      </dependency>
    </dependencies>
  </dependencyManagement>

  <build>
    <plugins>
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
        <configuration>
          <fork>true</fork>
          <addResources>true</addResources>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>
```

我们可以选择将父工程 打包到maven仓库，这样以来以后的子项目只需引入父工程即可实现统一jar包版本管理

```sh
mvn:install
```

#### 工程示例

本次为了演示SpringCloud，我们创建一个==支付模块==与==订单模块==的分布式项目工程

##### 支付模块构建

-   创建工程 pay

-   编写pom文件

```xml
<dependencies>
    <!--springboot-web-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!--监控-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>
    <!--mybatis-->
    <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
    </dependency>
    <!--druid-->
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
    </dependency>
    <!--mysql-connector-java-->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
    </dependency>
    <!--lombok-->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <optional>true</optional>
    </dependency>
    <!--springboot-test-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

-   编写yaml文件

```yaml
server:
  port: 8001

spring:
  application:
    name: cloud-provider-pay
  # 数据源
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    # MySQL
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/test
    username: root
    password: kate


mybatis:
  mapper-locations: classpath*:mapper/*.xml
  type-aliases-package: cn.kate.pojo
```

-   springboot启动类

-   创建数据库和表

```sql
SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for pay
-- ----------------------------
DROP TABLE IF EXISTS `pay`;
CREATE TABLE `pay`  (
  `id` int(0) NOT NULL AUTO_INCREMENT COMMENT 'id',
  `serializ` varchar(36) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_as_ci NULL DEFAULT NULL COMMENT '序列号(流水号)',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_as_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of pay
-- ----------------------------
INSERT INTO `pay` VALUES (1, '5efc07cf-eeaa-11ea-bc65-b025aa29c7a2');
INSERT INTO `pay` VALUES (2, '683a1c97-eeaa-11ea-bc65-b025aa29c7a2');

SET FOREIGN_KEY_CHECKS = 1;
```

-   创建pojo对象
-   创建JsonResult工具类用于封装json数据

```java
/**
 * @author kate
 * @Date 2020/9/4 20:46
 */
@Getter
@Setter
@ToString
public class JsonResult<T> {
    /**
     * 状态码
     */
    private Integer code;

    /**
     * 返回消息
     */
    private String message;

    /**
     * 封装返回的集合数据
     */
    private List<T> successList = new ArrayList<>();

    /**
     * 失败时封装失败信息的Map
     */
    private Map<String,String> failMap = new HashMap<>();
    private final static SimpleDateFormat sdf = new SimpleDateFormat("yyyy-mm-dd HH:mm:ss");
    /**
     * 私有构造
     */
    private JsonResult(){}

    /**
     * 响应成功
     * @return
     */
    public static JsonResult success(){
        JsonResult jsonResult = new JsonResult();
        jsonResult.setCode(200);
        jsonResult.setMessage("Response Success");
        return jsonResult;
    }

    /**
     * 响应失败
     * @return
     */
    public static JsonResult fail(){
        JsonResult jsonResult = new JsonResult();
        jsonResult.setCode(223);
        jsonResult.setMessage("Response Error");
        return jsonResult;
    }

    /**
     * 集合数据填充
     * @param t
     * @return
     */
    public JsonResult setData(T t){
        successList.add(t);
        return this;
    }

    /**
     * failMap信息封装
     * @param errorClass
     * @param errorMessage
     * @return
     */
    public JsonResult setFailMap(@NotNull String errorClass, @NotNull String errorMessage){
        failMap.put("errorClass", errorClass);
        failMap.put("errorMessage",errorMessage);
        failMap.put("timestamp",sdf.format(new Date()));
        return this;
    }
}
```

-   创建dao接口 (CRUD)
-   编写mapper配置文件
-   编写service业务接口
-   编写controller控制类
-   postman测试

##### JRebel热部署

热部署的JRebel必须是新版本支持springboot2.x的才可

##### XRebel应用

XRebel 是不间断运行在 web 应用的交互式分析器。可以看到网页上的每一个操作在前端以及服务端、数据库、网络传输都花费多少时间，当发现问题会在浏览器中显示警告信息。是调优程序、追踪性能问题的一大利器。

就是一个性能分析插件，可以跟JRebel一起使用也可以单独使用   破解需单独破解

在项目中使用XRebel启动后，左下方的框框便是XRebel的调试台，可以监控web类的加载时间、流程、SQL耗时、IO访问等

示例图：

![XRebel](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/05/125440-384114.png)

---

##### 订单模块构建

步骤跟上述差不多，唯一的区别就是客户端这边没有dao,service等，这些都是提供者的东西，客户端的端口也应设为默认的80，到达客户端的请求，客户端需再次请求提供者的信息才可消费服务

服务端只需提供pojo类和controller即可

controller

```java
/**
 * @author kate
 * @Date 2020/9/5 10:59
 */
@RestController
@RequestMapping("/consumer")
public class OrderController {

    @Autowired
    private RestTemplate restTemplate;
    public static final String URL = "http://localhost:8001/pay/";

    @GetMapping("/{id}")
    public JsonResult<Pay> query(@PathVariable("id") Integer id){
        System.out.println("xxxxxxxxx");
        return restTemplate.getForObject(URL + id, JsonResult.class);
    }

}
```

通过使用RestTemplate对象构造且发送Http请求来远程调用另一个服务的方法即可返回原提供者操作数据库后的数据

##### 问题引出

再说Dubbo那节就已经了解到，当进行分布式开发的时候，有点模块需要抽成公共的资源来统一访问公共资源即可，这样就避免了重复引用  造成代码冗余的问题，

##### 工程重构

创建一个单独的工程，命名为 `springcloud-api-commons`	将公共的接口、工具类、pojo对象都创建到此工程下，由每个工程单独引入即可。

## 组件

### Eureka

#### 简介

Eureka是Netflix开发的服务发现框架，本身是一个基于REST的服务，主要用于定位运行在AWS域中的中间层服务，以达到负载均衡和中间层服务故障转移的目的。SpringCloud将它集成在其子项目spring-cloud-Netflix中，以实现SpringCloud的服务发现功能。**它跟Dubbo做的功能相似**

服务注册与发现这里便不再细说

Eureka架构图

![image-20200905135807329](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/05/135808-60747.png)

---

##### 子组件

>   Eureka Server

Eureka Server提供服务注册服务，各个节点启动后，会在Eureka Server中进行注册，这样Eureka Server中的服务注册表中将会存储所有可用服务节点的信息，服务节点的信息可以在界面中直观的看到。

>   Eureka Client

Eureka Client是一个java客户端，用于简化与Eureka Server的交互，客户端同时也就是一个内置的、使用轮询(round-robin)负载算法的负载均衡器。

##### 说明

在应用启动后，将会向Eureka Server发送心跳,默认周期为30秒，如果Eureka Server在多个心跳周期内没有接收到某个节点的心跳，Eureka Server将会从服务注册表中把这个服务节点移除(默认90秒)。

Eureka Server之间通过复制的方式完成数据的同步，Eureka还提供了客户端缓存机制，即使所有的Eureka Server都挂掉，客户端依然可以利用缓存中的信息消费其他服务的API。综上，Eureka通过心跳检查、客户端缓存等机制，确保了系统的高可用性、灵活性和可伸缩性。

#### 起步

##### 单机构建

###### 创建Eureka工程作为注册中心

**1、创建工程**

**2、编写pom文件导入依赖**

```xml
<!--netflix-eureka-server-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
```

**3、编写yaml配置**

```yaml
server:
  port: 7001

eureka:
  client:
    # false表示不向注册中心注册自己
    register-with-eureka: false
    # false 表示自己代表的就是注册中心，职责就是维护服务实例，不需要检索服务
    fetchRegistry: false
    service-url:
      # 设置与Eureka Server交互的地址，查询服务和注册服务都需要此地址
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka # 使用${} 来动态声明，改动的时候不必顾忌此地
  instance:
    # 表示Eureka 服务端的实例名称
    hostname: localhost
```

**4、编写主启动类**

主配置类上且要加上此注解 `@EnableEurekaServer`来声明此实例为Eureka注册中心 

**5、运行实例并访问** [Eureka](http://localhost:7001)

访问结果如图：

![Eureka实例](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/05/173852-350844.png)

###### 支付工程入驻(provider)

将原本的 `springcloud-provider-pay` 工程上做一定的修改

>   添加pom依赖

```xml
<!--netflix-eureka-client-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

>   yaml文件添加相关配置

```yml
# Eureka Client 配置
eureka:
  client:
    # 表示将自己入驻入注册中心
    register-with-eureka: true
    fetchRegistry: true
    # 提供的服务的地址
    service-url:
      defaultZone: http://localhost:7001/eureka
```

>   主启动类上添加注解

```java
@EnableEurekaClient
```

启动工程观察注册中心 **Eureka Server一定要提前开启**

![image-20200905175701366](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/05/175701-50972.png)

显示服务已注册进入注册中心

此应用名对应支付工程模块中yaml配置里的  `spring.application.name`  

###### 订单工程入驻(consumer)

要完成远程过程调用，订单工程也需入驻入注册中心，且需订阅指定的服务提供者(provider)

步骤基本同上，这里不再详述

配置启动后，注册中心为下图

![image-20200905180938054](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/05/193559-411950.png)

此时，便可通过访问消费者的 http://localhost/consumer/1 来测试调用提供者的数据信息

##### 集群构建

###### 集群概念

为什么要集群？

集群的一个好处便是保证服务的高可用，当一台服务宕掉，其他服务依然可以支撑整个系统的运行

###### Eureka集群原理

![image-20200906090525493](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/06/090529-512852.png)

---

-   先启动Eureka注册中心
-   启动服务提供者pay支付业务
-   支付服务启动后把自身信息(比如服务地址以别名的方式注册进Eureka)
-   消费者order服务在需要调用接口时，使用服务别名去注册中心获取实际的RPC远程调用地址
-   消费者获得调用地址后，底层实际是利用HttpClient实现远程调用
-   消费者获得服务地址后会缓存到本地JVM内存中，默认每间隔30秒更新一次服务调用地址

**Eureka的集群搭建就是，工程与工程之间相互注册，相互守望**

###### Eureka集群环境搭建

-   创建新的工程 更改其端口为：7002
-   引入相关pom
-   修改本地hosts文件

因为是集群概念，本地的localhost无法同时被两台服务使用，配置一下hosts，使用虚拟域名来代替

```txt
127.0.0.1 eureka7001.cn
127.0.0.1 eureka7002.cn
```

-   编写yaml文件

```yaml
server:
  port: 7001

eureka:
  instance:
    # 表示Eureka 服务端的实例名称
    hostname: eureka7001.cn
  client:
    register-with-eureka: false
    fetchRegistry: false
    service-url:
      defaultZone: http://eureka7002.cn:7002/eureka # 将彼此注册到对方注册中心中
```

-   添加启动类上注解 `@EnableEurekaServer`

-   运行观察结果

[eureka7001](http:eureka7001.cn:7001)  or  [eureka7002](http://eureka7002.cn:7002)

---

###### 支付工程入驻

只需修改原yaml文件即可

```yaml
service-url:
	# 修改其配置，令其注册到两个注册中心去，使用 ‘ , ’ 隔开
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
```

###### 订单工程入驻

同上述无太大差别，修改订单工程，yaml文件，订阅每一个注册中心中的提供者地址，不再书写配置



启动服务后测试

**启动顺序一定要注意**

![image-20200906101115794](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/06/101115-131875.png)

服务成功注册，此时可访问order来消费提供者的服务

**问题**

我们在上述提供了注册中心集群配置，除了注册中心外，还有provider也要集群来使consumer**负载消费**

###### 支付工程集群

-   copy原有的支付工程

-   修改端口号
-   在`Paycontroller`添加如下代码

```java
@Value("${server.port}")
private Integer port;

// 在下方打印出 port即可知道是哪一个端口输出的信息
```

-   结果

**问题引出**

原消费者`OrderController` 处将访问的注册中心中的提供者信息写死，导致无法负载均衡访问另一台服务器。解决方法如下：

-   修改原消费者 `OrderController`

将

```java
/**
 * @author kate
 * @Date 2020/9/5 10:59
 */
@RestController
@RequestMapping("/consumer")
public class OrderController {

    @Autowired
    private RestTemplate restTemplate;
    public static final String URL = "http://localhost:8001/pay/";

    @GetMapping("/{id}")
    public JsonResult<Pay> query(@PathVariable("id") Integer id){
        System.out.println("xxxxxxxxx");
        return restTemplate.getForObject(URL + id, JsonResult.class);
    }
}
```

改为

```java
/**
 * @author kate
 * @Date 2020/9/5 10:59
 */
@RestController
@RequestMapping("/consumer")
public class OrderController {

    @Autowired
    private RestTemplate restTemplate;
    public static final String URL = "http://应用名"; // 此应用名便是 spring.application.name 的值

    @GetMapping("/{id}")
    public JsonResult<Pay> query(@PathVariable("id") Integer id){
        System.out.println("xxxxxxxxx");
        // 这个地方的路径要加上提供者基路径 不然404
        return restTemplate.getForObject(URL + "/pay/" + id, JsonResult.class); 
    }
}
```

-   修改 消费者 RestTemplate ，使其支持负载均衡

```java
@Configuration
public class RestConfig {

    @Bean
    // 加上如下注解 表示启动负载均衡
    @LoadBalanced
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
}
```

这样每次访问的结果变回使用负载均衡的方式，来访问提供者的信息。默认这种负载均衡策略为 ==轮询==

###### Actuator服务信息完善

问题引出：在上述微服务页面中我们发现，微服务的服务名有两个问题

-   服务名加上了主机名？
-   服务名没有显示具体IP？

使用Actuator可以完美解决这两个问题

我们要做的就是修改服务提供者或者消费者的yaml文件，一下举例为provider的yaml

在yaml  eureka 块中加入如下元素

```yaml
instance:
  instance-id: pay8001 # 此处配置为服务名
  prefer-ip-address: true # 次数配置显示IP
```

我们可以通过此路径来查看Actuator

```http
http://provider path or consumer path/actuator
```

查看健康状态

```http
http://provider path or consumer path/actuator/health
```

###### 服务发现Discovery

服务发现的功能就类似于网站上的==关于我们== 模块，里面罗列了各种服务的详细信息等

我们如果想要获得这些服务的信息，就可以通过Discovery来获得

示例如下：

使用订单模块演示

-   在订单模块的 `OrderController`  中加入如下代码

```java
@Autowired
private org.springframework.cloud.client.discovery.DiscoveryClient discoveryClient;

@GetMapping("/discovery/info")
    public Object info(){
        // 获取服务
        List<String> services = discoveryClient.getServices();
        for (String service : services) {
            log.info(service);
        }
        // 获得实例  此处为应用名 spring.application.name
        for (ServiceInstance instance : discoveryClient.getInstances("cloud-provider-pay")) {
            log.info(instance.getHost() + '\t' + instance.getPort() + '\t' + instance.getUri());
        }
        return discoveryClient;
    }
```

-   主启动类添加 `@EnableDiscoveryClient` 

-   结果如图

![image-20200906122926574](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/06/122929-43904.png)

#### Eureka自我保护

##### 什么是自我保护

![image-20200905180938054](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/05/193559-411950.png)

前面有说到Eureka自我保护机制，那什么是自我保护呢？

**保护模式主要用于一组客户端和Eureka Server之间存在网络分区场景下的保护。一旦进入保护模式，Eureka Server将会尝试保护其服务注册表中的信息，不再删除服务注册表中的数据，也就是不会注销任何微服务。**

##### 为什么会产生自我保护机制

默认情况下，如果EurekaServer在一定时间内没有接收到某个微服务实例的心跳，EurekaServer将会注销该实例（默认90秒)。但是当网络分区故障发生(延时、卡顿、拥挤)时，微服务与EurekaServer之间无法正常通信，以上行为可能变得非常危险了——因为微务本身其实是健康的，此时本不应该注销这个微服务。Eureka通过“自我保护模式”来解决这个问题——当EurekaServer节点在短时间内丢失过多客户端时(可能发生了网络分区故障)，那么这个节点就会进入自我保护模式。

**一句话：某一时刻某一个服务因为特殊原因暂时不可用了，Eureka不会立即清理掉该服务，依旧会对其进行保存处理**

==它是一种 高可用 的设计思想，属于CAP里面的AP分支==

##### 如何禁止自我保护

有时因为业务场景，我们需要将服务清理出去，此时就需要禁止Eureka开启自我保护功能

-   在Eureka Server yaml文件加入如下配置

```yaml
server:
    # 关闭自我保护模式
    enable-self-preservation: false
```

-   在提供者 Eureka Client yaml文件加入如下配置

```yaml
instance:
    # 客户端向服务端发送心跳的时间间隔 （默认30s）
    lease-renewal-interval-in-seconds: 1
    # Eureka Server 在收到最后一次心跳后等待下一次接受心跳的时间上限 (默认90s)
    lease-expiration-duration-in-seconds: 2
```

-   开启Eureka Server 后观察

![image-20200906163551535](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/06/163552-136077.png)

---

-   开启Eureka Client provider 后，服务可以正常被注册
-   关闭Eureka Client provider 程序后出现再刷新 Eureka Server后如下图

![image-20200906163744471](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/06/163745-473762.png)

此时Eureka Server 就会绝情的将 指定时间未发送心跳的服务剔除出 Server

#### Eureka停更说明

GitHub说明 [Eureka GitHub](https://github.com/Netflix/eureka/wiki)

Eureka 停更不停用，Eureka 1.x 依然是GitHub社区 Netflix 下活跃的项目

分布式注册中心的方案，除了Eureka ,我们还可以采用其他的在维护的方案使用

### Zookeeper

#### 简介

**Apache Zookeeper**是[Apache软件基金会](https://zh.wikipedia.org/wiki/Apache软件基金会)的一个软件项目，它为大型[分布式计算](https://zh.wikipedia.org/wiki/分布式计算)提供[开源](https://zh.wikipedia.org/wiki/开源)的分布式配置服务、同步服务和命名注册。 Zookeeper曾经是[Hadoop](https://zh.wikipedia.org/wiki/Hadoop)的一个子项目，但现在是一个独立的顶级项目。

#### 特点

Zookeeper的架构通过冗余服务实现[高可用性](https://zh.wikipedia.org/wiki/高可用性集群)。因此，如果第一次无应答，客户端就可以询问另一台Zookeeper主机。Zookeeper节点将它们的数据存储于一个分层的命名空间，非常类似于一个文件系统或一个[前缀树](https://zh.wikipedia.org/wiki/前缀树)结构。客户端可以在节点读写，从而以这种方式拥有一个共享的配置服务。更新是[全序](https://zh.wikipedia.org/wiki/全序)的。

#### 起步

安装就不再详说，详情可见Dubbo 或 Springboot高级 文档

Docker下查看Zookeeper的版本

```shell
docker inspect zookeeper

"WorkingDir": "/apache-zookeeper-3.6.1-bin"
```

##### SpringCloud整合Zookeeper

###### 支付工程入驻

-   复制pay工程、引入依赖

```xml
<!-- https://mvnrepository.com/artifact/org.apache.zookeeper/zookeeper -->
<dependency>
    <groupId>org.apache.zookeeper</groupId>
    <artifactId>zookeeper</artifactId>
    <version>3.6.1</version>
</dependency>
```

-   编写yaml文件

```yaml
server:
  port: 8003

spring:
  application:
    name: cloud-provider-pay
  cloud:
    zookeeper:
      connect-string: 39.98.204.116:2181
```

-   启动类加入  `@EnableDiscoveryClient`  用于发现服务获取服务信息

-   启动服务，注册到Zookeeper

注：如若遇到启动不起来的原因，排查可能是Zookeeper jar包冲突问题，可这样解决

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
    <!--排除zk依赖-->
    <exclusions>
        <exclusion>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<!--添加跟你Linux或Windows 上zk版本一致的依赖-->
<!-- https://mvnrepository.com/artifact/org.apache.zookeeper/zookeeper -->
<dependency>
    <groupId>org.apache.zookeeper</groupId>
    <artifactId>zookeeper</artifactId>
    <version>x.xx.xx</version>
</dependency>
```

-   从Zookeeper查看节点

Docker与Zookeeper zkClient 交互的命令

```shell
$ docker run -it --rm --link zktcl:zookeeper zookeeper zkCli.sh -server zookeeper
```

查看所有节点

```shell
$ ls /
```

查看Services下的节点

```shell
$ ls /services
```

###### Zookeeper节点问题

但支付工程注册到zk后，我们发现它多出来一个节点，节点下又存了一个`UUID`，那么这个节点是临时的呢？还是永久的呢？

我们来做一个实验：

停止provider-zk程序，观察Linux上节点下的`UUID` ，它是会像Eureka一样？过一段时间后接受不到心跳直接剔除？还是一没有直接就剔除？

![image-20200906183128014](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/06/183128-457781.png)

---

通过观察我们发现，它跟Eureka 一样，刚开始服务停掉的时候不会立即剔除，也是会接受服务的心跳，等到一定时间后没有再接受到服务的心跳时，再选择将服务剔除

>   再次重启provider-zk

---

我们发现再次重启后，服务就不再是之前的服务了，因此可以得到一个结论：==储存的节点是临时的==

###### 订单工程入驻

-   copy order工程
-   修改yaml文件

```yaml
server:
  port: 80

spring:
  application:
    name: cloud-consumer-order
  cloud:
    zookeeper:
      connect-string: 39.98.204.116:2181
```

-   启动主配置类

-   查看Linux下zk节点数
-   访问消费者地址

==注：服务端 `public static final String URL = "http://cloud-provider-pay"` 这个URL，是根据 yaml 文件内的 spring.application.name 的值决定的，它区分大小写，Eureka 的服务是大写，ZK的是小写==

#### 集群搭建

以后回过头再来看

### Consul

#### 简介

Consul是一个分布式、高可用的系统，是一个为了解决在生产环境中服务注册，服务发现，服务配置的一个工具，它有多个组件，提供如下几个关键功能：

##### 服务发现

Consul的某些客户端可以提供一个服务，例如api或者MySQL，其它客户端可以使用Consul去发现这个服务的提供者。使用DNS或者HTTP，应用可以很容易的找到他们所依赖的服务

##### 健康检查

Consul客户端可以提供一些健康检查，这些健康检查可以关联到一个指定的服务（服务是否返回200 OK），也可以关联到本地节点（内存使用率是否在90%以下）。这些信息可以被一个操作员用来监控集群的健康状态，被服务发现组件路由时用来远离不健康的主机

##### 键值存储

应用可以使用Consul提供的分层键值存储用于一些目的，包括动态配置、特征标记、协作、leader选举等等。通过一个简单的HTTP API可以很容易的使用这个组件

##### 多数据中心

Consul对多数据中心有非常好的支持，这意味着Consul用户不必担心由于创建更多抽象层而产生的多个区域

##### 可视化web界面

**更多详情可见官网**：https://www.consul.io/intro

**Consul中文文档**：https://www.springcloud.cc/spring-cloud-consul.html

**下载地址可见**：https://www.consul.io/downloads

#### 安装

下载好之后直接解压是一个.exe文件，使用cmd命令窗口打开查看版本

```shell
consul --version
```

运行

在命令窗口输入以下命令以启动consul

```sh
consul agent -dev
```

待到consul启动完毕后，我们可以通过浏览器来打开它web可视化界面

```http
http://localhost:8500
```

![image-20200908201934816](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/08/201935-451244.png)

---

#### 起步

##### 支付工程入驻

-   copy `provider `  工程
-   pom文件引入consul的依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-consul-discovery</artifactId>
</dependency>
```

-   yaml文件为

```yaml
server:
  port: 8006

spring:
  application:
    name: consul-provider-payment
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        service-name: ${spring.application.name}
```

-   编写controller代码用于测试

```java
/**
 * @author kate
 * @Date 2020/9/8 20:31
 */
@RestController
@RequestMapping("/pay")
@Slf4j
public class PayController {

    @Value("${server.port}")
    private String serverPort;

    @GetMapping(value = "/consul")
    public String paymentConsul(){
        return "springcloud with consul: "+serverPort+"\t"+ UUID.randomUUID().toString();
    }
}
```

-   启动程序

![image-20200908203503592](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/08/203504-220919.png)

---

consul注册中心成功入驻

##### 订单工程入驻

-   copy 消费者订单模块工程
-   添加consul的pom依赖
-   yaml文件如下

```yaml
server:
  port: 80

spring:
  application:
    name: consul-consumer-order
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        service-name: ${spring.application.name}
```

-   创建主启动类

-   Controller

```java
/**
 * @author kate
 * @Date 2020/9/8 20:40
 */
@RestController
@RequestMapping("/consumer")
@Slf4j
public class OrderController {

    public static final String INVOKE_URL = "http://consul-provider-payment";

    @Resource
    private RestTemplate restTemplate;

    @GetMapping("/consul")
    public String payment (){
        String result = restTemplate.getForObject(INVOKE_URL+"/pay/consul",String.class);
        return result;
    }
}
```

-   启动应用程序

![image-20200908204925317](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/08/204926-673463.png)

---

-   测试服务提供者的服务

```http
http://localhost/consumer/consul
```

我们试试停掉服务再观察consul控制台界面

![image-20200908210030726](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/08/210031-918376.png)

---

此时consul就将停止心跳的服务从注册中心剔除

#### 注册中心比对

##### 共同点

它们都遵从CAP架构思想和理念

>   什么是CAP

**C（Consistency）：强一致性**

**A（Availability）：可用性**

**P（Partition）：分区容错性**

==CAP理论关注粒度是数据，而不是整体系统设计的策略==

>   CAP图解

最多只能同时较好的满足两个。

CAP理论的核心是:一个分布式系统不可能同时很好的满足一致性，可用性和分区容错性这三个需求，因此，根据CAP原理将NoSQL数据库分成了满足CA原则、满足CР原则和满足AР原则三大类:

**CA-单点集群，满足一致性，可用性的系统，通常在可扩展性上不太强大**

**CP-满足一致性，分区容忍必的系统，通常性能不是特别高。**

**AP-满足可用性，分区容忍性的系统，通常可能对—致性要求低一些。I**

![image-20200908210832157](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/08/210833-780737.png)

##### 不同点

==Eureka属于 AP==

**当网络分区出现后，为了保证可用性，系统B可以返回旧值，保证系统的可用性。**

**结论:违背了一致性C的要求，只满足可用性和分区容错，即AP**

![image-20200908210446713](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/08/210447-618655.png)

---

==Consul 和 Zookeeper属于 CP==

**当网络分区出现后，为了保证一致性，就必须拒接请求，否则无法保证一致性**

**结论:违背了可用性A的要求，只满足一致性和分区容错，即CP**

![image-20200908211132557](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/08/211133-609246.png)

---

### Ribbon

#### 简介

Spring Cloud Ribbon**是一个基于HTTP和TCP的客户端负载均衡工具**，它基于Netflix Ribbon实现。通过Spring Cloud的封装，可以让我们轻松地将面向服务的REST模版请求自动转换成客户端负载均衡的服务调用。Spring Cloud Ribbon虽然只是一个工具类框架，它不像服务注册中心、配置中心、API网关那样需要独立部署，但是它几乎存在于每一个Spring Cloud构建的微服务和基础设施中。因为微服务间的调用，API网关的请求转发等内容，实际上都是通过Ribbon来实现的，包括后续我们将要介绍的Feign，它也是基于Ribbon实现的工具。所以，对Spring Cloud Ribbon的理解和使用，对于我们使用Spring Cloud来构建微服务非常重要

简而言之：**它是基于客户端的负载工具，是进程内的负载均衡，是RestTemplate + 负载均衡(Load Balance)**

详情可见官网：https://github.com/Netflix/ribbon/wiki/Getting-Started

Ribbon目前也已进入维护模式，未来的替代方案为SpringCloud-Load Balance，因为Ribbon优秀的性能，目前还在广泛使用，趋势是SpringCloud-Load Balance

#### 进程内和集中式负载均衡

>   进程内

**将负载均衡逻辑集成到consumer, consumer从注册中心获知有那些地址可以用,然后自己再从这些地址中选择出一个合适的 provider**

![preview](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/09/192429-609509.jpeg)

---

>   集中式

**即在 consumer 和provider 之间使用独立的负载均衡设施(比如Nginx等,通过某种策略转发给provider )**

![v2-a1b8be1be307b9b2c7faf70a2041c038_r](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/09/192543-795306.jpeg)

---

#### 进程内负载和集中式负载有什么区别

集中式负载均衡 , 有一个独立的负载均衡服务器部署在 客户端 与服务之间 ,由它来接受请求并分配给不同的服务

进程式负载均衡，将负载均衡逻辑集成到consumer .consumer从注册中心获悉那些服务可以用 ,再按照负载均衡策略使用服务

详情可见：https://www.jianshu.com/p/1bd66db5dc46  				https://zhuanlan.zhihu.com/p/79630044

#### 起步

##### 环境准备

所需环境为：Eureka Server 负载均衡工程，provider(pay)负载均衡工程，一个消费者(order)工程

##### 疑问

为什么Eureka 自己就可以做负载均衡还要引入Ribbon呢？

原因：并不是Eureka 本身做的负载均衡，而是高版本的Eureka依赖里引入了Ribbon的依赖，所有才有了负载均衡的能力

也就是说，我们在上述 `Eureka` 章节做过的负载均衡实例其实就是Ribbon的实例

本章节为了避免重复，此次采用另一种 `RestTemplate` 的方式来完成负载均衡的实例

##### RestTemplate使用

上述章节我们采用的是RestTemplate 的 `getForObject` 的方法请求查询返回数据，此次我们使用 `getForEntity` 的方式

```java
@GetMapping("/entity/{id}")
public JsonResult<Pay> query2(@PathVariable("id") Integer id){
    ResponseEntity<JsonResult> forEntity = new ResponseEntity<>(HttpStatus.valueOf(500));
    try {
        forEntity = restTemplate.getForEntity(URL + "/pay/" + id, JsonResult.class);
        HttpStatus statusCode = forEntity.getStatusCode();
        log.info("测试getForEntity" + forEntity.getStatusCode());
        return forEntity.getBody();
    } catch (Exception e) {
        return JsonResult.fail(forEntity.getStatusCodeValue()).setFailMap(e,"错误的消息");
    }
}
```

##### Ribbon的负载规则

###### Ribbon有哪几种负载策略

Ribbon的负载均衡由其核心组件`IRule` 来完成，**其目的就是根据特定算法从服务列表中选取一个要访问的服务** 

`IRule` 是一个Interface ，它有以下的实现类

`com.netflix.loadbalancer.RoundRobinRule`：轮询

`com.netflix.loadbalancer.RandomRule` ：随机

`com.netflix.loadbalancer.RetryRule` ：先按照RoundRobinRule的策略获取服务，如果获取服务失败则在指定时间内会进行重试

**WeightedResponseTimeRule**  ：对RoundRobinRule的扩展，响应速度越快的实例选择 权重越大，越容易被选择

**BestAvailableRule** ：会先过滤掉由于多次访问故障而处于**断路器**跳闸状态的服务，然后选择一个并发量最小的服务

断路器，会在以下章节将会详细叙说

**AvailabilityFilteringRule** ：先过滤掉故障实例，再选择并发较小的实例

**ZoneAvoidanceRule**：默认规则，复合判断server所在区域的性能和server的可用性选择服务器

Ribbon 默认采用轮询的方式进行负载，常用的负载算法为前三个

###### 如何替换负载规则

-   在Order工程中创建自定义的负载均衡类

```java
/**
 * @author kate
 * @Date 2020/9/10 19:45
 */
@Configuration
public class MyLoadBalance {
    /**
    * 表示启用 随机 的负载策略
    */
    @Bean
    public RandomRule randomRule(){
        return new RandomRule();
    }
}
```

注：==切记不可放在`@springbootApplication` 能供扫描到的包及其子包下，因为 `@CompoentScan` 能够扫描到的地方就会重新加载成默认的轮询方式的负载作为基本的负载方式==

-   主启动类上添加 `@RibbonClient` 并配置规则
    -   name：表示哪一个工程，此处所写的应该是 yaml文件中  spring.application.name
    -   configuration：表示启用的配置类，此处所写的是自定义类，支持多种配置规则，以数组形式出现

```java
@RibbonClient(name = "cloud-consumer-order",configuration = {MyLoadBalance.class})
public class MainApplicationWithOrder {}
```

-   启动order工程测试

###### 轮询负载原理

>   原理

它是如何实现的轮询调用服务呢？轮询的公式如下

```txt
请求次数 % 服务器集群总数 = 每次要轮询的服务的下标
```

请求次数从1 开始，如若服务器中间断开再次重启，那么计数再次从 1 开始

>   源码

类图如下，自行观看

![image-20200910204331954](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/10/204333-481616.png)

---

>   手写

简单手写一下基于Ribbon的  轮询  算法的实现

为了演示方便，我们需要先做以下几个步骤

-   在pay 的集群工程中 添加

```java
@GetMapping("/port")
public JsonResult<Integer> port(){
    return JsonResult.success().setData(port);
}
```

-   注释掉来自 order工程中的`@LoadBalance`  确保使用我们自定义的轮询方式
-   编写LoadBalance接口,采用面向接口编程

```java
/**
 * @author kate
 * @Date 2020/9/10 21:12
 */
public interface LoadBalance {
    /**
     * 返回注册中心中的容器实例集合
     * @param serviceInstances
     * @return
     */
    ServiceInstance serviceInstance(List<ServiceInstance> serviceInstances);
}
```

-   LoadBalance实现类

```java
/**
 * @author kate
 * @Date 2020/9/10 21:13
 */
@Component
public class LoadBalanceImpl implements LoadBalance {

    /**
     * 定义原子 整数初始值
     */
    private AtomicInteger atomicInteger = new AtomicInteger(0);

    /**
     * 每次返回的坐标
     *
     * @return
     */
    private final int getAndIncrement() {
        // 当前坐标
        int current;
        // 下一坐标
        int next;
        // 自旋锁开始
        do {
            // 当前值从 atomicInteger 获得
            current = atomicInteger.get();
            // 下一个值 如果不大于整形的最大值，就加 1，否则从 0 重新开始
            next = current > Integer.MAX_VALUE ? 0 : current + 1;
                                                // 第一个参数是期望值，第二个参数是修改值
            // 如果是期望值，取反，跳出循环并返回，否则一直循环
        } while (!this.atomicInteger.compareAndSet(current,next));
        System.out.println("当前的次数为：" + next);
        return next;
    }

    @Override
    public ServiceInstance serviceInstance(List<ServiceInstance> serviceInstances) {
        // 轮询算法开始，返回下标
        int index = getAndIncrement() % serviceInstances.size();
        return serviceInstances.get(index);
    }
}
```

-   OrderController

```java
@Autowired
private LoadBalance loadBalance;

@GetMapping("/lb")
    public JsonResult<Integer> lb(){
        // 填入服务名，获取服务实例集合
        List<ServiceInstance> instances = discoveryClient.getInstances("cloud-provider-pay");
        // 判空
        if(!(instances != null && instances.size() > 0))
            return null;
        // 调用开始
        ServiceInstance serviceInstance = loadBalance.serviceInstance(instances);
        // 获取URI
        URI uri = serviceInstance.getUri();
        // Rest调用
        Integer forObject = restTemplate.getForObject(uri + "/pay/port", Integer.class);
        return JsonResult.success().setData(forObject);
    }
```

-   测试

### OpenFeign

#### 简介

##### 什么是OpenFeign

在这之前我们先说一下Feign

**什么是Feign**

Feign是一个声明式的web服务客户端，让编写web服务客户端变得非常容易，**只需创建一个接口并在接口上添加注解即可**

Feign是Spring Cloud组件中的一个轻量级RESTful的HTTP服务客户端，用来做客户端负载均衡，去调用服务注册中心的
服务。Feign的使用方式是:使用Feign的注解定义接口，调用这个接口，就可以调用服务注册中心的服务

**Feign能做什么**

它旨在编写Java Http客户端时更加方便快捷

上述章节中我们讲述了，当两个微服务之间出现调用时，采用 Ribbon + RestTemplate 来完成，统一采用RestTemplate来完成对Http请求的封装，而在实际的开发中，一个接口往往会被多出依赖调用。==所以通常都会针对每个微服务自行封装一些客户端类来包装这些依赖服务的调用。所以，Feign在此基础上做了进一步封装，由他来帮助我们定义和实现依赖服务接口的定义。在Feign的实现下，我们只需创建一个接口并使用注解的方式来配置它（以前是Dao接口上面标注@Mapper注解,现在是一个微服务接口上面标注一个Feign注解即可==）即可完成对服务提供方的接口绑定，简化了使用Spring cloud Ribbon时，自动封装服务调用客户端的开发量

**集成Ribbon**

==利用Ribbon维护了Pay工程的服务列表信息，并且通过轮询实现了客户端的负载均衡。而与Ribbon不同的是，通过feign只需要定义服务绑定接口且以声明式的方法，优雅而简单的实现了服务调用==

**什么是OpenFeign**

它是一个封装了Restful风格的客户端调用组件，第一代是 `Feign` 现已很少有人使用。它内置了Ribbon，同样拥有负载均衡的能力。

OpenFeign是Spring Cloud 在Feign的基础上支持了SpringMvc的注解，如@RequestMapping等等。OpenFeign的@FeignClient可以解析SpringMvc的@RequestMapping注解下的接口，并通过动态代理的方式产生实现类，实现类中做负载均衡并调用其他服务。

#### 起步

##### 环境搭建

-   导入依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

-   yaml文件

```yaml
server:
  port: 80
eureka:
  client:
    register-with-eureka: false
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka, http://eureka7002.com:7002/eureka
```

-   主启动类加上 `@EnableFeignClients`
-   业务逻辑接口 Feign接口

```java
/**
 * @author kate
 * @Date 2020/9/14 19:41
 */
// 指定服务名 spring.application.name ,因为采用的是Eureka ,所以大写
@FeignClient("CLOUD-PROVIDER-PAY")
@Service
public interface PayFeignService {

    /**
     * 此接口代表映射到 provider 的 controller层的 /pay/port 方法
     * @return
     */
    @GetMapping("/pay/port")
    JsonResult<Integer> returnPort();
}
```

-   OrderController

```java
/**
 * @author kate
 * @Date 2020/9/14 19:45
 */
@RestController
@RequestMapping("/consumer")
public class OrderController {

    @Autowired
    private PayFeignService payFeignService;

    @GetMapping("/port")
    public JsonResult<Integer> returnPort(){
        return payFeignService.returnPort();
    }
}
```

-   启动服务

```http
http://localhost/consumer/port
```

因为OpenFeign集成Ribbon，所以拥有Ribbon的负载均衡能力！

通过使用OpenFeign提供的带有 @FeignClient 标注的 Service接口，我们可以轻松的使用服务调用，规避了使用RestTemplate的繁琐过程

##### 超时控制

我们知道，在实际的业务场景中，有的接口服务需要响应的时间会很长，而OpenFeign规定了默认的响应的时间为 1000ms ,一旦超过默认定义的时间，就会抛出 `ReadTimeOut` 异常。

我们可以手动的设置超时的时间，以便更好的访问控制

我们在上述章节中讲述了Ribbon的超时控制设置，因为OpenFeign内置Ribbon依赖，所以我们调节的其实还是Ribbon的超时时间

-   yaml文件如下

```yaml
ribbon:
  ReadTimeout:  5000
  ConnectTimeout: 5000
```

此处的单位为 `ms` ，代表的是超时等待的时间和链接等待的时间

##### 关于日志

OpenFeign为我们提供了详细日志的输出，使我们更方便的调试和观察接口访问的详细内容

它提供了以下日志级别

| 日志级别 | 描述                                                      |
| -------- | --------------------------------------------------------- |
| NONE     | 默认的，不显示任何日志                                    |
| BASIC    | 仅记录请求方法、URL、响应状态码及执行时间                 |
| HEADRES  | 除了BASIC中定义的信息之外，还有请求和响应的头信息         |
| FULL     | 除了HEADERS中定义的信息之外，还有请求和响应的正文及元数据 |

我们可根据需要自行设置

例

配置FULL级别的日志输出

-   编写配置类

```java
import feign.Logger;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * @author kate
 * @Date 2020/9/14 20:05
 */
@Configuration
public class FeignLogger {

    @Bean
    Logger.Level setLevel(){
        return Logger.Level.FULL;
    }
}
```

-   配置yaml文件

```yaml
logging:
  level:
    cn.kate.service.PayFeignService: debug # 配置为Debug 级别
```

-   可查看控制台详细信息

---

![image-20200914201405145](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/14/201405-77327.png)

---

### Hystrix

>   分布式系统问题的引出

==复杂分布式体系结构中的应用程序有数十个依赖关系，每个依赖关系在某些时候将不可避免地失败==

![image-20200914201814244](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/14/201815-901386.png)

---

>   服务雪崩

多个微服务之间调用的时候，假设微服务A调用微服务B和微服务C，微服务B和微服务C又调用其它的微服务，这就是所谓的“扇出”。
如果扇出的链路上某个微服务的调用响应时间过长或者不可用，对微服务A的调用就会占用越来越多的系统资源，进而引起系统崩溃，所谓的“雪崩效应”

对于高流量的应用来说，单一的后端依赖可能会导致所有服务器上的所有资源都在几秒钟内饱和。比失败更糟糕的是，这些应用程序还可能导致服务之间的延迟增加，备份队列，线程和其他系统资源紧张，导致整个系统发生更多的级联故障。这些都表示需要对故障和延迟进行隔离和管理，以便单个依赖关系的失败，不能取消整个应用程序或系统。所以

通常当你发现一个模块下的某个实例失败后，这时候这个模块依然还会接收流量，然后这个有问题的模块还调用了其他的模块，这样就会发生级联故障，或者叫雪崩。

>   断路器思想

能不能有一种工具能够在服务出故障的时候终止它的运行，避免级联故障，从而实现高可用呢？

#### 简介

>   什么是Hystrix

Hystrix是一个用于处理分布式系统的**延迟**和**容错**的开源库，在分布式系统里，许多依赖不可避免的会调用失败，比如超时、异常等。==Hystrix能够保证在一个依赖出问题的情况下，不会导致整体服务失败，避免级联故障，以提高分布式系统的弹性==

"断路器”本身是一种开关装置，当某个服务单元发生故障之后，通过断路器的故障监控（类似熔断保险丝)，向调用方返回一个符合预期的、可处理的备选响应(（FallBack)，而不是长时间的等待或者抛出调用方无法处理的异常，这样就保证了服务调用方的线程不会被长时间、不必要地占用，从而避免了故障在分布式系统中的蔓延，乃至雪崩。

>   用来做什么

-   服务降级
-   服务熔断
-   服务监控（接近实时的监控）
-   ......

>Hystrix停更说明

官网资料：https://github.com/Netflix/Hystrix/wiki/How-To-Use

Hystrix官宣，停更进维 不在接受合并，只被动的处理Bug

#### 重要概念

##### 服务降级

相信我们一定写过如下代码：

```java
if(){
    
}else if(){
    
}else if(){
    
}else{
    
}
```

从上述代码我们可以看出，当程序运行时，==我们需要返回一个结果，不管过程如果==，==最后一定要有一个兜底的结果返回==，而降级就可以这么理解：

**当服务发生错误时，我们需要友好的返回一个提示，不要让程序等待和抛出异常**，这就是降级的概念！

##### 服务熔断

我们上述叙说了什么是服务降级，那么什么是服务熔断呢？

从字面的意思我们了解到它应该是一个：当服务发生不可恢复的异常或者错误或者达到高并发访问的极限时，服务第一步会选择降级，接下来，会选择断开一部分服务的链接和提供的服务，最后再慢慢的恢复服务的提供。

它整体来说分为三步

**服务降级--->服务熔断---->服务慢慢恢复**

##### 服务限流

服务限流的意思就是：当并发访问量大时，会限制一部分用户的访问，会形成类似阻塞式的队列，限制访问人数，服务调用的频率，也是保障高可用的一种解决方案。

#### 起步

##### 环境搭建

-   导入Hystrix依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
</dependency>
```

-   新建 Hystrix 的 服务提供工程。服务注册中心由Eureka担任

-   为了方便测试，我们仍然使用单机版的Eureka
-   yaml文件

```yaml
server:
  port: 8001

eureka:
  client:
    register-with-eureka: true    
    fetch-registry: true   
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/
      
spring:
  application:
    name: cloud-provider-hystrix
```

-   编写主启动类

-   业务类Service 

一个正常接口，另一个访问耗时接口（模拟复杂的业务处理）

```java
/**
 * @author kate
 * @Date 2020/9/15 20:14
 */
@Service
public class HystrixServiceImpl implements HystrixService {

    @Override
    public String normalGetPort(Integer id) {
        return Thread.currentThread().getName() + " 传递过来的id为：" +id ;
    }

    @Override
    public String SleepGetPort(Integer id) {
        Integer ms = 3;
        try {
            TimeUnit.SECONDS.sleep(3);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return Thread.currentThread().getName() + " 传递过来的id为：" +id + ",耗时为(m)：" + ms;
    }
}
```

-   Controller

```java
/**
 * @author kate
 * @Date 2020/9/15 20:19
 */
@RestController
@RequestMapping("/hystrix")
public class HystrixController {

    @Autowired
    private HystrixService hystrixService;

    @Value("${server.port}")
    private Integer port;

    @GetMapping("/{id}")
    public String normalGetPort(@PathVariable("id") Integer id){
        return hystrixService.normalGetPort(port);
    }

    @GetMapping("/sleep/{id}")
    public String sleepGetPort(@PathVariable("id") Integer id){
        return hystrixService.SleepGetPort(port);
    }
}
```

上述都没什么好说的，只是一个搭建测试环境的过程，从下面开始才到Hystrix表演的时刻！

我们将按如下步骤演示

**从正确->错误->降级熔断->恢复**

制造高并发场景，使用 `Apache Jmeter` 压测工具

##### Apache Jmeter

>   什么是Apache Jmeter

是一个[Apache ](https://en.wikipedia.org/wiki/Apache_Software_Foundation)[项目](https://en.wikipedia.org/wiki/Project)，可以用作[负载测试](https://en.wikipedia.org/wiki/Load_testing)工具，用于分析和衡量各种服务的性能，重点是[Web应用程序](https://en.wikipedia.org/wiki/Web_application)。

Apache JMeter是Apache组织开发的基于Java的压力测试工具。用于对软件做压力测试，它最初被设计用于Web应用测试，但后来扩展到其他测试领域。 它可以用于测试静态和动态资源，例如静态文件、Java 小服务程序、CGI 脚本、Java 对象、数据库、FTP 服务器， 等等。JMeter 可以用于对服务器、网络或对象模拟巨大的负载，来自不同压力类别下测试它们的强度和分析整体性能。另外，JMeter能够对应用程序做功能/回归测试，通过创建带有断言的脚本来验证你的程序返回了你期望的结果。为了最大限度的灵活性，JMeter允许使用正则表达式创建断言。
Apache Jmeter 可以用于对静态的和动态的资源（文件，Servlet，Perl脚本，java 对象，数据库和查询，FTP服务器等等）的性能进行测试。它可以用于对服务器、网络或对象模拟繁重的负载来测试它们的强度或分析不同压力类型下的整体性能。你可以使用它做性能的图形分析或在大并发负载测试你的服务器/脚本/对象。

官网：http://jmeter.apache.org/download_jmeter.cgi

>   如何使用它进行压测

-   点击jemter.bat运行 windows脚本，弹出 Jmeter 框框
-   切换中文可从 Options 中 选择
-   创建线程组

![image-20200916191458672](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/16/191459-449151.png)

-   设置线程数和循环次数

![image-20200916191812605](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/16/191814-24562.png)

-   创建http请求并且配置我们需要进行测试的程序协议、地址和端口

![image-20200916193331191](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/16/193332-830367.png)

-   最后点击运行按钮即可创建线程发送请求

更详细的 `Jmeter` 用法请参照 ：https://www.cnblogs.com/stulzq/p/8971531.html

那么压测最后的结论是什么呢？

最后的结论是：当**8001/sleep/{id}** 被堆积的线程访问时，会拖慢 **8001/{id}** 的速度，如下图，8001/{id} 也开始转圈

![image-20200916194044368](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/16/194045-760117.png)

---

在服务提供者自身都需要耗时来解决的此时，我们再加入80 消费端会发生什么？

-   创建 引入了 `OpenFeign` 的 order 消费工程，引入Hystrix（==Hystrix 一般都用在 consumer 端，我们此次引入到provider端只是为了测试使用==）
-   yaml文件

```yaml
server:
 port: 80

eureka:
  client:
    register-with-eureka: true    
    fetch-registry: true  
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/

spring:
  application:
    name: cloud-consumer-hystrix
```

-   主启动类 `@EnableFeignClients`

-   Service

```java
/**
 * @author kate
 * @Date 2020/9/16 20:08
 */
@Service
@FeignClient("CLOUD-PROVIDER-HYSTRIX")
public interface OrderInvokePayFeignService {
    @GetMapping("/hystrix/{id}")
    public String normalGetPort(@PathVariable("id") Integer id);
    @GetMapping("/hystrix/sleep/{id}")
    public String sleepGetPort(@PathVariable("id") Integer id);
}
```

-   Controller

```java
/**
 * @author kate
 * @Date 2020/9/16 20:15
 */
@RestController
@RequestMapping("/consumer")
public class OrderController {

    @Autowired
    private OrderInvokePayFeignService orderInvokePayFeignService;

    @GetMapping("/{id}")
    public String normalGetPort (@PathVariable("id") Integer id){
        return orderInvokePayFeignService.normalGetPort(id);
    }

    @GetMapping("/sleep/{id}")
    public String sleepGetPort(@PathVariable("id") Integer id){
        return orderInvokePayFeignService.sleepGetPort(id);
    }
}
```

-   运行工程，访问：http://localhost/consumer/1  And  http://localhost/consumer/sleep/1 访问第二个会超时，需要设置超时时间。

启动压测线程，再来访问 80 看看？

80 consumer 端也在转圈圈，也拖慢了它的响应速度

原因如下：==8001同一层次的其他接口服务被困死，因为tomcat线程里面的工作线程已经被挤占完毕==

我们接下来使用Hystrix来进行改造工程

##### Hystrix改造

>   改造方案

-   对方服务（8001）超时了，调用者（80）不能一直卡死等待，必须有服务降级
-   对方服务（8001）down机了，调用者（80）不能一直卡死等待，必须有服务降级
-   对方服务（8001）OK，调用者（80）自己出故障或有自我要求（自己的等待时间小于服务提供者），自己处理降级

###### 服务降级

>   Provider 服务降级

配置 provider 服务降级，提供兜底方案

-   添加 `@HystrixCommand`  到有可能出现错误或者超时的方法上  （业务层方法）

```java
@Override
    @HystrixCommand(
            // 兜底方法
            fallbackMethod = "SleepGetPort_TimeOutHandler",
            // 配置属性
            commandProperties = {
                    // 表示 设定 3 秒 为正常业务实现，超出为超时
                    @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds",value = "3000")
            }
    )
    public String SleepGetPort(Integer id) {
        // 把这里故意调大些，看看会不会走fallback
        Integer ms = 5;
        try {
            TimeUnit.SECONDS.sleep(3);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return Thread.currentThread().getName() + " 端口为：" +id + ",耗时为(m)：" + ms;
    }
```

-   兜底方法   一旦上方服务调用超出预期结果，就会调用fallback指定的兜底方法

```java
/**
     * 兜底方法
     * @param id
     * @return
     */
public String SleepGetPort_TimeOutHandler(Integer id){
    return "线程池："+Thread.currentThread().getName()+"   系统繁忙, 请稍候再试  ,id：  "+id+"\t"+"哭了";
}
```

-   启动类激活注解 `@EnableCircuitBreaker`

结果为：由Hystrix提供线程服务，顺利的返回我们的兜底方法，即使上方出现的不是超时，而是异常，结果依旧可行

![image-20200916205951877](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/16/205952-934215.png)

---

>   Consumer 服务降级

80 端也做自己的服务降级，依照上述即可，这里不再过多叙述

-   yaml 文件启用 Hystrix

```yaml
feign:
  hystrix:
    enabled: true #如果处理自身的容错就开启。开启方式与生产端（provider）不一样。
```

-   启动类的注解也不一样，`@EnableHystrix`

-   业务类方法一样

**问题引出**

通过上述配置我们发现，如果在每一个方法前面都加上 `@HystrixCommand` 注解来配合兜底的服务降级方法，这无疑大大的加重了代码量，使代码看起来冗余，**此时我们可以通过设置默认兜底的方法来解决**

>   设置默认兜底方法

-   在OrderController上方添加  `@DefaultProperties(defaultFallback = "")` 由它来指定默认回调的方法

```java
@DefaultProperties(defaultFallback = "unitiveMethod")
public class OrderController {}
```

-   然后在其对应的可能出现问题的方法上换上 `@HystrixCommand`  

```java
@HystrixCommand
@GetMapping("/sleep/{id}")
public String sleepGetPort(@PathVariable("id") Integer id){
    return orderInvokePayFeignService.sleepGetPort(id);
}
```

-   最后在Controller中添加指定的回调的方法和逻辑

```java
/**
* 默认配置统一的兜底方法
*/
public String unitiveMethod(){
    return "线程池："+Thread.currentThread().getName()+"   系统繁忙, 请稍候再试  ";
}
```

==注：默认的回调兜底方法和每个方法自定义的配置并不冲突，细粒度的配置优先级更高==

>   统一处理回调

是不是还是发现代码有点混乱，即使经过了上述的改造？别着急，接下来我们就来说一下如何优雅的处理回调，使之与Controller分离

我们知道上述的服务调用是通过 `Feign` 来实现的，那么既然是通过 Feign来实现的，我们就可以在 Feign接口上做点文章

**实现如下：**

-   编写另一个类来选择实现 Feign 服务调用接口，在其实现类中来编写服务降级逻辑

```java
/**
 * @author kate
 * @Date 2020/9/22 20:26
 */
@Component
public class OrderInvokeFeignFallBackServiceImpl implements OrderInvokePayFeignService {
    @Override
    public String normalGetPort(Integer id) {
        return "嘿嘿，来我我这里了，服务有可能宕机，建议不要试了！" + id;
    }

    @Override
    public String sleepGetPort(Integer id) {
        return "嘿嘿，来我我这里了，服务有可能宕机，建议不要试了！";
    }
}
```

-   还需要在 Feign 接口的 `@FeignClient` 加上如下属性 指明要调用的服务降级处理的类Class

```java
@FeignClient(value = "CLOUD-PROVIDER-HYSTRIX",fallback = OrderInvokeFeignFallBackServiceImpl.class)
```

-   接下来的Controller显得清爽了许多

```java
@GetMapping("/{id}")
public String normalGetPort (@PathVariable("id") Integer id){
    return orderInvokePayFeignService.normalGetPort(id);
}
```

如此我们便发现，Controller甚至不需要加多余的任何一个注解，我们便优雅的解决了代买混乱的问题

###### 服务熔断

>   什么是服务熔断

当下游的服务因为某种原因突然**变得不可用**或**响应过慢**，上游服务为了保证自己整体服务的可用性，不再继续调用目标服务，直接返回，快速释放资源。如果目标服务情况好转则恢复调用。

**它会有一个慢慢恢复的状态，当它觉得情况有所好转时，会慢慢的恢复服务的调用**

熔断其实是一个框架级的处理，那么这套熔断机制的设计，基本上业内用的是`断路器模式`，如`Martin Fowler`提供的状态转换图如下所示

![image-20200924191416463](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/24/191418-722173.png)



---

-   最开始处于`closed`状态，一旦检测到错误到达一定阈值，便转为`open`状态；
-   这时候会有个 reset timeout，到了这个时间了，会转移到`half open`（半开）状态；
-   尝试放行一部分请求到后端，一旦检测成功便回归到`closed`状态，即恢复服务；

>   服务熔断与服务降级的区别

详情请参考：https://www.cnblogs.com/rjzheng/p/10340176.html

Hystrix 默认的 熔断配置为

```java
// 滑动窗口的大小，默认为20
circuitBreaker.requestVolumeThreshold 
// 过多长时间，熔断器再次检测是否开启，默认为5000，即5s钟
circuitBreaker.sleepWindowInMilliseconds 
// 错误率，默认50%
circuitBreaker.errorThresholdPercentage
// 相关配置可以从 HystrixCommandProperties 中看到
```

>   服务熔断案例

我们会通过一个例子向你表明服务熔断的 `慢慢恢复调用链路`的 概念

为了演示方便我们将在 `provider-Hystrix` 工程中进行

-   在`Service` 和 `ServiceImple` 中加入如下代码

```java
//服务熔断
@HystrixCommand(fallbackMethod = "paymentCircuitBreaker_fallback",commandProperties = {
    @HystrixProperty(name = "circuitBreaker.enabled",value = "true"),  //是否开启断路器
    @HystrixProperty(name = "circuitBreaker.requestVolumeThreshold",value = "20"),   //请求次数
    @HystrixProperty(name = "circuitBreaker.sleepWindowInMilliseconds",value = "5000"),  //时间范围
    @HystrixProperty(name = "circuitBreaker.errorThresholdPercentage",value = "50"), //失败率达到多少后跳闸
})
@Override
public String paymentCircuitBreaker(@PathVariable("id") Integer id){
    if (id < 0){
        throw new RuntimeException("*****id 不能负数");
    }
    String serialNumber = UUID.randomUUID().toString();

    return Thread.currentThread().getName()+"\t"+"调用成功,流水号："+serialNumber;
}
public String paymentCircuitBreaker_fallback(@PathVariable("id") Integer id){
    return "id 不能负数，请稍候再试,(┬＿┬)/~~     id: " +id;
}
```

-   Controller 代码如下

```java
// 服务熔断
@GetMapping("/circuit/{id}")
public String paymentCircuitBreaker(@PathVariable("id") Integer id){
    String result = hystrixService.paymentCircuitBreaker(id);
    log.info("*******result:"+result);
    return result;
}
```

我们可以通过 controller 提供的接口地址手动快速的刷新请求服务，==当错误请求数和比例达到指定阈值时，断路器开启，在这期间，即使访问的是正确的路径和请求，服务也不会马上恢复，响应的依旧是错误的状态，慢慢的，超过 reset timeout 的时间后，断路器尝试向服务 发送请求，当请求正确比例提高，错误比例下降时，服务也会慢慢尝试恢复，直至完全恢复，然后重新关闭断路器==

它的流程就是：

`closed` --> `open` --> `half open ` --> `closed`

==reset timeout 的时间是控制多长时间内服务慢慢恢复过来，如果时间不够，即使请求100%正确，服务也不会被恢复==

>   服务限流

以下Alibaba Sentinel 详说

>   Hystrix工作流程

官网说明：https://github.com/Netflix/Hystrix/wiki/How-it-Works

流程图如下：

![hystrix-command-flow-chart](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/24/202438-964869.png)

---

##### Hystrix图形化Dashboard

>   什么是Hystrix Dashboard

![image-20200924203024585](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202009/24/203025-542942.png)

`Dashboard` 环境搭建

-   创建 `springcloud-hystrix-dashboard` 工程
-   引入相关pom

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix-dashboard</artifactId>
</dependency>
```

-   yaml文件

```yaml
server:
 port: 9001
```

-     主启动类上加入 `@EnableHystrixDashboard`

-     保证所有的服务提供方工程都引入了如下依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

-   启动该工程并测试访问

```http
http://localhost:9001/hystrix
```

当看到`豪猪哥`的头像时表示启动成功，9001监控工程配置完毕。

工程配置到此结束，要测试观测效果，需要启动 provider提供方工程，注册入Eureka中，后访问提供方提供的服务，然后到dashboard 页面观测效果，这里有一个小坑：

**注：当使用的是新版本的Hystrix Dashboard时，我们需要在相应的服务提供方的主启动类上加入如下代码，表示指向指定的监控路径，否则Dashboard页面会报 Unable to connect to Command Metric Stream**

```java
@Bean
public ServletRegistrationBean getServlet(){
    HystrixMetricsStreamServlet streamServlet = new HystrixMetricsStreamServlet();
    ServletRegistrationBean registrationBean = new ServletRegistrationBean(streamServlet);
    registrationBean.setLoadOnStartup(1);
    registrationBean.addUrlMappings("/hystrix.stream");
    registrationBean.setName("HystrixMetricsStreamServlet");
    return registrationBean;
}
```

-   还需在Dashboard工程 yaml 中加入

```yaml
hystrix:
  dashboard:
    proxy-stream-allow-list: "*"
```

-   随后在9001 Dashboard工程中输入要监控的工程的地址，点击监控流即可到达监控页面

![image-20201011152243508](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202010/11/152244-606997.png)

---

-   点击访问提供方的方法，就可以看到左边流的监控线的跳动。

>   Dashboard的监控方法

7色：右上角的7中颜色对应了不同的7种请求状态，也对应了左边7中颜色的数字，左边的数字是请求次数

1圈：左边的实心圆有两种状态，一种灰色，一种红色，灰色代表请求成功，红色代表失败，请求次数越多，实心圆越大

1线：左边的绿色实线代表的是请求次数的频率线

复杂的监控图又如：

![image-20201011152834126](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202010/11/152835-140884.png)

---

![image-20201011152911709](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202010/11/152912-803482.png)

---

### Gateway

微服务架构中网关所处的位置

![Image](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202010/11/161057-604224.bmp)

---

在了解Gateway之前，我们先来说一下什么是Zuul

#### 简介

Zuul包含了对请求的路由和过滤两个最主要的功能:

其中路由功能负责将外部请求转发到具体的微服务实例上,是实现外部访问统一入口的基础而过滤器功能则负责对请求的处理过程进行干预,是实现请求校验、服务聚合等功能的基础.

Zuul和Eureka进行整合,将Zuul自身注册为Eureka服务治理下的应用,同时从Eureka中获得其他微服务的消息,也即以后的访问微服务都是通过Zuul跳转后获得.

注意:Zuul服务最终还是会注册进Eureka

Zuul更多的介绍这里不再过多叙述，详情可见官网：https://github.com/Netflix/zuul/wiki

#### 为什么还要出Gateway

既然Zuul跟Gateway都是来做网关的，那有了Zuul，为什么还要学习Gateway呢？

原因可总结为以下几点：

-   Zuul1.0已经进入了停更阶段，要出Zuul2.0来代替1.0

-   Netflix不太靠谱，Zuul2.0一直跳票,迟迟不发布（核心人员离职跳槽，技术选型迟迟未落定）

#### Gateway简介

##### 什么是Gateway

Spring Cloud Gateway是Spring Cloud官方推出的第二代网关框架，取代Zuul网关。网关作为流量的，在微服务系统中有着非常作用，网关常见的功能有路由转发、权限校验、限流控制等作用。

总结：**Spring Cloud Gateway 使用的Webflux中的reactor-Netty响应式编程组件，底层使用了Netty通讯框架**

更多详情来自官网说明：https://cloud.spring.io/spring-cloud-static/spring-cloud-gateway/2.2.1.RELEASE/reference/html/

##### 特点

SpringCloud Gateway拥有更多新特性：

-   Java 8

-   Spring Framework 5

-   Spring Boot 2

-   动态路由

-   内置到Spring Handler映射中的路由匹配

-   基于HTTP请求的路由匹配 (Path, Method, Header, Host, etc…)

-   过滤器作用于匹配的路由

-   过滤器可以修改下游HTTP请求和HTTP响应 (Add/Remove Headers, Add/Remove Parameters, Rewrite Path, Set Path, Hystrix, etc…)

-   通过API或配置驱动

-   支持Spring Cloud DiscoveryClient配置路由，与服务发现与注册配合使用

**抄了很多Zuul2.0的新概念作业，比Zuul2.0要提早一步问世。**

##### 执行流程

![img](https://img-blog.csdnimg.cn/2019050117140677.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VzZV9hZG1pbg==,size_16,color_FFFFFF,t_70)

##### Gateway位置定义

![image-20201018083534625](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202010/18/083535-302969.png)

##### 核心概念

​	网关提供API全托管服务，丰富的API管理功能，辅助企业管理大规模的API，以降低管理成本和安全风险，包括协议适配、协议转发、安全策略、防刷、流量、监控日志等贡呢。一般来说网关对外暴露的URL或者接口信息，我们统称为路由信息。如果研发过网关中间件或者使用过Zuul的人，会知道网关的核心是Filter以及Filter Chain（Filter责任链）。SpringCloud Gateway也具有路由和Filter的概念。下面介绍一下SpringCloud Gateway中几个重要的概念。

-   路由。路由是网关最基础的部分，路由信息有一个ID、一个目的URL、一组断言和一组Filter组成。如果断言路由为真，则说明请求的URL和配置匹配
-   断言。Java8中的断言函数。Spring Cloud Gateway中的断言函数输入类型是Spring5.0框架中的ServerWebExchange。Spring Cloud Gateway中的断言函数允许开发者去定义匹配来自于http request中的任何信息，比如请求头和参数等。
-   过滤器。一个标准的Spring webFilter。Spring cloud gateway中的filter分为两种类型的Filter，分别是Gateway Filter和Global Filter。过滤器Filter将会对请求和响应进行修改处理

##### Gateway与Zuul1.0的区别

​	Zuul基于servlet 2.5（使用3.x），使用阻塞API。 它不支持任何长连接，如websockets。而Gateway建立在Spring Framework 5，Project Reactor和Spring Boot 2之上，使用非阻塞API。 比较完美地支持异步非阻塞编程，先前的Spring系大多是同步阻塞的编程模式，使用thread-per-request处理模型。即使在Spring MVC Controller方法上加@Async注解或返回DeferredResult、Callable类型的结果，其实仍只是把方法的同步调用封装成执行任务放到线程池的任务队列中，还是thread-per-request模型。Gateway 中Websockets得到支持，并且由于它与Spring紧密集成，所以将会是一个更好的开发体验。

更多详情见：https://juejin.im/post/6844903683516268557#heading-1

##### 技术如何选型

新的微服务架构建议使用Gateway来作为服务网关

#### 起步

##### 环境搭建

-   创建名为 `spring-cloud-Gateway` 工程，配置相关网关服务

-   导入相关pom文件

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>
```

-   yaml

```yaml
server:
  port: 9527
  
spring:
  application:
    name: spring-cloud-Gateway
  cloud:
    gateway:
      routes:
        - id: hystrix_routh #路由的ID，没有固定规则但要求唯一，建议配合服务名
          uri: http://localhost:8001   #匹配后提供服务的路由地址
          predicates:
            - Path=/hystrix/**   #断言,路径相匹配的进行路由
            
		# 这里可以匹配多个，只要复制上面的一段就可以 例如：
        - id: payment_routh2
          uri: http://localhost:8001
          predicates:
            - Path=/hystrix/**   #断言,路径相匹配的进行路由

eureka:
  instance:
    hostname: cloud-gateway-service
  client:
    service-url:
      register-with-eureka: true
      fetch-registry: true
      defaultZone: http://eureka7001.com:7001/eureka
```

-   主启动类 加入 `@SpringbootApplication`  &   `@EnableEurekaClient`
-   启动 Eureka-7001,Hystrix-8001(provider),Gateway-9527
-   观看测试结果

原URL

```http
http://localhost:8001/hystrix/circuit/1
```

现URL

```http
http://localhost:9527/hystrix/circuit/1
```

**执行结果一致**

由于代理路由，原本访问 9527 的路径，由Gateway工程通过路由的方式转发到指定URI去，根据断言，匹配相符合的路径

**Gateway代码配置**

```java
@Bean
public RouteLocator routeLocator2(RouteLocatorBuilder routeLocatorBuilder){
    RouteLocatorBuilder.Builder routes = routeLocatorBuilder.routes();
    routes.route(
        "hystrix/circuit/1",
        r -> r.path("/hystrix/circuit/1").uri("http://localhost:8001/hystrix/circuit/1")
    ).build();
    return routes.build();
}
```

![image-20201018103522605](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202010/18/103546-608844.png)

---

##### 动态路由

默认情况下Gateway会根据注册中心的服务列表，以**注册中心上微服务名**为路径创建动态路由进行转发，从而实现动态路由的功能

yaml文件变动为：

```yaml
server:
  port: 9527
  
spring:
  application:
    name: spring-cloud-Gateway
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true  #开启从注册中心动态创建路由的功能，利用微服务名进行路由
      routes:
        - id: pay_routh #路由的ID，没有固定规则但要求唯一，建议配合服务名
        # uri: http://localhost:8001   #匹配后提供服务的路由地址
          uri: lb://cloud-provider-pay # lb:// 服务名
          predicates:
            - Path=/pay/port   #断言,路径相匹配的进行路由            
eureka:
  instance:
    hostname: cloud-gateway-service
  client:
    service-url:
      register-with-eureka: true
      fetch-registry: true
      defaultZone: http://eureka7001.com:7001/eureka
```

##### Predicate

断言有多种匹配规则，可根据业务场景的不同选择不同的断言方式，它们通过 **and** 的方式被连接，只有全部满足才能为 true

以下将列出所有的断言规则：

*1.After Route Predicate：在指定时间段之后匹配*

```yaml
- After=2020-11-07T21:30:52.574+08:00[Asia/Shanghai]
```

此时间串由java8 *ZonedDateTime* 类产生

```java
ZonedDateTime zonedDateTime = ZonedDateTime.now();
System.out.println(zonedDateTime);
```

*2.Before Route Predicate：在指定时间段之前匹配*

```yaml
- Before=2020-11-07T21:30:52.574+08:00[Asia/Shanghai]
```

*3.Between Route Predicate：在指定时间段之间匹配*

```yaml
 - Between
 	=2020-11-07T21:30:52.574+08:00[Asia/Shanghai],2020-11-07T21:30:52.574+08:00[Asia/Shanghai]
```

*4. Cookie Route Predicate：客户端请求携带指定Cookie时匹配*

```yaml
- Cookie=kate,tcl
```

通过 ==curl命令行== 方式来发送 ==Get== 请求，例子如下：

正常方式访问：

```shell
curl http://localhost:9527/pay/port
```

携带Cookie时：

```shell
curl http://localhost:9527/pay/port --cookie "kate=tcl"
```

*5. Header Route Predicate：客户端请求携带指定请求头时匹配*

```yaml
- Header=X-Request-Id, \d+   #请求头中要有X-Request-Id属性并且值为整数的正则表达式
```

curl 请求路径：(-d：表示正整数)

```shell
curl http://localhost:9527/pay/port -H "X-Request-Id:1234"
```

*6.Host Route Predicate：指定请求Host时匹配*

```yaml
- Host=**.ip
```

```shell
curl http://localhost:9527/pay/port -H "Host: www.ip"
```

*7.Method Route Predicate：指定请求方式时匹配*

```yaml
- Method=Get
```

```shell
curl http://localhost:9527/pay/port
```

*8.Path Route Predicate：指定请求路径时匹配*

```yaml
- Path=/pay/port
```

```shell
curl http://localhost:9527/pay/port
```

*9. Query Route Predicate：指定参数时匹配*

```yaml
- Query=username, \d+
```

```shell
curl http://localhost:9527/pay/port?username=1234
```

curl发送post请求请参考：https://www.cnblogs.com/fan-gx/p/12321351.html

##### Filter

我们可以通过配置过滤器来拦截在Pridicate之后发送过来的请求，请求的路线为

*Request --> Pridicate --> Filter --> Controller*

###### Filter的生命周期（Only Two）

pre：在业务逻辑之前

post：在业务逻辑之后

###### Filter的种类（Only Two）

GatewayFilter：单一

springcloud官网提供了**31种**可通过配置来提供过滤的规则

详情可参见：https://cloud.spring.io/spring-cloud-static/spring-cloud-gateway/2.2.3.RELEASE/reference/html/#gatewayfilter-factories

例子如下：

```yaml
spring:
  cloud:
    gateway:
      routes:
      - id: add_request_header_route
        uri: https://example.org
        filters:
        - AddRequestHeader=X-Request-red, blue
```

GlobalFilter：全局

官网还提供了**10种**基于全局配置的过滤规则

详情可参见：https://cloud.spring.io/spring-cloud-static/spring-cloud-gateway/2.2.3.RELEASE/reference/html/#global-filters

例子如下：

```java
@Bean
public GlobalFilter customFilter() {
    return new CustomGlobalFilter();
}

public class CustomGlobalFilter implements GlobalFilter, Ordered {
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        log.info("custom global filter");
        return chain.filter(exchange);
    }

    @Override
    public int getOrder() {
        return -1;
    }
}
```

###### 自定义全局Filter

大多时候，官网的配置规则于我们的业务而言并不是太过灵活，因此我们需要通过自定义过滤器规则来实现过滤

自定义过滤器步骤如下:

1、编写自定义过滤规则组件

2、写入自定义规则逻辑

假设现在我们要拦截指定参数为 **username=kate** 的请求，在过滤规则中我们希望判断它的值是否是**kate**，如果是则放行，否则拦截后不予放行，且阻断程序运行并返回

可实现如下：

```java
package cn.kate.config;

import lombok.extern.slf4j.Slf4j;
import org.springframework.cloud.gateway.filter.GatewayFilterChain;
import org.springframework.cloud.gateway.filter.GlobalFilter;
import org.springframework.core.Ordered;
import org.springframework.core.annotation.Order;
import org.springframework.http.HttpStatus;
import org.springframework.stereotype.Component;
import org.springframework.util.StringUtils;
import org.springframework.web.server.ServerWebExchange;
import reactor.core.publisher.Mono;

import java.lang.annotation.Annotation;
import java.util.Objects;

/**
 * @Author: kate
 * @Date: 2020/11/8 11:13
 */
@Component
@Slf4j
public class MyCustomFilterComponent implements GlobalFilter, Ordered {

    public static final String USER_NAME = "kate";
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        log.info("--------come in here--------");
        String username = exchange.getRequest().getQueryParams().getFirst("username");
        if(!StringUtils.isEmpty(username)){
            if(Objects.equals(username,USER_NAME)){
                log.info("匹配成功！给予放行");
                return chain.filter(exchange);
            }
        }
        // 未完成时的响应
        exchange.getResponse().setStatusCode(HttpStatus.BAD_REQUEST);
        return exchange.getResponse().setComplete();
    }

    @Override
    public int getOrder() {
        return 0;
    }
}
```

可通过一下Http请求访问测试：

```http
http://localhost:9527/pay/port?username=kate
```

错误访问测试：

```http
http://localhost:9527/pay/port?username=
```

结果

![image-20201108112745857](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/08/112748-629474.png)

---

### SpringCloud Config

#### 简介

**我们现在的分布式系统中所面临的问题**：

一个分布式系统中存在大大小小多个工程模块，每一个模块都依赖于一个`application.yaml` 或者 `application.properties`配置文件，这些配置文件中都有公共的相同的配置，我们为什么不能把它抽取出来成为一个公共的配置文件呢？既然可以抽取成为公共的配置文件，那么还涉及到了一个管理公共配置文件的容器。

##### 什么是SpringCloud Config

SpringCloud Config为微服务架构中所有的微服务提供*集中化*的外部配置支持，配置服务器为各个不同微服务应用的所有环境提供了一个*中心化的外部配置*。

##### SpringCloud Config的作用

- 集中管理配置文件
- 不同环境不同配置，动态化的配置更新，分环境部署比如dev/test/prod/beta/release
- 运行期间动态调整配置，不再需要在每个服务部署的机器上编写配置文件，服务会向配置中心统一拉取配置自己的信息
- 当配置发生变动时，服务不需要重启即可感知到配置的变化并应用新的配置
- 将配置信息以REST接口的形式暴露

##### 与GitHub整合配置

由于SpringCloud Config默认使用Git来存储配置文件（也有其它方式，比如支持svn和本地文件，但最推荐的还是Git，而且使用的是http/https访问的形式）

##### SpringCloud Config架构

SpringCloud Config分为*服务端和客户端两部分*。

服务端也称为*分布式配置中心*，*它是一个独立的微服务应用*，用来连接配置服务器并为客户端提供获取配置信息，加密/解密信息等访问接口

客户端则是通过指定的配置中心来管理应用资源，以及与业务相关的配置内容，并在启动的时候从配置中心获取和加载配置信息配置服务器默认采用git来存储配置信息，这样就有助于对环境配置进行版本管理，并且可以通过git客户端工具来方便的管理和访问配置内容

---

![image-20201108135833443](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/08/135834-74653.png)

---

更多详情可见官网：https://spring.io/projects/spring-cloud-config

#### 起步

##### 环境搭建

###### 服务端

- 在GitHub上新建一个名为sprincloud-config的新Repository
- 在本地将一个文件夹与GitHub上的仓库关联（可选择）
- 新建Module模块cloud-config-center-3366它既为Cloud的配置中心模块cloudConfig Center
- 新增如下pom

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-server</artifactId>
</dependency>
```

- 新建yaml文件

```yaml
server:
  port: 3366
spring:
  application:
    name: cloud-config-center
  cloud:
    config:
      server:
        git:
          uri: https://github.com/Angle-island/springcloud-config.git
          search-paths:
            - springcloud-config
      label: main # master已非默认分支
eureka:
  client:
    service-url:
      defaultZone:  http://localhost:7001/eureka
```

- 主启动类加入 `@EnableConfigServer`

```java
@SpringBootApplication
@EnableConfigServer
public class MainApplicationWithConfigCenter3366 {
    public static void main(String[] args) {
        SpringApplication.run(MainApplicationWithConfigCenter3366.class);
    }
}
```

- windows下修改hosts文件，增加映射(可选)

```txt
127.0.0.1 config-center.com
```

- 启动微服务3366，在此之前应先启动Eureka-7001
- 测试通过Config微服务是否可以从GitHub上获取配置内容

```http
http://config-center.com:3366/main/springcloud-dev.yaml
```

如果没有增加hosts映射，访问可用

```http
http://localhost:3366/main/springcloud-dev.yaml
```

**SpringCloud官网提供了如下几种获取配置文件的方式**

```txt
/{application}/{profile}[/{label}]
/{application}-{profile}.yml
/{label}/{application}-{profile}.yml
/{application}-{profile}.properties
/{label}/{application}-{profile}.properties
```

<u>巨坑：GitHub 于2020.10.1起，将默认新创建的仓库的默认分支改为main，而非再是master，所以这里的label，应该写成main最合适，Git提交时也应多需注意</u>

###### 客户端

测试过服务端总配置中心的搭建后，我们需要搭建一个模拟的客户端，*模拟多个微服务模块从服务端获得公共的配合文件*

-   新建3399工程
-   新增如下pom

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```

**注意**：此次不再是application.yaml文件的修改变动，而是新增了`bootstrap.yaml`文件，接下来我们就来说一下为什么要变动

*什么是bootstrap.yaml*

既然我们考虑到共享公共配置文件，那我们就需要意识到一个问题，新的配置文件的名字我们不可能也叫 application.yaml,而**Spring官方给我们提供了一个统一的系统级别的配置文件名就是bootstrap.yaml**

也就是说：

*application.yaml是用户级的资源配置项*

*bootstrap.yaml是系统级的，优先级更加高*

Spring Cloud会创建一个“**Bootstrap Context**”，作为Spring应用的'Application Context的父上下文。初始化的时候，'Bootstrap Context‘负责从外部源加载配置属性并解析配置。这两个上下文共享一个从外部获取的'Environment'。

Bootstrap`属性有高优先级，默认情况下，它们不会被本地配置覆盖。'Bootstrap context和Application Context有着不同的约定，所以新增了一个'bootstrap.yml'文件，保证`Bootstrap Context'和`Application Context`配置的分离。

要将Client模块下的application.yaml文件改为bootstrap.yaml,这是很关键的，因为bootstrap.yaml是比application.yaml先加载的。bootstrap.yaml优先级高于application.yaml

-   bootstrap.yaml

```yaml
server:
  port: 3399

spring:
  application:
    name: config-client
  cloud:
    config:
      label: main # 分支名称
      name: springcloud # 配置文件名称
      profile: dev # 读取后缀名称
      uri: http://localhost:3366 # 配置中心地址（本地服务端）
      # 以上3个综合之后为: main分支springcloud-dev.yaml的配置文件被读取http://localhost:3366/main/conf
eureka:
  client:
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka
```

-   编写主启动类

```java
/**
 * @author kate
 * @Date 2020/11/11 18:57
 */
@SpringBootApplication
public class MainApplicationWithConfigConsumer3399 {
    public static void main(String[] args) {
        SpringApplication.run(MainApplicationWithConfigConsumer3399.class,args);
    }
}
```

-   Controller代码可写为

```java
/**
 * @author kate
 * @Date 2020/11/11 18:58
 */
@RestController
public class ConfigConsumerController {
    /**
     * 读取我们来自本地服务端上的配置
     * key:config
     * value:info
     */
    @Value("${config.info}")
    private String configInfo;

    @GetMapping("/configInfo")
    public String getConfigInfo(){
        return configInfo;
    }
}
```

-   开始测试

我们先启动Eureka7001、3366，完成自测

测试成功后，我们启动3399，来完成从服务端获取配置信息的操作

```http
http://localhost:3399/configInfo
```

当成功完成响应后，我们思考一个问题，如果我们此时想要获得 *springcloud-test.yaml*，那么我们就可以手动的修改bootstrap.yaml的 读取的后缀名称即可

当做完这一切后，我们再来出一个难题：

手动修改远程GitHub上的配置文件信息，我们看3366 和 3399 的响应情况，我们将远程配置文件(springcloud-dev.yaml)修改为如下:

```yaml
config: 
  info: master branch,springcloud-dev.yaml,dev environment,version=2
```

此时我们再分别访问服务端和客户端配置中心

```http
http://localhost:3366/main/springcloud-dev.yaml
http://localhost:3399/configInfo
```

结果不出所料，**3366因为直连GitHub,所以GitHub的修改会直接作用在服务端的配置中，而3399，因为读取的是本地的3366，所以并未发生任何变化**

此时我们考虑解决，将3399重启后才可重新读取到最新的远程配置，这是为什么？

我们可以观察3366工程，<u>每次请求远程GitHub后或者每次当GitHub配置被修改后</u>，控制台都会出如下日志

---

![image-20201111191450310](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/11/191452-563717.png)

---

*我们顺着日志摸过去，发现本地磁盘有刚刚读取到的配置文件信息，因此猜测，3399，读取到的是上次跟他绑定时的本地文件，而不是最新请求下来的远程配置*

**思考**

难道每次更新完远程配置后都要重启3399？接着往下看

###### 客户端动态刷新

为了不每次重启微服务工程，我们可以考虑使用动态刷新来解决这个问题

我们修改一下3399工程

-   添加如下pom依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

-   bootstrap.yaml添加如下代码

```yaml
# 表示暴露指定的端口
management:
  endpoints:
    web:
      exposure:
        include: "*"
```

-   Controller添加 `@RefreshScope` 注解

-   之后我们重新修改GitHub并测试

我们发现在没有重启的情况下，3399依然没有完成远程配置的同步，我们还需运维人员增加如下操作：

```shell
# 发送如下POST命令，刷新指定的配置
curl -X POST "http://localhost:3399/actuator/refresh"
```

之后再重新测试3399，发现已经可以自动获得最新的配置了！

**再思考**

假如有多个微服务客户端3355/3366/3377。。。。每个微服务都要执行一次post请求，手动刷新？可否广播，一次通知，处处生效？我们想大范围的自动刷新，求方法。以我们目前的知识无法做到每个微服务工程的无差别精准控制的能力，所以接下来就引出了 `SpringCloud Bus 消息总线`

### SpringCloud Bus

#### 简介

我们在上一讲遇到了Client端无法应对动态刷新的问题，需要每次使用 curl 命令来发送post请求刷新从Server端拉取到本地的GitHub远程的配置，此节就是用来解决动态刷新的问题

**什么是 SpringCloud Bus**

Spring Cloud Bus是用来将分布式系统的节点与轻量级消息系统链接起来的框架，*它整合了Java的事件处理机制和消息中间件的功能*

*Spring Clud Bus目前支持RabbitMQ和Kafka*

SpringCloud Bus 工作流程

---

![image-20201112201237838](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/12/201238-758380.png)

---

也就是说SpringCloud Bus 整合了消息队列和事件处理，能够使其中一个Client订阅其消息，然后监听事件的改变后传播到其他微服务工程中去，解决了动态刷新的问题

**SpringCloud Bus能够做什么**

Spring Ccloud Bus能管理和传播分布式系统间的消息，就像一个分布式执行器，可用于广播状态更改、事件推送等，也可以当作微服务间的通信通道

---

![image-20201112201555115](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/12/201555-90422.png)

---

**上面这种工作方式是：由消息总线接受订阅监听事件后传播到所有Client端我们又将  接受消息订阅的这个工程叫做** *消息总线*

<u>什么是总线</u>

在微服务架构的系统中，通常会使用*轻量级的消息代理*来构建一个*共用的消息主题*，并让系统中所有微服务实例都连接上来。由于*该主题中产生的消息会被所有实例监听和消费，所以称它为消息总线*。在总线上的各个实例，都可以方便地广播─些需要让其他连接在该主题上的实例都知道的消息。

*基本原理*

ConfigClient实例都监听MQ中同一个topic(默认是springCloudBus)。当一个服务刷新数据的时候，它会把这个信息放入到Topic中，这样其它监听同一Topic的服务就能得到通知，然后去更新自身的配置。

这属于MQ的原理，有空要系统的学习一下**RabbitMQ**   和  **RocketMQ** 以及大数据相关的  **Kafka**

#### 起步

##### RabbitMQ安装

我们选择使用docker 安装RabbitMQ，此处不多做介绍，详情可参照Springboot 高级篇笔记

##### 技术选型

在简介中我们提到，通过该SpringCloud Bus达到动态刷新配置的两种解决方案

1、利用消息总线触发一个客户端/bus/refresh,而刷新所有客户端的配置

---

![image-20201114155910489](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/14/171440-856922.png)

---

2、利用消息总线触发一个服务端ConfigServer的/bus/refresh端点,而刷新所有客户端的配置（更加推荐）

---

![image-20201114160005184](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/14/160007-636484.png)

---

我们可以尝试的分析一下，到底使用哪种设计思想来实现动态刷新的功能：

如果使用消息总线来刷新Client端来刷新配置，那么会有以下缺点

1、会破坏微服务模块工程的平衡性，以及增加微服务模块的负担

为什么这么说？因为客户端工程可能会有集群，他们本来的代码和配置都是一模一样的，突然给其中一个微服务工程增加上了SpringCloud Bus的配置，明显会增加它的负担，平衡也随之被破坏，如果它宕机，那么消息总线就失去了另其动态刷新的依赖目标

2、有一定的局限性

例如，微服务在迁移时，它的网络地址常常会发生变化，此时如果想要做到自动刷新，那就会增加更多的修改

所以，最终：我们选择使用第二种方式：让总线对其对应的Server端的工程影响，从而使在其Server端下所有的Client端都收到传播

为了演示广度传播，我们需要以3399工程为例，新建一个3300工程，也就是一个Server下对应两个Client工程

##### 3300工程搭建

不在细说，详情可参照3399工程，配置一致

##### 各工程改造

###### config-server:3366

- pom文件添加如下依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bus-amqp</artifactId>
</dependency>
```

- yaml文件变更为如下

```yaml
server:
  port: 3366
spring:
  application:
    name: cloud-config-center
  cloud:
    config:
      server:
        git:
          uri: https://github.com/Angle-island/springcloud-config.git
          search-paths:
            - springcloud-config
      label: main # master已非默认分支
  rabbitmq:
    host: ip #Tencent云服务器RabbitMQ地址
    port: 5672
    username: guest
    password: guest
eureka:
  client:
    service-url:
      defaultZone:  http://localhost:7001/eureka
management:
  endpoints:
    web:
      exposure:
        include: 'bus-refresh' #刷新      
```

###### config-consumer:3399

- pom文件如上
- yaml文件增加上

```yaml
rabbitmq:
  host: ip #Tencent云服务器RabbitMQ地址
  port: 5672
  username: guest
  password: guest
```

###### config-consumer:3300

如3399，配置一致

启动Eureka7001、Server3366、Client3399、Client3300后，修改GitHub 仓库配置后测试

刷新3366一个Server端工程即可

```shell
curl -X POST "http://localhost:3366/actuator/bus-refresh"
```

配置已完成了一次刷新，处处生效

此时我们可以打开RabbitMQ,登录上去后，查看exchanges中多了一个 名为 ：`SpringCloud Bus`的topic，这个就是他们订阅的消息，RabbitMQ会根据指定的规则传播给已订阅 它的所有Client工程

##### 定点通知

我们在简介中还提到，如果能够精确的刷新动态的配置，那无疑会更加的灵活易用，为此，SpringCloud Bus的解决方案如下：

要想实现精确的通知，我们需要在 curl 的请求刷新的路径上带上 指定的 `destination` 

例：原请求为：

这是通知所有的Client工程

```shell
curl -X POST "http://localhost:3366/actuator/bus-refresh"
```

 现改为：

单单只通知指定的工程，例如**只通知3399**

```shell
curl -X POST "http://localhost:3366/actuator/bus-refresh/config-client:3399"
```

*destination的参数解析为：*

注册进erueka的spring-application-name + : 端口

改动GitHub后可再测试

##### 工作流程如下

---

![image-20201114171521700](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/14/171524-895222.png)

---

### SpringCloud Stream

#### 简介

我们在上节中提到，需要整合 *消息队列* 到 SpringCloud Bus中，为配置中心的消息总线提供动态刷新的功能，这时候可能会引出一个问题：

一个庞大的系统中，肯能整合进了多个消息队列技术栈，如果我们每一个都要学习，那无疑增添了庞大的学习成本，有没有一种技术能够转换MQ的底层，实现多个MQ技术栈的通用呢？

SpringCloud Stream应运而生

##### 什么是SpringCloud Stream

官网定义SpringCloud Stream是一个构建消息驱动微服务的框架

应用程序通过*inputs*或者 *outputs*来与SpringCloud Stream中*binder*对象交互

通过我们配置来**binding(绑定)**，而SpringCloud Stream 的 binder对象负责与消息中间件交互。所以，我们只需要搞清楚如何与SpringCloud Stream交互就可以方便使用消息驱动的方式

通过使用*Spring Integration*来连接消息代理中间件以实现消息事件驱动

SpringCloud Stream为一些供应商的消息中间件产品提供了个性化的自动化配置实现

更多SpringCloud Stream详情可见官网：https://spring.io/projects/spring-cloud-stream#overview

*目前仅支持RabbitMQ、Kafka*

**简而言之：屏蔽底层消息中间件的差异，降低切换版本，统一消息的编程模型**

##### 设计思想

我们先看看MQ的实现

---

![image-20201115085816567](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/15/085817-666716.png)

---

**Message： 生产者/消费者之间靠消息媒介传递信息内容**

**消息通道MessageChannel：消息必须走特定的通道**

**消息通道里的消息如何被消费呢，谁负责收发处理：消息通道MessageChannel的子接口SubscribableChannel,由MessageHandler消息处理器订阅**

我们不在此处讨论更多，有空必须学习一个MQ！

**为什么要使用 SpringCloud Stream**

比方说我们用到了RabbitMQ和Kafka，由于这两个消息中间件的架构上的不同，像RabbitMQ有exchange，kafka有Topic和Partitions分区

这些中间件的差异性导致我们实际项目开发给我们造成了一定的困扰，*我们如果用了两个消息队列的其中一种，后面的业务需求，我想往另外一种消息队列进行迁移，这时候无疑就是一个灾难性的，一大堆东西都要重新推倒重新做*，因为它跟我们的系统耦合了，这时候springcloud Stream给我们提供了—种解耦合的方式。

**SpringCloud Stream怎么解决差异**

在没有绑定器这个概念的情况下，我们的SpringBoot应用要直接与消息中间件进行信息交互的时候,由于各消息中间件构建的初衷不同，它们的实现细节上会有较大的差异性

通过定义绑定器 `Binder` 作为中间层，完美地实现了*应用程序与消息中间件细节之间的隔离*

通过向应用程序暴露统一的Channel通道，使得应用程序不需要再考虑各种不同的消息中间件实现

**Binder**

绑定器，绑定对象，Stream通过Binder与底层MQ中间件交互，可以做到MQ动态切换无感知

Input对应消费者

Output对应生产者

**Channel**

通道，是队列Queue的一种抽象，在消息通讯系统中就是实现存储和转发的媒介，通过对Channel对队列进行配置

**Source**和**Sink**

简单的可理解为参照对象是Spring Cloud Stream自身，从Stream发布消息就是输出，接受消息就是输入

##### 常用注解和API

| 注解            | expression                                                   |
| --------------- | ------------------------------------------------------------ |
| Middleware      | 中间件，目前只支持RabbitMQ和Kafka                            |
| Binder          | Binder是应用与消息中间件之间的封装，目前实行了Kafka和RabbitMQ的Binder，通过Binder可以很方便的连接中间件，可以动态的改变消息类型(对应于Kafka的topic,RabbitMQ的exchange)，这些都可以通过配置文件来实现 |
| @lnput          | 注解标识输入通道，通过该输入通道接收到的消息进入应用程序     |
| @Output         | 注解标识输出通道，发布的消息将通过该通道离开应用程序         |
| @StreamListener | 监听队列，用于消费者的队列的消息接收                         |
| @EnableBinding  | 指信道channel和exchange绑定在一起                            |

以下是 `Middleware` 和 `Binder` 所处位置

---

![image-20201115093053025](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/15/093054-864396.png)

---

![image-20201117205514545](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/17/205516-132210.png)

---

#### 起步

##### 案例

我们选择一个案例来实验整合SpringCloud Stream

工程中新建三个子模块

-   cloud-stream-rabbitmq-provider8801,作为生产者进行发消息模块
-   cloud-stream-rabbitmq-consumer8802,作为消息接收模块
-   cloud-stream-rabbitmq-consumer8803,作为消息接收模块

##### 消息驱动之生产者

我们先完成生产者的环境搭建，且保证自测通过

-   新建工程 `springcloud-stream-rabbitmq-provider8801`

-   pom文件新增如下依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-stream-rabbit</artifactId>
</dependency>
```

-   yaml文件

```yaml
server:
  port: 8801

spring:
  application:
    name: cloud-stream-provider
  cloud:
    stream:
      binders: # 在此处配置要绑定的rabbitmq的服务信息；
        defaultRabbit: # 表示定义的名称，用于于binding整合
          type: rabbit # 消息组件类型
          environment: # 设置rabbitmq的相关的环境配置
            spring:
              rabbitmq:
                host: ip
                port: 5672
                username: develop
                password: develop
      bindings: # 服务的整合处理
        output: # 这个名字是一个通道的名称
          destination: tclExchange # 表示要使用的Exchange名称定义
          content-type: application/json # 设置消息类型，本次为json，文本则设置“text/plain”
          binder: defaultRabbit # 设置要绑定的消息服务的具体设置（自己命名的通道配置名，表示与指定名称的通道关联）

eureka:
  client: # 客户端进行Eureka注册的配置
    service-url:
      defaultZone: http://localhost:7001/eureka
  instance:
    lease-renewal-interval-in-seconds: 2 # 设置心跳的时间间隔（默认是30秒）
    lease-expiration-duration-in-seconds: 5 # 如果现在超过了5秒的间隔（默认是90秒）
    instance-id: send-8801.com  # 在信息列表时显示主机名称
    prefer-ip-address: true     # 访问的路径变为IP地址
```

-   主启动类

```java
@SpringBootApplication
public class StreamWithRabbit8801 {
    public static void main(String[] args) {
        SpringApplication.run(StreamWithRabbit8801.class,args);
    }
}
```

-   <u>业务类（重点）</u>

发送消息的Service和实现

```java
public interface SendMessageForRabbitMQ {
    /**
     * 发送消息
     * @return string
     */
    String sendMessage();
}
```

Impl

```java
@EnableBinding(Source.class) // 定义消息的推送管道
public class SendMessageForRabbitMQImpl implements SendMessageForRabbitMQ {
    /**
     * 这里不再是调用任何的DAO操作数据库，我们要跟消息中间件交互
     */
    @Autowired
    private MessageChannel output;

    @Override
    public String sendMessage() {
        String serial = UUID.randomUUID().toString();
        output.send(MessageBuilder.withPayload(serial).build());
        return serial;
    }
}
```

Controller

```java
@RestController
public class SendMessageForRabbitMQController {

    @Autowired
    private SendMessageForRabbitMQ sendMessageForRabbitMQ;

    @GetMapping("/sendMessage")
    public String sendMessage(){
        return sendMessageForRabbitMQ.sendMessage();
    }
}
```

我们先保证自测通过

启动Eureka7001、RabbitMQ、8801，并测试访问Controller定义的接口，先做到发送消息到指定云端RabbitMQ信道

<u>注意：RabbitMQ的Guest用户只有在本机访问时才正常，假设你是云服务器上安装的，会有可能抛出连接异常，详情解决可见Springboot高级</u>

自测通过后我们来配置客户端消费者消费消息

##### 消息驱动之消费者

搭建消息驱动的消费者工程来消费来自8801生产出来的消息

-   新建工程 `springcloud-stream-rabbitmq-consumer8802`
-   pom文件同样新增如上
-   yaml文件略微修改为以下

```yaml
 bindings: # 服务的整合处理
   input: # 这个名字是一个通道的名称
   destination: tclExchange # 表示要使用的Exchange名称定义
   content-type: application/json # 设置消息类型，本次为json，文本则设置“text/plain”
   binder: defaultRabbit # 设置要绑定的消息服务的具体设置（自己命名的通道配置名，表示与指定名称的通道关联）
 instance:
   lease-renewal-interval-in-seconds: 2 # 设置心跳的时间间隔（默认是30秒）
   lease-expiration-duration-in-seconds: 5 # 如果现在超过了5秒的间隔（默认是90秒）
   instance-id: recive-8802.com  # 在信息列表时显示主机名称
   prefer-ip-address: true     # 访问的路径变为IP地址
```

-   主启动类
-   业务类

接收消息的Controller

```java
package cn.kate.controller;

import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.cloud.stream.annotation.EnableBinding;
import org.springframework.cloud.stream.annotation.StreamListener;
import org.springframework.cloud.stream.messaging.Sink;
import org.springframework.messaging.Message;
import org.springframework.stereotype.Component;

/**
 * @author kate
 * @Date 2020/11/18 20:09
 */
@Component
@EnableBinding(Sink.class) // 此处定义为接收消息
@Slf4j
public class ReceiveMessageController {
    @Value("${server.port}")
    private Integer port;

    @StreamListener(Sink.INPUT)
    public void receiveMessage(Message<String> message){
         log.info("端口为：" + port +"的程序，接收到的消息为：" + message.getPayload());
    }
}
```

-   测试，8801发送消息，8802看是否能接收

至此，RabbitMQ与Stream的简单整合就完成了

##### 分组消费与持久化

我们依照8802工程，复刻一个新的工程(8803)，测试观察一个生产者多个消费者的模式

**我们发现，当生产者(8801)发送完消息后，分别被不同的消费者(8802、8803)所获得到，这就是重复消费**

在实际的生产环境中，我们大多数都希望消息只被接受一次，例如：常见的订单业务，库存只剩下一条时，不可能同时被不同的顾客下单，肯定是其中一个客户没有获得此订单，因此，我们需要通过配置Stream来解决重复消费的问题

###### 分组

**在RabbitMQ中，当我们默认不选择给交换机（exchanges）内部注册的工程分组时，RabbitMQ会默认为我们创建不同的组流水号**

---

![image-20201118204453113](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/18/204454-138664.png)

---

根据RabbitMQ的规则，认为：

*不同的组就拥有共同接受消息的能力，因此才有了重复消费的概念*

*相同的组彼此之间是竞争关系，消息只会被一个工程消费，默认为轮询消费*

那么我们可不可以把他们放在相同的组中呢？

配置如下：

在8802和8803工程的yaml配置文件中加入如下配置

```yaml
bindings: 
  group: tclkate
```

再次启动程序，即可解决重复消费问题

###### 持久化

在实际应用中，我们的消息很容易丢失，伴随着各种问题的出现：

例如：工程的宕机，工程的异常等

如何解决工程的持久化问题，防止消息发送后没有被消费者消费的持久化问题呢？

*同样的，我们需要配置分组，只要有了分组，哪怕服务一段时间宕机了，再次重启时我们就能接受到原来生产者发送出来的消息*

*分组是一定要配置的*

### Sleuth

问题引出：

当微服务的工程模块越来越多时，调用的链路也越来越复杂，有时我们可能需要采集服务调用的一些信息，比如：调用时间、调用过程，需要把这些细节都暴露出来方便我们追踪问题

#### 简介

SpringCloud Sleuth提供了一套完整的服务跟踪的解决方案，在分布式系统中提供追踪解决方案并且兼容支持了**zipkin**

它大量借用了Google Dapper、 Twitter Zipkin和 Apache HTrace的设计，先来了解一下 Sleuth的术语， Sleuth借用了 Dapper的术语

**什么是zipkin**

Zipkin是一款开源的分布式实时数据追踪系统（Distributed Tracking System），基于 Google Dapper的论文设计而来，由 Twitter 公司开发贡献。其主要功能是聚集来自各个异构系统的实时监控数据

**为什么要使用Zipkin**

因为sleuth对于分布式链路的跟踪仅仅是一些数据的记录， 这些数据我们人为来读取和处理难免会太麻烦了，所以我们一般吧这种数据上交给Zipkin Server 来统一处理

**Sleuth术语**

**span（跨度）**：基本工作单元。 span用一个64位的id唯一标识。除ID外，span还包含其他数据，例如描述、时间戳事件、键值对的注解（标签）， spanID、span父 ID等。 span被启动和停止时，记录了时间信息。初始化 span被称为"rootspan"，该 span的 id和 trace的 ID相等

**trace（跟踪）**：一组共享"rootspan"的 span组成的树状结构称为 traceo trac也用一个64位的 ID唯一标识， trace中的所有 span都共享该 trace的 ID

**annotation（标注）**： annotation用来记录事件的存在，其中，核心annotation用来定义请求的开始和结束

**CS**（ Client sent客户端发送）：客户端发起一个请求，该 annotation描述了span的开 始。

**SR**（ server Received服务器端接收）：服务器端获得请求并准备处理它。如果用 SR减去 CS时间戳，就能得到网络延迟。c) 

**SS**（ server sent服务器端发送）：该 annotation表明完成请求处理（当响应发回客户端时）。如果用 SS减去 SR时间戳，就能得到服务器端处理请求所需的时间

**CR**（ Client Received客户端接收）： span结束的标识。客户端成功接收到服务器端的响应。如果 CR减去 CS时间戳，就能得到从客户端发送请求到服务器响应的所需的时间

**微服务调用实例**

---

![image-20201121091304088](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/21/091305-229341.png)

---

我们可将上图拆解来理解spanID 和 TraceID

![image-20201121091515672](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/21/091516-665910.png)

---

![image-20201121091616130](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/21/091617-47885.png)

---

SpringCloud Sleuth GitHub：https://github.com/spring-cloud/spring-cloud-sleuth

#### 起步

##### 环境搭建

SpringCloud从F版起已不需要自己构建Zipkin server了，只需要调用jar包即可

我们需要将Zipkin的 jar包下载下来放到本地启动

下载地址：https://dl.bintray.com/openzipkin/maven/io/zipkin/java/zipkin-server/

```jar
zipkin-server-2.12.9-exec.jar
```

下载后我们使用如下命令来启动该jar包

```shell
java -jar zipkin-server-2.12.9-exec.jar
```

启动成功后如下：

我们就得到了一个页面地址

```http
http://localhost:9411/zipkin/
```

---

![image-20201121092603449](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/21/092604-307071.png)

本次测试观察Sleuth的调用链路，我们使用最简单的原微**服务服务提供者和消费者**，接下来就进入服务的改造

##### 服务改造

我们将使用 `springcloud-provider-pay` 以及 `springcloud-consumer-order` 这两个简单工程的改造

###### 提供者改造

- pom文件新增如下依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-zipkin</artifactId>
</dependency>
```

在IDEA右侧的依赖栏中我们可以看到，此starter包含了Zipkin和Sleuth

---

![image-20201121093402567](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/21/104043-124954.png)

---

- yaml文件新增如下配置

你需要添加到spring下，跟spring.application.name同级

```yaml
  zipkin:
    base-url: http://localhost:9411 # 表示最后数据的流向，我们希望他指向我们刚刚生成的zipkin的页面
  sleuth:
    sampler:
    probability: 1 # 样例数据采集度，取值为 0 - 1 如果为 1表示全部采集（大型微服务工程可能比较耗时）
```

- Controller

为了不掺着原来的业务方法，我们新建一个用于测试

```java
@GetMapping("/zipkin")
public String zipkinOrSleuth(){
    return UUID.randomUUID().toString();
}
```

###### 消费者改造

- 如上述新增pom
- 如上述新增yaml
- Controller

调用指定上述提供者提供的服务地址

```java
@GetMapping("/zipkin")
public JsonResult consumerZipkinOrSleuth(){
    return restTemplate.getForObject(URL + "/pay/zipkin", JsonResult.class);
}
```

###### 测试开始

依次启动Eureka 7001、pay、order工程后，完成调用并观察9411界面的变化

---

![image-20201121095945986](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/21/104044-816693.png)

---

# SpringCloud Alibaba

在介绍SpringCloud Alibaba之前，我们先说点别的，说一下SpringCloud的发展史

SpringCloud最先由Netflix公司将其开源贡献出来，后来当Netflix组件大部分的停更进维后，由它商业合作的对象也就是Spring的母公司Pivotal正式接手，后来在Spring团队的开发中，逐渐的加入了Spring自己的元素，再到后来的2018年，阿里云正式发布了自己的一套分布式系统的解决方案，经过两年的高速迭代，再加上与Spring的合作关系，目前已被Spring官网推荐使用其中的某些组件

## 简介

Spring Cloud Alibaba 致力于提供微服务开发的一站式解决方案。此项目包含开发分布式应用微服务的必需组件，方便开发者通过 Spring Cloud 编程模型轻松使用这些组件来开发分布式应用服务。

依托 Spring Cloud Alibaba，您只需要添加一些注解和少量配置，就可以将 Spring Cloud 应用接入阿里微服务解决方案，通过阿里中间件来迅速搭建分布式应用系统

SpringCloud Alibaba的学习成本不高，如果是你已经掌握了SpringCloud的话

更多详情可见SpringCloud Alibaba 官网：https://spring.io/projects/spring-cloud-alibaba

SpringCloud Alibaba官网教程：https://spring-cloud-alibaba-group.github.io/github-pages/hoxton/en-us/index.html#_introduction

SpringCloud Alibaba GitHub博客教程：https://github.com/alibaba/spring-cloud-alibaba/blob/master/README-zh.md

### SpringCloud Alibaba版本规范

项目的版本号格式为 x.x.x 的形式，其中 x 的数值类型为数字，从 0 开始取值，且不限于 0~9 这个范围。项目处于孵化器阶段时，第一位版本号固定使用 0，即版本号为 0.x.x 的格式。

由于 Spring Boot 1 和 Spring Boot 2 在 Actuator 模块的接口和注解有很大的变更，且 spring-cloud-commons 从 1.x.x 版本升级到 2.0.0 版本也有较大的变更，因此我们采取跟 SpringBoot 版本号一致的版本:

- 1.5.x 版本适用于 Spring Boot 1.5.x
- 2.0.x 版本适用于 Spring Boot 2.0.x
- 2.1.x 版本适用于 Spring Boot 2.1.x
- 2.2.x 版本适用于 Spring Boot 2.2.x

## 起步

我们在前期已经引入了最新的SpringCloud Alibaba pom依赖，并将它构建进入了maven 的父工程中

我们可以直接使用子工程来引入其相关组件技术的实现pom

如果还未引入，详情可参考**SpringCloud 起步**

引入的Alibaba版本为：

```xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-alibaba-dependencies</artifactId>
    <version>2.2.1.RELEASE</version>
    <type>pom</type>
    <scope>import</scope>
</dependency>
```

## 组件

### Nacos

#### 简介

**为什么叫做Nacos**

前四个字母分别为Naming和Configuration的前两个字母，最后的s为Service

**Nacos是一个更易于构建云原生应用的动态服务发现，配置管理和服务管理中心**

原官方的话我们说回来就是：

就是*注册中心 + 消息总线*，所代替的原Netflix技术为：Eureka + SpringCloud Bus + SpringCloud Config

官网教程：https://spring-cloud-alibaba-group.github.io/github-pages/hoxton/en-us/index.html#_spring_cloud_alibaba_nacos_discovery

Nacos手册：https://nacos.io/zh-cn/docs/what-is-nacos.html

GitHub：https://github.com/alibaba/Nacos

#### 起步

##### 下载

要想使用Nacos我们需要去它的官网下载：https://nacos.io/zh-cn/

经由链接再次跳转到GitHub后，我们选择打开Tag，找到一个稳定的版本下载，截止到目前2020/11/21日，GitHub最新的Nacos稳定版本为：`1.4.0`

我们选择下载Linux版的（当然也可以选择下载windows格式的zip包），放到**Tencent**云上去

##### 安装

下载后由**Xftp**上传到指定文件夹目录中，我们假设它为：*/usr/local/software/nacos*

进入到指定文件夹后使用如下命令解压： 

```shell
tar -xvf xx.tar.gz
```

更改文件夹的名称为

```shell
mv folder A folder B
```

##### 启动

###### 单机启动

```shell
cd /usr/local/software/nacos/bin
sh startup.sh -m standalone
```

或

```shell
bash startup.sh -m standalone
```

单机启动后，会在后台生成日志文件 `start.out`，可在nacos安装目录的log目下查看启动信息

启动后可访问来查看nacos页面：

```http
http:ip:8848/nacos
```

密码和账号默认为 **nacos**

##### Make 注册中心

###### 服务提供者

新建基于Nacos的服务提供者工程 `springcloud-alibaba-nacos-provider9001` 工程

- pom文件

由于我们在父pom中已经添加了SpringCloud Alibaba的依赖，在其子模块中我们只需要引入nacos即可

```xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
```

- yaml文件

```yaml
server:
  port: 9001

spring:
  application:
    name: nacos-payment-provider
  cloud:
    nacos:
      discovery:
        server-addr: www.ip:8848 #配置Nacos地址

management:
  endpoints:
    web:
      exposure:
        include: '*'
```

- 主启动类

```java
/**
 * @Author: kate
 * @Date: 2020/11/21 17:45
 */
@SpringBootApplication
@EnableDiscoveryClient
public class MainApplicationWithNacosProvider9001 {
    public static void main(String[] args) {
        SpringApplication.run(MainApplicationWithNacosProvider9001.class);
    }
}
```

- 业务代码

```java
@RestController
@RequestMapping("/pay")
public class PayController {
    @Value("${server.port}")
    private String serverPort;

    @GetMapping(value = "/nacos/{id}")
    public String getPayment(@PathVariable("id") Integer id) {
        return "nacos registry, serverPort: "+ serverPort+"\t id"+id;
    }
}
```

我们依次启动 nacos、9001来观察nacos中是否有此服务被注册进来

---

![image-20201121175028354](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/21/175029-523734.png)

---

搭建好生产者之后我们为了后续测试nacos的负载均衡，还要仿照着9001创建9002工程

这里有一个小知识点，**我们如果不想再手动的创建工程，可以 copy一个以原工程为模板只有端口号不同的新工程**

实例如下：

---

![image-20201121175549829](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/22/105207-546538.png)

---

![image-20201121175753174](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/22/092451-158366.png)

---

完成即可在Service出生成一个9001的复制工程启动类，我们启动它后再次观察nacos控制台

![image-20201121175850358](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/21/175851-869508.png)

---

nacos控制台的实例数就变成了2个，**不过这种方式在实际工作中不推荐使用，因为毕竟是虚拟的，依托的还是原来的9001工程**

**我们依然需要创建9002工程来完成接下来实例的演示**

9002工程的创建这里不再叙述，过程同上！

###### 服务消费者

我们创建订单的消费者工程 `springcloud-alibaba-nacos-consumer81` 来测试nacos的负载均衡能力

- pom文件加入nacos的依赖
- yaml文件配置如下

```yaml
server:
  port: 81

spring:
  application:
    name: nacos-order-conusmer
  cloud:
    nacos:
      discovery:
        server-addr: ip:8848
# 这个就是controller等会要用到的地址，会使用springEL引用它
service:
  url: http://nacos-pay-provider
```

- 主启动类 加上 `@EnableDiscoveryClient`
- 负载均衡配置类

```java
@Configuration
public class LoadBalanceConfig {
    @Bean
    @LoadBalanced
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
}
```

- 业务类

```java
@RestController
@RequestMapping("/consumer")
public class OrderController {
    @Autowired
    private RestTemplate restTemplate;

    @Value("${service.url}")
    private String serviceURL;

    @GetMapping("/nacos/{id}")
    public String getLbPort(@PathVariable("id") Integer id){
        return restTemplate.getForObject(serviceURL + "/pay/nacos/" + id,String.class);
    }
}
```

- 依次启动nacos、9001、9002、81来测试负载均衡

###### 服务注册中心对比

Nacos能够不同于其他注册中心的是它能够在 `CP` 和 `AP` 之间自由切换

![image-20201122105016991](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/22/105020-841502.png)

我们在前面说过：CAP分别带表的意思

*C是所有节点在同一时间看到的数据是一致的*

*A的定义是所有的请求都会收到响应*

**何时选择使用何种模式?**

一般来说，如果不需要存储服务级别的信息目服务实例是通过nacos-clienti注册，并能够保持心跳上报，那么就可以选择AP模式。当前主涉的服务如Spring cloud 和Dubbo服务，都适用于AP模式，**AP模式为了服务的可能性而减弱了一致性，因此AP模式下只支持注册临时实例**

如果需要在服务级别编辑或者存储配置信息，那么CP是必须，K8S服务和DNS服务则适用于CP模式

**CP模式下则支持注册持久化实例，此时则是以Raft协议为集群运行模式**，该模式下注册实例之前必须先注册服务，如果服务不存在，则会返回错误

###### Nacos切换命令

```shell
curl -X PUT '$NACOS_SERVER:8848/nacos/v1/ns/operator/switches?entry=serverMode&value=CP'
```

###### Nacos全景图

![image-20201122104810463](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/22/104811-287414.png)

###### Nacos与其他注册中心的区别

![image-20201122104945762](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/22/104946-736250.png)

##### Make 配置中心

###### 基础配置

- 新建工程 `springcloud-alibaba-nacos-config-center3333`
- 新增如下pom

```xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
</dependency>
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
```

我们要创建两个yaml文件，一个用来做全局公共配置，另一个用来做自身项目的配置

- bootstrap.yaml

```yaml
server:
  port: 3333

spring:
  application:
    name: nacos-config-client
  cloud:
    nacos:
      discovery:
        server-addr: ip:8848 #服务注册中心地址
      config:
        server-addr: ip:8848 #配置中心地址
        file-extension: yaml #指定yaml格式的配置
```

- application.yaml

```yaml
spring:
  profiles:
    active: dev # 选择要读取的配置
```

- 主启动类加上 `@EnableDiscoveryClient`

- 业务类

```java
@RestController
@RequestMapping("/config")
@RefreshScope // 支持动态刷新
public class ConfigCenterController {
    
    @Value("${config.info}")
    private String info;
    
    @GetMapping("/info")
    public JsonResult<String> getInfo(){
        return JsonResult.success().setData(info);
    }
}
```

接下来我们为了测试程序获得Nacos上的配置文件，我们手动的在Nacos上创建几个文件，而Nacos中的文件的命名格式与Springboot大致相同

它的命名格式为：

```txt
${spring.application.name}-${spring.profile.active}.${spring.cloud.nacos.config.file-extension}
```

**${spring.application.name}：应用名**

**${spring.profile.active}：要激活的名字，一般我们都是 test、prod、dev**

**${spring.cloud.nacos.config.file-extension}：文件后缀**

这些在我们上述都有相应的配置

然后我们了解规则之后就可以登录Nacos的控制台进行文件的创建了

![image-20201122111606888](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/22/111609-283120.png)

---

**需要注意的是：Nacos支持的yaml文件的后缀格式为** `.yaml` 而不是 `.yml`

- 创建好相应文件后，我们就可以进行测试了

```http
http://localhost:3333/config/info
```

- 动态刷新无忧

*我们不需要任何以额外形式的外部刷新或者内部配置即可完成动态刷新的工能，只要改动相应的Nacos上配置文件*

###### 分类配置

我们需要分类配置的原因通常都是伴随着下面几个问题的出现：

**1、实际开发中，通常—个系统会准备dev开发环境、test测试环境、prod生产环境，如何保证指定环境启动时服务能正确读取到Nacos上相应环境的配置文件呢?**

**2、一个大型分布式微服务系统会有很多微服务子项目，每个微服务项目又都会有相应的开发环境、测试环境、预发环境、正式环境..那怎么对这些微服务配置进行管理呢?**

实际上这些问题在多个微服务的真实业务场景中很是平常

*分类配置是什么？*

*Nacos使用 Namespace + Group + DataId的配置管理方式来区分多个微服务配置之间存在的所有问题*

这种设计方式有些像Java里面的package名和类名，最外层的namespace是可以用于区分部署环境的，Group和DataID逻辑上区分两个目标对象

*架构图如下：*

---

![image-20201122141849988](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/22/141850-686902.png)

---

默认情况下：*Namespace=public，Group=DEFAULT_GROUP,Cluster=DEFAULT*

我们再来说一下他们各自都代表着什么：

比方说我们现在有三个环境:开发、测试、生产环境，我们就可以创建三个Namespace，不同的Namespace之间是隔离的

Group可以把不同的微服务划分到同一个分组里面去

Service就是微服务;一个Service可以包含多个Cluster (集群)，Cluster是对指定微服务的一个虚拟划分。比方说为了容灾，将Service微服务分别部署在了杭州机房和广州机房，这时就可以给杭州机房的Service微服务起一个集群名称(HZ)，给广州机房的Service微服务起一个集群名称(GZ)，还可以尽量让同一个机房的微服务互相调用，以提升性能

最后是Instance，就是微服务的实例

**分类配置方案**

*Namespace方案*

Namespace配置可尝试使用如下实例：

我们在Nacos控制台新建 `命名空间 (private)` ，且在新的命名空间中加入  `nacos-config-client-prod.yaml` 并使用如下配置读取

```yaml
config:
    server-addr: ip:8848 #配置中心地址
    file-extension: yaml #指定yaml格式的配置
    namespace: private # 指定命名空间
    #group: GROUP_TEST # 指定组
```

*Group方案*

Group方案，即同组，不同Namespace、DataID

我们在Nacos控制台新建 `GROUP_TEST` 组，且在组中加入  `nacos-config-client-dev.yaml` 并使用如下配置读取

bootstrap.yaml

```yaml
config:
    server-addr: ip:8848 #配置中心地址
    file-extension: yaml #指定yaml格式的配置
    namespace: d3030867-48b3-41ed-87ba-79f22a4635e1 # 指定命名空间ID
    #group: GROUP_TEST # 指定组
```

*DataID方案*

所谓DataID方案就是同Namespace、Group，不同的文件名

我们在nacos控制台新建 ` nacos-config-client-test.yaml` 尝试使用如下配置来读取它

application.yaml

```yaml
spring:
  profiles:
#   active: dev
#   active: test
    active: prod
```

#### 高级

##### 集群概念

在实际的生产环境中，nacos不可能只有一台，集群模式下的nacos的架构图是这样的：

---

![image-20201128082201633](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/28/082202-703933.png)

---

*VIP:代表的是虚拟的IP映射，这里我们可以把它理解为Nginx*

详细化官网架构后，得到的图架构：

---

![image-20201128082337663](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/28/082340-681323.png)

---

为什么需要MySQL？我们将在下节中详解

##### 持久化

我们使用Nacos做配置中心时，新建的配置文件被存储到了哪里？

如果是存在于内存中，为什么重启nacos还是会有，如果存在 阿里云端，是怎么跟本机 `localhost` 交互的？

*其实Nacos默认自带的是嵌入式数据库derby*

GitHub 上Nacos的依赖

https://github.com/alibaba/nacos/blob/develop/config/pom.xml

```xml
<dependency>
    <groupId>org.apache.derby</groupId>
    <artifactId>derby</artifactId>
</dependency>
```

**Nacos使用嵌入式数据库保存配置于你自己机器的本地，读取的也是你本地的资源，如果考虑集群的搭建，那么每一个Nacos都使用自己内嵌的数据库无疑会无法保证数据的管理**

我们考虑使用外部数据库来保存Nacos在运行期间所产生的任何数据文件

###### MySQL搭建

如何使Nacos从嵌入式数据库derby中迁徙到外部MySQL中？可参考如下几步：

**前提：确保MySQL版本不低于** *5.6.5*

- 在你安装Nacos的目录中，基于Nacos的根目录 `/nacos/conf/` 目录中有 `nacos-mysql.sql`  执行此脚本!
- 相同目录下 再找到 `application.properties` 在其配置的最后加入如下配置：

```properties
spring.datasource.platform=mysql
 
db.num=1
db.url.0=jdbc:mysql://ip:3306/nacos_config?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
db.user=root
db.password=kate
```

- 重启Nacos，完成配置！

```shell
sh shutdown.sh
```

##### 集群开始

###### 环境搭建

**1、搭建Nacos基于Linux的系统环境（前提需要有JDK环境）**

**2、MySQL上Linux（本次实例基于Docker）**

**3、Nginx上Linux（本次实例基于Docker）**

配置Nginx，集中映射到Nacos，我们再启动一个Nginx实例

```shell
docker run -d -p 1111:80 --name nginxnacos -v /data/nginx/nacos/conf:/etc/nginx/conf -v /data/nginx/nacos/nginx.conf:/etc/nginx/nginx.conf nginx
```

使用Nginx的端口+ip访问Nacos即可

**4、执行MySQL Nacos脚本**

**5、添加MySQL外部 URL 配置到 Nacos application.properties配置文件**

**6、复制Nacos集群文件 cluster.conf 并更改**

梳理出3台nacos机器的不同服务端口号

*在nacos的解压目录nacos/的conf目录下，有配置文件cluster.conf，请每行配置成ip:port。（请配置3个或3个以上节点）*

示例：

```shell
ip:3333
ip:6666
ip:9999
```

**7、编辑Nacos的启动脚本startup.sh，使它能够接受不同的启动端**

*集群启动，我们希望可以类似其它软件的shell命令，传递不同的端口号启动不同的nacos实例*

命令:sh /startup.sh  -p 3333表示启动端口号为3333的nacos服务器实例，和上一步的cluster.conf配置的一致

修改如下：

nacos解压目录下的 /bin目录有startup.sh

![image-20201128093039965](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/28/093041-411844.png)

- ```shell
  while getopts ":m:f:s:c:p:P:" opt
  do
      case $opt in
          m)
              MODE=$OPTARG;;
          f)
              FUNCTION_MODE=$OPTARG;;
          s)
              SERVER=$OPTARG;;
          c)
              MEMBER_LIST=$OPTARG;;
          p)
              EMBEDDED_STORAGE=$OPTARG;;
          P)
              PORT=$OPTARG;; # 由于原本的 小写 p 被占用，我们使用大写 P
          ?)
          echo "Unknown parameter"
          exit 1;;
      esac
  done
  ```

- ```shell
  nohup $JAVA -Dserver.port=${PORT} ${JAVA_OPT} nacos.nacos >> ${BASE_DIR}/logs/start.out 2>&1 &
  ```

查看启动的Nacos进程

```shell
ps -ef|grep nacos|grep -v grep|wc -l
```

<u>注意：官网使用三台机器配置Nacos集群，这里的一台机器始终没有成功，推荐生产环境使用三台机器集群</u>

### Sentinel

#### 简介

**什么是Sentinel**

*Sentinel （分布式系统的流量防卫兵）*

随着微服务的流行，服务和服务之间的稳定性变得越来越重要。Sentinel 以流量为切入点，从流量控制、熔断降级、系统负载保护等多个维度保护服务的稳定性

功能类似于Hystrix，拥有熔断降级、流量控制的功能

**特点**

- *丰富的应用场景*：Sentinel 承接了阿里巴巴近 10 年的双十一大促流量的核心场景，例如秒杀（即突发流量控制在系统容量可以承受的范围）、消息削峰填谷、集群流量控制、实时熔断下游不可用应用等。
- *完备的实时监控*：Sentinel 同时提供实时的监控功能。您可以在控制台中看到接入应用的单台机器秒级数据，甚至 500 台以下规模的集群的汇总运行情况。
- *广泛的开源生态*：Sentinel 提供开箱即用的与其它开源框架/库的整合模块，例如与 Spring Cloud、Dubbo、gRPC 的整合。您只需要引入相应的依赖并进行简单的配置即可快速地接入 Sentinel。
- *完善的 SPI 扩展点*：Sentinel 提供简单易用、完善的 SPI 扩展接口。您可以通过实现扩展接口来快速地定制逻辑。例如定制规则管理、适配动态数据源等。

**主要特征**

![image-20201129133514992](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/29/133516-143276.png)

**Sentinel 分为两个部分**

- 核心库（Java 客户端）不依赖任何框架/库，能够运行于所有 Java 运行时环境，同时对 Dubbo / Spring Cloud 等框架也有较好的支持。
- 控制台（Dashboard）基于 Spring Boot 开发，打包后可以直接运行，不需要额外的 Tomcat 等应用容器。

Sentinel官网：https://github.com/alibaba/Sentinel/wiki/%E4%BB%8B%E7%BB%8D

SpringCloud 官网文档：https://spring-cloud-alibaba-group.github.io/github-pages/greenwich/spring-cloud-alibaba.html#_spring_cloud_alibaba_sentinel

#### 起步

##### 安装

安装Sentinel Dashboard，基于Docker，详情可参见  `DIS.md`

还可基于本地安装，下载jar 包启动即可！

下载地址：https://github.com/alibaba/Sentinel/releases/tag/v1.8.0

```shell
java -jar sentinel-dashboard-1.7.1.jar
```

我们将使用 Sentinel 演示解决如下问题：

- 服务雪崩
- 服务降级
- 服务熔断
- 服务限流

##### 初始化工程

- 新建 `springcloud-alibaba-sentinel-service8401` 工程
- pom文件新增如下

```xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
<dependency>
    <groupId>com.alibaba.csp</groupId>
    <artifactId>sentinel-datasource-nacos</artifactId>
</dependency>
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

- yaml文件

```yaml
server:
  port: 8401

spring:
  application:
    name: cloudalibaba-sentinel-service
  cloud:
    nacos:
      discovery:
        server-addr: ip:8848 # nacos地址
    sentinel:
      transport:
#        dashboard: ip:8858
        dashboard: localhost:8080
        port: 8719  #默认8719，假如被占用了会自动从8719开始依次+1扫描。直至找到未被占用的端口

management:
  endpoints:
    web:
      exposure:
        include: '*'
```

- 主启动类加入注解  `@EnableDiscoveryClient`

- 简单业务方法

```java
@GetMapping("/testSentinel")
public Integer test(){
    return 222;
}
```

- 测试访问该方法后观察 Dashboard控制台

**注意：Nacos要提前启动**

```http
http://localhost:8858
```

##### 流控

###### 简介

![image-20201129150225285](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/29/150225-546130.png)

- *资源名*:唯一名称，默认请求路径

- *针对来源*: Sentinel可以针对调用者进行限流，填写微服务名，默认default (不区分来源)
- *阈值类型/单机阈值*：
  -  QPS(每秒钟的请求数量)︰当调用该api的QPS达到阈值的时候，进行限流
  -  线程数:当调用该api的线程数达到阈值的时候，进行限流
- *是否集群*:不需要集群
- *流控模式*:
  - 直接:api达到限流条件时，直接限流
  - 关联:当关联的资源达到阈值时，就限流自己
  - 链路:只记录指定链路上的流量（指定资源从入口资源进来的流量，如果达到阈值，就进行限流)【api级别的针对来源】
- *流控效果*:
  - 快速失败:直接失败，抛异常
  - Warm Up:根据codeFactor(冷加载因子，默认3）的值，从阈值codeFactor，经过预热时长，才达到设置的QPS阈值
  - 那队等待:匀速排队，让请求以匀速的速度通过，阈值类型必须设置为QPS，否则无效

###### QPS

**配置图：**

![image-20201129153255059](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202012/06/191902-212398.png)

---

*配置为QPS直接快速失败，表示每秒过来的请求数到达指定的阈值后，快速的响应失败！*

**响应内容为**：*Blocked by Sentinel (flow limiting)*

###### 线程数

**配置图：**

![image-20201129153331268](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/29/153332-116492.png)

---

*配置为线程直接快速失败后，表示当超过配置线程数时，直接快速响应失败，失败内容同上！*

快速直接失败返回为高级 *默认*

###### 关联

**什么是关联模式：**

*当关联的资源达到阈值时，就限流自己，当与A关联的资源B达到阈值后，就限流自己，相当于：B惹事，A挂了*

**配置图：**

![image-20201129153534446](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/29/153535-468961.png)

---

**关联模式的两种配置：**

- 关联-->QPS（快速请求B，QPS大于阈值，导致A挂掉）
- 关联-->线程数（请求B时线程数大于指定阈值，A挂掉）

**测试：**

我们使用PostMan或者Jmeter来测试线程数，QPS，使用浏览器测试即可

**PostMan测试步骤如下：**

![image-20201129154355770](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/29/154356-471083.png)

---

![image-20201129154808645](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/29/154809-392245.png)

**返回结果：**同上

###### 链路

**配置：**

![image-20201129160343313](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/29/160343-925185.png)

---

Sentinel 允许只根据某个入口的统计信息对资源限流，我们只关心从入口过来的流量请求，而不去处理其他的方向的调用

###### 预热

配置图：

![image-20201129162014190](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/29/162014-699184.png)

---

*当系统处于空闲时，突然大量的请求到来可能会瞬间压垮系统，此时给系统设置预热，指定时间段，让系统慢慢恢复到能够处理越来越多的QPS*

公式：阈值除以coldFactor（默认值为3），经过预热时长后才会达到阈值

默认coldFactor为3，即请求QPS从threshold/3开始，经预热时长逐渐升至设定的QPS阈值。

冷加载因子代码体现：com.alibaba.csp.sentinel.slots.block.flow.controller.WarmUpController

###### 排队等待

配置图：

![image-20201129162557777](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/29/162558-486703.png)

匀速排队（`RuleConstant.CONTROL_BEHAVIOR_RATE_LIMITER`）方式会严格控制请求通过的间隔时间，也即是让请求以均匀的速度通过，对应的是漏桶算法。详细文档可以参考 [流量控制 - 匀速器模式](https://github.com/alibaba/Sentinel/wiki/流量控制-匀速排队模式)，具体的例子可以参见 [PaceFlowDemo](https://github.com/alibaba/Sentinel/blob/master/sentinel-demo/sentinel-demo-basic/src/main/java/com/alibaba/csp/sentinel/demo/flow/PaceFlowDemo.java)

这种方式主要用于处理间隔性突发的流量，例如消息队列。想象一下这样的场景，在某一秒有大量的请求到来，而接下来的几秒则处于空闲状态，我们希望系统能够在接下来的空闲期间逐渐处理这些请求，而不是在第一秒直接拒绝多余的请求

<u>注：匀速排队模式暂时不支持 QPS > 1000 的场景</u>

![image-20201129162507883](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/29/162509-789129.png)

---

##### 降级

###### 简介

Sentinel熔断降级会在调用链路中某个资源出现不稳定状态时（例如调用超时或异常比例升高)，对这个资源的调用进行限制,让请求快速失败，避免影响到其它的资源而导致级联错误

当资源被降级后，在接下来的降级时间窗口之内，对该资源的调用都自动熔断（默认行为是抛出 DegradeException)

**Sentinel的断路器是没有半开状态的：**

半开的状态系统自动去检测是否请求有异常，没有异常就关闭断路器恢复使用，有异常则继续打开断路器不可用。具体可以参考Hystrix

**规则有如下三种：**

- RT
- 异常比例
- 异常数

###### RT

**配置图：**

![image-20201129165120632](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/29/165121-667666.png)

慢调用比例 (`SLOW_REQUEST_RATIO`)：选择以慢调用比例作为阈值，需要设置允许的慢调用 RT（即最大的响应时间），请求的响应时间大于该值则统计为慢调用。当单位统计时长（`statIntervalMs`）内请求数目大于设置的最小请求数目，并且慢调用的比例大于阈值，则接下来的熔断时长内请求会自动被熔断。经过熔断时长后熔断器会进入探测恢复状态（HALF-OPEN 状态），若接下来的一个请求响应时间小于设置的慢调用 RT 则结束熔断，若大于设置的慢调用 RT 则会再次被熔断

*一秒钟打进来10个线程(大于5个了)调用testD，我们希望200毫秒处理完本次任务，如果超过200毫秒还没处理完，在未来1秒钟的时间窗口内，断路器打开(保险丝跳闸)微服务不可用，保险丝跳闸断电了*

###### 异常比例

**配置图：**

![image-20201129170811603](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/29/170812-174838.png)

异常比例 (`ERROR_RATIO`)：当单位统计时长（`statIntervalMs`）内请求数目大于设置的最小请求数目，并且异常的比例大于阈值，则接下来的熔断时长内请求会自动被熔断。经过熔断时长后熔断器会进入探测恢复状态（HALF-OPEN 状态），若接下来的一个请求成功完成（没有错误）则结束熔断，否则会再次被熔断。异常比率的阈值范围是 `[0.0, 1.0]`，代表 0% - 100%

==指定时间窗口期内的所有请求，都会被保护==

###### 异常数

配置图：

![image-20201129170947497](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/29/170947-648046.png)

异常数 (`ERROR_COUNT`)：当单位统计时长内的异常数目超过阈值之后会自动进行熔断。经过熔断时长后熔断器会进入探测恢复状态（HALF-OPEN 状态），若接下来的一个请求成功完成（没有错误）则结束熔断，否则会再次被熔断

==请求到达指定异常数时，在指定时间窗口期内的再次请求会被保护==

##### 热点Key

###### 简介

何为热点？热点即经常访问的数据。很多时候我们希望统计某个热点数据中访问频次最高的 Top K 数据，并对其访问进行限制。比如：

- 商品 ID 为参数，统计一段时间内最常购买的商品 ID 并进行限制
- 用户 ID 为参数，针对一段时间内频繁访问的用户 ID 进行限制

配置图：

![image-20201201160119554](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202012/01/160119-986944.png)

###### 限流

使用参数的形式进行限流，从参数的QPS、类型、值来判断

```java
@GetMapping("/testHotKey")
@SentinelResource(value = "testHotKey",blockHandler = "objectRestResult")
public RestResult<Object> testHotKey(@RequestParam(value = "p1",required = false) String p1,@RequestParam(value = "p1",required = false) String p2) {
    //int age = 10/0;
    return RestResult.builder().withCode(HttpStatus.HTTP_OK).withMsg("------testHotKey").build();
}

// 负责兜底
public RestResult<Object> objectRestResult(String p1,String p2,BlockException blockException){
    return RestResult.builder().withCode(HttpStatus.HTTP_INTERNAL_ERROR).withMsg("deal sentinel for this request").build();
}
```

在上述代码中我们使用了注解 `@SentinelResource` ，它的作用在此处类似于 `HystrixCommand` 我们会在下一章节为你详细介绍

在此处此注解做一个兜底方法的回调

###### 参数例外项

当参数为某个值时，我们希望它能够获得不同于它值的QPS，例如，当参数为0时，我们希望QPS是1，当参数为9时，我们希望QPS为200

填入参数例外项等

<u>回调方法不支持异常处理的回调兜底，只负责内部QPS和线程数的判断</u>

##### 系统规则

相当于全局系统的配置，所有在系统规则一栏中出现的配置都会影响整个系统，属于总控规则和限流

不再贴图演示，过于简单，了解即可，工作中此配置不常用

##### @SentinelResource

###### 资源名称限流+后续处理

```java
@GetMapping("/testSource")
@SentinelResource(value = "/testSource",blockHandler = "objectRestResult")
public RestResult testSource(String var1){
    return RestResult.builder().withCode(200).withData(var1).withMsg("message").build();
}
```

配置增加流控，测试按资源名称限流的方法

###### URL限流+默认处理

```java
@GetMapping("/url")
@SentinelResource(value = "/url")
public RestResult testUrl(){
    return RestResult.builder().withCode(200).withData("url!!!").withMsg("message").build();
}
```

如果没有写明兜底的方法，那么系统就会采用默认的兜底方法

###### 问题

- 系统默认的，没有体现我们自己的业务要求
- 依照现有条件，我们自定义的处理方法又和业务代码耦合在一块，不直观
- 每个业务方法都添加一个兜底的，那代码膨胀加剧
- 全局统—的处理方法没有体现

###### 自定义限流处理

为了解决上述问题，我们选择重新创建一个类来封装系统限流时处理的方法，达到与业务逻辑的解耦

```java
public class CustomerBlockHandler {

    public RestResult csb1(){
        return RestResult.builder().withCode(200).withData("客户自定义限流时兜底方法-------1").withMsg("message").build();
    }

    public RestResult csb2(){
        return RestResult.builder().withCode(200).withData("客户自定义限流时兜底方法-------2").withMsg("message").build();
    }
}
```

业务层的Controller就可以写成这样：

```java
@GetMapping("/url")
@SentinelResource(value = "/url",
                  blockHandlerClass = {CustomerBlockHandler.class},
                  blockHandler = "csb1")
public RestResult testUrl(){
    return RestResult.builder().withCode(200).withData("url!!!").withMsg("message").build();
}
```

限流时就会回掉 `CustomerBlockHandler` 类的  `csb1()` 方法

##### 服务熔断（Ribbon）

Sentinel整合Ribbon做服务熔断

###### 环境搭建

- 新建工程 `springcloud-alibaba-sentinel-ribbon-provider9003` 和 `springcloud-alibaba-sentinel-ribbon-provider9004`
- pom文件引入如下依赖

```xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
<dependency>
    <groupId>cn.kate.cloud</groupId>
    <artifactId>api</artifactId>
    <version>0.1</version>
</dependency>
```

- yaml文件

```yaml
server:
  port: 9003

spring:
  application:
    name: sentinel-ribbon-pay-provider
  cloud:
    nacos:
      discovery:
        server-addr: riskate:8848 #配置Nacos地址

management:
  endpoints:
    web:
      exposure:
        include: '*'
```

- 启动类注解 `EnableDiscoveryClient`
- 业务类模拟查询数据库

```java
@RestController
public class ProviderController {
    @Value("${server.port}")
    private String serverPort;

    public static HashMap<Long, Pay> hashMap = new HashMap<>();
    static{
        hashMap.put(1L,new Pay(1,"28a8c1e3bc2742d8848569891fb42181"));
        hashMap.put(2L,new Pay(2,"bba8c1e3bc2742d8848569891ac32182"));
        hashMap.put(3L,new Pay(3,"6ua8c1e3bc2742d8848569891xt92183"));
    }

    @GetMapping(value = "/paymentSQL/{id}")
    public RestResult<Pay> paymentSQL(@PathVariable("id") Long id){
        Pay pay = hashMap.get(id);
        RestResult<Pay> result = new RestResult(200,"from mysql,serverPort:  "+serverPort,pay);
        return result;
    }
}
```

- 保证9003和9004工程启动通过自测

搭建消费者工程：`springcloud-alibaba-sentinel-ribbon-consumer84`

- pom

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
<dependency>
    <groupId>cn.kate.cloud</groupId>
    <artifactId>api</artifactId>
    <version>0.1</version>
</dependency>
```

- yaml

```yaml
server:
  port: 84


spring:
  application:
    name: sentinal-ribbon-order-consumer
  cloud:
    nacos:
      discovery:
        server-addr: ip:8848
    sentinel:
      transport:
        dashboard: localhost:8080
        port: 8719

service-url:
  nacos-provider-service: sentinel-ribbon-pay-provider
```

- 主启动类加 `@EnableDiscoveryClient`  和  `@EnableFeignClients`
- Ribbon负责均衡类配置

```java
@Configuration
public class ApplicationContextConfig
{
    @Bean
    @LoadBalanced
    public RestTemplate getRestTemplate()
    {
        return new RestTemplate();
    }
}
```

- 业务类代码

```java
@RestController
@Slf4j
public class CircleBreakerController {

    public String serviceUrl = "http://sentinel-ribbon-pay-provider";

    @Resource
    private RestTemplate restTemplate;

    @RequestMapping("/consumer/fallback/{id}")
    @SentinelResource(value = "fallback")

    public RestResult<Pay> fallback(@PathVariable Integer id) {
        RestResult<Pay> result = restTemplate.getForObject(serviceUrl + "/pay/" + id, RestResult.class, id);

        if (id == 4) {
            throw new IllegalArgumentException("IllegalArgumentException,非法参数异常....");
        } else if (result.getData() == null) {
            throw new NullPointerException("NullPointerException,该ID没有对应记录,空指针异常");
        }

        return result;
    }
}
```

- 测试84工程的正常轮询负载访问

访问此 URL [localhost:84/consumer/fallback/4](http://localhost:84/consumer/fallback/4) ，会抛出异常到控制台，我们可以配置 fallback 来处理服务异常时的兜底方法

###### 只配置Fallback

在 `CircleBreakerController` 中加入如下代码，并在调用方法的 `@SentinelResource` 的属性 加上 `fallback = "fallbackMethodInvoke"`

```java
public RestResult fallbackMethodInvoke(@PathVariable  Integer id,Throwable e) {
    Pay payment = new Pay(id,"null");
    return new RestResult(444,"兜底异常handlerFallback,exception内容  "+e.getMessage(),payment);
}
```

###### 只配置BlockHandler

在 `CircleBreakerController` 中加入如下代码，并在调用方法的 `@SentinelResource` 的属性 加上 `blockHandler = "blockHandlerMethodInvoke"`

```java
public RestResult blockHandlerMethodInvoke(@PathVariable  Integer id, BlockException blockException) {
    Pay pay = new Pay(id,"null");
    return new RestResult<>(445,"blockHandler-sentinel限流,无此流水: blockException  "+blockException.getMessage(),pay);
}
```

###### 都配置

二者都配置的情况下，当访问QPS达到指定阈值时，访问的又是 fallback 的错误回调方法

结论：限流不达到阈值时，由fallback回调执行，限流达到阈值时，从fallback后的页面再次转换到 blockHandler 调用的方法执行

###### 异常忽略项

即可以配置指定异常不由fallback降级处理

```java
@SentinelResource(value = "fallback",
            fallback = "fallbackMethodInvoke",
            blockHandler = "blockHandlerMethodInvoke",
            exceptionsToIgnore = {NullPointerException.class}) // 配置了此属性，指定的异常将不再触发fallback的回调
```

##### 服务熔断（Feign）

我们都知道，Ribbon的负载均衡的效应和服务的调用是由 `RestTemplate` 来完成的，我们当然也可以使用 Sentinel 来整合 Feign

我们在前面的环境中已经整合了Feign，并且添加了部分Feign的代码，我们将在此章节，完成对Feign的完全改造

Feign都是配置在客户端，因此我们改造一下84工程

- yaml配置

```yaml
# 添加对Feign的支持
feign:
  sentinel:
    enabled: true
```

- 业务类Service

```java
@FeignClient(value = "sentinel-ribbon-pay-provider", fallback = PayServiceImpl.class)
public interface PayService {

    @GetMapping("/pay/{id}")
    public RestResult<Pay> paymentSQL(@PathVariable("id") Integer id);
}
```

- 此接口的实现，此方法的fallback降级处理

```java
@Component
public class PayServiceImpl implements PayService {
    @Override
    public RestResult<Pay> paymentSQL(Integer id) {
        return new RestResult(44444,"服务降级返回,---PaymentFallbackService",new Pay(id,"errorSerial"));

    }
}
```

- Controller加入如下代码

```java
@Resource
private PayService payService;

@GetMapping("/consumer/paymentSQL/{id}")
public RestResult pay(@PathVariable Integer id){
    return payService.paymentSQL(id);
}
```

- 主启动类的 `@FeignClients` 注解
- 启动测试（关闭9003，9004）

```java
http://localhost:84/consumer/paymentSQL/1
```

##### 熔断框架比较

![image-20201206184749899](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202012/06/184751-585464.png)

##### Sentinel持久化

每次重启项目，上一次配置的Sentinel限流规则都会被消除，怎么能够做到持久化规则呢？

使用Nacos存储Sentinel的限流规则，迫使项目运行后且访问了Controller接口方法后，Sentinel自动提取存储在Nacos的配置，重新读回规则

- 修改8401工程

- 新增pom如下

```xml
<dependency>
    <groupId>com.alibaba.csp</groupId>
    <artifactId>sentinel-datasource-nacos</artifactId>
</dependency>
```

- yaml新增如下

```yaml
# 跟port同级
datasource:
          ds1:
            nacos:
              server-addr: ip:8848
              dataId: ${spring.application.name}
              groupId: DEFAULT_GROUP
              data-type: json
              rule-type: flow
```

- 添加Nacos业务规则配置（新建配置文件，格式为 json，名字为上述配置（yaml）指定的名字（没有后缀））

```json
 [
    {
         "resource": "/sentinel2/testSource",
         "limitApp": "default",
         "grade":   1,
         "count":   1,
         "strategy": 0,
         "controlBehavior": 0,
         "clusterMode": false    
    }
]
```

内容解析：

- resource:资源名称
- limitApp:来源应用
- grade:阈值类型，0表示线程数，1表示QPS
- count:单机阈值
- strategy:流控模式，0表示直接，1表示关联，2表示链路
- controlBehavior:流控效果，0表示快速失败，1表示Warm Up，2表示排队等待
- clusterMode:是否集群

最终再次测试，即使重启应用后，Sentinel的配置规则也不会消失

==@Fixme==

如果Sentinel是本地的，Nacos是远程云服务器上的，可能出现无法持久化！

### Seata

#### 简介

在介绍Seata之前，我们先提出一个问题？

在分布式系统中，一个微服务可能调用多个不同环境下的数据库，那么这就有可能造成事务不一致问题

##### **什么是Seata**

Seata 是一款开源的分布式事务解决方案，致力于提供高性能和简单易用的分布式事务服务。Seata 将为用户提供了 *AT*、*TCC*、*SAGA* 和 *XA* 事务模式，为用户打造一站式的分布式解决方案

详情事务模式请查看官网：[Seata事务模式](http://seata.io/zh-cn/docs/overview/what-is-seata.html)

##### Seata解决方案

![image-20201212075230351](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202012/12/075235-130740.png)

---

![image-20201212075816006](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202012/12/075817-210918.png)

##### Seata术语

*Transaction ID XID*

全局唯一的事务ID

*TC (Transaction Coordinator) - 事务协调者*

维护全局和分支事务的状态，驱动全局事务提交或回滚。

*TM (Transaction Manager) - 事务管理器*

定义全局事务的范围：开始全局事务、提交或回滚全局事务。

*RM (Resource Manager) - 资源管理器*

管理分支事务处理的资源，与TC交谈以注册分支事务和报告分支事务的状态，并驱动分支事务提交或回滚。

更多关于Seata介绍详情见官网：[Seata官网](http://seata.io/zh-cn/docs/overview/what-is-seata.html)

#### 起步

##### 下载

下载地址为：[Seata下载](https://github.com/seata/seata/releases/tag/v1.3.0)

本地我们采用1.3.0版本（2020-7-14）

##### 安装

或者我们可以使用 Docker 容器的方式安装和挂载相应的配置文件，此方式详情见 `DIS.md`

###### 拉取和解压

我们可以选择 Windows 的zip包或者使用 Linux的 tar.gz 下载，我们优先推荐使用 Linux。接下来我们会假设你是用的是 Linux tar.gz 的安装方式

拉取到 Linux 指定后文件夹后进行解压

###### file.conf

修改conf目录下的file.conf配置文件，例如你的安装目录为：

```shell
/usr/local/software/seata/conf
```

*修改之前，你可以先备份一份，以免因为修改失误而导致软件损坏*

主要修改：自定义事务组名称+事务日志存储模式为db+数据库连接信息

修改的地方如下：

```shell
store {
	# mode = "file"
	# 改为
	mode = "db"
}

db {
	driverClassName = "com.mysql.jdbc.Driver"
    url = "jdbc:mysql://ip:3306/seata"
    user = "root"
    password = "kate"
}

# 用的时候再改就行
redis{
	host = "ip"
    port = "6379"
    password = "kate"
}
```

###### 新建数据库

到目标数据库建立 `seata` 数据库

建表  `db_store.sql`  在/usr/local/software/seata/conf目录里面

seata官方从`1.0`版本后不再提供sql脚本,以及nacos推送配置脚本,需要从`0.9.0`的版本复制

需要复制的四个文件为：

```txt
db_store.sql
do_undo_log.sql
nacos_config.sh
nacos_config.txt
```

将 `db_store.sql` SQL脚本在新创建的 seata数据库中执行

###### registry.conf

修改/usr/local/software/seata/conf目录下的registry.conf配置文件

修改部分为：

```shell
registry{
    type = "nacos"
    
    nacos {
    application = "seata-server"
    serverAddr = "ip:8848"
    group = "SEATA_GROUP"
    namespace = ""
    cluster = "default"
    username = ""
    password = ""
    }
} 
```

修改从`0.9.0`版本拉过来的`nacos-conf.txt`文件

原文件此处为：

```shell
service.vgroup_mapping.my_test_tx_group=default
```

修改为：

```shell
service.vgroupMapping.kate=default
```

`1.0.0`版本后,红框位置改为驼峰命名,请及时修改

`service.vgroupMapping.此处为自定义名称=default`需要和项目中的名称一致

如：

```yaml
spring:
  cloud:
    alibaba:
      seata:
        tx-service-group: kate
```

修改完毕后执行`1.3.0/conf`下的`nacos-config.sh`命令为:`sh nacos-config.sh ip`
`ip`为nacos地址,按实际情况修改即可

将配置推送到nacos后可以在配置列表看到

---

![image-20201212085334067](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202012/12/085335-632308.png)

##### 启动

在`seata-1.3.0/bin`目录下有两个脚本,`linux`一个`windows`一个

Linux使用如下命令启动即可

```shell
sh seata-server.sh
```

随后观察Nacos服务列表，注册有seata-server服务即表示成功！

##### 数据库准备

###### 业务说明

下订单-->扣库存-->减账户（余额）-->改状态（订单）

我们要准备相应的业务数据库来用于接下来的实例演示备用

###### 数据库准备

```sql
CREATE DATABASE seata_order;
CREATE DATABASE seata_storage;
CREATE DATABASE seata_account;
```

数据库说明如下：

- seata_order: 存储订单的数据库
- seata_storage:存储库存的数据库
- seata_account: 存储账户信息的数据库

###### 业务表准备

t_order：订单表

```sql
CREATE TABLE t_order(    `id` BIGINT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY,    `user_id` BIGINT(11) DEFAULT NULL COMMENT '用户id',    `product_id` BIGINT(11) DEFAULT NULL COMMENT '产品id',    `count` INT(11) DEFAULT NULL COMMENT '数量',    `money` DECIMAL(11,0) DEFAULT NULL COMMENT '金额',    `status` INT(1) DEFAULT NULL COMMENT '订单状态：0：创建中; 1：已完结') ENGINE=INNODB AUTO_INCREMENT=7 DEFAULT CHARSET=utf8; SELECT * FROM t_order;
```

t_storage：库存表

```sql
CREATE TABLE t_storage(    `id` BIGINT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY,    `product_id` BIGINT(11) DEFAULT NULL COMMENT '产品id',   `total` INT(11) DEFAULT NULL COMMENT '总库存',    `used` INT(11) DEFAULT NULL COMMENT '已用库存',    `residue` INT(11) DEFAULT NULL COMMENT '剩余库存') ENGINE=INNODB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8; INSERT INTO seata_storage.t_storage(id,product_id,total,used,residue)VALUES(1,1,100,0,100);  SELECT * FROM t_storage;
```

t_account：用户表

```sql
CREATE TABLE t_account(    `id` BIGINT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY COMMENT 'id',    `user_id` BIGINT(11) DEFAULT NULL COMMENT '用户id',    `total` DECIMAL(10,0) DEFAULT NULL COMMENT '总额度',    `used` DECIMAL(10,0) DEFAULT NULL COMMENT '已用余额',    `residue` DECIMAL(10,0) DEFAULT '0' COMMENT '剩余可用额度') ENGINE=INNODB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8; INSERT INTO seata_account.t_account(`id`,`user_id`,`total`,`used`,`residue`) VALUES(1,1,1000,0,1000);select * from t_account;
```

订单-库存-账户3个库下都需要建各自的回滚日志表

回滚日志表SQL由Seata官方提供，我们需要把 `db_undo_log.sql` 在每一个业务库中执行以下

```sql
-- the table to store seata xid data
-- 0.7.0+ add context
-- you must to init this sql for you business databese. the seata server not need it.
-- 此脚本必须初始化在你当前的业务数据库中，用于AT 模式XID记录。与server端无关（注：业务数据库）
-- 注意此处0.3.0+ 增加唯一索引 ux_undo_log
-- drop table `undo_log`;
CREATE TABLE `undo_log` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `branch_id` bigint(20) NOT NULL,
  `xid` varchar(100) NOT NULL,
  `context` varchar(128) NOT NULL,
  `rollback_info` longblob NOT NULL,
  `log_status` int(11) NOT NULL,
  `log_created` datetime NOT NULL,
  `log_modified` datetime NOT NULL,
  `ext` varchar(100) DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `ux_undo_log` (`xid`,`branch_id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```

##### 工程开始

###### Order订单工程

- 新建工程 `springcloud-alibaba-seata-order-service2001`

- pom文件如下

```xml
<!--nacos-->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>

<!--seata-->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-seata</artifactId>
    <exclusions>
        <exclusion>
            <artifactId>seata-all</artifactId>
            <groupId>io.seata</groupId>
        </exclusion>
        <exclusion>
            <artifactId>seata-spring-boot-starter</artifactId>
            <groupId>io.seata</groupId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>io.seata</groupId>
    <artifactId>seata-all</artifactId>
    <version>1.3.0</version>
</dependency>

<!--feign-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>        
<!--web-actuator-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>

<!--mysql-druid-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.20</version>
</dependency>
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.1.10</version>
</dependency>
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>cn.kate.cloud</groupId>
    <artifactId>api</artifactId>
    <version>0.1</version>
</dependency>
```

- yaml配置文件

```yaml
server:
  port: 2001

spring:
  application:
    name: seata-order-service
  cloud:
    alibaba:
      seata:
        #自定义事务组名称需要与seata-server中的对应
        tx-service-group: default
    nacos:
      discovery:
        server-addr: ip
        port: 8848
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://ip:3306/seata_order
    username: root
    password: kate

feign:
  hystrix:
    enabled: false

logging:
  level: 
  	io: 
  	  seata: info

mybatis:
  mapper-locations: classpath:mapper/*.xml
```

- file.conf

```conf
## transaction log store, only used in seata-server
store {
  ## store mode: file、db、redis
  mode = "db"

  ## file store property
  file {
    ## store location dir
    dir = "sessionStore"
    # branch session size , if exceeded first try compress lockkey, still exceeded throws exceptions
    maxBranchSessionSize = 16384
    # globe session size , if exceeded throws exceptions
    maxGlobalSessionSize = 512
    # file buffer size , if exceeded allocate new buffer
    fileWriteBufferCacheSize = 16384
    # when recover batch read size
    sessionReloadReadSize = 100
    # async, sync
    flushDiskMode = async
  }

  ## database store property
  db {
    ## the implement of javax.sql.DataSource, such as DruidDataSource(druid)/BasicDataSource(dbcp)/HikariDataSource(hikari) etc.
    datasource = "druid"
    ## mysql/oracle/postgresql/h2/oceanbase etc.
    dbType = "mysql"
    driverClassName = "com.mysql.jdbc.Driver"
    url = "jdbc:mysql://ip:3306/seata"
    user = "root"
    password = "kate"
    minConn = 5
    maxConn = 30
    globalTable = "global_table"
    branchTable = "branch_table"
    lockTable = "lock_table"
    queryLimit = 100
    maxWait = 5000
  }

  ## redis store property
  redis {
    host = "127.0.0.1"
    port = "6379"
    password = ""
    database = "0"
    minConn = 1
    maxConn = 10
    queryLimit = 100
  }

}
```

- registry.conf

```conf
registry {
  # file 、nacos 、eureka、redis、zk、consul、etcd3、sofa
  type = "nacos"

  nacos {
    application = "seata-server"
    serverAddr = "ip:8848"
    group = "SEATA_GROUP"
    namespace = ""
    cluster = "default"
    username = ""
    password = ""
  }
  eureka {
    serviceUrl = "http://localhost:8761/eureka"
    application = "default"
    weight = "1"
  }
  redis {
    serverAddr = "localhost:6379"
    db = 0
    password = ""
    cluster = "default"
    timeout = 0
  }
  zk {
    cluster = "default"
    serverAddr = "127.0.0.1:2181"
    sessionTimeout = 6000
    connectTimeout = 2000
    username = ""
    password = ""
  }
  consul {
    cluster = "default"
    serverAddr = "127.0.0.1:8500"
  }
  etcd3 {
    cluster = "default"
    serverAddr = "http://localhost:2379"
  }
  sofa {
    serverAddr = "127.0.0.1:9603"
    application = "default"
    region = "DEFAULT_ZONE"
    datacenter = "DefaultDataCenter"
    cluster = "default"
    group = "SEATA_GROUP"
    addressWaitTime = "3000"
  }
  file {
    name = "file.conf"
  }
}

config {
  # file、nacos 、apollo、zk、consul、etcd3
  type = "file"

  nacos {
    serverAddr = "127.0.0.1:8848"
    namespace = ""
    group = "SEATA_GROUP"
    username = ""
    password = ""
  }
  consul {
    serverAddr = "127.0.0.1:8500"
  }
  apollo {
    appId = "seata-server"
    apolloMeta = "http://192.168.1.204:8801"
    namespace = "application"
  }
  zk {
    serverAddr = "127.0.0.1:2181"
    sessionTimeout = 6000
    connectTimeout = 2000
    username = ""
    password = ""
  }
  etcd3 {
    serverAddr = "http://localhost:2379"
  }
  file {
    name = "file:/data/seata/file.conf"
  }
}
```

- 实体类

```java
@Data
public class Order {
    private Long id;
    private Long userId;
    private Long productId;
    private Integer count;
    private BigDecimal money;
    /**
     * 订单状态：0 创建中、1 已完结
     */
    private Integer status; 
} 
```

- Dao接口和实现

OrderDao

```java
@Mapper
public interface OrderDao {

    /**
     * 创建订单
     * @param order
     */
    void createOrder(Order order);

    /**
     * 改变订单状态
     * @param userId
     * @param status
     */
    void updateOrderStatus(@Param("userId") Integer userId, @Param("status") Integer status);
}
```

OrderDao.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- 映射文件的空间名字唯一 -->
<mapper namespace="cn.kate.dao.OrderDao">

    <resultMap id="baseMap" type="cn.kate.domain.Order">
        <id column="id" property="id"/>
        <result column="user_id" property="userId"/>
        <result column="product_id" property="productId"/>
        <result column="count" property="count"/>
        <result column="money" property="money"/>
        <result column="status" property="status"/>
    </resultMap>

    <insert id="createOrder">
        insert into t_order (id,
        user_Id,
        product_Id,
        count,
        money,
        status)
        values (null,#{userId},#{productId},#{count},#{money},0);
    </insert>
    <update id="updateOrderStatus">
        update t_order set status = 1 where user_id = #{userId} and status=#{status};
    </update>
</mapper>
```

- Service

OrderService

```java
public interface OrderService {

    /**
     * 创建订单
     * @param order
     */
    void createOrder(Order order);
}
```

OrderServiceImpl

```java
@Service
@Slf4j
public class OrderServiceImpl implements OrderService {

    @Autowired
    private OrderDao orderDao;
    @Autowired
    private StorageService storageService;
    @Autowired
    private AccountService accountService;

    @Override
    public void createOrder(Order order) {
        log.info("--------创建订单流程已开始....");
        orderDao.createOrder(order);
        log.info("----------库存模块扣除数量start");
        storageService.deductCount(order.getProductId(),order.getCount());
        log.info("-----------库存模块扣除数量end");
        log.info("-----------账户模块余额变动start");
        accountService.deductMoney(order.getUserId(),order.getMoney());
        log.info("-----------账户模块余额变动end");
        log.info("-----------改变订单状态");
        orderDao.updateOrderStatus(order.getUserId(),1);
        log.info("------------订单生成流程结束....");
    }

}
```

StorageService

```java
@FeignClient(name = "seata-storage-service")
public interface StorageService {

    /**
     * 扣除库存
     * @param productId
     * @param count
     * @return
     */
    @PostMapping("/storage/deductCount")
    RestResult deductCount(@RequestParam("productId")Long productId,@RequestParam("count")Integer count);
}
```

AccountService

```java
@FeignClient(name = "seata-account-service")
public interface AccountService {

    /**
     * 扣除账户余额
     * @param userId
     * @param money
     * @return
     */
    @PostMapping("/account/deductMoney")
    RestResult deductMoney(@RequestParam("userId")Long userId, @RequestParam("money") BigDecimal money);
}
```

- Controller

```java
@RestController
@RequestMapping("/order")
public class OrderController {

    @Autowired
    private OrderService orderService;

    @GetMapping("/createOrder")
    public RestResult createOrder(Order order){
        orderService.createOrder(order);
        return new RestResult(HttpStatus.HTTP_OK,"订单创建成功！");
    }
}
```

- Config配置

DataSourceConfig

```java
package cn.kate.config;

import com.alibaba.druid.pool.DruidDataSource;
import io.seata.rm.datasource.DataSourceProxy;
import org.apache.ibatis.session.SqlSessionFactory;
import org.mybatis.spring.SqlSessionFactoryBean;
import org.mybatis.spring.transaction.SpringManagedTransactionFactory;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.io.support.PathMatchingResourcePatternResolver;

import javax.sql.DataSource;

/**
 * @Author: cltong
 * @Date: 2020/12/13 18:35
 */
@Configuration
public class DataSourceProxyConfig {
    @Value("${mybatis.mapperLocations}")
    private String mapperLocations;

    @Bean
    @ConfigurationProperties(prefix = "spring.datasource")
    public DataSource druidDataSource() {
        return new DruidDataSource();
    }

    @Bean
    public DataSourceProxy dataSourceProxy(DataSource dataSource) {
        return new DataSourceProxy(dataSource);
    }

    @Bean
    public SqlSessionFactory sqlSessionFactoryBean(DataSourceProxy dataSourceProxy) throws Exception {
        SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
        sqlSessionFactoryBean.setDataSource(dataSourceProxy);
        sqlSessionFactoryBean.setMapperLocations(new PathMatchingResourcePatternResolver().getResources(mapperLocations));
        sqlSessionFactoryBean.setTransactionFactory(new SpringManagedTransactionFactory());
        return sqlSessionFactoryBean.getObject();
    }
}
```

MybatisConfig

```java
@Configuration
@MapperScan("cn.kate.dao")
public class MybatisConfig {}
```

- 主启动类

```java
@SpringBootApplication(exclude = DataSourceAutoConfiguration.class) // 剔除原程序装载的数据源
@EnableFeignClients
@EnableDiscoveryClient
public class MainApplicationWithSeataOrderService2001 {
    public static void main(String[] args) {
        SpringApplication.run(MainApplicationWithSeataOrderService2001.class);
    }
}
```

- 启动测试

###### Storage库存工程

- 新建工程 `springcloud-alibaba-seata-storage-service2002`
- pom文件如上，此处不再粘贴
- yaml文件如上，变动如下

```yaml
server:
  port: 2002

spring:
  application:
    name: seata-storage-service

  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://ip:3306/seata_storage
    username: root
    password: kate 
    
# 去掉 feign 配置    
```

- file.conf如上
- register.conf如上
- domain实体类如下

```java
@Data
public class Storage {
    private Long id;
    /**
     * 产品id
     */
    private Long productId;
    /**
     * 总库存
     */
    private Integer total;
    /**
     * 已用库存
     */
    private Integer used;
    /**
     * 剩余库存
     */
    private Integer residue;
}
```

- Dao

```java
@Mapper
public interface StorageDao {

    /**
     * 扣减库存数量
     * @param productId
     * @param count
     * @return
     */
    int deductCount(@Param("productId")Long productId,@Param("count") Integer count);

    /**
     * 查询剩余库存
     * @param productId
     * @return
     */
    Integer selectStorage(@Param("productId")Long productId);
}
```

StorageDao.xml

```xml
<mapper namespace="cn.kate.dao.StorageDao">

    <resultMap id="baseMap" type="cn.kate.domain.Storage">
        <id column="id" property="id"/>
        <result column="product_id" property="productId"/>
        <result column="total" property="total"/>
        <result column="used" property="used"/>
        <result column="residue" property="residue"/>
    </resultMap>

    <update id="deductCount">
        update t_storage
        <trim prefix="set" suffixOverrides=",">
            <if test="count != null">used=used+#{count},</if>
            <if test="count != null">residue=residue-#{count}</if>
        </trim>
        where product_id = #{productId};
    </update>

    <select id="selectStorage" resultType="java.lang.Integer">
        select residue from t_storage where product_id = #{productId};
    </select>
</mapper>
```

- Service

```java
public interface StorageService {

    /**
     * 扣减库存数量
     * @param productId
     * @param count
     * @return
     */
    int deductCount(Long productId, Integer count);

    /**
     * 查询剩余库存
     * @param productId
     * @return
     */
    Integer selectStorage(Long productId);
}
```

StorageServiceImpl

```java
@Service
@Slf4j
public class StorageServiceImpl implements StorageService {
    @Autowired
    private StorageDao storageDao;

    @Override
    public int deductCount(Long productId, Integer count) {
        log.info("------------扣减库存开始...");
        int i = storageDao.deductCount(productId, count);
        log.info("------------扣减库存结束...");
        return i;
    }

    @Override
    public Integer selectStorage(Long productId) {
        return storageDao.selectStorage(productId);
    }
}
```

- Controller

```java
@RestController
@RequestMapping("/storage")
public class StorageController {

    @Autowired
    private StorageService storageService;

    @PostMapping("/deductCount")
    public RestResult deductCount(@RequestParam("productId")Long productId, @RequestParam("count")Integer count){
        // 先调用库存剩余数量，如果小于等于0，则返回 0
        Integer integer = storageService.selectStorage(productId);
        if(integer!=null && integer != 0){
            int i = storageService.deductCount(productId, count);
            if(i>0){
                Integer residue = storageService.selectStorage(productId);
                return new RestResult(200,"库存模块数量减少！！！当前剩余库存为：" + residue);
            }
        }
        return new RestResult(400,"错误的请求！！！");
    }
}
```

- DataSourceProxyConfig如上
- MybatisConfig如上

- 主启动类如上
- 启动测试，保证自己通过

###### Account账户工程

- 新建工程 `springcloud-alibaba-seata-account-service2003`
- pom文件如上
- yaml文件如上，变动如下

```yaml
server:
  port: 2003

spring:
  application:
    name: seata-account-service

  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://ip:3306/seata_account
    username: root
    password: kate 

# 去掉 feign 配置  
```

- file.conf如上
- register.conf如上
- domain实体类

```java
@Data
public class Account {
    private Long id;
    /**
     * 用户id
     */
    private Long userId;
    /**
     * 总额度
     */
    private BigDecimal total;
    /**
     * 已用额度
     */
    private BigDecimal used;
    /**
     * 剩余额度
     */
    private BigDecimal residue;
} 
```

- Dao

dao改动不大，可参照上述 `StorageDao`，mapper.xml也可参照上述略微修改

- Service

参照上述修改

- Controller

参照上述修改

- Config如上述
- 主启动类如上述
- 启动测试，保证自己通过

##### 工程改造

当所有的工程结束后，全部启动，然后来测试访问订单工程，看是否能够完成下订单的一套逻辑

```http
http://localhost:2001/order/createOrder?userId=1&productId=1&count=10&money=200
```

只需要使用一个注解 `@GlobalTransactional` 加到你需要处理事务的方法上，即可完成对全局事务的管理和回滚

```java
@GlobalTransactional(rollbackFor = {Exception.class},name = "seata-create-order")
public void createOrder(Order order) {}
```

#### 原理

![image-20201214105945620](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202012/14/105946-568692.png)

详情课件官网：[Seata原理](http://seata.io/zh-cn/docs/overview/what-is-seata.html)








# 九、Springboot与缓存

## 1、Springboot缓存概念

**a、缓存概念JSR107**

Java Caching定义了5个核心接口，分别是**CachingProvider**, **CacheManager**, **Cache**, **Entry** 和 **Expiry**。

•**CachingProvider**定义了创建、配置、获取、管理和控制多个**CacheManager**。一个应用可以在运行期访问多个CachingProvider。

•**CacheManager**定义了创建、配置、获取、管理和控制多个唯一命名的**Cache**，这些Cache存在于CacheManager的上下文中。一个CacheManager仅被一个CachingProvider所拥有。

•**Cache**是一个类似Map的数据结构并临时存储以Key为索引的值。一个Cache仅被一个CacheManager所拥有。

•**Entry**是一个存储在Cache中的key-value对。

•**Expiry** 每一个存储在Cache中的条目有一个定义的有效期。一旦超过这个时间，条目为过期的状态。一旦过期，条目将不可访问、更新和删除。缓存有效期可以通过ExpiryPolicy设置。

**b、缓存注解**

| Cache          | 缓存接口，定义缓存操作。实现有：RedisCache***、***EhCacheCache**、**ConcurrentMapCache等 |
| -------------- | ------------------------------------------------------------ |
| CacheManager   | 缓存管理器，管理各种缓存（Cache）组件                        |
| @Cacheable     | 主要针对方法配置，能够根据方法的请求参数对其结果进行缓存     |
| @CacheEvict    | 清空缓存                                                     |
| @CachePut      | 保证方法被调用，又希望结果被缓存。                           |
| @EnableCaching | 开启基于注解的缓存                                           |
| keyGenerator   | 缓存数据时key生成策略                                        |

## 2、使用SpringBoot缓存

**a、导入依赖**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
版本由springboot自动仲裁
```

**b、使用@EnableCaching开启缓存标注到主程序上或者配置类上**

```java
@MapperScan("cn.kate.dao")
@SpringBootApplication
@EnableCaching
public class SpringBootApplication1 {}
    
```

### @Cacheable

使用**@Cacheable来标注方法，使用 @Cacheable注解后, 调用方法时会先从缓存中查找是否有对应的数据, 如果没有才会去执行目标方法去数据库查找, 并将查找的结果放入缓存中.如果在缓存中查询到数据则不会访问目标方法，**第一次查询的时候生成的key的策略默认为 KeyGenerator生成** **查询时存储的地方为ConcurrentMap**

**@Cacheable 一般作用到查询方法上**

```java
/**
     *
     * @Cacheable 表示结果会被缓存
     * @param id
     * @return
     */
@Override
@Cacheable(value = "emp")
public Employee selectByPrimaryKey(Integer id) {
    System.out.println("来到了这里");
    return employeeDao.selectByPrimaryKey(id);
}
```

**@Cacheable属性**

| 属性          | 作用                                                         |
| ------------- | ------------------------------------------------------------ |
| cacheNames    | 指定一个缓存组件的名字，数组格式                             |
| key           | 指定缓存的key，如果不写，默认为方法参数的值 value为方法的返回值 ，支持SpEL表达式写法 |
| keyGenerator  | 自定义keyGenerator                                           |
| cacheManager  | 缓存管理器，可以指定使用哪个缓存管理                         |
| cacheResolver | 缓存解析器，与cacheManager二选一使用                         |
| condition     | 条件判断 ，如果条件为true，则执行缓存                        |
| unless        | 如果条件为true，则不执行缓存 **注意，此处unless的优先级大于condition** |
| sync          | 是否使用异步缓存                                             |

**自定义Key生成器**

```java
@Bean(name = "myKeyGenerator")
public KeyGenerator keyGenerator(){
    return new KeyGenerator() {
        @Override
        public Object generate(Object o, Method method, Object... objects) {
            List<Object> objects1 = Arrays.asList(objects);
            return "自定义key生成规则 " + objects1.get(0) + " 并返回";
        }
    };
}

引用
@Cacheable(value = "emp",keyGenerator = "myKeyGenerator 此处为容器的组件的name属性")
```

### @CachEvict

```java
@Override
@CacheEvict(value = "emp",key = "#id")
public void deleteEmp(Integer id){
    System.out.println(id);
}
```

### @CachePut 

达到了同步更新缓存和数据库中的数据,每次都会调用方法

测试

​	1、查询指定员工，让结果存入缓存

​	2、此时更新员工，然后再查询数据

​		此时值并没有发生改变，原因为：**@Cacheable的执行时机为方法调用前，而key的生成策略导致和更新缓存的key不一致，@Cacheput所修改后的值就无法覆盖掉原值，所以我们应该在@Cacheput上和@Cacheable上统一key**

```java
@Override
@CachePut(value = "emp",key = "#result.id")
public Employee updateByPrimaryKey(Employee record) {
    employeeDao.updateByPrimaryKey(record);
    return record;
}
```

**注意：如果他们被实现类标注，那么该方法必须存在一个接口中，并且由Controller直接调用缓存才会生效**

**@Caching**

定义复杂的缓存规则

```java
@Override
@Caching(
    cacheable = {
        @Cacheable(value = "emp",key = "#lastname")
    },
    put = {
        @CachePut(value = "emp",key = "#result.id")
    }
)
public Employee selectByLastName(String lastname){
    return employeeDao.selectByLastName(lastname);
}
```

**如果定义了@Caching注解，那么此时这个方法中的其他注解(被包含在caching内的，并且指定同一个缓存组名字时，那个缓存注解可能不生效)**

**@CacheConfig**

抽取缓存的公共配置

```java
@CacheConfig(cacheNames = "emp",keyGenerator = "",cacheManager = "",cacheResolver = "")
标注到service类上
```

**支持的SpEl**

| 名字            | 位置               | 描述                                                         | 示例                 |
| --------------- | ------------------ | ------------------------------------------------------------ | -------------------- |
| methodName      | root object        | 当前被调用的方法名                                           | #root.methodName     |
| method          | root object        | 当前被调用的方法                                             | #root.method.name    |
| target          | root object        | 当前被调用的目标对象                                         | #root.target         |
| targetClass     | root object        | 当前被调用的目标对象类                                       | #root.targetClass    |
| args            | root object        | 当前被调用的方法的参数列表                                   | #root.args[0]        |
| caches          | root object        | 当前方法调用使用的缓存列表（如@Cacheable(value={"cache1",   "cache2"})），则有两个cache | #root.caches[0].name |
| *argument name* | evaluation context | 方法参数的名字. 可以直接 #参数名 ，也可以使用 #p0或#a0 的形式，0代表参数的索引； | #iban 、 #a0 、  #p0 |
| result          | evaluation context | 方法执行后的返回值（仅当方法执行之后的判断有效，如‘unless’，’cache put’的表达式 ’cache evict’的表达式beforeInvocation=false） | #result              |

## 3、SpringBoot整合Redis

### **1、导入依赖或者使用快速工程创建**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>

<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
</dependency>
```

### **2、序列化方式**

​	**默认使用的是JDK的序列化方式，springboot提供了Jackson2的序列化方式，当然我们也可以使用我们自己的序列化方式(FastJson)**

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.62</version>
</dependency>
```

### **3、在配置文件中配置Redis的链接属性和信息**

```properties
#配置redis链接的ip
spring.redis.host=39.98.204.116
#prot 默认 6379
spring.redis.port=6379
#密码
spring.redis.password=kate
```

### **4、configuration配置文件 自定义序列化方式**

```java
@Bean
public RedisTemplate redisTemplate(RedisConnectionFactory redisConnectionFactory){
    RedisTemplate redisTemplate = new RedisTemplate();
    redisTemplate.setKeySerializer(new StringRedisSerializer());
    redisTemplate.setValueSerializer(new FastJsonRedisSerializer<>(Employee.class));
    //        redisTemplate.setDefaultSerializer(new GenericJackson2JsonRedisSerializer());
    redisTemplate.setConnectionFactory(redisConnectionFactory);
    return redisTemplate;
}
```

### **5、测试注入**

```java
@Resource
private RedisTemplate redisTemplate;

@Autowired
private StringRedisTemplate stringRedisTemplate;

@Resource(name = "redisTemplate") //这个地方必须用@Resource注解来指定名字注入，如果使用@Autowired和@Qulifier就会报错，因为容器中没有这个类型的bean
private ListOperations<String,Employee> list;
```

## 4、自定义缓存规则

**引入Redis后，更改序列化方式通过Configuration配置类 springboot2.x**

```java
@Bean
public CacheManager cacheManager(RedisConnectionFactory factory){
    RedisCacheConfiguration cacheConfiguration = RedisCacheConfiguration.defaultCacheConfig()
        .entryTtl(Duration.ofDays(1))
        .disableCachingNullValues()
        .serializeValuesWith(RedisSerializationContext.SerializationPair.fromSerializer(new GenericJackson2JsonRedisSerializer()));
    return RedisCacheManager.builder(factory).cacheDefaults(cacheConfiguration).build();
}
```

**通过缓存存储的数据的key会使用缓存注解的cachename做为前缀**

**不管是手动缓存还是通过注解缓存都使用new GenericJackson2JsonRedisSerializer()来序列化值，通过注解缓存时，会将对象序列化为string，值为json保存 过期时间为0.98天左右，手动缓存时，值为hash，过期时间为-1，且两者的key的存在方式不同**

# 十、Springboot与消息

## 1、消息队列概述

**1.大多应用中，可通过消息服务中间件来提升系统异步通信、扩展解耦能力**

**2.消息服务中两个重要概念：**

​       **消息代理（message broker）和目的地（destination）**

**当消息发送者发送消息以后，将由消息代理接管，消息代理保证消息传递到指定目的地。**

**3.消息队列主要有两种形式的目的地**

​	**1.队列（queue）：点对点消息通信（point-to-point）**

​	**2.主题（topic）：发布（publish）/订阅（subscribe）消息通信**

**4.点对点式：**

**–消息发送者发送消息，消息代理将其放入一个队列中，消息接收者从队列中获取消息内容，消息读取后被移出队列**

**–消息只有唯一的发送者和接受者，但并不是说只能有一个接收者**

**–**

**5.发布订阅式：**

**–发送者（发布者）发送消息到主题，多个接收者（订阅者）监听（订阅）这个主题，那么就会在消息到达时同时收到消息**

**–**

**6.JMS（Java Message Service）JAVA消息服务：**

**–基于JVM消息代理的规范。ActiveMQ、HornetMQ是JMS实现**

**–**

**7.AMQP（Advanced Message Queuing Protocol）**

**–高级消息队列协议，也是一个消息代理的规范，兼容JMS**

**–RabbitMQ是AMQP的实现**



**异步处理**

**应用解耦**

**流量削峰**

**JMS和AMQP的对比**

|                  | **JMS**                                                      | **AMQP**                                                     |
| :--------------: | ------------------------------------------------------------ | ------------------------------------------------------------ |
|     **定义**     | **Java   api**                                               | **网络线级协议**                                             |
|    **跨语言**    | **否**                                                       | **是**                                                       |
|    **跨平台**    | **否**                                                       | **是**                                                       |
|    **Model**     | **提供两种消息模型：   （1）、Peer-2-Peer   （2）、Pub/sub** | **提供了五种消息模型：   （1）、direct   exchange   （2）、fanout   exchange   （3）、topic   change   （4）、headers   exchange   （5）、system   exchange   本质来讲，后四种和JMS的pub/sub模型没有太大差别，仅是在路由机制上做了更详细的划分；** |
| **支持消息类型** | **多种消息类型：   TextMessage   MapMessage   BytesMessage   StreamMessage   ObjectMessage   Message   （只有消息头和属性）** | **byte[]   当实际应用时，有复杂的消息，可以将消息序列化后发送。** |
|   **综合评价**   | **JMS   定义了JAVA   API层面的标准；在java体系中，多个client均可以通过JMS进行交互，不需要应用修改代码，但是其对跨平台的支持较差；** | **AMQP定义了wire-level层的协议标准；天然具有跨平台、跨语言特性。** |

**Spring与Springboot的支持**

**8.Spring支持**

**–spring-jms提供了对JMS的支持**

**–spring-rabbit提供了对AMQP的支持**

**–需要ConnectionFactory的实现来连接消息代理**

**–提供JmsTemplate、RabbitTemplate来发送消息**

**–@JmsListener（JMS）、@RabbitListener（AMQP）注解在方法上监听消息代理发布的消息**

**–@EnableJms、@EnableRabbit开启支持**

**–**

**9.Spring Boot自动配置**

**–JmsAutoConfiguration**

**–RabbitAutoConfiguration**

## 2、RabbitMQ简介

**RabbitMQ是一个由erlang开发的AMQP(Advanved Message Queue Protocol)的开源实现。**

**核心概念**

**Message**

**消息，消息是不具名的，它由消息头和消息体组成。消息体是不透明的，而消息头则由一系列的可选属性组成，这些属性包括routing-key（路由键）、priority（相对于其他消息的优先权）、delivery-mode（指出该消息可能需要持久性存储）等。**



**Publisher**

**消息的生产者，也是一个向交换器发布消息的客户端应用程序。**



**Exchange**

**交换器，用来接收生产者发送的消息并将这些消息路由给服务器中的队列。**

**Exchange有种类型：默认，, topic, headersExchange**



**Queue**

**消息队列，用来保存消息直到发送给消费者。它是消息的容器，也是消息的终点。一个消息可投入一个或多个队列。消息一直在队列里面，等待消费者连接到这个队列将其取走。**



**Binding**

**绑定，用于消息队列和交换器之间的关联。一个绑定就是基于路由键将交换器和消息队列连接起来的路由规则，所以可以将交换器理解成一个由绑定构成的路由表。**

**Exchange 和Queue的绑定可以是多对多的关系。**



**Connection**

**网络连接，比如一个TCP连接。**



**Channel**

**信道，多路复用连接中的一条独立的双向数据流通道。信道是建立在真实的TCP连接内的虚拟连接，AMQP 命令都是通过信道发出去的，不管是发布消息、订阅队列还是接收消息，这些动作都是通过信道完成。因为对于操作系统来说建立和销毁 TCP 都是非常昂贵的开销，所以引入了信道的概念，以复用一条 TCP 连接。**

**Consumer**

**消息的消费者，表示一个从消息队列中取得消息的客户端应用程序。**



**Virtual Host**

**虚拟主机，表示一批交换器、消息队列和相关对象。虚拟主机是共享相同的身份认证和加密环境的独立服务器域。每个 vhost 本质上就是一个 mini 版的 RabbitMQ 服务器，拥有自己的队列、交换器、绑定和权限机制。vhost 是 AMQP 概念的基础，必须在连接时指定，RabbitMQ 默认的 vhost 是 / 。**



**Broker**

**表示消息队列服务器实体**

### **1、Exchange的三种类型**

**Exchange分发消息时根据类型的不同分发策略有区别，目前共四种类型：direct、fanout、topic、headers 。headers 匹配 AMQP 消息的 header 而不是路由键， headers 交换器和 direct 交换器完全一致，但性能差很多，目前几乎用不到了，所以直接看另外三种类型：**

#### **a、Direct（点对点）**

​	**消息中的路由键（routing key）如果和 Binding 中的 binding key 一致，交换器就将消息发到对应的队列中。路由键与队列名完全匹配，如果一个队列绑定到交换机要求路由键为“dog”，则只转发 routing key 标记为“dog”的消息，不会转发“dog.puppy”，也不会转发“dog.guard”等等。它是完全匹配、单播的模式。**

#### **b、fanout （广播模式/订阅模式）**

​	**每个发到 fanout 类型交换器的消息都会分到所有绑定的队列上去。fanout 交换器不处理路由键，只是简单的将队列绑定到交换器上，每个发送到交换器的消息都会被转发到与该交换器绑定的所有队列上。很像子网广播，每台子网内的主机都获得了一份复制的消息。fanout 类型转发消息是最快的。**

#### **c、topic（按规则匹配）**

​	**topic 交换器通过模式匹配分配消息的路由键属性，将路由键和某个模式进行匹配，此时队列需要绑定到一个模式上。它将路由键和绑定键的字符串切分成单词，这些单词之间用点隔开。它同样也会识别两个通配符：符号“#”和符号 * 。#匹配0个或多个单词，* 匹配一个单词。**

### **3、RabbitMQ安装(docker)**

**1、拉取镜像 带有management的版本**

```shell
docker pull rabbitmq:3.7.26-management
```

**2、运行**

**开放两个端口，5672为默认，15672为management访问端口**

非挂载配置文件的启动方式如下

```shell
docker run -d -p 5672:5672 -p 15672:15672 --name rabbitmqtcl 镜像id
```

*此处应该注意：*

默认guest用户是只能访问本机的RabbitMQ,我们在云服务器中登录时，Springboot可能会跑出连接异常，因此我们使用docker挂载配置文件的方式来消除异常

配置文件如下

在data目录下新建 *rabbitmq.config（名字一定要是这个）*

```config
[{rabbit, [{loopback_users, []}]}].
```

启动方式也变成了

```shell
docker run -d -p 5672:5672 -p 15672:15672 -v /data/rabbitmq/rabbitmq.config:/etc/rabbitmq/rabbitmq.config --name rabbitmqtcl 镜像id
```

此方式可能行不通，我们就需要自己创建自定义的rabbitMQ用户来解决

**1、添加用户**

---

![image-20201118194347463](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/18/194347-293302.png)

---

**2、配置用户拥有Virtual Host的访问权限**

---

![image-20201118194450823](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/18/194451-350221.png)

---

![image-20201118194604719](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/18/194604-867214.png)

---

**3、做完如上配置后，在详情页点击最下方的 Update User 即可**

## 4、Springboot整合RabbitMQ

#### **1、导入依赖**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```

#### **2、properties配置文件**

```properties
spring.rabbitmq.host=39.98.204.116
spring.rabbitmq.port=5672
spring.rabbitmq.username=guest
spring.rabbitmq.password=guest
```

#### **3、测试**

```java
@Autowired
private RabbitTemplate rabbitTemplate;


@Test
public void send(){
    //        这个方法只会发送Message(里面包含了所有信息 消息头，路由键等)类型的消息，如果我们需要发送对象，我们还要讲对象转换成 消息体
    //        rabbitTemplate.send(Message message);

    //        这个方法可以把我们的对象转化成Message后发送，我们需要指定 交换器和路由键
    //        rabbitTemplate.convertAndSend(String exchange,String RoutingKey,Object obj);

    Map<String,Object> map = new HashMap<>();
    map.put("msg","aaa");
    map.put("data", Arrays.asList("1",2,false));
    rabbitTemplate.convertAndSend("exchange.direct","kate.news",map);
}

@Test
public void receive(){
    //        rabbitTemplate.receiveAndConvert(String queueName);
    Object o = rabbitTemplate.receiveAndConvert("kate.news");
    System.out.println(o.getClass().getSimpleName());
    System.out.println(o.toString());
}
```

#### **4、更改序列化方式**

```java
@Bean
public MessageConverter messageConverter(){
    return new Jackson2JsonMessageConverter();
}
```

#### **5、@RabbitListener + @EnableRabbit**

**监听消息队列的消息，一有消息马上报告并捕获**

```java
@RabbitListener(queues = "kate")
public void receive(Book book){
    System.out.println("book = "+book);
}

@RabbitListener(queues = "kate.news")
public void receive2(Message message){
    System.out.println(message.getBody());
    System.out.println(message.getMessageProperties());
}

主程序
@EnableRabbit
@SpringBootApplication
public class SpringBootApplicationMq {
```

#### **6、AmqpAdmin组件管理**

**AmqpAdmin允许我们可以在程序中通过编码的方式来创建Exchange 和 Queue 并设置绑定规则**

```java
//创建交换器 选择类型
TopicExchange topicExchange = new TopicExchange("amqp.topic");
amqpAdmin.declareExchange(topicExchange);

//创建队列 是否为持久化
String s = amqpAdmin.declareQueue(new Queue("amqp.queue", true));
System.out.println(s);

//创建队列与交换器的绑定规则
/**
     * 
     * @param destination 代表你想要绑定到的目的地 
     * @param destinationType 代表目的地的类型 可以是一个队列(queue)或者交换器(exchange)
     * @param exchange 跟那一个交换器绑定
     * @param routingKey 路由键是什么
     * @param arguments 参数是什么
     */
public Binding(String destination, Binding.DestinationType destinationType, String exchange, String routingKey, Map<String, Object> arguments) {
    this.destination = destination;
    this.destinationType = destinationType;
    this.exchange = exchange;
    this.routingKey = routingKey;
    this.arguments = arguments;
}
```

# 十一、Springboot与检索

## 1、ElasticSearch简介

**1、检索**

​	**我们的应用经常需要添加检索功能，开源的 [ElasticSearch](https://www.elastic.co/) 是目前全文搜索引擎的首选。他可以快速的存储、搜索和分析海量数据。Spring Boot通过整合Spring Data ElasticSearch为我们提供了非常便捷的检索功能支持；**

**Elasticsearch是一个分布式搜索服务，提供Restful API，底层基于Lucene，采用多shard（分片）的方式保证数据安全，并且提供自动resharding的功能，github等大型的站点也是采用了ElasticSearch作为其搜索服务**

**2、elasticsearch概念**

​	**以 员工文档 的形式存储为例：一个文档代表一个员工数据。存储数据到 ElasticSearch 的行为叫做 索引 ，但在索引一个文档之前，需要确定将文档存储在哪里。**

**一个 ElasticSearch 集群可以 包含多个 索引 ，相应的每个索引可以包含多个 类型 。 这些不同的类型存储着多个 文档 ，每个文档又有 多个 属性 。**

**类似关系：**

**–索引-数据库**

**–类型-表**

**–文档-表中的记录**

**–属性-列**

## 2、ElasticSearch安装(docker)

**1、拉取镜像**

```shell
docker pull elasticsearch:7.7.0
```

**2、运行**

**注意：elasticsearch初始化默认开启2G的堆内存空间，我们测试期间用不了这么大的内存，所以限制一下堆内存的使用**

**开放两个端口 默认为9200,9300为分布式的开放端口**

```shell
docker run -d -e ES_JAVA_OPTS="-Xms256m -Xmx256m" -p 9200:9200 -p 9300:9300 --name elasticsearchtcl elasticsearch:7.7.0
```

## 3、Springboot整合ElasticSearch

### **1、使用Rest来操作ES**

**HTTP协议，支持的客户端有Jest client和Rest client**

**Native Elasticsearch binary协议，也就是Transport client和Node client  他们都样都被弃用**

**Jest client和Rest client区别      Jest client非官方支持，在ES5.0之前官方提供的客户端只有Transport client、Node client。在5.0之后官方发布Rest client，并大力推荐**

**a、引入依赖**

```java
<!-- https://mvnrepository.com/artifact/io.searchbox/jest -->
<dependency>
    <groupId>io.searchbox</groupId>
    <artifactId>jest</artifactId>
    <version>5.3.3</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.elasticsearch.client/rest -->
<dependency>
    <groupId>org.elasticsearch.client</groupId>
    <artifactId>rest</artifactId>
    <version>5.5.3</version>
</dependency>
```

**b、properties配置文件**

```properties
spring.elasticsearch.rest.uris=http://39.98.204.116:9200
```

**c、添加数据**

```java
@Qualifier("elasticsearchRestClient")
@Autowired
private RestClient restClient;
@Test
public void send() throws Exception{
    Request request = new Request("PUT","/book/doc/1");
    Book book = new Book();
    book.setName("演义");
    book.setPrice(22d);
    book.setId(1);
    Gson gson = new Gson();
    String s = gson.toJson(book);
    HttpEntity entity = new StringEntity(s, ContentType.APPLICATION_JSON);
    request.setEntity(entity);

    Response response = restClient.performRequest(request);
    String s1 = EntityUtils.toString(response.getEntity());
    System.out.println(s1);

}
```

**d、查询数据**

```java
@Test
public void search() throws Exception{
    Request request = new Request("GET", "/book/doc/_search");
    String jsonString = "{\"query\":{\"match\":{\"name\":\"演义\"}}}";
    HttpEntity entity = new NStringEntity(jsonString, ContentType.APPLICATION_JSON);
    request.setEntity(entity);
    Response response = restClient.performRequest(request);
    String result = EntityUtils.toString(response.getEntity());
    System.out.println(result);
}
```

# 十二、Springboot与任务

## 1、异步任务

```java
@Async //表示该方法是一个异步方法
public void async(){
    try {
        Thread.sleep(3000);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
    System.out.println("任务执行....");
}
```

开启spring支持异步

```java
@SpringBootApplication
@EnableAsync //开启spring支持异步任务
public class SpringBootAsyncApplication {}
```

## 2、定时任务

springboot中有自己的一套跟定时任务相关的注解来完成定时任务

```java
//开启定时任务
@SpringBootApplication
@EnableAsync //开启spring支持异步任务
@EnableScheduling//开启定时任务
public class SpringBootAsyncApplication {}
```

使用@Scheduled来标注定时任务，属性cron支持相对应的表达式

```java
/**
     * 秒 分 时 日 月 周
     *@Scheduled(cron = "0 * * * * 1-7") 代表不管几月几日的 周一到周日 的不管几时几分的0秒就会触发
     * @Scheduled(cron = "0 0 0 * * *") 代表每一月，不管星期几不管那一天的0:00:00就会触发
     * */
@Scheduled(cron = "0 * * * * 1-7")
public void job(){
    asyncService.job();
}
```

cron支持的表达式如下

| **字段** | **允许值**             | **允许的特殊字符** |
| -------- | ---------------------- | ------------------ |
| 秒       | 0-59                   | , -  * /           |
| 分       | 0-59                   | , -  * /           |
| 小时     | 0-23                   | , -  * /           |
| 日期     | 1-31                   | , -  * ? / L W C   |
| 月份     | 1-12                   | , -  * /           |
| 星期     | 0-7或SUN-SAT  0,7是SUN | , -  * ? / L C #   |

表达式中特殊字符所代表的含义

| **特殊字符** | **代表含义**               |
| ------------ | -------------------------- |
| ,            | 枚举                       |
| -            | 区间                       |
| *            | 任意                       |
| /            | 步长                       |
| ?            | 日/星期冲突匹配            |
| L            | 最后                       |
| W            | 工作日                     |
| C            | 和calendar联系后计算过的值 |
| #            | 星期，4#2，第2个星期四     |

## 3、发送邮件

springboot中集成了javaMailSender作为发送邮件的对象，并且帮助我们完成了某些自动配置，现在来写一个简单实例

### 1、首先你要这样做 引入相关场景启动器

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```

### 2、配置properties配置文件(Springboot2.x)

```properties
#你的邮箱用户名
spring.mail.username=2895110093@qq.com
#如果使用qq邮箱发送，就应该有他的校验码，需要发送短信后接受到，如果使用其他邮箱，就直接写密码
spring.mail.password=pztakgikddwnddha
#你邮箱所在的服务器
spring.mail.host=smtp.qq.com
#springboot2.x的配置如下
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
spring.mail.properties.mail.smtp.starttls.required=true
```

### 3、发送邮件的业务逻辑

```java
//自动注入JavaMailSender对象实现
@Autowired
private JavaMailSenderImpl mailSender;

@Test
public void t1(){
    SimpleMailMessage simpleMailMessage = new SimpleMailMessage();
    //设置邮件主题
    simpleMailMessage.setSubject("这是一个邮件主题");
    //设置邮件内容
    simpleMailMessage.setText("这是一段邮件的内容");

	//需要发送到哪里
    simpleMailMessage.setTo("2895110093@qq.com");
    //这个邮件来自哪里
    simpleMailMessage.setFrom("2895110093@qq.com");
    //发送
    mailSender.send(simpleMailMessage);
}
```

2、发送一个稍稍复杂的邮件再来试一下

```java
@Test
public void t2() throws Exception{
    //通过mailSender来创建一个MimeMessage对象
    MimeMessage mimeMessage = mailSender.createMimeMessage();
    //这里的两个参数第一个是MimeMessage对象，第二个为Boolean multipart表示是否上传文件
    MimeMessageHelper helper = new MimeMessageHelper(mimeMessage, true);

	//设置邮件主题
    helper.setSubject("这是一个携带html和文件的邮件主题");
    //设置邮件内容，设置为true代表启用html
    helper.setText("<p style = 'color:red'>这是一段html代码</p>",true);
	
    //发送静态文件所调用的方法，第一个参数为文件的名字，要指定后缀，第二个参数可以是一个流，也可以是一个文件
    helper.addAttachment("a.jpg",new File("C:\\Users\\Machenike\\Pictures\\Saved Pictures\\b.jpg"));
    //需要发送到哪里
    helper.setTo("2895110093@qq.com");
    //这个邮件来自哪里
    helper.setFrom("2895110093@qq.com");
    mailSender.send(mimeMessage);
}
```

# 十三、Springboot与安全

## 1、什么是Spring Security

​	市面上常见的安全框架有shiro和Spring Security。Spring Security是针对Spring项目的安全框架，也是Spring Boot底层安全模块默认的技术选型。他可以实现强大的web安全控制。对于安全控制，我们仅需引入spring-boot-starter-security模块，进行少量的配置，即可实现强大的安全管理。

**WebSecurityConfigurerAdapter：自定义Security策略**

**AuthenticationManagerBuilder：自定义认证策略**

**@EnableWebSecurity：开启WebSecurity模式**

## 2、什么是认证和授权

应用程序的两个主要区域是“认证”和“授权”（或者访问控制）。这两个主要区域是Spring Security 的两个目标。

**“认证”（Authentication），是建立一个他声明的主体的过程（一个“主体”一般是指用户，设备或一些可以在你的应用程序中执行动作的其他系统）。**

**“授权”（Authorization）指确定一个主体是否允许在你的应用程序执行一个动作的过程。为了抵达需要授权的店，主体的身份已经有认证过程建立。**

这个概念是通用的而不只在Spring Security中。

## 3、一个Spring Security模型示例(如何使用)

### a、导入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

### b、编写一个配置类

首先应该编写一个自定义PasswordEncoder类来等下认证的时候用来校验密码

```java
/**
 * @author kate
 * @Date 2020/5/19 21:16
 * PasswordEncoder组件为spring-security5.x提供的基于密码的保护功能
 * 在使用认证的时候用SpringSecurityPasswordEncoder去校验密码
 */
@Configuration
public class SpringSecurityPasswordEncoder implements PasswordEncoder {


    @Override
    public String encode(CharSequence charSequence) {
        return charSequence.toString();
    }

    @Override
    public boolean matches(CharSequence charSequence, String s) {
        return s.equals(charSequence.toString());
    }
}
```

认证和授权的配置类

```java
@Configuration
@EnableWebSecurity//启用Spring-security
public class SpringSecurityConfig extends WebSecurityConfigurerAdapter {


    /**
     * //定制授权规则
     * @param http
     * @throws Exception
     */
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        //        super.configure(http);
        //定制授权规则
        http.authorizeRequests().antMatchers("/").permitAll()
            .antMatchers("/level1/**").hasRole("VIP1")
            .antMatchers("/level2/**").hasRole("VIP2")
            .antMatchers("/level3/**").hasRole("VIP3");

        //如果没有登陆，没有权限，就来到spring-security的登录页
        http.formLogin();
        //会发 /login请求 ，如果登录过程中出现异常，就会往这里跳转 /login?error
    }

    /**
     * 定义用户认证规则
     * @param auth
     * @throws Exception
     */
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        //        super.configure(auth);
        //SpringSecurityPasswordEncoder校验密码
        //如果不校验就不予许密码以明文的方式进行传输 ，校验了后才允许
        auth.inMemoryAuthentication().passwordEncoder(new SpringSecurityPasswordEncoder()).withUser("zhangsan").password("123456").roles("VIP1")
            .and().passwordEncoder(new 
								/*声明了用户名，密码，角色在内存中存储，当然也可以使用数据库存储，调用对象时要将inMemoryAuthentication()换成jdbcAuthentication*/            	SpringSecurityPasswordEncoder()).withUser("lisi").password("xxx").roles("VIP2")
            .and().passwordEncoder(new SpringSecurityPasswordEncoder()).withUser("wangwu").password("yyy").roles("VIP3");
    }
}
```

**注销和权限控制**

注销使用http.logout()来实现，而页面的权限控制可以引入thymeleaf和spring security的整合包，用thymeleaf实现

注意：下面是各版本的spring security与thymeleaf整合jar的对照表

| thymeleaf-extras-springsecurity版本 | Spring Security 版本 |
| ----------------------------------- | -------------------- |
| 3                                   | 3.x                  |
| 4                                   | 4.x                  |
| 5                                   | 5.x                  |

**注销**

```java
http.logout();//使用默认的自动配置的注销方法，它会调用/logout方法，去实现session的销毁，调用完方法后，默认跳转到登录页 /login?logout，我么可以修改它默认跳转的页面，例如：http.logout().logoutSuccessUrl("/")
```

**权限控制**

```html
1、引入html的头(spring security5整合thymeleaf5的命名空间)
xmlns:sec="http://www.thymeleaf.org/extras/spring-security"

2、具体整合的页面权限控制
<!--没登录显示请登录-->
<div sec:authorize="!isAuthenticated()">
	<h2 align="center">游客您好，如果想查看武林秘籍 <a th:href="@{/login}">请登录</a></h2>
</div>

<!--认证后-->
<div sec:authorize="isAuthenticated()">
	您好 <span sec:authentication="name"></span> ,您拥有的角色权限有 ：<span sec:authentication="principal.authorities"></span>
</div>
<hr/>

<!--根据授权不同来显示不同的模块-->
<div sec:authorize="hasRole('VIP1')">
	<h3>普通武功秘籍</h3>
	<ul>
		<li><a th:href="@{/level1/1}">罗汉拳</a></li>
		<li><a th:href="@{/level1/2}">武当长拳</a></li>
		<li><a th:href="@{/level1/3}">全真剑法</a></li>
	</ul>
</div>

<div sec:authorize="hasRole('VIP2')">
	<h3>高级武功秘籍</h3>
	<ul>
		<li><a th:href="@{/level2/1}">太极拳</a></li>
		<li><a th:href="@{/level2/2}">七伤拳</a></li>
		<li><a th:href="@{/level2/3}">梯云纵</a></li>
	</ul>
</div>


<div sec:authorize="hasRole('VIP3')">
	<h3>绝世武功秘籍</h3>
	<ul>
		<li><a th:href="@{/level3/1}">葵花宝典</a></li>
		<li><a th:href="@{/level3/2}">龟派气功</a></li>
		<li><a th:href="@{/level3/3}">独孤九剑</a></li>
	</ul>
</div>


<!--注销按钮-->
<!--没登录不显示-->
<div sec:authorize="isAuthenticated()">
	<form  th:action="@{/logout}" method="post">
		<input type="submit" value="注销"/>
	</form>
</div>
```

thymeleaf-SpingSecurity5.x标签使用

```html
<!-- springsecurity5标签的使用，是否已授权 -->
<div sec:authorize="isAuthenticated()">
    <p>已登录</p >
    <!-- 获取登录用户名 -->
    <p>登录名：<span sec:authentication="name"></span></p >
    <!-- 获取登录用户所具有的角色、菜单code -->
    <p>角色权限：<span sec:authentication="principal.authorities"></span></p >
    <!-- 获取登录用户名 -->
    <p>Username：<span sec:authentication="principal.username"></span></p >
    <!-- 获取登录的其他属性，比如密码 -->
    <p>Password：<span sec:authentication="principal.password"></span></p >
    
    <p>拥有的角色：
    <!-- 角色判断，是否拥有某个角色，从authorities取值判断 -->
    <span sec:authorize="hasRole('ROLE_ADMIN')">管理员</span>
    <!-- 授权判断，是否拥有某种授权，从authorities取值判断 -->
    <span sec:authorize="hasAnyAuthority('ROLE_ADMINISTRATOR')" >超级管理员</span>
    <!-- 授权判断，是否拥有某种授权，从authorities取值判断 -->
    <span sec:authorize="hasPermission('index','read')" >bbb</span>   
    <!-- 授权判断，是否拥有某种授权，从authorities取值判断 --> 
    <span sec:authorize="hasAnyAuthority('index:write')" >eeee</span>
    </p >
</div>
```

## 4、自定义登录页面

**自定义规则时要注意Spring-security的默认规则**

```java
//loginPage指定自己的登录页跳转，而登录页的请求应该为：
/**
* 以下是默认的规则
* 1、Spring-security默认的规则是提交 /login GET请求表示去登录的表单，也就是我们这个上段代码去表单其实发送的是GET请求
* 2、发送POST /login请求表示提交表单
* 3、登录错误地址为 /login?error
* 4、注销会发送 /logout请求
* 
* 一旦定义了默认规则，那么此时登录的地址不应该为 /login 了，而是使用 loginPage里的请求作为登录地址，发送POST请求，而登录表单的用户名和密码的name属性可以支持自定义，见以下，如果不定义，默认为 username 和 password
*/
http.formLogin().loginPage("/userlogin").usernameParameter("user")
            .passwordParameter("pwd");
```

**自定义登录页**

```html
<div align="center">
    <form th:action="@{/userlogin}" method="post">
        用户名:<input name="user"/><br>
        密码:<input name="pwd"><br/>
        <input type="checkbox" name="remember"/>记住我<br/>
        <input type="submit" value="登陆">
    </form>
</div>
```

**追加记住我功能**

记住我功能，是Spring-security提供的一个基于客户端存储的cookie实现的，退出浏览器后再进入，依旧生效，基于cookie存储，默认存储14天

```java
//记住我功能
http.rememberMe().rememberMeParameter("remember");//此处也为checkbox框实行了自定义name
```

效果图如下

# 十四、Springboot与分布式

## 1、分布式介绍

### a、什么是分布式

​	分布式计算是计算机科学中一个研究方向，它研究如何把一个需要非常巨大的计算能力才能解决的问题分成许多小的部分，然后把这些部分分配给多个计算机进行处理，最后把这些计算结果综合起来得到最终的结果。分布式[网络存储技术](https://baike.baidu.com/item/网络存储技术)是将数据分散地存储于多台独立的机器设备上。[分布式网络](https://baike.baidu.com/item/分布式网络/8951687)[存储系统](https://baike.baidu.com/item/存储系统/944115)采用可扩展的[系统结构](https://baike.baidu.com/item/系统结构/10394712)，利用多台[存储服务器](https://baike.baidu.com/item/存储服务器/1625801)分担存储负荷，利用位置服务器定位存储信息，不但解决了传统集中式存储系统中单存储服务器的瓶颈问题，还提高了[系统的可靠性](https://baike.baidu.com/item/系统的可靠性/5327458)、可用性和扩展性。

### b、分布式框架的应用场景

​	简单来说，假如现在有一个电商项目，用户访问量大的情况下采用分布式部署项目，将用户模块和订单模块分开部署到不同的服务器上，而此时，如果用户模块需要访问订单模块时，就需要经过==RPC==(远程连接调用) 此时就需要分布式框架来统一处理，当处理的服务越来越多时，我们可能还需要一个"中间层"或者叫 注册中心，来帮我们协调这些服务

### c、常见的分布式框架

==Dubbo== 阿里云提供的分布式框架

==Spring-cloud== spring一站式全家桶分布式框架

应用程序协调服务工具

==Zookeeper==

### d、Zookeeper和Dubbo

​	ZooKeeper 是一个分布式的，开放源码的分布式应用程序协调服务。它是一个为分布式应用提供一致性服务的软件，提供的功能包括：配置维护、域名服务、分布式同步、组服务等。

​	Dubbo是Alibaba开源的分布式服务框架，它最大的特点是按照分层的方式来架构，使用这种方式可以使各个层之间解耦合（或者最大限度地松耦合）。从服务模型的角度来看，Dubbo采用的是一种非常简单的模型，要么是提供方提供服务，要么是消费方消费服务，所以基于这一点可以抽象出服务提供方（Provider）和服务消费方（Consumer）两个角色。

### e、Dubbo的执行流程

## 2、安装zookeeper（docker）

### 1、拉取镜像

```shell
docker pull zookeeper
```

### 2、运行zookeeper

开放指定端口

zookeeper端口映射规则

2181 : 为单机默认端口

2888 ：集群端口

3888 ：election port

8080 ：AdminServer port

==注意：官方建议每次开机时启动==

```shell
docker run --name zktcl -p 2181:2181 -d zookeeper
```

## 3、Springboot整合Zookeeper、Dubbo

### 1、创建服务提供者和服务消费者

使用springInitilizer 快速创建sprinboot项目

### 2、引入相关的依赖

```xml
<!--dubbo-->
<!-- https://mvnrepository.com/artifact/org.apache.dubbo/dubbo-spring-boot-starter -->
<dependency>
    <groupId>org.apache.dubbo</groupId>
    <artifactId>dubbo-spring-boot-starter</artifactId>
    <version>2.7.8</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.apache.curator/curator-recipes -->
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-recipes</artifactId>
    <version>2.7.1</version>
</dependency>

<!--zookeeper-->
<!-- https://mvnrepository.com/artifact/com.github.sgroschupf/zkclient -->
<dependency>
    <groupId>com.github.sgroschupf</groupId>
    <artifactId>zkclient</artifactId>
    <version>0.1</version>
</dependency>
```

### 3、服务提供者

#### 1、代码

```java
@Component
@DubboService // 标注这是一个dubbo 发布的服务类
public class TicketServiceImpl implements TicketService {
    @Override
    public String ticket() {
        return "<script>alert('没有票了！！！')</script>";
    }
}
```

#### 2、配置

配置properties文件

```properties
# 服务提供者起一个名字
dubbo.application.name=ticket

# 注册中心(zookeeper在虚拟机)的地址
dubbo.registry.address=zookeeper://39.98.204.116:2181

# dubbo 注册时要扫描的包 他会去扫描 @dubboService 注解所标注的类
dubbo.scan.base-packages=cn.kate.service
```

#### 3、启动服务

![image-20200825205005728](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202008/25/205006-966.png)

### 4、服务消费者

**1、服务提供者发布项目到注册中心，服务消费者从注册中心拉取，并持有一个服务提供者一模一样的类和方法** 

依赖不再重复粘贴

#### 2、代码

```java
/**
 * @author kate
 * @Date 2020/8/25 20:19
 */
@Service("cs")
public class Consumer {

    @DubboReference // 关联起来注册中心的TicketService
    private TicketService ticketService;

    public void hello(){
        System.err.println(ticketService.ticket());
    }
}
```

#### 3、配置

```properties
# 服务消费者起一个名字
dubbo.application.name=consumer

# 注册中心(zookeeper在虚拟机)的地址 一定是同一个注册中心
dubbo.registry.address=zookeeper://39.98.204.116:2181 
```

#### 4、结果

```java
@SpringBootTest
class ConsumerApplicationTests {

    @Autowired
    @Qualifier("cs") // 注：这里说明一下，spring中好像已经有了一个叫做consumer的配置，所以换个名字
    private Consumer consumer;

    @Test
    void contextLoads() {
        consumer.hello();
    }
}
```

结果图实例：

![image-20200825205956211](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202008/25/205957-659240.png)

==需要注意是：当服务消费者要消费的前提是服务提供者已经启动==









## 4、Springboot整合Spring Cloud

**以后再学**

# 十五、Springboot与热部署

## 1、springboot提供的starter

-   a、导入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <scope>runtime</scope>
    <optional>true</optional>
</dependency>

<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <!--没有该项配置，热部署不会起作用-->
            <configuration>
                <fork>true</fork>
            </configuration>
        </plugin>
    </plugins>
</build>
```

-   application.properties  配置不配置都可

```properties
# 页面修改后立即生效，关闭缓存，立即刷新
spring.thymeleaf.cache=false
# 热部署生效
spring.devtools.restart.enabled=true
# 设置需要重启的目录
spring.devtools.restart.additional-paths=src/java/main
# 设置不需要重启的目录
spring.devtools.restart.exclude=static/**,public/**,WEB-INF/**
# 为 mybatis 设置，生产环境可删除
# restart.include.mapper=/mapper-[\\w-\\.]+jar
# restart.include.pagehelper=/pagehelper-[\\w-\\.]+jar
```

-   设置idea自动编译 `Build project automatically`

-   使用快捷键：`Ctrl + Alt + Shift + /` 调出 Registry 窗口,勾选 `compiler.automake.allow.when.app.running` 选项

## 2、JRebel插件

web项目怎么用的，它就是怎么用的，唯一不同的是他们的Jrebel打开的窗口不一样，主程序上使用Jrebel运行springboot项目即可

**前提条件是 ctrl+alt+shift+/ 弹窗中选择registry 后勾中compiler.automake.allow.when.app.running**  表示自动编译

1、安装Jrebel插件

2、右侧Jrebel选项中勾中所需热部署项目

3、右键Jrebel运行springboot主程序

4、如果修改文件，按照配置好的Jrebel规则进行热加载（我的是5秒一加载）

5、等待控制台响应LazyLoading后表示项目热部署完毕

# 十六、Springboot与监控管理

# 十七、Springboot与Swagger

## 1、Swagger简介

**接口文档所引发的问题？**

1、版本更新，文档就要更新，就要添加新的方法，无法实时同步

2、规范往往也限制不住人们

3、写接口文档时的应用繁杂，没有规范，有用word的，有用md

总结：缺少这方面的框架支持，缺少自动化构建API工具。Swagger完美解决

### a、什么是Swagger

Swagger 是一套基于 OpenAPI 规范构建的开源工具，可以帮助我们设计、构建、记录以及使用 Rest API。Swagger 主要包含了以下三个部分：

1. Swagger Editor：基于浏览器的编辑器，我们可以使用它编写我们 OpenAPI 规范。
2. Swagger UI：它会将我们编写的 OpenAPI 规范呈现为交互式的 API 文档，后文我将使用浏览器来查看并且操作我们的 Rest API。
3. Swagger Codegen：它可以通过为 OpenAPI（以前称为 Swagger）规范定义的任何 API 生成服务器存根和客户端 SDK 来简化构建过程。

### b、Swagger的优点

当下很多公司都采取前后端分离的开发模式，前端和后端的工作由不同的工程师完成。在这种开发模式下，维持一份及时更新且完整的 Rest API 文档将会极大的提高我们的工作效率。传统意义上的文档都是后端开发人员手动编写的，相信大家也都知道这种方式很难保证文档的及时性，这种文档久而久之也就会失去其参考意义，反而还会加大我们的沟通成本。而 Swagger 给我们提供了一个全新的维护 API 文档的方式，下面我们就来了解一下它的优点：

1. 代码变，文档变。只需要少量的注解，Swagger 就可以根据代码自动生成 API 文档，很好的保证了文档的时效性。
2. 跨语言性，支持 40 多种语言。
3. Swagger UI 呈现出来的是一份可交互式的 API 文档，我们可以直接在文档页面尝试 API 的调用，省去了准备复杂的调用参数的过程。
4. 还可以将文档规范导入相关的工具（例如 SoapUI）, 这些工具将会为我们自动地创建自动化测试。

## 2、集成Swagger

### a、导入依赖

```xml
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
    <exclusions>
        <exclusion>
            <artifactId>spring-expression</artifactId>
            <groupId>org.springframework</groupId>
        </exclusion>
        <exclusion>
            <artifactId>spring-core</artifactId>
            <groupId>org.springframework</groupId>
        </exclusion>
        <exclusion>
            <artifactId>spring-context</artifactId>
            <groupId>org.springframework</groupId>
        </exclusion>
        <exclusion>
            <artifactId>spring-beans</artifactId>
            <groupId>org.springframework</groupId>
        </exclusion>
        <exclusion>
            <artifactId>spring-aop</artifactId>
            <groupId>org.springframework</groupId>
        </exclusion>
    </exclusions>
</dependency>

<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
```

### b、创建Swagger配置类

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {
}
```

### c、测试运行

访问  http://localhost:8080/swagger-ui.html 来到swagger默认UI界面

### e、配置Swagger，更改其默认配置

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {

    @Bean
    public Docket docket(ApiInfo apiInfo){
        Docket docket = new Docket(DocumentationType.SWAGGER_2);
        docket.apiInfo(apiInfo);
        return docket;
    }


    @Bean
    public ApiInfo apiInfo(){
        return new ApiInfo(
            "TCL API文档开始", // 标题title
            "这是我的第一次Swagger和Springboot继承文档", // 描述 desc
            "V1.0", // 版本 version
            "APIS", // 组
            DEFAULT_CONTACT,  // 作者信息描述和简介
            "Apache 2.0", // 开源协议和版本号
            "http://121.89.216.65:86/xsk_car/login.html", // url地址
            new ArrayList()
        );
    }
}
```

### f、扫描指定包生成API

```java
@Bean
public Docket docket(ApiInfo apiInfo){
    Docket docket = new Docket(DocumentationType.SWAGGER_2);
    docket.apiInfo(apiInfo);
    // 选择禁用swagger，默认为true，代表启用
    docket.enable(false);
    docket.select()
        /**
                 * 由工具类生成  扫描规则为：
                 * basePackage 基于包扫描  一般我们都用这个
                 * any 扫描所有
                 * none 不扫描
                 * withClassAnnotation(Annotation anno) 基于类上的注解的扫描
                 * withMethodAnnotation(Annotation anno) 基于方法上的注解的扫描
                 */
        .apis(RequestHandlerSelectors.basePackage("cn.kate.controller"))
        /**
                 * 由工具类生成  过滤路径规则为
                 * any 不拦截
                 * none 拦截所有
                 * ant
                 * regex 基于正则过滤
                 */
        //                .paths(PathSelectors.ant("/**"))
        .build();
    return docket;
}
```

#### 题：生产环境使用swagger，发布后(线上)不使用

首先配置多环境

接下来：

```java
@Bean
public Docket docket(ApiInfo apiInfo, Environment environment /*注入环境用于判断*/){
    // 设置要显示的环境
    Profiles profiles = Profiles.of("pro");  // 生产环境才启用
    boolean flag = environment.acceptsProfiles(profiles);
    
    Docket docket = new Docket(DocumentationType.SWAGGER_2);
    docket.apiInfo(apiInfo);
    // 选择禁用swagger，默认为true，代表启用
    docket.enable(flag);
    docket.select()
        /**
                 * 由工具类生成  扫描规则为：
                 * basePackage 基于包扫描  一般我们都用这个
                 * any 扫描所有
                 * none 不扫描
                 * withClassAnnotation(Annotation anno) 基于类上的注解的扫描
                 * withMethodAnnotation(Annotation anno) 基于方法上的注解的扫描
                 */
        .apis(RequestHandlerSelectors.basePackage("cn.kate.controller"))
        /**
                 * 由工具类生成  过滤路径规则为
                 * any 不拦截
                 * none 拦截所有
                 * ant
                 * regex 基于正则过滤
                 */
        //                .paths(PathSelectors.ant("/**"))
        .build();
    return docket;
}
```

### g、分组

```java
Docket docket = new Docket(DocumentationType.SWAGGER_2);
docket.groupName("xxx");
```

**多个组就创建出多个docket对象并添加到spring容器中即可**

## 3、使用Swagger注解

项目中我们通常要给一些实体类和方法加上注释，以便于阅读，此时我们可以使用swagger提供的一套注解API来完成

**注：如果想要实体类(pojo)也被生成接口文档，则Controller中必须返回此实体类(pojo)对象**

| 注解              | 作用在           |
| ----------------- | ---------------- |
| @ApiModel         | 实体类           |
| @ApiModelProperty | 实体类属性       |
| @ApiParam         | 方法参数         |
| @ApiOperation     | controller方法上 |

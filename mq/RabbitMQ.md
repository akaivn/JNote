# RabbitMQ

​																![RabbitMQ](https://img.shields.io/badge/rabbitmq-v3.7.26-ff69b4?logo=rabbitmq)![Springboot](https://img.shields.io/badge/Springboot-v2.3.4-blue?logo=spring)

## Introduction

>   什么是MQ

`MQ` (Message Queue) :翻译为 `消息队列`,通过典型的`生产者`和`消费者`模型生产者不断向消息队列中生产消息，消费者不断的从队列中获取消息。因为消息的生产和消费都是异步的，而且只关心消息的发送和接收，没有业务逻辑的侵入轻松的实现系统间解耦。别名为`消息中间件`。通过利用高效可靠的消息传递机制进行平台无关的数据交流，并基于数据通信来进行分布式系统的集成。

>   MQ都有哪些

当今市面上有很多主流的消息中间件，如老牌的 **ActiveMQ**、**RabbitMQ**，炙手可热的 **Kafka**，阿里巴巴自主开发 **RocketMQ** 等

>   不同MQ的特点

ActiveMQ

```txt
是Apache出品，最流行的，能力强劲的开源消息总线。它是一个完全支持JMS规范的的消息中间件。丰富的API,多种集群架构模式让ActiveMQ在业界成为老牌的消息中间件,在中小型企业颇受欢迎!
```

Kafka

```txt
Kafka是LinkedIn开源的分布式发布-订阅消息系统，目前归属于Apache顶级项目。Kafka主要特点是基于Pull的模式来处理消息消费,追求高吞吐量，一开始的目的就是用于日志收集和传输。0.8版本开始支持复制，不支持事务，对消息的重复、丢失、错误没有严格要求,适合产生大量数据的互联网服务的数据收集业务。
```

RocketMQ

```txt
RocketMQ是阿里开源的消息中间件，它是纯Java开发，具有高吞吐量、高可用性、适合大规模分布式系统应用的特点。RocketMQ思路起源于Kafka，但并不是Kafka的一个Copy，它对消息的可靠传输及事务性做了优化，目前在阿里集团被广泛应用于交易、充值、流计算、消息推送、日志流式处理、binglog分发等场景。
```

RabbitMQ

```txt
RabbitMQ是使用Erlang语言开发的开源消息队列系统，基于AMQP协议来实现。AMQP的主要特征是面向消息、队列、路由（包括点对点和发布/订阅)、可靠性、安全。AMQP协议更多用在企业系统内对数据一致性、稳定性和可靠性要求很高的场景，对性能和吞吐量的要求还在其次。
```

>   说明

RabbitMQ 比以 Kafka可靠，Kafka更适合 IO 高吞吐的处理，一般应用在大数据日志处理或对实时性(少量延迟)，可靠性《少量丢数据）要求稍低的场景使用，比如ELK日志收集。

## Self Introduction

RabbitMQ是一个基于 `AMQP`协议的消息中间件工具，基于Erlang语言开发和编写

![image-20210111192843499](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/11/192844-598053.png)

---

>   特点

-   基于`AMQP`
-   与Spring整合便捷
-   点对点，发布--订阅模式下，消息难以丢失
-   版本迭代快，社区活跃

>   什么是AMQP

上面我们说了很多次AMQP，那么什么是 AMQP呢

```txt
AMQP (advanced message queuing protocol) 在2003年时被提出，最早用于解决金融领域不同平台之间的消息传递交互问题。顾名思义，AMQP是一种协议，更准确的说是一种binary wire-level protocol(链接协议)，这是其和JMS的本质差别。AMQP不从API层进行限定，而是直接定义网络交换的数据格式。这使得实现了AMQP的provider天然性就是跨平台的。以下是AMQP协议模型:
```

>   AMQP协议模型

![image-20210111192757429](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/11/192800-299696.png)

>   官网

地址：https://www.rabbitmq.com/

官方教程：https://www.rabbitmq.com/getstarted.html

## Install

因为RabbitMQ基于Erlang语言，因此 如果直接使用 Windows 或者 CentOS 二进制包或者 .exe文件安装的话，还需要提前准备Erlang所依赖的库和环境，所以此次安装采用 Docker 的方式安装，详细安装步骤可参见 **DIS.md**

### 常用命令

```shell
服务启动关闭：
#启动服务
rabbitmq-server-detached
#关闭服务
rabbitmqctl stop

用户管理：
#添加用户
rabbitmqctl add_user username password
#删除用户
rabbitmqctl delete_user username
#修改用户密码
rabbitmqctl change_password username password
#查看当前用户
rabbitmqctl list_users
#设置用户角色
rabbitmqctl set_user_tags username tag  # tag分为：administrator, monitoring, management, policymaker

插件管理：
#开启插件
rabbitmq-plugins enable plugin_name
#关闭插件
rabbitmq-plugins disable plugin_name
#查看插件状态
rabbitmq-plugins list

集群配置：
#加入node到集群
rabbitmqctl stop_app
rabbitmqctl reset 
rabbitmqctl join_cluster rabbit_cluster_name
#查看集群状态
rabbitmqctl cluster_status
#从当前集群剔除节点
rabbitmqctl forget_cluster_node rabbit_node_name

vhost管理：
#添加vhost
rabbitmqctl add vhost vhost_name
#删除vhost
rabbitmqctl delete vhost vhost_name
权限管理：
#配置用户vhost的权限
rabbitmqctl set_permissions [-p vhostpath] {user} {conf} {write} {read}
conf:一个正则表达式match哪些配置资源能够被该用户访问。 
write:一个正则表达式match哪些配置资源能够被该用户读。 
read:一个正则表达式match哪些配置资源能够被该用户访问。
#查看指定vhost的用户权限
rabbitmqctl list_permissions [-p vhostPath]
#查看指定用户的权限
rabbitmqctl list_user_permissions username
#删除用户的权限
rabbitmqctl clear_permissions [-p vhostPath] {username}

节点管理：
#设置节点为磁盘模式
rabbitmqctl stop_app 
rabbitmqctl change_cluster_node_type disc 
rabbitmqctl start_app 
#设置节点为内存模式
rabbitmqctl stop_app 
rabbitmqctl change_cluster_node_type ram
rabbitmqctl start_app
```

**注：因为使用Docker安装，命令已经很少操作，所以，不要求完全掌握**

## Web Management Page

web界面总览介绍图

![image-20210111201518844](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/11/201519-241201.png)

内部界面众多，不再一 一介绍，使用非常简单快捷

### 核心概念

-   Message

```txt
消息，消息是不具名的，它由消息头和消息体组成。消息体是不透明的，而消息头则由一系列的可选属性组成，这些属性包括routing-key（路由键）、priority（相对于其他消息的优先权）、delivery-mode（指出该消息可能需要持久性存储）等。
```

-   Publisher（provider）

```txt
消息的生产者，也是一个向交换器发布消息的客户端应用程序
```

-   Exchange


```txt
交换器，用来接收生产者发送的消息并将这些消息路由给服务器中的队列。

Exchange有4种类型：默认为direct、其余三种分别为： topic, headers, fanout
```

-   Queue


```txt
消息队列，用来保存消息直到发送给消费者。它是消息的容器，也是消息的终点。一个消息可投入一个或多个队列。消息一直在队列里面，等待消费者连接到这个队列将其取走。
```

-   Binding


```txt
绑定，用于消息队列和交换器之间的关联。一个绑定就是基于路由键将交换器和消息队列连接起来的路由规则，所以可以将交换器理解成一个由绑定构成的路由表。

Exchange 和Queue的绑定可以是多对多的关系。
```

-   Connection


```txt
网络连接，比如一个TCP连接。
```

-   Channel


```txt
信道，多路复用连接中的一条独立的双向数据流通道。信道是建立在真实的TCP连接内的虚拟连接，AMQP 命令都是通过信道发出去的，不管是发布消息、订阅队列还是接收消息，这些动作都是通过信道完成。因为对于操作系统来说建立和销毁 TCP 都是非常昂贵的开销，所以引入了信道的概念，以复用一条 TCP 连接。
```

-   Consumer


```txt
消息的消费者，表示一个从消息队列中取得消息的客户端应用程序。
```

-   Virtual Host


```txt
虚拟主机，表示一批交换器、消息队列和相关对象。虚拟主机是共享相同的身份认证和加密环境的独立服务器域。每个 virtual host本质上就是一个 mini 版的 RabbitMQ 服务器，拥有自己的队列、交换器、绑定和权限机制。virtual host是 AMQP 概念的基础，必须在连接时指定，RabbitMQ 默认的 virtual host 是 / 。
```

-   Broker

```txt
表示消息队列服务器实体
```

### 交换机类型

**Exchange分发消息时根据类型的不同分发策略有区别，目前共四种类型：direct、fanout、topic、headers 。headers 匹配 AMQP 消息的 header 而不是路由键， headers 交换器和 direct 交换器完全一致，但性能差很多，目前几乎用不到了，所以直接看另外三种类型：**

#### Direct（点对点）

​	**消息中的路由键（routing key）如果和 Binding 中的 binding key 一致，交换器就将消息发到对应的队列中。路由键与队列名完全匹配，如果一个队列绑定到交换机要求路由键为“dog”，则只转发 routing key 标记为“dog”的消息，不会转发“dog.puppy”，也不会转发“dog.guard”等等。它是完全匹配、单播的模式。**

#### fanout （广播模式/订阅模式）

​	**每个发到 fanout 类型交换器的消息都会分到所有绑定的队列上去。fanout 交换器不处理路由键，只是简单的将队列绑定到交换器上，每个发送到交换器的消息都会被转发到与该交换器绑定的所有队列上。很像子网广播，每台子网内的主机都获得了一份复制的消息。fanout 类型转发消息是最快的。**

#### topic（按规则匹配）

​	**topic 交换器通过模式匹配分配消息的路由键属性，将路由键和某个模式进行匹配，此时队列需要绑定到一个模式上。它将路由键和绑定键的字符串切分成单词，这些单词之间用点隔开。它同样也会识别两个通配符：符号“#”和符号 * 。#匹配0个或多个单词，* 匹配一个单词。**

## Getting Start

>   回顾AMQP协议流程

![image-20210111201207140](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/11/201209-678704.png)

>   文字流程

```txt
生产者生产消息--->通过信道（channel）传递数据到 exchange（交换机）
消费者消费消息--->到队列（Queue）中的请求根据匹配规则去交换机中寻找到自己想要的数据--->通过信道（channel）返回消息
交换机和队列的外层为虚拟主机
最外层为RabbitMQ服务
```

>   官网提供的消息模型和各语言的支持

![image-20210111204556820](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/11/204557-118287.png)

![image-20210111204619475](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/11/204620-365130.png)

![image-20210111204637977](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/11/204639-785770.png)

---

### 引入依赖

```xml
<dependency>
	<groupId>com.rabbitmq</groupId>
	<artifactId>amqp-client</artifactId>
    <version>5.9.0</version>
</dependency>
```

### 配置虚拟主机

虚拟主机是共享相同的身份认证和加密环境的独立服务器域

>   创建虚拟主机

![image-20210111211727243](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/11/211727-310854.png)

**我们为了演示和学习，再另外创建一个用户来操作单独的虚拟主机**

>   创建用户

![image-20210111211532496](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/11/211532-150110.png)

>   设置虚拟主机

![image-20210111212017905](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/11/212018-318549.png)

---

### 消息模型

#### 直连

![image-20210112085405998](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/12/092825-342131.png)

- p代表生产者(provider/publisher)
- c代表消费者(consumer)

**该方式为 p直连 queue 生产消息并投放，c从queue中取出 p生产的数据**

> 生产者Api

```java
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;
@Test
public void send() throws Exception{
    // 1、创建链接工厂
    ConnectionFactory factory = new ConnectionFactory();
    // 2、设置rabbitmq主机地址
    factory.setHost("ip");
    // 3、设置端口
    factory.setPort(port);
    // 4、设置连接哪个虚拟主机
    factory.setVirtualHost("ems");
    // 5、设置访问虚拟主机的用户名和密码
    factory.setUsername("ems");
    factory.setPassword("ems");

    // 6、获取链接对象
    Connection connection = factory.newConnection();
    // 7、获取链接中的 信道 用于绑定队列来消费消息
    Channel channel = connection.createChannel();
    /**
         * 8、定义队列开始
         * @param s：队列名称，如果获取的队列名称不存在会自动创建
         * @param b：durable是否持久化，如果持久化，消息队列关闭时存储到磁盘 true为持久化
         * @param b1：exclusive是否独占队列，如果为true表示独占，只有此链接能消费
         * @param b2：autoDelete队列消费完成时，是否自动删除，true表肯定
         * @param map：arguments额外的参数
         */
    channel.queueDeclare("hello",false,false,false,null);

    /**
         * 9、发送消息到队列
         * @param s：exchange交换机名称，直连模式下无交换机
         * @param s1：queue队列名称
         * @param basicProperties：除消息外额外的设置
         * @param byte[]：消息体字节数组
         */
    channel.basicPublish("","hello",null,"hello world2!".getBytes());

    // 10、关闭链接和通道（先开后关）
    channel.close();
    connection.close();
}
```

> web界面变化

![image-20210112092708761](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/12/092838-199080.png)

> 消费者Api

```java
import com.rabbitmq.client.*;
public static void main(String[] args) throws Exception{
    ConnectionFactory factory = new ConnectionFactory();
    factory.setHost("ip");
    factory.setPort(port);
    factory.setVirtualHost("ems");
    factory.setUsername("ems");
    factory.setPassword("ems");

    Connection connection = factory.newConnection();
    Channel channel = connection.createChannel();

    channel.queueDeclare("hello",false,false,false,null);
    /**
         * @param s:queue队列名称
         * @param b:消息的自动确认
         * @param consumer：消费时的回调
         */
    channel.basicConsume("hello",true, new DefaultConsumer(channel){
        /**
             * @param consumerTag
             * @param envelope
             * @param properties
             * @param body 消息体
             * @throws IOException
             */
        @Override
        public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
            System.out.println(new String(body));
        }
    });

    //不关闭链接，客户端会监听队列，一有消息马上消费
}
```

部分代码重复，可以提取一个工具类来封装公共的代码

**注：如果队列中有很多条消息，当开启consumer客户端时，c会直接消费完所有消息队列中的消息**

##### 工具类封装

```java
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

/**
 * @Author: cltong
 * @Date: 2021/1/12 9:52
 */
public class RabbitMQUtil {
    private static ConnectionFactory factory = new ConnectionFactory();
    public static Connection connection = null;
    private static Channel channel = null;
    static {
        factory.setHost(ip);
        factory.setPort(port);
        factory.setVirtualHost(host);
        factory.setUsername(username);
        factory.setPassword(password);
    }

    public static Connection getConnection() throws Exception{
        connection = factory.newConnection();
        return connection;
    }

    public static Channel getChannel () throws Exception{
        connection = factory.newConnection();
        channel = connection.createChannel();
        return channel;
    }

    public static void release(Channel var1) throws Exception{
        if(var1==null){
            if(channel!=null){
                channel.close();
            }
        }else{
            var1.close();
        }

        if(connection!=null){
            connection.close();
        }
    }
}
```

##### 参数详解

在上述消息模型中我们简单的实现了一下点对点发布的代码，其中 **信道与消息推送时的部分参数** 我们在此做一个详解

上述代码是这样的

>   provider/publisher

```java
// 只是定义信道
channel.queueDeclare("hello",false,false,false,null);
// 这一步才是推送消息到队列
channel.basicPublish("","hello",null,"hello world2!".getBytes());
```

>   consumer

```java
channel.queueDeclare("hello",false,false,false,null);
```

虽然上述文档中的 **Javadoc注释** 已经向我们说明了参数的意义，但这里还有要补充的另外几点

-   消息生产者生产什么样的消息，消费者就要对应起来消费，不然会抛出IOException
-   想要队列持久化到硬盘时，需要配置 **durable为true** ，想要消息也一同跟着队列持久化时，需要配置 **prop为 MessageProperties.PERSISTENT_TEXT_PLAIN**
-   autoDelete 配置如果为 true，表示当消费者消费完队列后就会自动删除队列(前提是消费者断开了与队列的链接)

#### 工作队列(Work Queue)

Work Queues，也被称为（Task queues)，任务模型。当消息处理比较耗时的时候，可能**生产消息的速度会远远大于消息的消费速度**。长此以往，**消息就会堆积越来越多，无法及时处理**。此时就可以使用work模型:**让多个消费者绑定到一个队列，共同消费队列中的消息**。队列中的消息一旦消费，就会消失，因此任务是不会被重复执行的。

![image-20210112204634077](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/12/204637-766466.png)

-   P:生产者:任务的发布者
-   c1:消费者,领取任务并且完成任务，假设完成速度较慢
-   C2:消费者2:领取任务并完成任务，假设完成速度快

##### 示例

>   provider/publisher

```java
@Test
public void send() throws Exception{
    Channel channel = RabbitMQUtil.getChannel();

    // 设置信道与队列绑定
    channel.queueDeclare("work",true,false,false,null);

    // 循环发送消息
    for (int i = 0; i < 10; i++) {
        channel.basicPublish("","work", MessageProperties.PERSISTENT_TEXT_PLAIN, (i + "work queue").getBytes());
    }

    // 关闭链接
    RabbitMQUtil.release(channel);
}
```

>   consumer1/consumer2

```java
public static void main(String[] args) throws Exception {

    Channel channel = RabbitMQUtil.getChannel();

    channel.queueDeclare("work",true,false,false,null);

    channel.basicConsume("work",true,new DefaultConsumer(channel){
        @Override
        public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
            System.out.println("消费者一：" + new String(body));
        }
    });
}
```

##### 启动测试

![image-20210112210324610](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/12/210325-324791.png)

![image-20210112210338589](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/12/210339-204883.png)

**总结:默认情况下，RabbitMQ将按顺序将每个消息发送给下一个使用者。平均而言，每个消费者都会收到相同数量的消息。这种分发消息的方式称为循环。**

##### 问题引出

我们此时做一个猜想，既然默认是平均分配的，那么假如使其中一个消费者消费的速度变慢，会发生什么？

>   consumer1改造

```java
channel.basicConsume("work",true,new DefaultConsumer(channel){
    @Override
    public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
        try {
            // 加入线程休眠
            TimeUnit.SECONDS.sleep(2);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("消费者一：" + new String(body));
    }
});
```

新的测试结果如下

![image-20210112210707217](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/12/210707-457270.png)

此时consumer1虽然也在消费队列，但是消费的速度变慢，**在队列中如果有大量的消息时，就会造成消息堆积**，故平均分配的模式弊端就此展露

解决方式引出？如何更改平均分配的策略？

##### 消息确认和能者多劳

在以往的代码中，我们都会选择使用消费者自动确认消息，这样消息队列会认为所有的消息都已分配给不同的消费者而马上清空所有队列中的消息，消费者1虽然执行慢，但是它已经从消息队列中拿到了所有的消息，故而会一个一个消费，从而拖慢速度，因此我们需要**先关闭消息自动确认**

```java
channel.basicConsume("work",false,new DefaultConsumer(channel){});
```

紧接着因为执行消费队列的速度很快，我们不好把握队列中消息流失的速度，因此需要设置每次只消费一条消息，避免线程执行快带来的不可预估性

```java
channel.basicQos(1);
```

最后，因为我们关闭了自动确认，如果一直不确认消息被消费，那么消息队列中就会一直保留该消息，因此我们需要手动确认消息

```java
/**
 * 手动确认消息，执行一条确认一条
 * @param1 tag:代表确认哪条消息被消费的标志
 * @param2 b:是否开启多个消息同时确认？因为我们在上述代码中每次只执行一条，Qps都是1，所以这里选择false
 */
channel.basicAck(envelope.getDeliveryTag(),false);
```

上述示例完成后，就可以实现能者多劳，而业务执行慢的消费者或者中途宕机的消费者因为没有确认消息，就会被剩下的消费者接替掉手中的工作，从而达到消息的不丢失

>   测试如下

![image-20210112214555031](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/12/214555-111682.png)

![image-20210112214608502](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/12/214609-191410.png)

#### 扇出(fanout)

某些业务场景下，我们希望同一条消息可以被不同的消费者消费，fanout模型就可以帮助我们完成这项业务

fanout:又叫广播，指生产者将消息发送到交换机后，由交换机来选择该消息可以被哪些消费者消费，决定消费者可以消费消息的不再是生产者和队列，而是exchange

![image-20210113193811292](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/13/193818-93912.png)

-   P:生产者，向exchange发送消息
-   X: exchange(交换机)，接收生产者的消息，然后把消息递交给与之绑定的队列
-   c1∶消费者，消费队列中的消息
-   c2∶消费者，消费队列中的消息



在广播模式下，消息发送流程是这样的:

-   可以有多个消费者
-   每个消费者有自己的queue (队列)
-   每个队列都要绑定到Exchange(交换机)
-   生产者发送的消息，只能发送到交换机，交换机来决定要发给哪个队列，生产者无法决定
-   交换机把消息发送给绑定过的所有队列
-   队列的消费者都能拿到消息。实现一条消息被多个消费者消费

##### 示例

>   provider/publisher

```java
@Test
public void send() throws Exception{
    Channel channel = RabbitMQUtil.getChannel();

    /**
         * 定义交换机
         * @param1 exchange：指定交换机的名称，如果虚拟主机中没有此交换机则会自动创建
         * @param2 type：指定交换机的类型，此处为广播模式：fanout
         */
    channel.exchangeDeclare("logs","fanout");

    /**
         * 发送消息
         * @param1 exchange：交换机名称
         * @param2 routingKey：路由键，此模型用不到，后面的模型再说
         * @param3 props：消息的其他属性——路由头等
         * @param4 byte[]：消息体
         */
    channel.basicPublish("logs","", null,"fanout模型消息".getBytes());

    RabbitMQUtil.release(channel);
}
```

>   consumer1/consumer2/conumer3 ...

```java
public static void main(String[] args) throws Exception{
    Channel channel = RabbitMQUtil.getChannel();

    // 声明交换机
    channel.exchangeDeclare("logs","fanout");

    // 创建临时队列
    String queueName = channel.queueDeclare().getQueue();

    /**
         * 绑定交换机与队列
         * @param1 queue：队列名
         * @param2 exchange：交换机名
         * @param3 routingKey：路由键
         */
    channel.queueBind(queueName,"logs",null);

    // 消费消息
    channel.basicConsume(queueName,true,new DefaultConsumer(channel){
        @Override
        public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
            System.out.println("消费者一：" + new String(body));
        }
    });
}
```

注：此处需要注意的是 **routingKey** 虽然此模型没有用到，但是不能赋值为 null，需要为空字符串，否则会抛出 IOException

#### 路由(Routing)

在上述 **fanout** 模型处，我们留了一个小尾巴 `routingKey`，此节来详细说明一下

首先，我们需要提出一个问题：fanout 模型是任何消费者都能消费的广播模型，如果我们假设只允许某些消费者才能消费到，应该如果实现？ fanout 无法指定消费者消费指定的消息

故，路由的概念就衍生了，我们通过使用一个路由键（routingKey）来绑定交换机与队列，这样，交换机只会根据路由键的规则将消息推送给指定的队列，而与指定队列绑定的消费者则指定消费到此队列的消息

##### Routing-Direct

**Direct** 路由模式，又叫做直连。为路由的模式之一

![image-20210113201404176](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/13/201406-415022.png)

-   P:生产者，向exchange发送消息，发送消息时，会指定一个routing key
-   X: exchange(交换机)，接收生产者的消息，然后把消息递交给与routing key完全匹配的队列
-   c1∶消费者，其所在队列指定了需要routing key为error的消息
-   c2∶消费者，其所在队列指定了需要routing key为info、error、warning 的消息



在Direct模式下：

-   队列与交换机的绑定，不能是任意绑定了，而是要指定一个 routingKey(路由键)
-   消息的发送方在向exchange发送消息时，也必须指定消息的 routingKey
-   exchange不再把消息交给每一个绑定的队列，而是根据消息的routingKey进行判断，只有队列的routingKey与消息的routingKey完全一致，才会接收到消息

###### 示例

>   provider/publisher

```java
@Test
public void send() throws Exception {
    Channel channel = RabbitMQUtil.getChannel();
    channel.exchangeDeclare("logs_direct","direct");
    String routingKey = "error";
    String message = "direct发出的日志等级为：[" + routingKey +"] 的消息";
    channel.basicPublish("logs_direct",routingKey,null,message.getBytes());
    RabbitMQUtil.release(channel);
}
```

>   consumer1/consumer2

```java
public static void main(String[] args) throws Exception {
    Channel channel = RabbitMQUtil.getChannel();
    channel.exchangeDeclare("logs_direct","direct");
    String queue = channel.queueDeclare().getQueue();
    String routingKey1 = "warning";
    String routingKey2 = "debug";
    String routingKey3 = "error";
    // 表示该信道同时绑定了三种不同的路由键
    channel.queueBind(queue,"logs_direct",routingKey1);
    channel.queueBind(queue,"logs_direct",routingKey2);
    channel.queueBind(queue,"logs_direct",routingKey3);
    channel.basicConsume(queue,true,new DefaultConsumer(channel){
        @Override
        public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) throws IOException {
            System.out.println("消费者一消费的消息为：" + new String(body));
        }
    });
}
```

##### Routing-Topic

**Topic**：又叫动态路由，解决的问题为上述直连的不灵活

通过上述路由规则，我们实现了使不同的消费者能够消费到不同的消息，但是我们会发现，绑定的路由键不够灵活，可能会出现以下代码冗余的情况

```java
channel.queueBind(queue,"logs_direct",routingKey1);
channel.queueBind(queue,"logs_direct",routingKey2);
channel.queueBind(queue,"logs_direct",routingKey3);
...
```

针对此问题，RabbitMQ的解决方案就是使用 **动态路由**，它支持以通配符的形式来声明路由键，匹配时也需依照声明的通配符来匹配消费对应的消息

这种模型routingKey一般都是由一个或多个单词组成，多个单词之间以"."分割，例如: item.insert

![image-20210113205542843](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/13/205543-428443.png)

此流程图角色不再阐述

>   支持的通配符

-   *：必须匹配一个单词
-   #：匹配 0 个或多个单词

###### 示例

>   provider/publisher

```java
channel.exchangeDeclare("topics","topic");
```



>   consumer1/consumer2

```java
channel.queueBind(queue,"topics","user.#");
channel.queueBind(queue,"topics","user.*");
```

或

```java
channel.queueBind(queue,"topics","*.#.user.#");
channel.queueBind(queue,"topics","#.user.*");
```

### Springboot整合

与Springboot整合后，操作RabbitMQ使用 `RabbitTemplate` 对象的一系列Api并联合Spring给我们提供的RabbitMQ的 **注解** 即可

#### boot hello world

- 引入依赖 (或使用Spring Initializr)

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.amqp</groupId>
    <artifactId>spring-rabbit-test</artifactId>
    <scope>test</scope>
</dependency>
```

- provider

```java
@SpringBootTest
public class Provider {
    @Autowired
    private RabbitTemplate rabbitTemplate;

    @Test
    void t1(){
        // 单单创建消息是不会创建发送的，只有有消费者时才会发送
        rabbitTemplate.convertAndSend("boot-queue","springboot-rabbitmq");
    }
}
```

- consumer

```java
@Component
/**
 * @RabbitListener 代表此组件为RabbitMQ的消费类
 * 它的详情属性对应了我们操作RabbitMQ Api时的队列的设置
 * queuesToDeclare：表示没有则创建队列
 */
@RabbitListener(queuesToDeclare = {@Queue("boot-queue")})
public class Consumer {
    @Autowired
    private RabbitTemplate rabbitTemplate;

    /**
     * @RabbitHandler：表示此方法为RabbitMQ消费消息后的回调方法，参数为RabbitMQ传递过来的
     * @param message
     */
    @RabbitHandler
    public void outputMessage(String message){
        System.out.println(message);
    }
}
```

#### boot work queue

work queue工作队列模型整合

provider

```java
@Autowired
private RabbitTemplate rabbitTemplate;

rabbitTemplate.convertAndSend("boot-work","springboot-rabbitmq" + i);
```

consumer

```java
@Component
public class WrokConsumer {

    // 此注解也可以作用于方法，表示会顺带处理回调的消息
    @RabbitListener(queuesToDeclare = {@Queue("boot-work")})
    public void receiveMessage(String message){
        System.out.println(message);
    }

    // 此注解也可以作用于方法，表示会顺带处理回调的消息
    @RabbitListener(queuesToDeclare = {@Queue("boot-work")})
    public void receiveMessage2(String message){
        System.out.println(message);
    }
}
```

**注：在Springboot中，默认的工作队列也采用的是平均分配消息的模式，如果想要实现能者多劳，我们需要额外的配置**

#### boot fanout

扇出，广播模式

provider

```java
@Autowired
private RabbitTemplate rabbitTemplate;

/**
         * @param1：exchange
         * @param2：routingKey
         * @param3：message
         */
rabbitTemplate.convertAndSend("logs","","fanout扇出模型");
```

consumer

```java
@RabbitListener(
    bindings = {
        @QueueBinding(
            value = @Queue,// 如果不声明队列名，就会创建临时队列
            exchange = @Exchange(value = "logs",type = "fanout")// 绑定交换机
        )
    }
)
public void receiveMessage1(String message){
    System.out.println(message);
}
```

#### boot routing direct

routingKey 路由直连模式

provider

```java
// 使用routingKey路由
rabbitTemplate.convertAndSend("aaa","info","direct路由key");
```

consumer

```java
@RabbitListener(
    bindings = {
        @QueueBinding(
            value = @Queue,
            exchange = @Exchange(value = "aaa",type = "direct"),
            key = {"warn"}
        )
    }
)
public void receiveMessage1(String message){
    System.out.println(message);
}
```

#### boot routing topic

routingKey 动态路由模式，支持通配符

provider

```java
rabbitTemplate.convertAndSend("bbb","warn.a","topic路由规则");
```

consumer

```java
@RabbitListener(
    bindings = {
        @QueueBinding(
            value = @Queue,
            exchange = @Exchange(value = "bbb",type = "topic"),
            key = {"warn.*"}
        )
    }
)
```

### MQ应用场景

#### 异步处理

场景说明：用户注册后，需要发注册邮件和注册[短信](https://cloud.tencent.com/product/sms?from=10680),传统的做法有两种：串行与并行

- 串行方式:将注册信息写入数据库后,发送注册邮件,再发送注册短信,以上三个任务全部完成后才返回给客户端。 这有一个问题是,邮件,短信并不是必须的,它只是一个通知,而这种做法让客户端等待没有必要等待的东西

![vvmok1ubkr](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/15/160044-361299.png)

- 将注册信息写入数据库后,发送邮件的同时,发送短信,以上三个任务完成后,返回给客户端,并行的方式能提高处理的时间

![c8pw4v1qoe](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/15/160122-130759.png)

假设三个业务节点分别使用50ms,串行方式使用时间150ms,并行使用时间100ms。虽然并性已经提高的处理时间,但是,前面说过,邮件和短信对我正常的使用网站没有任何影响，客户端没有必要等着其发送完成才显示注册成功,因而是写入数据库后就返回

- 消息队列：引入消息队列后，把发送邮件,短信不是必须的业务逻辑异步处理 

![5r2kyi4vu7](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/15/160209-945288.png)

#### 应用解耦

场景：双11是购物狂节,用户下单后,订单系统需要通知库存系统,传统的做法就是订单系统调用库存系统的接口

![weaz95z0u3](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/15/160339-7498.png)

这种做法有一个缺点:

- 当库存系统出现故障时,订单就会失败
- 订单系统和库存系统高耦合

引入消息队列 

![uufioa2r7r](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/15/160342-346197.png)

- 订单系统:用户下单后,订单系统完成持久化处理,将消息写入消息队列,返回用户订单下单成功
- 库存系统:订阅下单的消息,获取下单消息,进行库操作。 就算库存系统出现故障,消息队列也能保证消息的可靠投递,不会导致消息丢失

#### 流量削峰

流量削峰一般在秒杀活动中应用广泛 

场景:秒杀活动，一般会因为流量过大，导致应用挂掉,为了解决这个问题，一般在应用前端加入消息队列

作用: 

1、可以控制活动人数，超过此一定阀值的订单直接丢弃

2、可以缓解短时间的高流量压垮应用(应用程序按自己的最大处理能力获取订单) 

![t0qgtelj6a](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/15/160533-88143.png)

- 用户的请求,服务器收到之后,首先写入消息队列,加入消息队列长度超过最大值,则直接抛弃用户请求或跳转到错误页面
- 2.秒杀业务根据消息队列中的请求信息，再做后续处理

## Advance

### 集群

为了提供系统的可用性和承载力，RabbitMQ为我们提供了集群，集群模式一共有两种，我们接下来 一 一介绍一下

#### 普通集群（非高可用）

默认情况下:**RabbitMQ** 代理操作所需的所有数据/状态都将跨所有节点复制。这方面的一个例外是消息队列，默认情况下，消息队列位于一个节点上，尽管它们可以从所有节点看到和访问

##### 集群架构图

![image-20210117203716887](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/17/203718-217744.png)

上面那句话的意思就是：**默认情况下，RabbitMQ提供的集群模式为普通集群，只是单纯的将Master主节点的Exchange复制了一份到每个子节点上，尽管所有子节点都可以拿到交换机和所有队列的信息，但实际上子节点不具备有故障转移、自动备份和容灾等功能**。当子节点复制了Master节点的交换机后，消费者也可以订阅子节点消费消息（**实际上，当消费者订阅子节点后，消费消息时，子节点只是单纯的去调用了Master节点的消息，然后返回给消费者**）

>   解决的问题

当Master节点宕机或异常时，Slave节点能够复制备份Master的节点（Queue）数据

接下来我们针对于普通集群做一个实例，以便于我们更好的理解RabbitMQ的集群模式和概念

##### 集群搭建

###### 环境准备

由于我们在上述章节中并没有采用RabbitMQ普通的安装方式，所以我们依旧在本章节中使用基于Docker的方式来部署集群

> 修改以往的运行命令

- **rabbit_erlang_cookie** ，集群要求，所有的集群节点的erlang cookie都必须一致
- **hostname** ，设置集群节点的主机名

第一台RabbitMQ

```shell
docker run -d --hostname rabbit1 --name myrabbit1 -p 15672:15672 -p 5672:5672 -e RABBITMQ_ERLANG_COOKIE='rabbitcookie' 镜像id或镜像名
```

第二台RabbitMQ

- **link** 是连接集群节点的重要属性

```shell
docker run -d --hostname rabbit2 --name myrabbit2 -p 15673:15672 5673:5672 --link myrabbit1:rabbit1 -e RABBITMQ_ERLANG_COOKIE='rabbitcookie' 镜像id或镜像名
```

第三台RabbitMQ

连接前两台RabbitMQ的服务

```shell
docker run -d --hostname rabbit3 --name myrabbit3 -p 15674:15672 5674:5672 --link myrabbit1:rabbit1 --link myrabbit2:rabbit2 -e RABBITMQ_ERLANG_COOKIE='rabbitcookie' 镜像id或镜像名
```

注：启动命令上的 **RABBITMQ_ERLANG_COOKIE** 必须相同

> 加入RabbitMQ节点到集群

第一台RabbitMQ容器

```shell
docker exec -it myrabbit1 bash
rabbitmqctl stop_app
rabbitmqctl reset
rabbitmqctl start_app
exit
```

第二台RabbitMQ容器

```shell
docker exec -it myrabbit2 bash
rabbitmqctl stop_app
rabbitmqctl reset
rabbitmqctl join_cluster --ram rabbit@rabbit1
rabbitmqctl start_app
exit
```

第二台RabbitMQ容器

```shell
docker exec -it myrabbit3 bash
rabbitmqctl stop_app
rabbitmqctl reset
rabbitmqctl join_cluster --ram rabbit@rabbit1
rabbitmqctl start_app
exit
```

参数 **--ram** 表示设置为内存节点，忽略次参数默认为磁盘节点

启动了3个节点，1个磁盘节点和2个内存节点

> 配置相同的Erlang Cookie

有些特殊的情况，比如已经运行了一段时间的几个单个物理机，我们在之前没有设置过相同的Erlang Cookie值，现在我们要把单个的物理机部署成集群，实现我们需要同步Erlang的Cookie值

> 1.为什么要配置相同的erlang cookie？

因为RabbitMQ是用Erlang实现的，Erlang Cookie相当于不同节点之间相互通讯的秘钥，Erlang节点通过交换Erlang Cookie获得认证。

> 2.Erlang Cookie的位置

要想知道Erlang Cookie位置，首先要取得RabbitMQ启动日志里面的home dir路径，作为根路径。使用：docker logs 容器名称 查看，如下图：

![img](http://icdn.apigo.cn/blog/rabbitmq-homedir.png)

所以Erlang Cookie的全部路径就是

```shell
/var/lib/rabbitmq/.erlang.cookie
```

**注意：每个人的erlang cookie位置可能不同，一定要查看自己的home dir路径。**

> 3.复制Erlang Cookie到其他RabbitMQ节点

获取到第一个RabbitMQ的Erlang Cookie之后，只需要把这个文件复制到其他RabbitMQ节点即可。

物理机和容器之间复制命令如下：

- 容器复制文件到物理机：docker cp 容器名称:容器目录 物理机目录
- 物理机复制文件到容器：docker cp 物理机目录 容器名称:容器目录

设置Erlang Cookie文件权限：

```shell
chmod 600 /var/lib/rabbitmq/.erlang.cookie
```

> 打开测试地址，查看集群节点

![image-20210123121603897](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/23/121604-649738.png)

我们访问三个web管理界面，尝试使用程序发送消息到队列，我们会发现，消息队列和交换机如果是在master节点创建的，那么立刻就会同步到其余的slave节点

slave节点的RabbitMQ服务虽然也能给我们提供服务，但是，如果主节点down掉，所有的slave节点都会丢失信息，即使slave节点在正常运行，也无法正常提供服务，所以，这种普通的集群是需要master节点一直存活着的

###### 实例

接下来我们来使用一个实例证明

> 1、回归到第一章节我们的 hello world 实例，我们将**RabbitMQUtil**的端口先改到master主节点对应的RabbitMQ服务

```java
static {
    factory.setPort(5672);
}
```

> 2、启动消费者发送消息到对应master节点队列

> 3、发送消息后，我们将端口改为两个内存节点(slave)中的一个

```java
static {
    factory.setPort(5673);
    // 或
    factory.setPort(5674);
}
```

> 4、启动消费者消费消息

![image-20210124100556871](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/24/100557-37722.png)

我们看到消息依旧被消费，证明了salve节点可以接受用户请求、消费消息，它调用master节点的消息，然后把消息返回给消费者，与此同时，master节点必须保持可用，当master down掉时，剩下的slave节点也无法继续服务

**slave节点相当于为master节点做了负载的效果**

注：`当使用docker集群时，重启或关闭master节点会导致配置失效，slave节点不可用`

![image-20210124102051853](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/24/102052-23314.png)

解决方式：==重启所有的slave节点即可==

#### 镜像集群（高可用）

`RabbitMQ普通集群模式，是将交换机、绑定、队列的元数据复制到集群里的任何一个节点，但队列内容只存在于特定的节点中，客户端通过连接集群中任意一个节点，即可以生产和消费集群中的任何队列内容（因为每个节点都有集群中所有队列的元数据信息，如果队列内容不在本节点，则本节点会从远程节点获取内容，然后提供给消费者消费）`

从该模式不难看出，普通集群可以让不同的繁忙队列从属于不同的节点，这样可以减轻单节点的压力，提升吞吐量，**但是普通集群不能保证队列的高可用性，因为一旦队列所在节点宕机直接导致该队列无法使用，只能等待重启**

显然，对于大型分布式系统而言，这并不是一个高可用的解决方案，基于普通集群的架构之上RabbitMQ还为我们提供了另一种集群模式：**镜像集群（高可用）**

**镜像的存在等于打包了一个一个的队列到镜像中，使所有的节点复制镜像到自己的环境中，拥有此队列的镜像相当于拥有了真正的队列**

##### 集群架构图

![image-20210124103556150](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/24/103557-384160.png)

##### 集群搭建

RabbitMQ高可用的镜像集群不需要额外编写代码，只需要在普通集群的架构上配置一下相应的策略即可

策略说明

```shell
rabbitmqctl set_policy [-p vhost>] [--priority <priority>] [--apply-to <apply-to>] <name> <cpattern> <definition>

-p Vhost:可选参数，针对指定vhost下的queue进行设置
Name :policy的名称
Pattern : queue的匹配模式(正则表达式)
Definition:镜像定义，包括三个部分ha-mode，ha-params，ha-sync-mode
	ha-mode:指明镜像队列的模式，有效值为all/exactly/nodes
		all:表示在集群中所有的节点上进行镜像
		exactly:表示在指定个数的节点上进行镜像，节点的个数由ha-params指定
		nodes:表示在指定的节点上进行镜像，节点名称通过ha-params指定
	ha-params: ha-mode模式需要用到的参数
	ha-sync-mode:进行队列中消息的同步方式，有效值为automatic和manual
	priority:可选参数, policy的优先级
```

###### 命令行模式

需要先进入到docker容器中

```shell
docker exec -it <容器名 或 容器 id> bash 
```

```shell
# 查看当前所有策略
rabbitmqctl list_policies

# 添加策略
rabbitmqctl set_policy ha-all '>hello' '{"ha-mode" : "all","ha-sync-mode":"automatic"}'

# 说明:
# 1、'ha-all':策略名
# 2、策略正则表达式为 ^ 表示所有匹配所有队列名称^hello:匹配hello开头队列
# 3、定义
	# 'ha-mode'
		# all:表示所有的集群都进行镜像
	# 'ha-sync-mode'
		# atomatic:表示所有的队列中的消息自动同步		
		
# 移除策略
rabbitmqctl clear_policy <name>

# 命令行帮助
rabbitmqctl --help
```

###### web界面模式

![image-20210124111748922](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/24/111750-59997.png)

![image-20210124112525084](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/24/112525-491661.png)

##### Springboot整合

详情见：https://blog.csdn.net/qq_45491757/article/details/105712530

### 系统架构图

![yypy8o1azr](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202101/15/160746-400946.jpeg)


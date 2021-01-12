# RabbitMQ

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

### Common Command

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

### Term Introduction

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

### Exchange Type Introduction

**Exchange分发消息时根据类型的不同分发策略有区别，目前共四种类型：direct、fanout、topic、headers 。headers 匹配 AMQP 消息的 header 而不是路由键， headers 交换器和 direct 交换器完全一致，但性能差很多，目前几乎用不到了，所以直接看另外三种类型：**

#### Direct（点对点）

​	**消息中的路由键（routing key）如果和 Binding 中的 binding key 一致，交换器就将消息发到对应的队列中。路由键与队列名完全匹配，如果一个队列绑定到交换机要求路由键为“dog”，则只转发 routing key 标记为“dog”的消息，不会转发“dog.puppy”，也不会转发“dog.guard”等等。它是完全匹配、单播的模式。**

#### fanout （广播模式/订阅模式）

​	**每个发到 fanout 类型交换器的消息都会分到所有绑定的队列上去。fanout 交换器不处理路由键，只是简单的将队列绑定到交换器上，每个发送到交换器的消息都会被转发到与该交换器绑定的所有队列上。很像子网广播，每台子网内的主机都获得了一份复制的消息。fanout 类型转发消息是最快的。**

#### topic（按规则匹配）

​	**topic 交换器通过模式匹配分配消息的路由键属性，将路由键和某个模式进行匹配，此时队列需要绑定到一个模式上。它将路由键和绑定键的字符串切分成单词，这些单词之间用点隔开。它同样也会识别两个通配符：符号“#”和符号 * 。#匹配0个或多个单词，* 匹配一个单词。**

## Hello World

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
    factory.setHost("81.68.111.193");
    // 3、设置端口
    factory.setPort(5672);
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
    factory.setHost("81.68.111.193");
    factory.setPort(5672);
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

#### 工具类封装

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

    public static void release(Channel var2) throws Exception{
        if(var2==null){
            if(channel!=null){
                channel.close();
            }
        }
        var2.close();

        if(connection!=null){
            connection.close();
        }
    }
}
```




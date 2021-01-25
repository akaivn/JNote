# ElasticSearch

​										![ElasticSearch](https://img.shields.io/badge/ElasticSearch-v7.7.0-green?logo=elasticsearch)![Kibana](https://img.shields.io/badge/Kibana-v7.7.0-ff69b4?logo=Kibana)![IK Analyzer](https://img.shields.io/badge/IK Analyzer-v7.7.0-red)![Springboot](https://img.shields.io/badge/Sprinboot-v2.3.4-blue?logo=spring)

ES简介不多说,Springboot高级有相关概述

## 1、Solr简介

​	Solr 是Apache下的一个顶级开源项目，采用Java开发，它是基于==Lucene==的全文搜索服务器。Solr提供了比Lucene更为丰富的查询语言，同时实现了可配置、可扩展，并对索引、搜索性能进行了优化Solr可以独立运行，运行在Jetty、Tomcat等这些Servlet容器中，Solr 索引的实现方法很简单，==用 POST方法向 Solr 服务器发送一个描述 Field 及其内容的 XML 文档，Solr根据xml文档添加、删除、更新索引==。Solr 搜索只需要发送 HTTP GET 请求，然后对 Solr 返回Xml、json等格式的查询结果进行解析，组织页面布局。Solr不提供构建UI的功能，Solr提供了一个管理界面，通过管理界面可以查询Solr的配置和运行情况。solr是基于lucene开发企业级搜索服务器，实际上就是封装了lucene。Solr是一个独立的企业级搜索应用服务器，==它对外提供类似于Web-service的API接口==。用户可以通过http请求，向搜索引擎服务器提交一定格式的文件，生成索引；也可以通过提出查找请求，并得到返回结果  

**solr与Es的区别**

**1、es基本是开箱即用（解压就可以用 ! ），非常简单。Solr安装略微复杂一丢丢！**
**2、Solr 利用 Zookeeper 进行分布式管理，而 Elasticsearch 自身带有分布式协调管理功能。**
**3、Solr 支持更多格式的数据，比如JSON、XML、CSV，而 Elasticsearch 仅支持json文件格式。**
**4、Solr 官方提供的功能更多，而 Elasticsearch 本身更注重于核心功能，高级功能多有第三方插件提供，例如图形化界面需要kibana友好支撑~!**
**5、Solr 查询快，但更新索引时慢（即插入删除慢），用于电商等查询多的应用；ES建立索引快（即查询慢），即实时性查询快，用于facebook新浪等搜索。Solr 是传统搜索应用的有力解决方案，但 Elasticsearch 更适用于新兴的实时搜索应用。**
**6、Solr比较成熟，有一个更大，更成熟的用户、开发和贡献者社区，而 Elasticsearch相对开发维护者较少，更新太快，学习使用成本较高。（趋势！）**  

## 2、ElasticSearch安装 (docker)

详情见 ==DIS.md==

### 3、安装ElasticSearch数据展示工具   (来自chrome扩展)

github上搜索elasticsearch-head 连接elasticsearch可以直接避免跨域问题和避免安装node.js环境

效果展示

![es-head](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202008/23/233941-851034.png)

### 4、了解ELK

​	ELK是==Elasticsearch==、==Logstash==、==Kibana==三大开源框架首字母大写简称。市面上也被成为==ElasticStack==。其中Elasticsearch是一个基于Lucene、分布式、通过Restful方式进行交互的近实时搜索平台框架。像类似百度、谷歌这种大数据全文搜索引擎的场景都可以使用Elasticsearch作为底层支持框架，可见Elasticsearch提供的搜索能力确实强大,市面上很多时候我们简称Elasticsearch为es。Logstash是ELK的中央数据流引擎，用于从不同目标（文件/数据存储/MQ）收集的不同格式数据，经过过滤后支持输出到不同目的地（文件/MQ/redis/elasticsearch/kafka等）。Kibana可以将elasticsearch的数据通过友好的页面展示出来，提供实时分析的功能。市面上很多开发只要提到ELK能够一致说出它是一个日志分析架构技术栈总称，但实际上ELK不仅仅适用于日志分析，它还可以支持其它任何数据分析和收集的场景，日志分析和收集只是更具有代表性。并非唯一性。  

### 5、安装Kibana

**Kibana简介**

​	==Kibana是一个针对Elasticsearch的开源分析及可视化平台，用来搜索、查看交互存储在Elasticsearch索引中的数据==。使用Kibana，可以通过各种图表进行高级数据分析及展示。Kibana让海量数据更容易理解。它操作简单，基于浏览器的用户界面可以快速创建仪表板（dashboard）实时显示Elasticsearch查询动态。设置Kibana非常简单。无需编码或者额外的基础架构，几分钟内就可以完成Kibana安装并启动Elasticsearch索引监测  

**Kibana安装(docker)**

==Kibana要求必须和ES版本一致，如果使用非docker安装还要自己配置node.js环境==

详情见 ==DIS.md==

访问效果

![kibana](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202008/23/233944-634079.png)

## 3、ElasticSearch核心概念

| Databases | ElasticSearch              |
| --------- | -------------------------- |
| 数据库    | 索引 index                 |
| 表        | 类型 type   **慢慢被弃用** |
| 行        | 文档 document              |
| 字段      | 属性field                  |

**物理设计**：elasticsearch 在后台把每个索引划分成多个分片，每分分片可以在集群中的不同服务器间迁移。默认自身就是一个集群！集群名称就是 elaticsearh  

**逻辑设计**：一个索引类型中，包含多个文档，比如说文档1，文档2。 当我们索引一篇文档时，可以通过这样的一各顺序找到 它: 索引 ▷ 类型 ▷ 文档ID ，通过这个组合我们就能索引到某个具体的文档。 注意:ID不必是整数，实际上它是个字 符串。  

**物理设计 ：节点和分片 如何工作**  

​	一个集群至少有一个节点，而一个节点就是一个elasticsearch进程，节点可以有多个索引。默认的，如果你创建索引，那么索引将会有个5个分片 ( primary shard ,又称主分片 ) 构成的，每一个主分片会有一个副本 ( replica shard ,又称复制分片 )  

![](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202008/23/233948-129583.png)

上图是一个有3个节点的集群，可以看到主分片和对应的复制分片都不会在同一个节点内，这样有利于某个节点挂掉 了，数据也不至于丢失。 实际上，一个分片是一个Lucene索引，一个包含==倒排索引==的文件目录，倒排索引的结构使 得elasticsearch在不扫描全部文档的情况下，就能告诉你哪些文档包含特定的关键字。

elasticsearch使用的是一种称为倒排索引的结构，采用Lucene倒排索作为底层。这种结构适用于快速的全文搜索， 一个索引由文档中所有不重复的列表构成，对于每一个词，都有一个包含它的文档列表。 例如，现在有两个文档， 每个文档包含如下内容：

```tcl
Study every day, good good up to forever # 文档1包含的内容
To forever, study every day, good good up # 文档2包含的内容
```

> **倒排索引**

**为了创建倒排索引，我们首先要将每个文档拆分成独立的词(或称为词条或者tokens)，然后创建一个包含所有不重 复的词条的排序列表，然后列出每个词条出现在哪个文档 :**  

| term    | doc_1 | doc_2 |
| ------- | ----- | ----- |
| Study   | √     | ×     |
| To      | x     | ×     |
| every   | √     | √     |
| forever | √     | √     |
| day     | √     | √     |
| study   | ×     | √     |
| good    | √     | √     |
| every   | √     | √     |
| to      | √     | ×     |
| up      | √     | √     |

现在，我们试图搜索 to forever，只需要查看包含每个词条的文档 就会得到以下结果：

| term    | doc_1 | doc_2 |
| ------- | ----- | ----- |
| to      | √     | ×     |
| forever | √     | √     |
| total   | 2     | 1     |

结果很明显，文档一包含的词更全面，因此它的相关性得分(score)也是最高的

**ES倒排索引的特点就是底层采用Lucene 的倒排索引，它实现了倒排索引后在不检索全文的时候就拿出你想要的数据，且数据因为经过倒排索引，他们都被分成了类似一组一组的，因此，只要查询的时候去指定的组即可找到所有的相关数据而不用排查全文。**

## 4、IK分词器

### 1、什么是Ik分词器

分词：即把一段中文或者别的划分成一个个的关键字，我们在搜索时候会把自己的信息进行分词，会把数据库中或者索引库中的数据进行分词，然后进行一个匹配操作，默认的中文分词是将每个字看成一个词，比如 “我爱jc” 会被分为"我","爱","j","c"，这显然是不符合要求的，所以我们需要安装==中文分词器ik==来解决这个问题  

**如果要使用中文，建议使用ik分词器！**
**IK提供了两个分词算法：ik_smart 和 ik_max_word，其中 ik_smart 为最少切分，ik_max_word为最细粒度划分**

### 2、安装IK(docker)

#### a、进入已经启动的ES容器

```shell
docker exec -it 容器名 /bin/bash
```

#### b、执行在线安装

```shell
./bin/elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.7.0/elasticsearch-analysis-ik-7.7.0.zip
```

==注意:Ik分词器的版本必须与ES一致==

#### c、查看安装结果

```shell
cd plugins/
```

#### d、退出并重启该容器使IK生效

```shell
exit
docker restart 容器名
```

#### e、测试分词器

两种不同的算法分词

- **Ik_smart :最少分词器，它将一句话拆分成中文词语，不是词语的拆成单字**

![](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202008/23/233953-490953.png)

- **Ik_max_word：最详细分词器，它将一段文本拆分成尽可能多的词，包含重复拆分，不是词语的拆成单字**

![](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202008/23/233954-180223.png)

**问题：如何自定义自己的词语，使之拆分时不拆分自定义词语，而把它当做一个词来看？**

#### f、自定义分词器分词词典

##### 1、定义自己的词典 并修改文件的编码格式为UTF-8

```shell
#确保你在config/analysis-ik/
vi tcl.dic
#改变文件编码格式
iconv -f GBK -t UTF-8 tcl.dic -o tcl.dic 
```

##### 2、添加自己的词典到ik分词器的xml配置文件中

```xml
vi IKAnalyzer.cfg.xml

<properties>
        <comment>IK Analyzer</comment>
		<!--在这里加上自定义的词典-->
    	<entry key="ext_dict">tcl.dic</entry>
        <entry key="ext_stopwords"></entry>
</properties>
```

##### 3、重启ES，使配置生效

```shell
exit
docker restart 容器名
```

##### 4、测试效果

![](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202008/23/233957-709027.png)

## 5、关于索引的操作

### a、基于Restful风格的请求

| method | url地址                                         | 描述                   |
| ------ | ----------------------------------------------- | ---------------------- |
| PUT    | localhost:9200/索引名称/类型名称/文档id         | 创建文档（指定文档id） |
| POST   | localhost:9200/索引名称/类型名称                | 创建文档（随机文档id） |
| POST   | localhost:9200/索引名称/类型名称/文档id/_update | 修改文档               |
| DELETE | localhost:9200/索引名称/类型名称/文档id         | 删除文档               |
| GET    | localhost:9200/索引名称/类型名称/文档id         | 查询文档通过文档id     |
| POST   | localhost:9200/索引名称/类型名称/_search        | 查询所有数据           |

### b、CRUD

#### 1、添加

语法：	**PUT /索引名/~类型名~/文档id   {请求体}**

示例：

```json
PUT /test1/type1/1
{
    "name":"tcl",
    "age":19,
    "bithday":"151234324",
    "shiro":2
}
```

返回结果：

```json
{
  "_index" : "test1",// 索引名
  "_type" : "type1",// 索引类型
  "_id" : "1",// id
  "_version" : 1,// 版本
  "result" : "created",// 状态：表示刚被创建
  "_shards" : {// 表示分片信息
    "total" : 2, 
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 0,
  "_primary_term" : 1
}
```

**也可以使用head插件查看具体信息**

下面来添加索引时，不带文档和类型，只指定属性类型，相当于数据库中创建空库空表

```json
PUT /索引名
{
  "mappings": {
    "properties": {
      "name": {"type": "text"},
      "age":{"type": "integer"},
      "bithday":{"type": "binary"},
      "money":{"type": "double"}
    }
  }
}
这里type的类型有很多种，详情见Kinbana提示
```

**获得索引类型规则**

```json
GET /索引名
```



#### 2、修改

##### a、第一种方式

**使用PUT来覆盖原有值**，如果有的值你没写，那么新覆盖的原本有的属性，覆盖后不会再有

语法：**PUT /索引名/_doc/你要修改的文档id   {请求体}**

示例：

```json
PUT /test1/_doc/1
{
    "name":"仝成龙"
}
```

返回结果：

```json
{
  "_index" : "test1",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 2,	// 版本发生改变，
  "result" : "updated", // 状态发生改变，表示完成了update操作
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 1,
  "_primary_term" : 1
}

```

##### b、第二种方式

使用_update发送POST请求来完成更新操作，这个是指定修改值，原有值不会被覆盖

语法：**POST /索引名/_doc/文档id/__update   {“doc":{请求体}}**

示例：

```json
POST /test1/_doc/1/_update
{
    "doc":{
        "name":"TCL"
    }
}
```

返回结果：

```json
{
  "_index" : "test1",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 3, // 版本号发生变化
  "result" : "updated",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 2,
  "_primary_term" : 1
}
```

#### 3、查询

**查询文档索引信息**

语法：**GET /索引名/_doc/文档id** 

示例：

```json
GET /test2/_doc/1
```

返回结果：

```json
{
  "_index" : "test2",
  "_type" : "_doc",
  "_id" : "1",
  "found" : false
}
```



##### 4、删除

语法：**DELETE /索引名**

示例：**如果后面不跟类型和文档名，就会删除一整个索引**

```json
DELETE /test3
```

返回结果：

```json
{
  "acknowledged" : true //代表删除成功
}
```

## 6、 关于文档的操作 (重点)

### 1、添加

语法：PUT /索引名/类型/文档id

示例：

```json
PUT /tcl/user/1
{
    "name": "TCL",
    "age": 19,
    "desc": "一顿操作猛如虎，一看工资2500",
    "tags": ["技术宅","温暖","直男"]
}
```

返回结果：

```json
{
  "_index" : "tcl",
  "_type" : "user",
  "_id" : "1",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 0,
  "_primary_term" : 1
}
```

### 2、获取

> 简单查询

```json
GET /索引/类型/文档id
```

> 查询结果

```json
{
  "_index" : "tcl",
  "_type" : "user",
  "_id" : "1",
  "_version" : 2,
  "_seq_no" : 1,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "name" : "tcl2",
    "age" : 32,
    "desc" : "very good12",
    "tags" : [
      "xxx",
      "yyy",
      "zzz"
    ]
  }
}
```

> 2、另一种GET方式

```json
GET /tcl/user/_search?q=name:tcl6
```

**精准匹配，不是模糊匹配**

### 3、修改

> 1、PUT  /索引/类型/文档id {}   ||   POST /索引/类型/文档id {}

示例：

```json
PUT /tcl/user/1 
{
    "name":"tcl3"
}
```

结果：

```json
{
  "_index" : "tcl",
  "_type" : "user",
  "_id" : "1",
  "_version" : 3,
  "result" : "updated",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 2,
  "_primary_term" : 1
}

```

**注意：凡是被PUT更改过的数据，只要是原来有数据，新修改时没加的全部修改为空**



> 2、POST   /索引/类型/文档id/_update {}   **更推荐**

示例：

```json
POST /tcl/user/1/_update
{
  "doc":{
    "name":"tcl6"
  }
}
```

结果

```json
{
  "_index" : "tcl",
  "_type" : "user",
  "_id" : "1",
  "_version" : 8,
  "result" : "updated",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 7,
  "_primary_term" : 1
}
```

**这样就只会修改你所填写的字段的数据，其他每填写到的就不会更新**

### 4、复杂查询 

#### 1、query

> 单条件精准匹配

```json
GET tcl/user/_search
{
  "query": {
    "match": {
      "name": "tcl6"
    }
  }
}
```

>对结果地段的过滤    似  ：select id,name from xxx

```json
GET tcl/user/_search
{
  "query": {
    "match": {
      "name": "tcl2"
    }
  }
  , "_source": ["age","name"] //标注只需返回指定的字段
}
```

#### 2、排序

```json
GET tcl/user/_search
{
  "sort": [
    {
      "age": {
        "order": "desc"
      }
    }
  ]
}
```

#### 3、分页

```json
GET tcl/user/_search
{
  "from":0,   //从哪开始
  "size":1    //一页多少条数据
}

```

#### 4、bool

> 必须符合   must   似   MYSQL    and

```json
GET tcl/user/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "name": "tcl2"
          }
        },
        {
          "match": {
            "age": 32
          }          
        }
      ]
    }
  }
}
//所有条件必须都符合
```

> should   似  MYSQL  or  只要有一个符合就可以

```json
GET tcl/user/_search
{
  "query": {
    "bool": {
      "should": [
        {
          "match": {
            "name": "tcl2"
          }
        },
        {
          "match": {
            "age": 1
          }          
        }
      ]
    }
  }
}
```

> must_not  不能符合

```json
GET tcl/user/_search
{
  "query": {
    "bool": {
      "must_not": [
        {
          "match": {
            "name": "tcl2"
          }
        },
        {
          "match": {
            "age": 1
          }          
        }
      ]
    }
  }
}
```

#### 5、Filter

> filter 支持多条件过滤

```json
GET tcl/user/_search
{
  "query": {
    "bool": {
      "filter": [
        {
          "range": {
            "age": {
              "gte": 1,
              "lte": 20
            }
          }
        }
      ]
    }
  }
}

// gt gte lt lte
```

#### 6、多条件查询

```json
GET tcl/user/_search
{
  "query": {
    "match": {
      "tags": "xxx yyy2"  //使用空格将条件隔开
    }
  }
}
```

#### 7、高亮查询

```json
GET tcl2/_search
{
  
  "query": {
    "match": {
      "name": "tcl"
    }
  }, 
  "highlight": {
    "fields": {
      "name": {}
    }
  }
}
```

最终查询的结果会被加上<em></em>标签

我们也可以自定义标签  **pre_tags   post_targs**

```json
GET tcl2/_search
{
  
  "query": {
    "match": {
      "name": "tcl"
    }
  }, 
  "highlight": {
    "pre_tags": "<p style = 'color:red;font-size:15px;'>",
    "post_tags": "</p>",
    "fields": {
      "name": {}
    }
  }
}
```

## 7、Springboot整合ES

### 1、导入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
</dependency>
```

### 2、解决ES版本不符合服务器ES版本的问题

```xml
<!--定义es版本-->
<properties>
    <elasticsearch.version>7.7.0</elasticsearch.version>
</properties>
```

### 3、整合使用

[ES官方文档地址 ](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/7.7/java-rest-high.html)

#### 1、初始化ES

```java
/**
 * @author kate
 * @Date 2020/6/17 9:00
 */
@Configuration
public class ESConfig {

    @Bean
    public RestHighLevelClient restHighLevelClient(){
        RestHighLevelClient client = new RestHighLevelClient(
            RestClient.builder(
                        new HttpHost("39.98.20.116", 9200, "http")));
        return client;
    }
}
```

#### 2、操作API

##### a、索引

> 创建索引  **老式**

```java
CreateIndexRequest request = new CreateIndexRequest("idea");
        CreateIndexResponse response = rest.indices().create(request, RequestOptions.DEFAULT);
```

**注：索引的名字必须小写**

##### b、文档

###### 1、创建索引并创建文档

> 1、使用String字符串拼接Json创建

```java
IndexRequest request = new IndexRequest("idea");
request.id("1");
String jsonString = "{" +
    "\"user\":\"kimchy\"," +
    "\"postDate\":\"2013-01-30\"," +
    "\"message\":\"trying out Elasticsearch\"" +
    "}";
request.source(jsonString, XContentType.JSON);


//执行  同步
IndexResponse indexResponse = rest.index(request, RequestOptions.DEFAULT);
System.out.println(indexResponse);
```

> 2、使用Map创建

```json
Map<String, Object> jsonMap = new HashMap<>();
        jsonMap.put("user", "kimchy");
        jsonMap.put("postDate", new Date());
        jsonMap.put("message", "trying out Elasticsearch");
        IndexRequest indexRequest = new IndexRequest("posts")
                .id("1").source(jsonMap);

        indexRequest.source(indexRequest, XContentType.JSON);


        //执行  同步
        IndexResponse indexResponse = rest.index(indexRequest, RequestOptions.DEFAULT);

        System.out.println(indexResponse);
```

> 3、使用内置帮助器实现

```java
XContentBuilder builder = XContentFactory.jsonBuilder();
builder.startObject();
{
    builder.field("user", "kimchy");
    builder.timeField("postDate", new Date());
    builder.field("message", "trying out Elasticsearch");
}
builder.endObject();
IndexRequest indexRequest = new IndexRequest("posts")
    .id("1").source(builder);
```

> 4、单据源作为密钥对提供

```java
IndexRequest indexRequest = new IndexRequest("posts")
    .id("1")
    .source("user", "kimchy",
        "postDate", new Date(),
        "message", "trying out Elasticsearch"); 
```

> 返回结果

```json
IndexResponse[
    index=idea,
    type=_doc,
    id=1,
    version=1,
    result=created,
    seqNo=0,
    primaryTerm=1,
    shards={
        "total":2,
        "successful":1,
        "failed":0
}]
```

###### 2、获得文档信息

```java
GetRequest getRequest = new GetRequest("idea","1");

        //同步执行
        GetResponse getResponse = client.get(getRequest, RequestOptions.DEFAULT);

        //获取结果文档
        Map<String, Object> sourceAsMap = getResponse.getSourceAsMap();
        Set<Map.Entry<String, Object>> entries = sourceAsMap.entrySet();
        Iterator<Map.Entry<String, Object>> iterator = entries.iterator();
        while(iterator.hasNext()){
            System.out.println(iterator.next());
        }
```

###### 3、查看某个文档是否存在

```java
GetRequest getRequest = new GetRequest(
                "idea",
                "1");
        getRequest.fetchSourceContext(new FetchSourceContext(false));
        getRequest.storedFields("_none_");
        boolean exists = client.exists(getRequest, RequestOptions.DEFAULT);

        System.out.println(exists);
```

###### 4、删除

```java
DeleteRequest request = new DeleteRequest(
                "idea",
                "1");

        DeleteResponse deleteResponse = client.delete(
                request, RequestOptions.DEFAULT);

        System.out.println(deleteResponse);
```

> 返回结果

```java
DeleteResponse[index=idea,type=_doc,id=1,version=2,result=deleted,shards=ShardInfo{total=2, successful=1, failures=[]}]
```

###### 5、更新

> 1、使用脚本更新(一旦有的字段更新的时候没有值，那么就会把它更为空)

```java
Map<String, Object> parameters = singletonMap("count", 4); 

Script inline = new Script(ScriptType.INLINE, "painless",
        "ctx._source.field += params.count", parameters);  
request.script(inline); 
```

> 2、使用现有数据进行更新(只更新传入的指定的字段) 它同样可以使用添加时的那几种方式，map、json、内置、等

```java
XContentBuilder builder = XContentFactory.jsonBuilder();
        builder.startObject();
        {
            builder.field("age", 30);
        }
        builder.endObject();

        UpdateRequest request = new UpdateRequest(
                "idea",
                "1").doc(builder);

        UpdateResponse updateResponse = client.update(
                request, RequestOptions.DEFAULT);

        System.out.println(updateResponse);
```

> 更新结果

```json
UpdateResponse[index=idea,type=_doc,id=1,version=2,seqNo=3,primaryTerm=1,result=updated,shards=ShardInfo{total=2, successful=1, failures=[]}]
```

###### 6、多获取(文档)

```java
 MultiGetRequest request = new MultiGetRequest();
        request.add(
                new MultiGetRequest.Item("idea","1")
        );
        request.add(
                new MultiGetRequest.Item("idea2","1")
        );

        //异步执行
        MultiGetResponse response = client.mget(request, RequestOptions.DEFAULT);

        Iterator<MultiGetItemResponse> iterator = response.iterator();
        while(iterator.hasNext()){
            System.out.println(iterator.next().getResponse());
        }

```

###### 7、批处理

**它可用于使用单个请求执行多个索引、更新和/或删除操作**

批量创建索引并 添加值

```java
BulkRequest request = new BulkRequest(); 
request.add(new IndexRequest("posts").id("1")  
        .source(XContentType.JSON,"field", "foo"));
request.add(new IndexRequest("posts").id("2")  
        .source(XContentType.JSON,"field", "bar"));
request.add(new IndexRequest("posts").id("3")  
        .source(XContentType.JSON,"field", "baz"));
```

**增删改**

```java
BulkRequest request = new BulkRequest();
        request.add(new IndexRequest("idea3").id("1").source(XContentType.JSON,"name","xxx"));
        request.add(new UpdateRequest("idea2","1").doc(XContentType.JSON,"name","yyy"));
        request.add(new DeleteRequest("idea1","1"));

        BulkResponse bulkResponses = client.bulk(request, RequestOptions.DEFAULT);


        for (BulkItemResponse bulkItemResponse : bulkResponses) {
            DocWriteResponse itemResponse = bulkItemResponse.getResponse();

            switch (bulkItemResponse.getOpType()) {
                case INDEX:
                case CREATE:
                    IndexResponse indexResponse = (IndexResponse) itemResponse;
                    //这里是插入的响应
                    System.out.println("插入："+indexResponse);
                    break;
                case UPDATE:
                    UpdateResponse updateResponse = (UpdateResponse) itemResponse;
                    //这里是修改的响应
                    System.out.println("修改："+updateResponse);
                    break;
                case DELETE:
                    DeleteResponse deleteResponse = (DeleteResponse) itemResponse;
                    //这里是删除的响应，没有值返回 null
                    System.out.println("删除："+deleteResponse);
            }
        }
    }
```

##### c、搜索

###### 1、查询所有

```java
SearchRequest searchRequest = new SearchRequest("idea");
        SearchSourceBuilder searchSourceBuilder = new SearchSourceBuilder();
        searchSourceBuilder.query(QueryBuilders.matchAllQuery());//查询所有指定索引的所有数据
        searchRequest.source(searchSourceBuilder);


        SearchResponse searchResponse = client.search(searchRequest, RequestOptions.DEFAULT);

        System.out.println(searchResponse);
```

###### 2、查询封装了条件的

例如分页

```java
SearchRequest searchRequest = new SearchRequest("idea");
        SearchSourceBuilder searchSourceBuilder = new SearchSourceBuilder();

        searchSourceBuilder.from(0);
        searchSourceBuilder.size(2);
        searchRequest.source(searchSourceBuilder);

        SearchResponse searchResponse = client.search(searchRequest, RequestOptions.DEFAULT);

        System.out.println(searchResponse.toString());
```

[更多详情见官网文档](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/7.7/java-rest-high-search.html)

## 8、ES实战京东

### 1、爬取数据

**使用Jsoup包来解析获取网页html数据**

- 拉取依赖

	```xml
	<!-- https://mvnrepository.com/artifact/org.jsoup/jsoup -->
	<dependency>
	    <groupId>org.jsoup</groupId>
	    <artifactId>jsoup</artifactId>
	    <version>1.13.1</version>
	</dependency>
	```

- 获取数据

	```java
	
	```

	
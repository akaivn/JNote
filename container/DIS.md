> 说明

**此文档的所有ip，port都需要替换为安装时对应的ip，port**

#### Common Comment

```shell
systemctl start docker 

systemctl stop docker 

systemctl restart docker

docker pull [image]

docker search

docker images

docker ps

docker ps -a

docker rm [container]

docker rm -f [container]

docker rmi [image]

docker rmi -f [image]

docker logs -f [container]

docker exec -it [container] /bin/bash

docker run -it [image] /bin/bash

docker start [container]

docker stop [container]

docker restart [container]

apt-get update

apt-get install vim
```

#### Docker

##### 1、检查内核版本

必须是3.10及以上

```shell
uname -r
```

##### 2、安装docker

```shell
yum install docker -y
```

##### 3、启动

```shell
systemctl start docker
```

##### 4、查看版本

```shell
docker -v

Docker version 1.12.6, build 3e8e77d/1.12.6
```

##### 5、开机启动

```shell
$ systemctl enable docker
# 提示如下
# Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service
```

##### 6、配置阿里云镜像仓库

```shell
1、访问自己的阿里云地址为
https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors

2、粘贴类似于如下命令放到shell里运行即可

sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  xxx
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

##### 7、停止

```shell
systemctl stop docker
```

#### MySQL

##### 1、拉取镜像

```shell
docker pull mysql:5.6
```

或

```shell
docker pull mysql
```

##### 2、运行命令

```shell
docker run -p 3306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=kate -d mysql:[tag] --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
```

##### 3、是否挂载

```http
可选，如果挂载，可选择挂载配置文件 即 my.ini 或 my.conf
```

##### 4、访问

```txt
Navicat 直连即可
```

#### Redis

##### 1、拉取镜像

```shell
docker pull redis:5.0
```

##### 2、运行命令

```shell
docker run -d -p 6379:6379 --name redis -v /data/redis/conf/redis.conf:/usr/local/etc/redis/redis.conf -v /data/redis/data:/data redis:5.0 --appendonly yes
```

##### 3、与内置客户端容器交互

```shell
docker exec -it 容器名 redis-cli -a 密码
```

##### 4、是否挂载

```http
website:https://pan.baidu.com/s/1LhlRaOHGHf27RXu2uXmIFw
code:kate
```

##### 5、访问

```txt
RedisDesktopManager直连
```

#### Tomcat 

##### 1、拉取镜像

```shell
docker pull tomcat:9.0
```

##### 2、运行命令

```shell
docker run -d --name tomcat -p 8080:8080 -v /data/tomcat/webapps:/usr/local/tomcat/webapps 镜像名:[Tag]
```

*到tomcat8以后，通过浏览器访问默认为8080页面，因为tomcat的文件夹的webapps文件夹里没有任何文件，它默认的测试文件在webapps.dist下*

##### 3、是否挂载

```http
website: https://pan.baidu.com/s/1w_aZRsQcSf0hLWyOVL2DpA
code: kate
```

##### 4、访问

```http
http://ip:port
```

#### Nginx

##### 1、拉取镜像

```shell
docker pull nginx
```

##### 2、运行命令

```shell
docker run -d -p 80:80 --name nginx -v /data/nginx/conf:/etc/nginx/conf -v /data/nginx/nginx.conf:/etc/nginx/nginx.conf nginx
```

##### 3、是否挂载

```http
website: https://pan.baidu.com/s/1lkNNh4gCvonBNHzE8ivaow
code: kate
```

##### 4、访问

```http
http://ip
```

#### Jenkins

##### 1、拉取镜像

```shell
docker pull jenkins/jenkins
```

##### 2、运行命令

```shell
docker run -u root -d -p 50000:50000 -p 56354:8080 --name jenkins -v /data/jenkins:/var/jenkins_home [镜像id]
```

##### 3、访问

```http
http://ip:56354
```

##### 4、账号/密码 (默认)

```shell
跟随指引
可从挂载文件 /data/jenkins/secrets/initialAdminPassword 处查看
或
docker logs -f [容器id | 容器名]
```

##### 5、是否创建用户

```txt
如果使用 admin 账户继续、此后账户密码为 admin/默认生成密码
否则，创建用户需自己牢记密码和分配对应的权限
```

#### RabbitMQ

##### 1、拉取镜像

```shell
docker pull rabbitmq:3.9.7-management
```

##### 2、运行命令

```shell
docker run -d -p 5672:5672 -p 15672:15672 --name rabbitmq 镜像id
```

##### 3、访问

```http
http://ip:15672
```

##### 4、账号/密码 (默认)

```txt
guest/guest
```

#### ElasticSearch

##### 1、拉取镜像

```shell
docker pull elasticsearch:7.7.0
```

##### 2、运行命令

注意：**elasticsearch初始化默认开启2G的堆内存空间，我们测试期间用不了这么大的内存，所以限制一下堆内存的使用并**

开放两个端口 默认为9200  9300为分布式的开放端口

```shell
docker run -d -e ES_JAVA_OPTS="-Xms256m -Xmx256m" -e "discovery.type=single-node" -p 9200:9200 -p 9300:9300 --name elasticsearch elasticsearch:7.7.0
```

#### Kibanna

##### 1、拉取镜像

```shell
docker pull kibana:7.7.0
```

##### 2、运行镜像产生容器

```shell
docker run --name kibana -e ELASTICSEARCH_HOSTS=http://ip:9200 -p 5601:5601 -d kibana:7.7.0
```

##### 3、访问

```http
http://ip:5601
```

注：**Kibana要求必须和ES版本一致**

#### Portainer

##### 1、拉取镜像

```shell
docker pull portainer/portainer
```

##### 2、运行命令

```shell
docker run -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock --name prtainer  portainer/portainer
```

##### 3、访问

```shell
http://ip:9000
```

##### 4、首次进入

创建密码账号，选择连接Local docker 或 Remote docker

#### Zipkin

##### 1、拉取镜像

```shell
docker pull openzipkin/zipkin
```

##### 2、运行镜像

```shell
docker run -d -p 9411:9411 --name zipkin openzipkin/zipkin 
```

##### 3、访问

```http
http://riskate.club:9411/zipkin/
```

#### FastDFS

##### 1、拉取镜像

```shell
docker pull delron/fastdfs
```

##### 2、启动tracker，挂载文件

```shell
docker run -d --network=host --privileged=true --name tracker -v /data/tracker:/var/fdfs docker.io/delron/fastdfs tracker
```

##### 3、启动storage，挂载文件

```shell
docker run -d --network=host --name storage -e TRACKER_SERVER=ip:22122 -v /data/storage:/var/fdfs -e GROUP_NAME=default_group delron/fastdfs storage
```

##### 4、进入storage容器命令

```shell
docker exec -it storage bash
```

##### 5、端口说明

```txt
8888是默认的nginx代理端口

23000是storage服务端口

22122是tracker服务端口
```

##### 6、使用命令行上传文件到fastdfs

```shell
在宿主机的挂载目录/data/storage目录下放置一个文件： 1.jpg

cd /var/fdfs

运行：
/usr/bin/fdfs_upload_file /etc/fdfs/client.conf 1.jpg
返回文件在fastdfs中的：组+/+路径+/+文件名.后缀
```

##### 7、访问刚刚上传的文件

```http
http://ip:8888/刚返回的那一串地址
```

#### Nacos

##### 1、拉取镜像

```shell
docker pull nacos/nacos-server:1.1.4
```

##### 2、运行MySQL，执行如下脚本

*docker内启动nacos时，不能再使用常规的内嵌式数据库 derby，我们选择MySQL数据库提供给Nacos作为存储服务*

```sql
/*
 * Copyright 1999-2018 Alibaba Group Holding Ltd.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *      http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

/******************************************/
/*   数据库全名 = nacos_config   */
CREATE DATABASE nacos_conif;
/*   表名称 = config_info   */
/******************************************/
CREATE TABLE `config_info` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
  `data_id` varchar(255) NOT NULL COMMENT 'data_id',
  `group_id` varchar(255) DEFAULT NULL,
  `content` longtext NOT NULL COMMENT 'content',
  `md5` varchar(32) DEFAULT NULL COMMENT 'md5',
  `gmt_create` datetime NOT NULL DEFAULT '2010-05-05 00:00:00' COMMENT '创建时间',
  `gmt_modified` datetime NOT NULL DEFAULT '2010-05-05 00:00:00' COMMENT '修改时间',
  `src_user` text COMMENT 'source user',
  `src_ip` varchar(20) DEFAULT NULL COMMENT 'source ip',
  `app_name` varchar(128) DEFAULT NULL,
  `tenant_id` varchar(128) DEFAULT '' COMMENT '租户字段',
  `c_desc` varchar(256) DEFAULT NULL,
  `c_use` varchar(64) DEFAULT NULL,
  `effect` varchar(64) DEFAULT NULL,
  `type` varchar(64) DEFAULT NULL,
  `c_schema` text,
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_configinfo_datagrouptenant` (`data_id`,`group_id`,`tenant_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='config_info';

/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = config_info_aggr   */
/******************************************/
CREATE TABLE `config_info_aggr` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
  `data_id` varchar(255) NOT NULL COMMENT 'data_id',
  `group_id` varchar(255) NOT NULL COMMENT 'group_id',
  `datum_id` varchar(255) NOT NULL COMMENT 'datum_id',
  `content` longtext NOT NULL COMMENT '内容',
  `gmt_modified` datetime NOT NULL COMMENT '修改时间',
  `app_name` varchar(128) DEFAULT NULL,
  `tenant_id` varchar(128) DEFAULT '' COMMENT '租户字段',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_configinfoaggr_datagrouptenantdatum` (`data_id`,`group_id`,`tenant_id`,`datum_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='增加租户字段';


/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = config_info_beta   */
/******************************************/
CREATE TABLE `config_info_beta` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
  `data_id` varchar(255) NOT NULL COMMENT 'data_id',
  `group_id` varchar(128) NOT NULL COMMENT 'group_id',
  `app_name` varchar(128) DEFAULT NULL COMMENT 'app_name',
  `content` longtext NOT NULL COMMENT 'content',
  `beta_ips` varchar(1024) DEFAULT NULL COMMENT 'betaIps',
  `md5` varchar(32) DEFAULT NULL COMMENT 'md5',
  `gmt_create` datetime NOT NULL DEFAULT '2010-05-05 00:00:00' COMMENT '创建时间',
  `gmt_modified` datetime NOT NULL DEFAULT '2010-05-05 00:00:00' COMMENT '修改时间',
  `src_user` text COMMENT 'source user',
  `src_ip` varchar(20) DEFAULT NULL COMMENT 'source ip',
  `tenant_id` varchar(128) DEFAULT '' COMMENT '租户字段',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_configinfobeta_datagrouptenant` (`data_id`,`group_id`,`tenant_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='config_info_beta';

/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = config_info_tag   */
/******************************************/
CREATE TABLE `config_info_tag` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
  `data_id` varchar(255) NOT NULL COMMENT 'data_id',
  `group_id` varchar(128) NOT NULL COMMENT 'group_id',
  `tenant_id` varchar(128) DEFAULT '' COMMENT 'tenant_id',
  `tag_id` varchar(128) NOT NULL COMMENT 'tag_id',
  `app_name` varchar(128) DEFAULT NULL COMMENT 'app_name',
  `content` longtext NOT NULL COMMENT 'content',
  `md5` varchar(32) DEFAULT NULL COMMENT 'md5',
  `gmt_create` datetime NOT NULL DEFAULT '2010-05-05 00:00:00' COMMENT '创建时间',
  `gmt_modified` datetime NOT NULL DEFAULT '2010-05-05 00:00:00' COMMENT '修改时间',
  `src_user` text COMMENT 'source user',
  `src_ip` varchar(20) DEFAULT NULL COMMENT 'source ip',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_configinfotag_datagrouptenanttag` (`data_id`,`group_id`,`tenant_id`,`tag_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='config_info_tag';

/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = config_tags_relation   */
/******************************************/
CREATE TABLE `config_tags_relation` (
  `id` bigint(20) NOT NULL COMMENT 'id',
  `tag_name` varchar(128) NOT NULL COMMENT 'tag_name',
  `tag_type` varchar(64) DEFAULT NULL COMMENT 'tag_type',
  `data_id` varchar(255) NOT NULL COMMENT 'data_id',
  `group_id` varchar(128) NOT NULL COMMENT 'group_id',
  `tenant_id` varchar(128) DEFAULT '' COMMENT 'tenant_id',
  `nid` bigint(20) NOT NULL AUTO_INCREMENT,
  PRIMARY KEY (`nid`),
  UNIQUE KEY `uk_configtagrelation_configidtag` (`id`,`tag_name`,`tag_type`),
  KEY `idx_tenant_id` (`tenant_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='config_tag_relation';

/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = group_capacity   */
/******************************************/
CREATE TABLE `group_capacity` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `group_id` varchar(128) NOT NULL DEFAULT '' COMMENT 'Group ID，空字符表示整个集群',
  `quota` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '配额，0表示使用默认值',
  `usage` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '使用量',
  `max_size` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '单个配置大小上限，单位为字节，0表示使用默认值',
  `max_aggr_count` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '聚合子配置最大个数，，0表示使用默认值',
  `max_aggr_size` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '单个聚合数据的子配置大小上限，单位为字节，0表示使用默认值',
  `max_history_count` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '最大变更历史数量',
  `gmt_create` datetime NOT NULL DEFAULT '2010-05-05 00:00:00' COMMENT '创建时间',
  `gmt_modified` datetime NOT NULL DEFAULT '2010-05-05 00:00:00' COMMENT '修改时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_group_id` (`group_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='集群、各Group容量信息表';

/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = his_config_info   */
/******************************************/
CREATE TABLE `his_config_info` (
  `id` bigint(64) unsigned NOT NULL,
  `nid` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `data_id` varchar(255) NOT NULL,
  `group_id` varchar(128) NOT NULL,
  `app_name` varchar(128) DEFAULT NULL COMMENT 'app_name',
  `content` longtext NOT NULL,
  `md5` varchar(32) DEFAULT NULL,
  `gmt_create` datetime NOT NULL DEFAULT '2010-05-05 00:00:00',
  `gmt_modified` datetime NOT NULL DEFAULT '2010-05-05 00:00:00',
  `src_user` text,
  `src_ip` varchar(20) DEFAULT NULL,
  `op_type` char(10) DEFAULT NULL,
  `tenant_id` varchar(128) DEFAULT '' COMMENT '租户字段',
  PRIMARY KEY (`nid`),
  KEY `idx_gmt_create` (`gmt_create`),
  KEY `idx_gmt_modified` (`gmt_modified`),
  KEY `idx_did` (`data_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='多租户改造';


/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = tenant_capacity   */
/******************************************/
CREATE TABLE `tenant_capacity` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `tenant_id` varchar(128) NOT NULL DEFAULT '' COMMENT 'Tenant ID',
  `quota` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '配额，0表示使用默认值',
  `usage` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '使用量',
  `max_size` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '单个配置大小上限，单位为字节，0表示使用默认值',
  `max_aggr_count` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '聚合子配置最大个数',
  `max_aggr_size` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '单个聚合数据的子配置大小上限，单位为字节，0表示使用默认值',
  `max_history_count` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '最大变更历史数量',
  `gmt_create` datetime NOT NULL DEFAULT '2010-05-05 00:00:00' COMMENT '创建时间',
  `gmt_modified` datetime NOT NULL DEFAULT '2010-05-05 00:00:00' COMMENT '修改时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_tenant_id` (`tenant_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='租户容量信息表';


CREATE TABLE `tenant_info` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
  `kp` varchar(128) NOT NULL COMMENT 'kp',
  `tenant_id` varchar(128) default '' COMMENT 'tenant_id',
  `tenant_name` varchar(128) default '' COMMENT 'tenant_name',
  `tenant_desc` varchar(256) DEFAULT NULL COMMENT 'tenant_desc',
  `create_source` varchar(32) DEFAULT NULL COMMENT 'create_source',
  `gmt_create` bigint(20) NOT NULL COMMENT '创建时间',
  `gmt_modified` bigint(20) NOT NULL COMMENT '修改时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_tenant_info_kptenantid` (`kp`,`tenant_id`),
  KEY `idx_tenant_id` (`tenant_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='tenant_info';

CREATE TABLE users (
	username varchar(50) NOT NULL PRIMARY KEY,
	password varchar(500) NOT NULL,
	enabled boolean NOT NULL
);

CREATE TABLE roles (
	username varchar(50) NOT NULL,
	role varchar(50) NOT NULL,
	constraint uk_username_role UNIQUE (username,role)
);

CREATE TABLE permissions (
    role varchar(50) NOT NULL,
    resource varchar(512) NOT NULL,
    action varchar(8) NOT NULL,
    constraint uk_role_permission UNIQUE (role,resource,action)
);

INSERT INTO users (username, password, enabled) VALUES ('nacos', '$2a$10$EuWPZHzz32dJN7jexM34MOeYirDdFAZm2kuWj7VEOJhhZkDrxfvUu', TRUE);

INSERT INTO roles (username, role) VALUES ('nacos', 'ROLE_ADMIN');
```

##### 3、运行镜像（Old）

```shell
docker run -d -e PREFER_HOST_MODE=ip -e MODE=standalone -e SPRING_DATASOURCE_PLATFORM=mysql -e MYSQL_MASTER_SERVICE_HOST=ip -e MYSQL_MASTER_SERVICE_PORT=3306 -e MYSQL_MASTER_SERVICE_USER=root -e MYSQL_MASTER_SERVICE_PASSWORD=kate -e MYSQL_MASTER_SERVICE_DB_NAME=nacos_config -e MYSQL_SLAVE_SERVICE_HOST=ip -e MYSQL_SLAVE_SERVICE_PORT=3306 -v /data/nacos/logs:/home/nacos/logs -p 8848:8848 --name nacos nacos/nacos-server:1.1.4
```
##### 3、运行镜像（New）
```shell
docker run -d \
	-e PREFER_HOST_MODE=ip/hostname \
	-e MODE=standalone \
	-e SPRING_DATASOURCE_PLATFORM=mysql \
	-e MYSQL_SERVICE_HOST=ip/hostname \
	-e MYSQL_SERVICE_PORT=3306 \
	-e MYSQL_SERVICE_USER=username \
	-e MYSQL_SERVICE_PASSWORD=password \
	-e MYSQL_SERVICE_DB_NAME=表名 \
	-v /data/nacos/logs:/home/nacos/logs \
	-p 8848:8848 \
	--name nacos nacos/nacos-server
```
##### 参数说明
```txt
属性名称    描述  选项
MODE    系统启动方式: 集群/单机   cluster/standalone默认 cluster
NACOS_SERVERS   nacos cluster address   p1:port1空格ip2:port2 空格ip3:port3
PREFER_HOST_MODE    支持IP还是域名模式  hostname/ip 默认 ip
NACOS_SERVER_PORT   Nacos 运行端口  默认 8848
NACOS_SERVER_IP 多网卡模式下可以指定IP    
SPRING_DATASOURCE_PLATFORM  standalone support mysql    mysql / 空 默认:空
MYSQL_SERVICE_HOST  mysql host  
MYSQL_SERVICE_PORT  mysql database port 默认 : 3306
MYSQL_SERVICE_DB_NAME   mysql database name 
MYSQL_SERVICE_USER  username of database    
MYSQL_SERVICE_PASSWORD  password of database
```
详情见：https://github.com/nacos-group/nacos-docker/blob/master/README_ZH.md

##### 4、访问

```http
http://riskate.club:8848/nacos
```

`account and password: [nacos nacos]`

##### 相关问题

**Failed to obtain JDBC Connection; nested exception is org.apache.commons.dbcp.SQLNestedException: Cannot create PoolableConnectionFactory (Could not create connection to database server. Attempted reconnect 3 times. Giving up.)**

这是因为Nacos自带MySQL链接版本太低，解决方法如下：

**docker外可使用如下方式解决**

在nacos目录下新建/plugins/mysql目录，将mysql对应版本的mysql-connection的jar包放到该文件夹中

重新启动nacos即可

#### Sentinel

##### 1、拉取镜像

```shell
docker pull bladex/sentinel-dashboard
```

##### 2、运行命令

```shell
docker run --name sentinel -d -p 8858:8858 bladex/sentinel-dashboard
```

##### 3、访问

```http
http://riskate.club:8858
```

`account and password: [sentinel sentinel]`

#### Seata

##### 1、拉取镜像

```shell
docker pull seataio/seata-server
```

##### 2、运行命令

```shell
docker run -d --name seata -p 8091:8091 -e SEATA_CONFIG_NAME=file:/root/seata-config/registry -v /data/seata/:/root/seata-config --privileged=true seataio/seata-server
```

-e 为环境变量配置，后面的值表示使用挂载的那个目录的配置文件

更多环境变量配置可见：[Seata环境变量配置](http://seata.io/zh-cn/docs/ops/deploy-by-docker.html)

##### 3、是否挂载

```http
website: 链接：https://pan.baidu.com/s/1FyOgGzKnu7J63KLP6L0OSQ
code: kate
```

##### 4、访问

登录Nacos界面，服务列表处注册有seata-server的服务即表示成功

#### Code6-server

**code6-server为根据关键字扫描github的系统，语言为php**

该开源项目地址为：[code6-server](https://github.com/4x99/code6)

##### 1、运行命令

```shell
docker run -d -p 666:80 -e MYSQL_HOST=ip -e MYSQL_PORT=3306 -e MYSQL_DATABASE=github -e MYSQL_USERNAME=root -e MYSQL_PASSWORD=kate --name code6-server code6
```

##### 2、访问

```http
http:ip:666
```


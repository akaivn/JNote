# Jenkins

<p align = "center">
    <a href="https://www.jenkins.io/" target="_Blank"><img src="https://img.shields.io/badge/Jenkins-2.263.3 LTS-orange?logo=Jenkins" alt="Jenkins"/></a>
</p>

## Introduction

### Jenkins是什么

Jenkins是一款开源 CI&CD 软件，用于自动化各种任务，包括构建、测试和部署软件。

Jenkins 支持各种运行方式，可通过系统包、Docker 或者通过一个独立的 Java 程序

官网地址：https://www.jenkins.io/

官网教程：https://www.jenkins.io/zh/doc

### Jenkins解决了什么问题

持续集成(CI) / 持续交付 (CD) 

项目部署到服务器这个过程 原本需要我们自己做的事情都有：

推送代码到版本控制器仓库--->使用maven clean清除原本的编译文件--->使用maven package打成指定的jar/war包--->将打好的包手动拷贝到服务器内的Servlet容器（Tomcat）--->重启Servlet容器

变为现在的：推送代码到版本控制器仓库--->到Jenkins确认一下--->项目完成更新

可如图所示：

![Jenkins-Flow](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202102/08/094003-710577.png)

## Getting Start

### 下载和安装

#### 下载

官网提供多种下载方式，可根据自己的情况如实下载和安装。

本节以通用 war 包的形式来下载和安装、还可通过 `DIS.md` 查看docker详情安装

war包官网下载地址：https://www.jenkins.io/download/

![image-20210208100051763](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202102/08/100055-164144.png)

在此页面下载指定版本的war文件即可

#### 安装

Jenkins的运行离不开Java环境的支持、而且依赖于Servlet容器，故介绍两种安装war的方式，可选其一使用：

- 将下载好的war文件放入 Servlet容器，这里以Tomcat为例，随后启动Tomcat后，可访问

```http
http://ip:port/jenkins/
```

来到指定Jenkins控制台

或

- 因Jenkins自带Servlet容器（Jetty）、内置容器后不需依赖外部容器也能运行，但依旧依赖于Java环境

```shell
java -jar jenkins.war
```

可使用如上命令运行指定war包

Jenkins默认启动端口为8080，如若端口被占用启动失败，可自定义启动端口

```shell
java -jar jenkins.war --httpPort=xxxx (1-65535)
```

启动后可访问

```http
http://ip:自定义端口或默认端口
```

##### 安装后向导

###### 解锁 Jenkins

当第一次访问新的Jenkins实例时，系统会要求使用自动生成的密码对其进行解锁

访问上述链接后可能会来到如下界面

![jenkins1-page](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202102/08/102317-970195.jpeg)

Windows系统的文件路径可能为 `C:/Users/用户名/.jenkins/secrets/initialAdminPassword`

如果使用docker安装，挂载文件处可以找到 `/secrets` 文件，向下找即可，如未挂载文件运行容器，则可使用进入 容器 `docker exec -it 容器id` 去指定目录查找，或访问 `docker logs -f`查看实时日志来寻找打印到控制台的密码

密码的前后通常伴随着 一串 `*` 号

可如图：

![setup-jenkins2](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202102/08/102327-994007.png)

将密码粘贴到框内后，点击继续

###### 自定义Jenkins插件

解锁 Jenkins之后，在 **Customize Jenkins** 页面内， 您可以安装任何数量的有用插件作为您初始步骤的一部分

插件对后期的集成部署有一定的帮助，如果你是刚刚接触Jenkins，我们建议你安装**社区推荐的插件集**

如果您不确定需要哪些插件，请选择 **安装建议的插件** 。 您可以通过Jenkins中的**Manage Jenkins** > **Manage Plugins** 页面在稍后的时间点安装（或删除）其他Jenkins插件

除此之外，你可能需要用到额外的几个插件，可依次下载：

- Maven Integration：可以在 **create-job** 处创建 Maven项目
- Deploy to container：可以打包后部署到容器
- Publish Over SSH：可以以 ssh 的方式推送给远程Servlet容器

###### 创建第一个管理员用户

最后，在customizing Jenkins with plugins之后，Jenkins要求您创建第一个管理员用户。 . 出现**创建第一个管理员用户**页面时， 请在各个字段中指定管理员用户的详细信息，然后单击 **保存完成** 当 **Jenkins准备好了** 出现时，单击*开始使用 Jenkins*。

如果不创建用户，选择使用 admin 方式继续访问，那么 密码和账号被默认更改为 `admin/默认生成密码` 可访问 **admin->settings->password** 修改密码

### 使用Maven构建程序


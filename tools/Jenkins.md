# Jenkins

<p align = "center">
    <a href="https://www.jenkins.io/" target="_Blank"><img src="https://img.shields.io/badge/Jenkins-v2.263.3 LTS-orange?logo=Jenkins" alt="Jenkins"/></a>
    <a href="https://maven.apache.org/" target="_Blank"><img src="https://img.shields.io/badge/Maven-v3.3.6-yellow?logo=Apache Maven" alt="Apache Maven"/></a>
    <a href="https://git-scm.com/" target="_Blank"><img src="https://img.shields.io/badge/Git-v2.29.0-pink?logo=Git" alt="Git"/></a>
    <a href="https://tomcat.apache.org/" target="_Blank"><img src="https://img.shields.io/badge/Tomcat-v9.0.39-purple?logo=Apache Tomcat" alt="Apache Tomcat"/></a>
    <a href="https://www.docker.com/" target="_Blank"><img src="https://img.shields.io/badge/Docker-v1.13.1-green?logo=Docker" alt="Docker"/></a>
    <a href="https://www.spring.io/" target="_Blank"><img src="https://img.shields.io/badge/Springboot-v2.3.4-blue?logo=spring" alt="spring"/></a>
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

![Jenkins-Flow](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029093956712.png)

## Getting Start

### 下载和安装

#### 下载

官网提供多种下载方式，可根据自己的情况如实下载和安装。

本节以通用 war 包的形式来下载和安装、还可通过 `DIS.md` 查看docker详情安装

war包官网下载地址：https://www.jenkins.io/download/

![image-20210208100051763](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029094000539.png)

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

![jenkins1-page](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029094007285.jpeg)

Windows系统的文件路径可能为 `C:/Users/用户名/.jenkins/secrets/initialAdminPassword`

如果使用docker安装，挂载文件处可以找到 `/secrets` 文件，向下找即可，如未挂载文件运行容器，则可使用进入 容器 `docker exec -it 容器id` 去指定目录查找，或访问 `docker logs -f`查看实时日志来寻找打印到控制台的密码

密码的前后通常伴随着 一串 `*` 号

可如图：

![setup-jenkins2](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029094013393.png)

将密码粘贴到框内后，点击继续

Linux下输入命令查看：

```shell
cat /root/.jenkins/secrets/initialAdminPassword
```

###### Jenkins插件

解锁 Jenkins之后，在 **Customize Jenkins** 页面内， 您可以安装任何数量的有用插件作为您初始步骤的一部分

插件对后期的集成部署有一定的帮助，如果你是刚刚接触Jenkins，我们建议你安装**社区推荐的插件集**

如果您不确定需要哪些插件，请选择 **安装建议的插件** 。 您可以通过Jenkins中的**Manage Jenkins** > **Manage Plugins** 页面在稍后的时间点安装（或删除）其他Jenkins插件

除此之外，你可能需要用到额外的几个插件，可依次下载：

- **Maven Integration：可以在 create-job 处创建 Maven项目**
- **Deploy to container：可以打包后部署到容器**
- **Publish Over SSH：可以以 ssh 的方式推送给远程Servlet容器**
- **Warnings Next Generation：静态代码分析和检查**
- **Email Extension Template：扩展发送邮箱配置**
- **Gitee: 与gitee的代码 CI/CD**
- **Blue Ocean: 支持以新的方式运行Jenkins**
- **Config File Provider：支持的运行外部的配置文件，例如Maven的Settings.xml**
- **Git Parameter: 允许在构建时传入指定分支，按照分支构建项目**
- **Pipeline Utility Steps：允许使用readMavenPom()等方法，详情请见：[https://www.jenkins.io/zh/doc/pipeline/steps/pipeline-utility-steps/]()**

###### 创建第一个管理员用户

![image-20211027153641699](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029093902019.png)

最后，在customizing Jenkins with plugins之后，Jenkins要求您创建第一个管理员用户。 . 出现**创建第一个管理员用户**页面时， 请在各个字段中指定管理员用户的详细信息，然后单击 **保存完成** 当 **Jenkins准备好了** 出现时，单击*开始使用 Jenkins*。

如果不创建用户，选择使用 admin 方式继续访问，那么 密码和账号被默认更改为 `admin/默认生成密码` 可访问 **admin->settings->password** 修改密码

###### 实例配置

配置Jenkins的节点URL，可使用默认配置，也可延后配置

![image-20211027153818736](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029093915701.png)

### 全局配置

Jenkins全局设置

![image-20210210180552955](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029093943800.png)

> Maven

![image-20210210181208623](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029094053593.png)

![image-20210210180933575](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029094059694.png)

> JDK

![image-20210210181025868](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029094104092.png)

> Git

![image-20210210181056317](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029094108253.png)

#### 全局凭证配置

![image-20211027154620391](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029094112327.png)

配置Gitee用户名

配置Gitee APIV5 Token

配置 ssh私钥 （可选）

配置好后为

![image-20211027155811001](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029094117028.png)

---

#### 设置配置

Gitee配置

![image-20211027160434396](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029094146249.png)

Email配置

Extended E-mail Notification

先配置Jenkins Location下的邮件管理员地址

![image-20211027160642966](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029094150718.png)

等下要配置的 from 邮箱要跟上面的管理员地址邮箱一致

![image-20211027161307609](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029094153927.png)

![image-20211027162353985](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029094157442.png)

继续往下看：

![image-20211027161608588](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029094200699.png)

还有最后一点：

![image-20211027161738969](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029094203671.png)

配置完后我们可以在下面使用测试的方式，发下邮件测试是否成功

![image-20211027161847069](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029094206931.png)

在Jenkins中添加全局Maven文件，文件的目的是为了指定仓库地址（将项目打成docker镜像并上传到仓库）

![image-20211029101606710](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029101609043.png)

---

![image-20211029101630578](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029101632390.png)

---

![image-20211029101719545](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029101721273.png)

提交后，编写和配置settings.xml

在servers标签中添加如下标签

```xml
<server>
    <id>你的id</id>
    <username>你的用户名</username>
    <password>你的密码</password>
</server>
```

之后在全局配置中我们更改Maven的配置

![image-20211029102051308](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029102053091.png)

### 整合Springboot项目实现自动部署

项目工程预览

![image-20211029102521027](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029102523980.png)

#### Dockerfile

```dockerfile
FROM openjdk:11-jre
ARG JAR_FILE
ADD ${JAR_FILE} app.jar
RUN echo 'Asia/Shanghai' >/etc/timezone
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
ENTRYPOINT ["java","-Xms256m","-Xmx256m","-jar","/app.jar"]
```

#### Jenkinsfile

```shell
pipeline {
    agent any

    tools {
        git '2.27.0'
        maven '3.8.3'
    }

    environment {
        GIT_PATH="https://gitee.com/jcsun/weed.git"
        PROJECT_NAME=readMavenPom().getArtifactId()
        PROJECT_VERSION=readMavenPom().getVersion()
        // Jenkins需要开放脚本执行权限 --> 管理Jenkins(Manage Jenkins) --> 开放执行脚本(In-process Script Approval) --> Approval
        IMAGE_PATH=readMavenPom().getProperties().getProperty('docker.image.prefix')
    }

    stages {
        stage('Pull Code') {
            steps {
                echo '--------------------------------------------- Get the code from Gitee ---------------------------------------------'
                checkout([$class: 'GitSCM', branches: [[name: '*/${branch}']], extensions: [], userRemoteConfigs: [[credentialsId: 'Gitee Account (JC)', url: "${GIT_PATH}"]]])
            }
        }

        stage('Push Image') {
            steps {
                echo '--------------------------------------------- clean package build push ---------------------------------------------'
                sh 'mvn clean deploy'
            }
        }

        stage('Deploy Project') {
            steps {
                echo '--------------------------------------------- Deployment the project ---------------------------------------------'
                sh "docker ps -aq --filter 'name=${PROJECT_NAME}' | grep -q . && docker stop ${PROJECT_NAME} && docker rm --force ${PROJECT_NAME} || true"

                // other method
                script {
                    def image = sh returnStdout: true, script: 'docker images -q --filter "reference=${IMAGE_PATH}/${PROJECT_NAME}:*"'
                    if (image != null && image != "") {
                        echo "Delete old image \n ${image}"
                        sh 'docker rmi --force $(docker images -q --filter "reference=${IMAGE_PATH}/${PROJECT_NAME}:*")'
                    }
                }

                sh "docker run -d -p 666:666 --name ${PROJECT_NAME} ${IMAGE_PATH}/${PROJECT_NAME}:${PROJECT_VERSION}"

                script {
                    def unmarked = sh returnStdout: true, script: 'docker images -q --filter "dangling=true"'
                    if(unmarked != null && unmarked != ""){
                        echo "Delete unlabeled images \n $unmarked"
                        sh 'docker rmi $(docker images -q --filter "dangling=true")'
                    }
                }

            }
        }
    }
    post {
        always {
            echo '--------------------------------------------- post always code check start... ---------------------------------------------'
            recordIssues enabledForFailure: true, tools: [mavenConsole(), java(), javaDoc()]
            recordIssues enabledForFailure: true, tool: pmdParser()
            emailext (
               subject: '${DEFAULT_SUBJECT}',
               body: '${DEFAULT_CONTENT}',
               to: '${DEFAULT_RECIPIENTS}'
            )
        }
    }
}
```

#### pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.5.5</version>
        <relativePath/>
    </parent>

    <groupId>com.jc</groupId>
    <artifactId>weed</artifactId>
    <version>1.4.6</version>
    <name>weed</name>
    <description>Demo project for Spring Boot</description>
    <packaging>jar</packaging>

    <properties>
        <java.version>11</java.version>
        <dockerfile-maven-version>1.4.13</dockerfile-maven-version>
        <docker.image.prefix>registry.cn-shanghai.aliyuncs.com/imgcenter</docker.image.prefix>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-deploy-plugin</artifactId>
                <configuration>
                    <skip>true</skip>
                </configuration>
            </plugin>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-pmd-plugin</artifactId>
                <version>3.15.0</version>
                <executions>
                    <execution>
                        <goals>
                            <goal>check</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <!--加快PMD的执行速度，同时保持分析的质量。-->
                    <analysisCache>true</analysisCache>
                    <!-- 如果出现错误，则中止构建并抛出错误 -->
                    <failOnViolation>false</failOnViolation>
                    <!-- 打印错误结果 -->
                    <printFailingErrors>true</printFailingErrors>
                    <!--禁止link插件启用-->
                    <linkXRef>false</linkXRef>
                </configuration>
            </plugin>

            <plugin>
                <groupId>com.spotify</groupId>
                <artifactId>dockerfile-maven-plugin</artifactId>
                <version>${dockerfile-maven-version}</version>
                <executions>
                    <execution>
                        <id>aliyun</id>
                        <goals>
                            <!--如果package时不想用docker打包,就注释掉这个goal-->
                            <goal>build</goal>
                            <goal>push</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <tag>${project.version}</tag>
                    <repository>${docker.image.prefix}/${project.artifactId}</repository>
                    <useMavenSettingsForAuth>true</useMavenSettingsForAuth>
                    <!--Dockerfile参数-->
                    <buildArgs>
                        <JAR_FILE>target/${project.build.finalName}.${project.packaging}</JAR_FILE>
                    </buildArgs>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

#### 快速开始

创建一个Jenkins Pipline Job

![image-20211029103030102](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029103031755.png)

---

![image-20211029103102259](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029103103967.png)

---

![image-20211029103343214](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029103344924.png)

---

继续往下走来到 `构建触发器`

这一步需要配置webhook相关的配置，目的是为了Git上传到代码仓库后，代码仓库能够通知Jenkins拉取最新的代码

![image-20211029103721399](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029103724335.png)

将链接拷贝到Gitee Webhook处，将密码在Jenkins生成后也拷贝过去

![image-20211029103900877](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029103902602.png)

---

之后可以测试一下是否联通成功，如果成功，应该是下面的页面

![image-20211029103954736](https://gitee.com/QianKa/image-bucket/raw/master/typora/2021/10/29/20211029103956350.png)

---


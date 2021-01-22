# Git的安装与部署

## 简介

> Git（读音为/gɪt/）是一个开源的分布式版本控制系统，可以有效、高速地处理从很小到非常大的项目版本管理。

- 友情链接[Git官网](https://git-scm.com/)

## 下载

- 进入Git的官网

- ![image-20210120135238889](https://jcsun-images.oss-cn-shanghai.aliyuncs.com/typora/longtimelongtimeimage-20210120135238889.png)

- 点击下载
- ![image-20210120135338449](https://jcsun-images.oss-cn-shanghai.aliyuncs.com/typora/longtimeimage-20210120135338449.png)

- 然后 根据您的计算机 选择合适的版本即可 (Windows/maxOs/Linux Unix)
- 但是由于Git官网将 安装包放到了**Github**上，所以有的地区的下载网络速度贼慢,故我选择了镜像下载。
- 友情链接：[镜像链接](https://npm.taobao.org/mirrors/git-for-windows/)
- <img src="https://jcsun-images.oss-cn-shanghai.aliyuncs.com/typora/longtimeimage-20210120140117900.png" alt="image-20210120140117900" style="zoom:80%;" />

- <img src="https://jcsun-images.oss-cn-shanghai.aliyuncs.com/typora/longtimeimage-20210120140442068.png" alt="image-20210120140442068" style="zoom: 80%;" />
- 这里我选择了一个64位 最新稳定版

## 安装

待到下载完成之后,双击打开,然后进入 安装步骤 ..

### 第一步

![image-20210120141836739](https://jcsun-images.oss-cn-shanghai.aliyuncs.com/typora/longtimeimage-20210120141836739.png)

- 这一步是许可协议，点击 Next (下一步)

### 第二步

![image-20210120142025417](https://jcsun-images.oss-cn-shanghai.aliyuncs.com/typora/longtimeimage-20210120142025417.png)

- 这一步是 选择安装目录 ,然后点击 Next (下一步)
- 你可以自己选择一个目录进行安装,也可以直接使用默认的(只要可用磁盘大于259.1m即可)

### 第三步

![image-20210120145426111](https://jcsun-images.oss-cn-shanghai.aliyuncs.com/typora/longtimeimage-20210120145426111.png)

- 这一步是 询问你需要安装哪些组件?
  - Additional icons 添加图标 **√**
  - On the Desktop 在桌面上 **√**
  - Windows Explorer integration **√**
  - Git Bash Here **√**
  - Git GUI Here **√**
  - Git LFS (Large File Support)  大文件支持 **√**
  - Associate .git\* configuration files with the default text editor 将 .git 配置文件与默认文本编辑器相关联 **√**
  - Associate .sh files to be run with Bash 将.sh文件关联到Bash运行 **√**
  - Use a TrueType font in all console windows 在所有控制台窗口中使用TrueType字体 
  - Check daily for Git for Windows updates  每天检查Git是否有Windows更新 
- 本人在默认勾选 的情况下,又勾选了 在桌面上添加图标，然后点击 Next (下一步)

### 第四步

![image-20210120145611603](https://jcsun-images.oss-cn-shanghai.aliyuncs.com/typora/longtimeimage-20210120145611603.png)

- 这一步是 询问 将程序启动方式放到哪个开始文件夹下,勾选左下角 就是不添加 启动方式到开始文件夹
- 我直接默认 然后点击 Next (下一步)

### 第五步

![image-20210120145914450](https://jcsun-images.oss-cn-shanghai.aliyuncs.com/typora/longtimeimage-20210120145914450.png)

- 这一步是 选择编辑方式。默认是Vim编辑模式，还有其他编辑模式 可自行查看
- 不过 根据官方介绍，推荐在 window 上使用 GUI
- 我直接使用的默认的Vim编辑模式,它更加 适合练习 Git的命令 然后点击 Next (下一步)

### 第六步

![image-20210120150704164](https://jcsun-images.oss-cn-shanghai.aliyuncs.com/typora/longtimeimage-20210120150704164.png)

- 这一步是设置初始分支的名字
- 我直接以默认的 'master' 为 初始分支, 当然 也可以改变
- 然后点击 Next (下一步)

### 第七步

![image-20210120150943185](https://jcsun-images.oss-cn-shanghai.aliyuncs.com/typora/longtimeimage-20210120150943185.png)

- 这一步是 设置什么时候能够使用 Git 命令
- 选项一：仅从Git Bash中使用Git
- 选项二：Git可以在命令行，也可以在第三方软件 使用
- 选项三：在命令提示符中使用Git和可选的Unix工具中使用
- 这里 我选择的是默认的，(第二个) 因为后期我还要在idea中整合Git使用 
- 然后点击 Next (下一步)

### 第八步

![image-20210120151515201](https://jcsun-images.oss-cn-shanghai.aliyuncs.com/typora/longtimeimage-20210120151515201.png)

- 选择HTTPS传输backeno您希望Git使用哪个SSL/Ls库来进行HITTPS连接?
- 默认即可 然后点击 Next (下一步)

### 第九步

![image-20210120151634585](https://jcsun-images.oss-cn-shanghai.aliyuncs.com/typora/longtimeimage-20210120151634585.png)

- Git应该如何处理文本文件中的行结束符?
- 选项一：检出的时候 使用windows风格、提交的时候使用 Unix风格
- 选项二：检出的时候 原格式检出、提交的时候使用 Unix风格
- 选项三：原格式检出 原格式提交
- 我选择了第三项 然后点击 Next (下一步)

### 第十步

![image-20210220153146734](https://jcsun-images.oss-cn-shanghai.aliyuncs.com/typora/longtimeimage-20210220153146734.png)

- 您希望在您的Git Bash中使用哪个终端模拟器?
- 选项一 是 Git官方提供的窗口
- 选项二 是 使用windows 窗口
- 我选择默认 然后点击 Next (下一步)

### 第十一步

![image-20210220153704647](https://jcsun-images.oss-cn-shanghai.aliyuncs.com/typora/longtimeimage-20210220153704647.png)

- 默认情况下，git pull应该做什么?
- Default：将本地的仓库更新到最新、如果有冲突的话，解决冲突并提交
- Rebase: 将远程的仓库 直接覆盖到本地仓库上 忽略冲突
- Only ever fast-forward:更新本地的仓库，如果有冲突 就直接失败
- 我选择默认 然后点击 Next (下一步)

### 第十二步

![image-20210220154723213](https://jcsun-images.oss-cn-shanghai.aliyuncs.com/typora/longtimeimage-20210220154723213.png)

- 选择一个凭据帮助器应该配置哪个凭据帮助程序?
- 我这里选择默认 然后点击 Next (下一步)

### 第十三步、

![image-20210220154842165](https://jcsun-images.oss-cn-shanghai.aliyuncs.com/typora/longtimeimage-20210220154842165.png)

- 额外的配置选项您想启用哪些特性?
- 默认 然后点击 Next (下一步)

### 第十四步

![image-20210220154937629](https://jcsun-images.oss-cn-shanghai.aliyuncs.com/typora/longtimeimage-20210220154937629.png)

- 开始安装

然后 在 window的 Dos窗口查看 Git版本

```shell
# 通过 windows + R 
# 然后 输入 cmd 
# 再输入以下命令 查看版本
 Git version
```

图例：![image-20210220155220246](https://jcsun-images.oss-cn-shanghai.aliyuncs.com/typora/longtimeimage-20210220155220246.png)

至此，安装完毕

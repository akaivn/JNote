#### 提交流程 New For GitHub

##### 0、在GitHub上创建一个新的仓库

假设GitHub上已有main分支最新代码

##### 1、克隆远程主分支代码到本地

```shell
git clone 仓库地址
```

##### 2、在本地创建一个develop分支，并且提交到远程

```shell
git branch  develop
git push -u origin develop
```

##### 3、任何一个参与该项目的开发人员首先要做的就是从develop分支上切一个新分支进行功能开发

```shell
git checkout -b 本地分支名 origin/develop
```

切新分支异常时如下解决：(原因是因为远程develop分支上没有数据，而主分支上有)

```shell
git remote show origin

git remote update

git fetch

git checkout -b 本地分支名 origin/remote-name
```

##### 4、开始开发

开发完后可以多次提交

```shell
git status
git add
git commit
```

##### 5、提交了几次后，感觉差不多了，就可以合并到develop分支

```shell
# 先拉取develop中的代码，因为有可能别人已经往上提交过代码了
git pull origin develop
# 切到develop分支
git checkout  develop
# 合并本地分支中的代码到develop中
git merge 本地分支名
# 提交到develop远程分支上
git push origin develop
```

##### 6、合并完后，代码就可以顺利合并到远程main上

在GitHub main上发起一个Pull Request,填入合并信息后 commit即可

##### 7、删除本地分支和远程分支

```shell
# 删除本地分支
#删除分支时，记得切换到其他分支进行操作
git branch -D branchName
#删除远程分支
git push origin --delete branchName
```

#### 提交流程 Old For GitHub

##### 1、克隆远程仓库上的项目

```shell
git clone 仓库地址
```

##### 2、拉取最新的远程develop分支上的代码

```shell
git pull  
```

或

```shell
git pull origin develop
```

##### 3、切新分支到本地

首先创建本地develop，并切回

```shell
git checkout -b develop origin/develop
```

##### 4、从本地develop创建并切换到个人开发分支

```shell
git branch
git checkout -b 个人开发分支名 
```

##### 5、在个人分支上开发,修改代码

```shell
git status
git add .
git commit -a -m
```

##### 6、回到New的第5步后继续

#### 注意事项

每个公司可能都会有自己的提交流程，这是基于本人学习时的提交流程。

##### 1、idea 处使用 过滤文件出现不能及时过滤的问题

清除idea的缓存

```shell
git rm -r --cached .idea
```

##### 2、与远程仓库的中断问题

```shell
git remote add origin 仓库地址
```

##### 2.1、与远程仓库关联

```shell
git branch --set-upstream-to=origin/<branch>
```

##### 3、idea修改文件后，相应的文件没有变色

第一种方式：重启idea

第二种方式：file-->settings-->version control-->将原项目关联的git给删除，再重新添加 配图如下：

---

![image-20201111213632232](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/11/213633-597869.png)

---

第三种方式：file-->settings-->version control-->勾选父级工程也受影响

---

![image-20201111213742434](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/11/213744-739101.png)

---

##### 4、修改idea Git颜色

file-->settings-->version control-->File Status Colors

---

![image-20201111214010559](https://typora-i-1302727418.cos.ap-shanghai.myqcloud.com/typora/202011/11/214011-61524.png)

---

##### 5、将本地分支与指定的远程分支关联

```shell
git branch --set-upstream-to=origin/分支名（远程）
```

显示(表示成功)

```shell
Branch 'master' set up to track remote branch 分支名（远程）from 'origin'
```

关联了指定远程分支后:

拉取代码可用

```shell
git pull
```

推送代码可用

```shell
git push
```

而不用再带上远程 + 分支名

##### 6、Git push报错

```shell
Logon failed, use ctrl+c to cancel basic credential prompt. # 链接被拒绝登录

# 输入如下命令，关闭所有console后，重新打开console即可 
setx GIT_TRACE ""
setx GCM_TRACE ""
```

此方式不行时，只能重装，网上的方式都试过了，应该是更换github账号的原因，导致原来设置的全局账户出现了缓存

##### Git status乱码

```shell
git config --global core.quotepath false
```

##### log 日志乱码

```shell
git config --global i18n.logoutputencoding gbk
```

##### Git Bash窗口乱码

**右键窗体-->Operations-->Text-->字符编码为：zh-CN / UTF-8(Unicode)**

##### 全局设置

```shell
git config --global user.name "nameVal"
git config --global user.email "eamil@qq.com"
```

#### Git常用命令

```shell
git config

git clone

git branch

git checkout -b newbranchname

git checkout -- <file>

git merge

git add

git commit

git status

git diff

git push

git pull

git fetch

git log

git tag

git reset head

git stash [注意stash 和 stage(index)的区别]
```

#### GitEmoji

| emoji              | emoji 代码                    | commit 说明           |
| ------------------ | ----------------------------- | --------------------- |
| 🎉 (庆祝)           | `:tada:`                      | 初次提交              |
| 🆕 (全新)           | `:new:`                       | 引入新功能            |
| 🔖 (书签)           | `:bookmark:`                  | 发行/版本标签         |
| 🐛 (bug)            | `:bug:`                       | 修复 bug              |
| 🚑 (急救车)         | `:ambulance:`                 | 重要补丁              |
| 🌐 (地球)           | `:globe_with_meridians:`      | 国际化与本地化        |
| 💄 (口红)           | `:lipstick:`                  | 更新 UI 和样式文件    |
| 🎬 (场记板)         | `:clapper:`                   | 更新演示/示例         |
| 🚨 (警车灯)         | `:rotating_light:`            | 移除 linter 警告      |
| 🔧 (扳手)           | `:wrench:`                    | 修改配置文件          |
| ➕ (加号)           | `:heavy_plus_sign:`           | 增加一个依赖          |
| ➖ (减号)           | `:heavy_minus_sign:`          | 减少一个依赖          |
| ⬆️ (上升箭头)       | `:arrow_up:`                  | 升级依赖              |
| ⬇️ (下降箭头)       | `:arrow_down:`                | 降级依赖              |
| ⚡ (闪电) 🐎 (赛马)  | `:zap:` `:racehorse:`         | 提升性能              |
| 📈 (上升趋势图)     | `:chart_with_upwards_trend:`  | 添加分析或跟踪代码    |
| 🚀 (火箭)           | `:rocket:`                    | 部署功能              |
| ✅ (白色复选框)     | `:white_check_mark:`          | 增加测试              |
| 📝 (备忘录) 📖 (书)  | `:memo:` `:book:`             | 撰写文档              |
| 🔨 (锤子)           | `:hammer:`                    | 重大重构              |
| 🎨 (调色板)         | `:art:`                       | 改进代码结构/代码格式 |
| 🔥 (火焰)           | `:fire:`                      | 移除代码或文件        |
| ✏️ (铅笔)           | `:pencil2:`                   | 修复 typo             |
| 🚧 (施工)           | `:construction:`              | 工作进行中            |
| 🗑️ (垃圾桶)         | `:wastebasket:`               | 废弃或删除            |
| ♿ (轮椅)           | `:wheelchair:`                | 可访问性              |
| 👷 (工人)           | `:construction_worker:`       | 添加 CI 构建系统      |
| 💚 (绿心)           | `:green_heart:`               | 修复 CI 构建问题      |
| 🔒 (锁)             | `:lock:`                      | 修复安全问题          |
| 🐳 (鲸鱼)           | `:whale:`                     | Docker 相关工作       |
| 🍎 (苹果)           | `:apple:`                     | 修复 macOS 下的问题   |
| 🐧 (企鹅)           | `:penguin:`                   | 修复 Linux 下的问题   |
| 🏁 (旗帜)           | `:checkered_flag:`            | 修复 Windows 下的问题 |
| 🔀 (交叉箭头)       | `:twisted_rightwards_arrows:` | 分支合并              |
| :sparkles:（火花） | `:sparkles:`                  | 小更新一下下          |
# Quartz

​	**What**

​		Quartz是OpenSymphony开源组织在Job scheduling领域又一个开源项目，它可以与J2EE与J2SE应用程序相结合也可以单独使用。Quartz可以用来创建简单或为运行十个，百个，甚至是好几万个Jobs这样复杂的程序。Jobs可以做成标准的Java组件或 EJBs。Quartz的最新版本为Quartz 2.3.2。

​	**Why**

​		监听器和插件

​		Quartz应用能被[集群](https://baike.baidu.com/item/集群)，是水平集群还是垂直集群取决于你自己的需要。集群提供以下好处：

​		·伸缩性

​		·高可用性

​		·负载均衡

​		Quartz可以借助关系数据库和JDBC作业存储支持集群。

​		Terracotta扩展quartz提供集群功能而不需要数据库支持

​	**应用场景**

​		Quartz被用来做定时任务，比如在某一天的某个时间点使其触发完成调度任务，他可以很精却的调度工作任务，也可以选择性的执行次数等

## 关键字

​	**Job**(工作任务)

​		是一个接口，只有一个方法void execute(JobExecutionContext context)，开发者实现该接口定义运行任务，JobExecutionContext类提供了调度上下文的各种信息。Job运行时的信息保存在JobDataMap实例中

​	**JobDetail**(job的描述类)

​		JobDetail：Quartz在每次执行Job时，都重新创建一个Job实例，所以它不直接接受一个Job的实例，相反它接收一个Job实现类，以便运行时通过newInstance()的反射机制实例化Job。因此需要通过一个类来描述Job的实现类及其它相关的静态信息，如Job名字、描述、关联监听器等信息，JobDetail承担了这一角色。

​	**JobDataMap**()

​		每一个JobDetail都会有一个JobDataMap。JobDataMap本质就是一个Map的扩展类，只是提供了一些更便捷的方法，比如getString()之类的。Job运行时的信息保存在JobDataMap实例中

​	**Trigger**(触发器)

​		是一个类，描述触发Job执行的时间触发规则。主要有SimpleTrigger和CronTrigger这两个子类。当仅需触发一次或者以固定时间间隔周期执行，SimpleTrigger是最适合的选择；而CronTrigger则可以通过Cron表达式定义出各种复杂时间规则的调度方案：如每早晨9:00执行，周一、周三、周五下午5:00执行等

​	**Scheduler**(调度器)

​		代表一个Quartz的独立运行容器，Trigger和JobDetail可以注册到Scheduler中，两者在Scheduler中拥有各自的组及名称，组及名称是Scheduler查找定位容器中某一对象的依据，Trigger的组及名称必须唯一，JobDetail的组和名称也必须唯一（但可以和Trigger的组和名称相同，因为它们是不同类型的）。Scheduler定义了多个接口方法，允许外部通过组及名称访问和控制容器中Trigger和JobDetail

## 测试实例

​	1.编写一个Job工作任务(实现Job接口)

```java
public class Job implements org.quartz.Job {

    public Job() {
        System.out.println("我出生了。。。");
    }

    /*具体执行的任务在这里*/
    @Override
    public void execute(JobExecutionContext jobExecutionContext) throws JobExecutionException {
```

​	2.编写测试实例

```java
/**
 * @author Machenike
 * @Date 2020/4/23 10:47
 */
public class TestQuartz {

    public static void main(String[] args) throws Exception{
        //任务调度器实例
        Scheduler scheduler = StdSchedulerFactory.getDefaultScheduler();

        //jobDetail对象
        JobDetail build = JobBuilder.newJob(Job.class)
                .withIdentity("job1", "group1")
                .usingJobData("message","jobDetail")
                .build();

        //Trigger对象
        Trigger build1 = TriggerBuilder.newTrigger()
                .withIdentity("trigger1", "group2")
                .startNow()
                .withSchedule(
                        SimpleScheduleBuilder.simpleSchedule()
                                .withIntervalInSeconds(2)
                                .withRepeatCount(1)
                ).usingJobData("message","trigger").build();

        //调度加入任务，就是让任务和触发器绑定
        scheduler.scheduleJob(build,build1);
        //开始
        scheduler.start();

        //执行完之后关闭调度器
        Thread.sleep(4000);
        scheduler.shutdown(true);
    }
}
```

### Context

```java
context.getJobDetail();//获得job任务工作类的描述信息
context.getTrigger();//获得任务触发器
context.getJobRunTime();//获得任务的运行时间
context.getNextFireTime();//获得下次任务开始执行的时间
context.getScheduler();//获得任务调度器
context.getJobInstance();//获得任务工作的实例
```

### JobDetail

```java
jobDetail.getJobDataMap();//获得jobDataMap对象
jobDetail.getJobClass();//获得当前任务的实例名称(全类名)
jobDetail.getKey();//返回一个JobKey对象，他有三个方法，getName(),getGroup(),getClass()
jobDetail.getJobBuilder();//获得任务工作创建者
```

### JobDataMap

```java
这个东西实现了Map<k,v>接口，所有map有的方法它都有
jobDataMap.get数据类型(数据类型 key);//获得任务实例的值根据不同类型key返回不同类型的值
```

### Trigger

```java
trigger.getStartTime();//获得本次任务开始时间
trigger.getEndTime();//获得本此任务结束时间
trigger.getTriggerBuilder();//获得触发器创建者
trigger.getScheduleBuilder();//获得调度器创建者
```

### Scheduler

```java
scheduler.start();//开启任务调度器
scheduler.shutdown(boolean b);//结束任务调度器
scheduler.scheduleJob(JobDetail jobDetail,Trigger trigger);//配置任务 将任务和触发器绑定
scheduler.checkExists(JobKey jobKey | TriggerKey triggerKey);//检查是否存在任务
scheduler.addJob(JobDetail jobDetail,boolean b);//添加任务
scheduler.deleteJob(JobKey k);//删除任务
scheduler.deleteJobs(List<JobKey> listk);//删除很多任务
scheduler.isStarted();//调度器是否开启
scheduler.isShutdown();//调度器是否关闭
```

## Springboot整合Quartz


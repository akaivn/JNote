# Mybatis-Plus

**特性和优点 详情见官网**   **[https://mybatis.plus]()**

## 1、配合Springboot快速入门案例

### A、导入依赖

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.3.0</version>
</dependency>
```

### B、编写properties文件

```properties
#数据源和数据库连接配置
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql:///test
spring.datasource.username=root
spring.datasource.password=kate
spring.datasource.type=com.mchange.v2.c3p0.ComboPooledDataSource
```

### C、编写mapper接口继承BaseMapper<T>接口

```java
public interface UserDao extends BaseMapper<User> {}
```

### D、测试

**注入dao直接测试**

```java
@Autowired
private UserDao userDao;
@Test
public void t1(){
    List<User> users = userDao.selectList(null);
    for (User user : users) {
        System.out.println(user);
    }
}
```

### E、开启控制台日志

```properties
#日志 控制台输出
mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```

**语法糖** 更多详情请见 [https://www.cnblogs.com/duanxz/p/3916028.html]()

```java
//遍历集合
List<User> users = userDao.selectList(null);
users.forEach(System.out::println);
```

## 2、CRUD

**MybatisPlus默认就是驼峰换 " _ " 格式**

### 查询（select）

```java
@Test
public void t3(){
    //传入多个id查询用户
    List<User> users = userDao.selectBatchIds(Arrays.asList(1, 2, 3));
    //SQL为 SELECT id,name,age,email,create_time,update_time FROM user WHERE id IN ( ? , ? , ? )
    users.forEach(System.out::println);

    //查询一个
    User user = userDao.selectById(1L);
    //SQL为 SELECT id,name,age,email,create_time,update_time FROM user WHERE id=?

    //按条件查询 Map 有一个条件就拼接一个查询条件
    Map<String,Object> map = new HashMap<>();
    map.put("id",1261127903677116420L);
    map.put("age",3);
    List<User> users = userDao.selectByMap(map);
    for (User user : users) {
        System.out.println(user);
    }
    //SQL为 SELECT id,name,age,email,create_time,update_time FROM user WHERE id = ? AND age = ?
}
```

#### 分页查询

##### 1、编写配置类 

```java
@EnableTransactionManagement
@Configuration
@MapperScan("com.baomidou.cloud.service.*.mapper*")
public class MybatisPlusConfig {

    @Bean
    public PaginationInterceptor paginationInterceptor() {
        PaginationInterceptor paginationInterceptor = new PaginationInterceptor();
        // 设置请求的页面大于最大页后操作， true调回到首页，false 继续请求  默认false
        // paginationInterceptor.setOverflow(false);
        // 设置最大单页限制数量，默认 500 条，-1 不受限制
        // paginationInterceptor.setLimit(500);
        // 开启 count 的 join 优化,只针对部分 left join
        paginationInterceptor.setCountSqlParser(new JsqlParserCountOptimize(true));
        return paginationInterceptor;
    }
}
```

##### 2、测试操作

```java
@Test
public void t4(){
    Page<User> page = new Page<>(1,5);
    Page<User> userPage = userDao.selectPage(page, null);
    //获取当前页集合
    List<User> records = userPage.getRecords();
    for (User record : records) {
        System.out.println(record);
    }
    //获取总条数
    long total = userPage.getTotal();
    //获取总页数
    long pages = userPage.getPages();
    //获取当前页
    long current = userPage.getCurrent();
    //页面大小
    System.out.println(userPage.getSize());
}
可以基本满足分页所必须的几个属性
```

### 插入（insert）

测试示例

```java
@Autowired
private UserDao userDao;
@Test
public void t1(){
    User user = new User();
    user.setName("tcl");
    user.setAge(3);
    user.setEmail("");
    int row = userDao.insert(user);//返回受影响行数
    System.out.println(row);
}
```

**默认不插入id时，它会帮我们生成一个id并插入，它是根据雪花算法生成的主键，我们也可以自己定义主键生成策略**

#### 雪花算法(主键生成)

常见的主键生成策略有 Redis生成、主键自增、UUID、时间戳等

详情见  [分布式数据库主键自增策略](https://juejin.im/post/5deb64f0f265da33d83e64e2)

**需要注意的是：如果使用雪花算法来生成主键id，那么id的属性类型必须为Long，如果写Integer或int，就会报错，但结果会成功**

#### **选择不同的主键生成策略**（在雪花算法的基础上进行）

在实体类上加上注解@TableId

```java
@Data
public class User {
    @TableId(type = IdType.AUTO)
    private Long id;
```

**@TableId**

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.FIELD})
public @interface TableId {
    String value() default "";

    IdType type() default IdType.NONE; //选择使用type属性类指定主键生成策略，默认为NONE
}
```

**IdType枚举属性**

```java
AUTO(0),//表示设置数据库主键自增，如果数据库没有设置自增，运行报错，结果不成功
NONE(1),//表示不适用任何其他策略，那么默认就是生成唯一的雪花id
INPUT(2),//表示手动输入id，如果不传id，数据库主键又没设置自增，数据库约束不过关
ASSIGN_ID(3),//分配ID(主键类型为Number(Long和Integer)或String)(since 3.3.0),使用接口IdentifierGenerator的方法nextId(默认实现类为DefaultIdentifierGenerator雪花算法)
ASSIGN_UUID(4),//分配UUID,主键类型为String(since 3.3.0),使用接口IdentifierGenerator的方法nextUUID(默认default方法)
/** @deprecated */
@Deprecated
ID_WORKER(3),//全局唯一id，被弃用
/** @deprecated */
@Deprecated
ID_WORKER_STR(3),//全局唯一id，字符串写法，被弃用
/** @deprecated */
@Deprecated
UUID(4);//UUID被弃用
```

**实际开发中我们可能AUTO和INPUT用的更多**

### 更新（udpate）

```java
@Test
public void t2(){
    User user = new User();
    user.setId(5L);
    user.setName("仝成龙");
    int i = userDao.updateById(user);//返回受影响行数 该方法参数传一个对象而不是ID
    System.out.println(i);
    //执行的SQL语句为：UPDATE user SET name=? WHERE id=? 
}

二次测试
User user = new User();
user.setId(5L);
user.setName("仝成龙");
user.setAge(22);
int i = userDao.updateById(user);//返回受影响行数
System.out.println(i);
//执行的SQL语句为：UPDATE user SET name=?,age=? WHERE id=? 
```

**我们发现它会根据你设置的User的属性自动的去生成动态SQL，你设置一个属性就改一个字段，设置两个属性就改两个字段**

#### **自动填充功能**

**阿里开发手册规定，所有表中都要有 gmt_create、 gmt_modified这两个字段，并且必须自动化。日常测试可以使用create_time 和update_time** 

两种解决方式

##### 1、数据库方式解决(实际开发中不允许)

给这个两个字段都加上默认值，当数据库修改时就会触发修改时间，当添加时也会有新的时间

##### 2、代码方式解决（推荐使用）

数据库字段不加默认值，使用mybatis-plus提供的注解完成自动填充

**1、在实体类需要填充的字段上标注@TableField注解**

```java
public class User {
    @TableId(type = IdType.AUTO)
    private Long id;
    private String name;
    private Integer age;
    private String email;

    @TableField(fill = FieldFill.INSERT)
    private Date createTime;
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private Date updateTime;
}
```

**FieldFill枚举属性**

```java
DEFAULT,//表示默认不处理
INSERT,//插入填充字段
UPDATE,//更新填充字段
INSERT_UPDATE;//插入和更新填充字段
```

**2、编写处理注解的处理器**

```java
@Component /**添加到IOC容器中*/
@Slf4j /**使用SLF4J日志*/
public class MybatisPlusAutoFill implements MetaObjectHandler {

    /**
     * 插入时填充
     * @param metaObject
     */
    @Override
    public void insertFill(MetaObject metaObject) {
        log.info("start insert fill ....");
        this.strictInsertFill(metaObject,"createTime", Date.class,new Date());
    }

    /**
     * 更新时填充
     * @param metaObject
     */
    @Override
    public void updateFill(MetaObject metaObject) {
        log.info("start update fill ....");
        this.strictUpdateFill(metaObject, "updateTime",  Date.class,new Date());
    }
}

```

源码

```java
/**
     * 
     * @param metaObject 被填充的对象
     * @param fieldName 被填充的对象的指定的属性
     * @param fieldType 被填充的对象的指定的类型
     * @param fieldVal 插入相同类型的值
     * @param <T> 不同的对象
     * @return 返回这个strictInsertFill
     */
default <T> MetaObjectHandler strictInsertFill(MetaObject metaObject, String fieldName, Class<T> fieldType, Object fieldVal) {
    return this.strictInsertFill(this.findTableInfo(metaObject), metaObject, Collections.singletonList(StrictFill.of(fieldName, fieldType, fieldVal)));
}
```

### 删除（delete）

```java
@Test
public void t5(){
    //根据传入id删除指定用户(多个)
    userDao.deleteBatchIds(Collection<? extends Serializable > idList);
    //根据传入id删除指定用户
    userDao.deleteById(Serializable id);
    //根据传入map封装条件来删除指定对象，满足条件则删除
    userDao.deleteByMap(Map<String, Object> columnMap);
    /**
     * 全部返回受影响行数
     */
}
```

### 逻辑删除

#### 1、使用update进行逻辑删除

```java
@Test
public void t6(){
    //逻辑删除
    User user = new User();
    user.setId(1261127903677116419L);
    user.setIsDelete(1);
    int i = userDao.updateById(user);
    System.out.println(i);
}
```

#### 2、使用MybatisPlus提供的逻辑删除

##### a、POJO类逻辑删除属性上加@TableLogic注解

```java
@TableLogic
private Integer isDelete;
```

##### b、application.properties配置文件

```properties
#配置逻辑删除
mybatis-plus.global-config.db-config.logic-not-delete-value=0
mybatis-plus.global-config.db-config.logic-delete-value=1

如果你的配置跟MP的默认配置一样，那么就不需要指定 not-value 和 delete-value
```

##### c、使用mp自带方法删除和查找都会附带逻辑删除功能 (自己写的xml不会)

再次执行查询时的SQL就变成了这样，它往上追加了一个逻辑查询的条件

```sql
SELECT id,name,age,email,create_time,update_time,is_delete FROM user WHERE id IN ( ? , ? , ? ) AND is_delete=0 
```

#### 3、使用全局的逻辑删除功能

数据库的字段必须为flag，使用此配置则不需要在实体类上添加 @TableLogic，当然如果实体类上还是有@TableLogic，以实体类的为准

```properties
mybatis-plus.global-config.db-config.logic-delete-field=flag
```

## 3、条件构造器Wrapper（高级查询）

**使用条件构造器支持我们自定义SQL查询语句**

详情请见官方文档，这里贴出一个示例  凡是查询和修改入参的地方可以传wrapper的都支持定制化SQL

### 1、QueryWrapper<T>

根据指定条件来完成查询

```java
@Test
public void t9(){
    //条件查询器Wrapper
    QueryWrapper<User> queryWrapper = new QueryWrapper<>();
    queryWrapper
        .likeRight("name","t") //z%
        .ge("age",20)
        .isNull("email")
        .isNotNull("create_time");
    System.out.println(userDao.selectOne(queryWrapper));
}
```

### 2、UpdateWrapper`<T>`

根据指定条件来完成修改

```java
@Test
public void t10(){
    UpdateWrapper<User> updateWrapper = new UpdateWrapper<>();
    updateWrapper.eq("id",1261127903677116421L);
    User user = new User();
    user.setAge(30);
    userDao.update(user,updateWrapper);
}
```

## 4、代码自动生成器

**参考官方文档，另可以使用插件生成**


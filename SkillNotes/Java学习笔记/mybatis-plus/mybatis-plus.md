# 1 简介

> [MyBatis-Plus (opens new window)](https://github.com/baomidou/mybatis-plus)（简称 MP）是一个 [MyBatis (opens new window)](https://www.mybatis.org/mybatis-3/)的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。



## 1.1 特性

- **无侵入**：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑
- **损耗小**：启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作
- **强大的 CRUD 操作**：内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求
- **支持 Lambda 形式调用**：通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错
- **支持主键自动生成**：支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题
- **支持 ActiveRecord 模式**：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操作
- **支持自定义全局通用操作**：支持全局通用方法注入（ Write once, use anywhere ）
- **内置代码生成器**：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用
- **内置分页插件**：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List 查询
- **分页插件支持多种数据库**：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer 等多种数据库
- **内置性能分析插件**：可输出 SQL 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
- **内置全局拦截插件**：提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作

---



## 1.2 支持数据库

> 任何能使用 `MyBatis` 进行 CRUD, 并且支持标准 SQL 的数据库，具体支持情况如下，如果不在下列表查看分页部分教程 PR 您的支持。

- MySQL，Oracle，DB2，H2，HSQL，SQLite，PostgreSQL，SQLServer，Phoenix，Gauss ，ClickHouse，Sybase，OceanBase，Firebird，Cubrid，Goldilocks，csiidb

---



# 2 基础使用

## 2.1 数据库表

现有一张 `User` 表，其表结构如下：

| id   | name   | age  | email              |
| ---- | ------ | ---- | ------------------ |
| 1    | Jone   | 18   | test1@baomidou.com |
| 2    | Jack   | 20   | test2@baomidou.com |
| 3    | Tom    | 28   | test3@baomidou.com |
| 4    | Sandy  | 21   | test4@baomidou.com |
| 5    | Billie | 24   | test5@baomidou.com |



其对应的数据库 `Schema` 脚本如下：

```sql
DROP TABLE IF EXISTS user;

CREATE TABLE user
(
    id BIGINT(20) NOT NULL COMMENT '主键ID',
    name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
    age INT(11) NULL DEFAULT NULL COMMENT '年龄',
    email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
    PRIMARY KEY (id)
);
```



其对应的数据库 `Data` 脚本如下：

```sql
DELETE FROM user;

INSERT INTO user (id, name, age, email) VALUES
(1, 'Jone', 18, 'test1@baomidou.com'),
(2, 'Jack', 20, 'test2@baomidou.com'),
(3, 'Tom', 28, 'test3@baomidou.com'),
(4, 'Sandy', 21, 'test4@baomidou.com'),
(5, 'Billie', 24, 'test5@baomidou.com');
```

---



## 2.2 初始化工程

创建一个空的 Spring Boot 工程（工程将以 H2 作为默认数据库进行演示）

提示: 可以使用 [Spring Initializer (opens new window)](https://start.spring.io/)快速初始化一个 Spring Boot 工程

---



## 2.3 添加依赖

引入 Spring Boot Starter 父工程：

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.0+ 版本</version>
    <relativePath/>
</parent>
```

引入 `spring-boot-starter`、`spring-boot-starter-test`、`mybatis-plus-boot-starter`、`h2` 依赖：

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>最新版本</version>
    </dependency>
    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```



## 2.4 配置

在 `application.yml` 配置文件中添加 `MySQL` 数据库的相关配置：

```yaml
# DataSource Config
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/demospace?useSSL=true&useUnicode=true&characterEncoding=UTF
    		8&serverTimezone=Asia/Shanghai
    username: root
    password: yan19991001

```

在 Spring Boot 启动类中添加 `@MapperScan` 注解，扫描 Mapper 文件夹：

```java
@SpringBootApplication
@MapperScan("com.learn.mapper")
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```



## 2.5 编码

编写实体类 `User.java`（此处使用了 [Lombok](https://www.projectlombok.org/) 简化代码）

```java
@Data
public class User {
    private Long id;
    private String name;
    private Integer age;
    private String email;
}
```



编写 Mapper 包下的 `UserMapper`接口

```java
public interface UserMapper extends BaseMapper<User> {

}
```



## 2.6 开始使用

添加测试类，进行功能测试：

```java
@SpringBootTest
public class SampleTest {

    @Autowired
    private UserMapper userMapper;

    @Test
    public void testSelect() {
        System.out.println(("----- selectAll method test ------"));
        List<User> userList = userMapper.selectList(null);
        Assert.assertEquals(5, userList.size());
        userList.forEach(System.out::println);
    }

}
```



**提示**

UserMapper 中的 `selectList()` 方法的参数为 MP 内置的条件封装器 `Wrapper`，所以不填写就是无任何条件

控制台输出：

```log
User(id=1, name=Jone, age=18, email=test1@baomidou.com)
User(id=2, name=Jack, age=20, email=test2@baomidou.com)
User(id=3, name=Tom, age=28, email=test3@baomidou.com)
User(id=4, name=Sandy, age=21, email=test4@baomidou.com)
User(id=5, name=Billie, age=24, email=test5@baomidou.com)
```



# 3 饭后甜点

## 3.1 配置日志

```yaml
# 配置日志
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```



## 3.2 CRUD 操作

### 1) Insert 操作

- `Code`

  ```java
  @Test
  void insert() {
      System.out.println(("----- insert method test ------"));
      User user = new User(null,"黎明",30,"12121@qq,com");
      int result = userMapper.insert(user);
      List<User> userList = userMapper.selectList(null);
      userList.forEach(System.out::println);
      System.out.println(result);
  }
  ```

- `ID 自动填充`

  如下 `1572855468403822594` 为自动生成的 ID

  ```java
  User user = new User(null,"黎明",30,"12121@qq,com");
  >>>
  1572855468403822594, 黎明, 30, 12121@qq,com
  ```

- `主键生成策略`

  |      值       |                             描述                             |
  | :-----------: | :----------------------------------------------------------: |
  |     AUTO      |                         数据库ID自增                         |
  |     NONE      | 无状态,该类型为未设置主键类型(注解里等于跟随全局,全局里约等于 INPUT) |
  |     INPUT     |                    insert前自行set主键值                     |
  |   ASSIGN_ID   | 分配ID(主键类型为Number(Long和Integer)或String)(since 3.3.0),使用接口`IdentifierGenerator`的方法`nextId`(默认实现类为`DefaultIdentifierGenerator`雪花算法) |
  |   ASSIGN_ID   | 分配UUID,主键类型为String(since 3.3.0),使用接口`IdentifierGenerator`的方法`nextUUID`(默认default方法) |
  |   ID_WORKER   |     分布式全局唯一ID 长整型类型(please use `ASSIGN_ID`)      |
  |     UUID      |           32位UUID字符串(please use `ASSIGN_UUID`)           |
  | ID_WORKER_STR |     分布式全局唯一ID 字符串类型(please use `ASSIGN_ID`)      |

- `ID 填充方式自定义`

  在主键属性地上方添加 `@TableId` 属性

  ```java
      @TableId(type = IdType.AUTO)
      private Long id;
  
  ```

- `雪花算法`

  **第一位** 占用1bit，其值始终是0，没有实际作用。 **2.时间戳** 占用41bit，精确到毫秒，总共可以容纳约69年的时间。 **3.工作机器id** 占用10bit，其中高位5bit是数据中心ID，低位5bit是工作节点ID，做多可以容纳1024个节点。 **4.序列号** 占用12bit，每个节点每毫秒0开始不断累加，最多可以累加到4095，一共可以产生4096个ID。

  SnowFlake算法在同一毫秒内最多可以生成多少个全局唯一ID呢：： **同一毫秒的ID数量 = 1024 X 4096 = 4194304**

  

### 2) Search 操作

#### selectById

`单查`

```java
@Test
void Search(){
    User user = userMapper.selectById(1L);
    System.out.println(user);
}
```



#### selectBatchIds 

`列表查询`

```java
@Test
void SearchLists(){
    List<User> userList = userMapper.selectBatchIds(Arrays.asList(1,2,3));
    System.out.println(userList);
}
```



####  selectByMap 

`Map集合查询`

```java
@Test
void SearchMap(){
    HashMap<String, Object> map = new HashMap<>();
    map.put("name","Tom");
    List<User> userList = userMapper.selectByMap(map);
    System.out.println(userList);
}
```



#### 分页查询

**配置拦截器组件**

```java
package com.learn.config;

import com.baomidou.mybatisplus.annotation.DbType;
import com.baomidou.mybatisplus.autoconfigure.ConfigurationCustomizer;
import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.OptimisticLockerInnerInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
@MapperScan("com.learn.mapper")
public class MybatisPlusConfig {
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
        return interceptor;
    }

    @Bean
    public ConfigurationCustomizer configurationCustomizer() {
        return configuration -> configuration.setUseDeprecatedExecutor(false);
    }
}

```

**测试**

```java
    //分页查询
    @Test
    public void PageTest(){
        Page<User> page = new Page<>(1,3);
        userMapper.selectPage(page,null);
        page.getRecords().forEach(System.out::println);
    }
```



### 3) 删除操作

#### a. 基本删除

```java
@Test
void delete(){
    userMapper.deleteById(1L);
}

@Test
void deleteLists(){
    userMapper.deleteBatchIds(Arrays.asList(2,3));
}

@Test
void deleteMap(){
    HashMap<String, Object> map = new HashMap<>();
    map.put("name","changed");
    userMapper.deleteByMap(map);
}
```



#### b. 逻辑删除

> 这里删除后并不会真正的删除数据库表内的属性值, 而是会将指定的字段如 `deleted` 置为 1(或某个设定好的 tag 值), 未删除则 `deleted = 0` 

**配置 `application.yml`**

 ```java
 mybatis-plus:
   global-config:
     db-config:
       logic-delete-field: flag # 全局逻辑删除的实体字段名(since 3.3.0,配置后可以忽略不配置步骤2)
       logic-delete-value: 1 # 逻辑已删除值(默认为 1)
       logic-not-delete-value: 0 # 逻辑未删除值(默认为 0)
 ```



**实体类字段上加上`@TableLogic`注解 (可选)**

```java
@TableLogic
private Integer deleted;
```



**测试类**

```java
@Test
void delete(){
    userMapper.deleteById(4L);
}
```





**数据库表**

![image-20220922191530584](D:\GitRepository\notes-and-resource\学习笔记\技术学习笔记\Java学习笔记\mybatis-plus\imgs\image-20220922191530584.png)

---



### 4) 自动填充

#### 日期自动填充

> **注意点**
>
> ​	**使用 mybatis-plus 实现日期自动填充时, 不能使用 java.util.Date 包, 要使用 LocalDateTime 类型定义日期**





**User 类**

>`@TableField(fill = FieldFill.INSERT)`
>
>**更新策略**
>
>​    DEFAULT,					默认不处理
>​    INSERT,						插入填充字段
>​    UPDATE,			 		更新填充字段
>​    INSERT_UPDATE		插入和更新填充字段



```java
package com.learn.bean;

import com.baomidou.mybatisplus.annotation.FieldFill;
import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.annotation.TableField;
import com.baomidou.mybatisplus.annotation.TableId;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.time.LocalDateTime;
import java.util.Date;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    @TableId(type = IdType.AUTO)
    private Long id;
    private String name;
    private int age;
    private String email;

    @TableField(fill = FieldFill.INSERT)
    private LocalDateTime createTime;

    @TableField(fill = FieldFill.INSERT_UPDATE)
    private LocalDateTime  updateTime;
}
```



**MyMetaObjectHandler 实现类**

```java
package com.learn.handler;

import com.baomidou.mybatisplus.core.handlers.MetaObjectHandler;
import lombok.extern.slf4j.Slf4j;
import org.apache.ibatis.reflection.MetaObject;
import org.springframework.stereotype.Component;

import java.time.LocalDateTime;

@Slf4j
@Component
public class MyMetaObjectHandler implements MetaObjectHandler {
    @Override
    public void insertFill(MetaObject metaObject) {
        log.info("start insert fill ....");
        this.strictInsertFill(metaObject, "createTime", LocalDateTime.class, LocalDateTime.now()); // 起始版本 3.3.0(推荐使用)
        this.strictInsertFill(metaObject, "updateTime", LocalDateTime.class, LocalDateTime.now()); // 起始版本 3.3.0(推荐使用)

    }

    @Override
    public void updateFill(MetaObject metaObject) {
        log.info("start update fill ....");
        this.strictUpdateFill(metaObject, "updateTime", LocalDateTime.class, LocalDateTime.now()); // 起始版本 3.3.0(推荐)
    }
}
```



**数据库表**

![image-20220922164823449](D:\GitRepository\notes-and-resource\学习笔记\技术学习笔记\Java学习笔记\mybatis-plus\imgs\image-20220922164823449.png)



### 5) 锁概念

#### a. 乐观锁

**概念**

> 乐观锁在操作数据时非常乐观，认为别人不会同时修改数据。因此乐观锁不会上锁，只是在执行更新的时候判断一下在此期间别人是否修改了数据：如果别人修改了数据则放弃操作，否则执行操作。



**乐观锁插件使用**

注解

```java
package com.learn.bean;

import com.baomidou.mybatisplus.annotation.*;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.time.LocalDateTime;
import java.util.Date;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    @TableId(type = IdType.AUTO)
    private Long id;
    private String name;
    private int age;
    private String email;
    @Version            //乐观锁
    private int version;

    @TableField(fill = FieldFill.INSERT)
    private LocalDateTime createTime;

    @TableField(fill = FieldFill.INSERT_UPDATE)
    private LocalDateTime  updateTime;
}
```

Mybatis-Plus 配置类

```java
package com.learn.config;

import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.OptimisticLockerInnerInterceptor;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
@MapperScan("com.learn.mapper")
public class MybatisPlusConfig {
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
        return interceptor;
    }
}
```

测试类

```java
    //成功执行乐观锁
    @Test
    void optimisticLocking(){
        User user = userMapper.selectById(1L);
        user.setName("changed");
        user.setAge(18);
        user.setEmail("changed@qq.com");
        userMapper.updateById(user);
    }

    //失败执行乐观锁
    @Test
    void optimisticLockingFailed(){
        User user = userMapper.selectById(1L);
        user.setName("changed111");
        user.setAge(18);
        user.setEmail("changed111@qq.com");

        User user2 = userMapper.selectById(1L);
        user2.setName("changed111");
        user2.setAge(18);
        user2.setEmail("changed111@qq.com");
        userMapper.updateById(user2);
        //由于乐观锁的存在, 在更新同一条信息时, 会根据 version 判断该信息是否在之前被修改过。
        userMapper.updateById(user);
    }
```

数据库

![image-20220922182504858](D:\GitRepository\notes-and-resource\学习笔记\技术学习笔记\Java学习笔记\mybatis-plus\imgs\image-20220922182504858.png)

#### b. 悲观锁

**概念**

> 观锁在操作数据时比较悲观，认为别人会同时修改数据。因此操作数据时直接把数据锁住，直到操作完成后才会释放锁；上锁期间其他人不能修改数据。







### 6) 条件查询器 wrapper

```java
@Test
void test1() {
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    wrapper
            .isNotNull("name")
            .ge("age", 10);
    userMapper.selectList(wrapper).forEach(System.out::println);
}

@Test
void test2() {
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    wrapper
            .eq("name","Tom");
    System.out.println(userMapper.selectOne(wrapper));
}

@Test
void test3() {
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    wrapper
            .between("age",20,30);
    int count = userMapper.selectCount(wrapper);
    System.out.println(count);
}
```




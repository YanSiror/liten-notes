# 1 redis 概述

redis是一款高性能的NOSQL系列的非关系型数据库



## 1.1 NOSQL 是什么

> NoSQL(NoSQL = Not Only SQL)，意即“不仅仅是SQL”，是一项全新的数据库理念，泛指非关系型的数据库。
> 随着互联网web2.0网站的兴起，传统的关系数据库在应付web2.0网站，特别是超大规模和高并发的SNS类型的web2.0纯动态网站已经显得力不从心，暴露了很多难以克服的问题，而非关系型的数据库则由于其本身的特点得到了非常迅速的发展。NoSQL数据库的产生就是为了解决大规模数据集合多重数据种类带来的挑战，尤其是大数据应用难题。



### 1.1.1 NOSQL和关系型数据库比较

- **优点**

> 1）成本：nosql数据库简单易部署，基本都是开源软件，不需要像使用oracle那样花费大量成本购买使用，相比关系型数据库价格便宜。
> 2）查询速度：nosql数据库将数据存储于缓存之中，关系型数据库将数据存储在硬盘中，自然查询速度远不及nosql数据库。
> 3）存储数据的格式：nosql的存储格式是key,value形式、文档形式、图片形式等等，所以可以存储基础类型以及对象或者是集合等各种格式，而数据库则只  		支持基础类型。
> 4）扩展性：关系型数据库有类似join这样的多表查询机制的限制导致扩展很艰难。

- **缺点**

> 1）维护的工具和资料有限，因为nosql是属于新的技术，不能和关系型数据库10几年的技术同日而语。
> 2）不提供对sql的支持，如果不支持sql这样的工业标准，将产生一定用户的学习和使用成本。
> 3）不提供关系型数据库对事务的处理。



### 1.1.2 关系型与非关系型数据库的优势

#### 1) 非关系型数据库的优势

> 性能 NOSQL 是基于键值对的，可以想象成表中的主键和值的对应关系，而且不需要经过SQL层的解析，所以性能非常高。
> 可扩展性同样也是因为基于键值对，数据之间没有耦合性，所以非常容易水平扩展。



#### 2) 关系型数据库的优势

> 复杂查询可以用SQL语句方便的在一个表以及多个表之间做非常复杂的数据查询。
> ​事务支持使得对于安全性能很高的数据访问要求得以实现。对于这两类数据库，对方的优势就是自己的弱势，反之亦然。



### 1.1.3 总结

> 关系型数据库与NoSQL数据库并非对立而是互补的关系，即通常情况下使用关系型数据库，在适合使用NoSQL的时候使用NoSQL数据库，
> ​让NoSQL数据库对关系型数据库的不足进行弥补。
> ​一般会将数据存储在关系型数据库中，在nosql数据库中备份存储关系型数据库的数据



## 1.2 主流的NOSQL产品

#### 1.2.1 键值(Key-Value)存储数据库

​		相关产品： Tokyo Cabinet/Tyrant、Redis、Voldemort、Berkeley DB
​		典型应用： 内容缓存，主要用于处理大量数据的高访问负载。 
​		数据模型： 一系列键值对
​		优势： 快速查询
​		劣势： 存储的数据缺少结构化

#### 1.2.2 列存储数据库

​			相关产品：Cassandra, HBase, Riak
​			典型应用：分布式的文件系统
​			数据模型：以列簇式存储，将同一列数据存在一起
​			优势：查找速度快，可扩展性强，更容易进行分布式扩展
​			劣势：功能相对局限

#### 1.2.3 文档型数据库

​			相关产品：CouchDB、MongoDB
​			典型应用：Web应用（与Key-Value类似，Value是结构化的）
​			数据模型： 一系列键值对
​			优势：数据结构要求不严格
​			劣势： 查询性能不高，而且缺乏统一的查询语法

#### 1.2.4 图形(Graph)数据库

​			相关数据库：Neo4J、InfoGrid、Infinite Graph
​			典型应用：社交网络
​			数据模型：图结构
​			优势：利用图结构相关算法。
​			劣势：需要对整个图做计算才能得出结果，不容易做分布式的集群方案。



## 1.3 什么是Redis

​		Redis是用C语言开发的一个开源的高性能键值对（key-value）数据库，官方提供测试数据，50个并发执行100000个请求,读的速度是110000次/s,写的速度是81000次/s ，且Redis通过提供多种键值数据类型来适应不同场景下的存储需求，目前为止Redis支持的键值数据类型如下：

> 字符串类型 string
>
> 哈希类型 hash
>
> 列表类型 list
>
> 集合类型 set
>
> 有序集合类型 sortedset



### 1.3.1 redis的应用场景

[Blog link]: https://www.imooc.com/wiki/springbootlesson/redis.html

•	缓存（数据查询、短连接、新闻内容、商品内容等等）
•	聊天室的在线好友列表
•	任务队列。（秒杀、抢购、12306等等）
•	应用排行榜
•	网站访问统计
•	数据过期处理（可以精确到毫秒
•	分布式集群架构中的session分离



# 2 redis 使用

## 2.1 redis 数据结构

### 2.1.1 语法

redis存储 key value格式的数据，其中key都是字符串，value有5种不同的数据结构

```
 command key value
```

***value* 的数据结构**

> 字符串类型 string
>
> 哈希类型 hash ： map格式  
>
> 列表类型 list ： linkedlist格式。支持重复元素
>
> 集合类型 set  ： 不允许重复元素
>
> 有序集合类型 sortedset：不允许重复元素，且元素有顺序



### 2.1.2 String 字符串类型

- **语法**

  ```
  set key value   //存储
  	set username super     
  get key			//获取
  	get username
  del key 		//删除
  	del username
  ```

  

### 2.1.3 Hash 哈希类型

- **语法**

  ```
  hset key field value 		//存储
  	hset person username super
  hget person username		//获取
  	hget person username
  hgetall key					//获取所有的key : value
  	hgetall person	
  hdel key field				//删除
  	hdel person username
  ```

  

### 2.1.4 List 列表类型

- **语法**

  ```
  lpush key value			//元素加入左列表
  	lpush username super
  rpush key value			//元素加入右列表
  	rpush username super
  lrange key start end		//范围获取
  	lrange person 0 -1			//获取全部
  lpop person				//删除列表最左边的元素，并将元素返回
  	lpop person
  rpop key				//删除列表最右边的元素，并将元素返回
  	rpop person
  ```

  

### 2.1.5 Set 集合类型(禁止重复)

- **语法**

  ```
  sadd key value		//存储
  	sadd person a
  smembers key 		/获取所有
  	smembers person
  srem key value		//删除元素
  	srem person a	
  ```

  

### 2.1.6 Sortedset 有序集合类型

- **语法**

  ```
  zadd key score value		//存储
  	zadd person 30 math
  zrange key start end		//获取
  	zrange person 0 -1   		//获取所有
  zrem key value				//删除
  	zren person math	
  ```

  

### 2.1.7 通用命令

- **语法**

  ```
  keys * 		//查询所有的键
  type key	//获取键对应的 value 类型
  del key		//删除指定的key value
  ```

  

## 2.2 持久化

### 2.1 redis 持久化概述

- **介绍**

  redis是一个内存数据库，当redis服务器重启，获取电脑重启，数据会丢失，我们可以将redis内存中的数据持久化保存到硬盘的文件中。



### 2.2 redis 持久化机制

### 2.2.1 RDB 方式

- **概述**

  在一定的间隔时间中，检测key的变化情况，然后持久化数据。 *redis* 默认方式，不需要进行配置，默认就使用这种机制。

- **步骤**

  - **编辑redis.windwos.conf文件**

    ```
    # after 900 sec (15 min) if at least 1 key changed
    save 900 1 
    # after 300 sec (5 min) if at least 10 keys changed
    save 300 10
    # after 60 sec if at least 10000 keys changed
    save 60 10000
    ```

  - **重新启动redis服务器，并指定配置文件名称**

    ```
    D:\JavaWeb2018\day23_redis\资料\redis\windows-64\redis-2.8.9>redis-server.exe redis.windows.conf	
    ```

    

### 2.2.2 AOF 方式

- **概述**

  日志记录的方式，可以记录每一条命令的操作。可以每一次命令操作后，持久化数据

- **步骤**

  - **编辑redis.windwos.conf文件**

    ```
    appendonly no（关闭aof） --> appendonly yes （开启aof）
    # appendfsync always ： 每一次操作都进行持久化
    appendfsync everysec ： 每隔一秒进行一次持久化
    # appendfsync no	 ： 不进行持久化
    ```



## 2.3 Redis 缓存穿透和雪崩

### 2.3.1 缓存穿透

**概念**

>缓存穿透是指**缓存和数据库中都没有的数据**，而用户不断发起请求。由于缓存是不命中时被动写的，并且出于容错考虑，如果从存储层查不到数据则不写入缓存，这将导致这个不存在的数据每次请求都要到存储层去查询，失去了缓存的意义。
>
>在流量大时，可能DB就挂掉了，要是有人利用不存在的key频繁攻击我们的应用，这就是漏洞。
>
>如发起为id为“-1”的数据或id为特别大不存在的数据。这时的用户很可能是攻击者，攻击会导致数据库压力过大。



**解决方案**

| **接口层增加校验**，如用户鉴权校验，id做基础校验，id<=0的直接拦截； |
| ------------------------------------------------------------ |
| 从缓存取不到的数据，在数据库中也没有取到，这时也可以将key-value对写为**key-null**，**缓存有效时间可以设置短点**，如30秒（设置太长会导致正常情况也没法使用）。这样可以防止攻击用户反复用同一个id暴力攻击 |



### 2.3.2 缓存击穿

**概念**

>==缓存击穿是指缓存中没有但数据库中有的数据（一般是缓存时间到期)==，这时由于并发用户特别多，同时读缓存没读到数据，又同时去数据库去取数据，引起数据库压力瞬间增大，造成过大压力。

**解决方案**

>**1、设置热点数据永远不过期。**
>
>**2、接口限流与熔断，降级。**重要的接口一定要做好限流策略，防止用户恶意刷接口，同时要降级准备，当接口中的某些 服务 不可用时候，进行熔断，失败快速返回机制。
>
>**3、布隆过滤器****。**bloomfilter就类似于一个hash set，用于快速判某个元素是否存在于集合中，其典型的应用场景就是快速判断一个key是否存在于某容器，不存在就直接返回。布隆过滤器的关键就在于hash算法和容器大小，
>
>**4、加互斥锁**，互斥锁参考代码如下：

![img](D:\GitRepository\notes-and-resource\学习笔记\技术学习笔记\Java学习笔记\Redis笔记\imgs\70.png)

 说明：

​     1）缓存中有数据，直接走上述代码13行后就返回结果了

​     2）缓存中没有数据，第1个进入的线程，获取锁并从数据库去取数据，没释放锁之前，其他并行进入的线程会等待100ms，再重新去缓存取数据。这样就防止都去数据库重复取数据，重复往缓存中更新数据情况出现。

​     3）当然这是简化处理，理论上如果能根据key值加锁就更好了，就是线程A从数据库取key1的数据并不妨碍线程B取key2的数据，上面代码明显做不到这点。



### 2.3.3 缓存雪崩

**描述**

>[缓存雪崩](https://so.csdn.net/so/search?q=缓存雪崩&spm=1001.2101.3001.7020)是指缓存中数据大批量到过期时间，而查询数据量巨大，引起数据库压力过大甚至down机。和缓存击穿不同的是，    缓存击穿指并发查同一条数据，缓存雪崩是不同数据都过期了，很多数据都查不到从而查数据库。



**解决方案**

>1. 缓存数据的过期时间设置随机，防止同一时间大量数据过期现象发生。
>2. 如果缓存数据库是分布式部署，将热点数据均匀分布在不同搞得缓存数据库中。
>3. 设置热点数据永远不过期。



# 3 Jedis 使用

## 3.1 Jedis 概述

- **概述**

  **Jedis** 是一款 *java* 操作 *redis* 数据库的工具.
  **使用步骤**：

  ```
  1. 下载jedis的jar包
  2. 使用
      //1. 获取连接
      Jedis jedis = new Jedis("localhost",6379);
      //2. 操作
      jedis.set("username","zhangsan");
      //3. 关闭连接
      jedis.close();
  ```

  

## 3.2 Jedis 使用

### 3.2.1 Jedis 操作 String

- **案例**

  ```java
  //1. 获取连接
  Jedis jedis = new Jedis();//如果使用空参构造，默认值 "localhost",6379端口
  //2. 操作
  //存储
  jedis.set("username","zhangsan");
  //获取
  String username = jedis.get("username");
  System.out.println(username);
  
  //可以使用setex()方法存储可以指定过期时间的 key value
  jedis.setex("activecode",20,"hehe");//将activecode：hehe键值对存入redis，并且20秒后自动删除该键值对
  
  //3. 关闭连接
  jedis.close();
  ```

  

### 3.2.2 Jedis 操作 Hash

- **案例**

  ```java
  //1. 获取连接
  Jedis jedis = new Jedis();//如果使用空参构造，默认值 "localhost",6379端口
  //2. 操作
  // 存储hash
  jedis.hset("user","name","lisi");
  jedis.hset("user","age","23");
  jedis.hset("user","gender","female");
  
  // 获取hash
  String name = jedis.hget("user", "name");
  System.out.println(name);
  // 获取hash的所有map中的数据
  Map<String, String> user = jedis.hgetAll("user");
  
  // keyset
  Set<String> keySet = user.keySet();
  for (String key : keySet) {
      //获取value
      String value = user.get(key);
      System.out.println(key + ":" + value);
  }
  
  //3. 关闭连接
  jedis.close();
  ```

  

### 3.2.3 Jedis 操作 List

- **案例**

  ```java
  //1. 获取连接
  Jedis jedis = new Jedis();//如果使用空参构造，默认值 "localhost",6379端口
  //2. 操作
  // list 存储
  jedis.lpush("mylist","a","b","c");//从左边存
  jedis.rpush("mylist","a","b","c");//从右边存
  
  // list 范围获取
  List<String> mylist = jedis.lrange("mylist", 0, -1);
  System.out.println(mylist);
  
  // list 弹出
  String element1 = jedis.lpop("mylist");//c
  System.out.println(element1);
  
  String element2 = jedis.rpop("mylist");//c
  System.out.println(element2);
  
  // list 范围获取
  List<String> mylist2 = jedis.lrange("mylist", 0, -1);
  System.out.println(mylist2);
  
  //3. 关闭连接
  jedis.close();
  ```

  

### 3.2.4 Jedis 操作 Set

- **案例**

  ```java
  //1. 获取连接
  Jedis jedis = new Jedis();//如果使用空参构造，默认值 "localhost",6379端口
  //2. 操作
  // set 存储
  jedis.sadd("myset","java","php","c++");
  
  // set 获取
  Set<String> myset = jedis.smembers("myset");
  System.out.println(myset);
  
  //3. 关闭连接
  jedis.close(); 
  ```

  

### 3.2.5 Jedis 操作 Sortedset

- **案例**

  ```java
  //1. 获取连接
  Jedis jedis = new Jedis();//如果使用空参构造，默认值 "localhost",6379端口
  //2. 操作
  // sortedset 存储
  jedis.zadd("mysortedset",3,"亚瑟");
  jedis.zadd("mysortedset",30,"后裔");
  jedis.zadd("mysortedset",55,"孙悟空");
  
  // sortedset 获取
  Set<String> mysortedset = jedis.zrange("mysortedset", 0, -1);
  
  System.out.println(mysortedset);
  //3. 关闭连接
  jedis.close();
  ```

  

### 3.2.6 JedisPool 连接池

- **案例**

  ```java
  //0.创建一个配置对象
  JedisPoolConfig config = new JedisPoolConfig();
  config.setMaxTotal(50);
  config.setMaxIdle(10);
  
  //1.创建Jedis连接池对象
  JedisPool jedisPool = new JedisPool(config,"localhost",6379);
  
  //2.获取连接
  Jedis jedis = jedisPool.getResource();
  //3. 使用
  jedis.set("hehe","heihei");
  //4. 关闭 归还到连接池中
  jedis.close();
  ```

- **工具类**

  ```java
  public class JedisPoolUtils {
  	private static JedisPool jedisPool;
  	
  	static{
  		//读取配置文件
  		InputStream is = JedisPoolUtils.class.getClassLoader().getResourceAsStream("jedis.properties");
  		//创建Properties对象
  		Properties pro = new Properties();
  		//关联文件
  		try {
  			  pro.load(is);
  		} catch (IOException e) {
  			  e.printStackTrace();
  		}
  		//获取数据，设置到JedisPoolConfig中
  		JedisPoolConfig config = new JedisPoolConfig();
  		config.setMaxTotal(Integer.parseInt(pro.getProperty("maxTotal")));
  		config.setMaxIdle(Integer.parseInt(pro.getProperty("maxIdle")));
  			
  		//初始化JedisPool
  		jedisPool = new JedisPool(config,pro.getProperty("host")
  					,Integer.parseInt(pro.getProperty("port")));
  	}
          /**
          * 获取连接方法
          */
          public static Jedis getJedis(){
          	return jedisPool.getResource();
          }
  }
  ```



### 3.2.7 案例

- **要求**

  ```
  案例需求：
  	1. 提供index.html页面，页面中有一个省份 下拉列表
  	2. 当 页面加载完成后 发送ajax请求，加载所有省份
  * 注意：使用redis缓存一些不经常发生变化的数据。
  	* 数据库的数据一旦发生改变，则需要更新缓存。
  		* 数据库的表执行 增删改的相关操作，需要将redis缓存数据情况，再次存入
  		* 在service对应的增删改方法中，将redis数据删除。
  ```

  

# 4 SpringBoot 集成 Redis

[参考link]: https://blog.csdn.net/dreaming9420/article/details/124146666



## 4.1 工具类

### 4.1.1 自定义 RedisTemplate

$RedisTemplate 自定义, 防止 JDK 序列化格式影响数据写入$

```java
@Configuration
public class RedisConfig {

    //使用 自定义的配置 替换 JdkSerializationRedisSerializer， 否则序列化进 redis 会显示为特殊字符；
    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory connectionFactory) {
        // 创建redisTemplate
        RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();
        redisTemplate.setConnectionFactory(connectionFactory);
        // 使用Jackson2JsonRedisSerialize替换默认序列化
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper objectMapper = new ObjectMapper();
        objectMapper.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        objectMapper.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);

        jackson2JsonRedisSerializer.setObjectMapper(objectMapper);
        // key采用String的序列化方式
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        // value序列化方式采用jackson
        redisTemplate.setValueSerializer(jackson2JsonRedisSerializer);
        // hash的key也采用String的序列化方式
        redisTemplate.setHashKeySerializer(new StringRedisSerializer());
        // hash的value序列化方式采用jackson
        redisTemplate.setHashValueSerializer(jackson2JsonRedisSerializer);
        redisTemplate.afterPropertiesSet();
        return redisTemplate;
    }
}
```



### 4.1.2  Redis 自定义函数

```java
@Component
public final class RedisUtil {

    @Autowired
    private RedisTemplate<String, Object> redisTemplate;

    // =============================common============================
    /**
     * 指定缓存失效时间
     * @param key  键
     * @param time 时间(秒)
     */
    public boolean expire(String key, long time) {
        try {
            if (time > 0) {
                redisTemplate.expire(key, time, TimeUnit.SECONDS);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 根据key 获取过期时间
     * @param key 键 不能为null
     * @return 时间(秒) 返回0代表为永久有效
     */
    public long getExpire(String key) {
        return redisTemplate.getExpire(key, TimeUnit.SECONDS);
    }


    /**
     * 判断key是否存在
     * @param key 键
     * @return true 存在 false不存在
     */
    public boolean hasKey(String key) {
        try {
            return redisTemplate.hasKey(key);
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 删除缓存
     * @param key 可以传一个值 或多个
     */
    @SuppressWarnings("unchecked")
    public void del(String... key) {
        if (key != null && key.length > 0) {
            if (key.length == 1) {
                redisTemplate.delete(key[0]);
            } else {
                redisTemplate.delete(CollectionUtils.arrayToList(key));
            }
        }
    }


    // ============================String=============================

    /**
     * 普通缓存获取
     * @param key 键
     * @return 值
     */
    public Object get(String key) {
        return key == null ? null : redisTemplate.opsForValue().get(key);
    }

    /**
     * 普通缓存放入
     * @param key   键
     * @param value 值
     * @return true成功 false失败
     */

    public boolean set(String key, Object value) {
        try {
            redisTemplate.opsForValue().set(key, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 普通缓存放入并设置时间
     * @param key   键
     * @param value 值
     * @param time  时间(秒) time要大于0 如果time小于等于0 将设置无限期
     * @return true成功 false 失败
     */

    public boolean set(String key, Object value, long time) {
        try {
            if (time > 0) {
                redisTemplate.opsForValue().set(key, value, time, TimeUnit.SECONDS);
            } else {
                set(key, value);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 递增
     * @param key   键
     * @param delta 要增加几(大于0)
     */
    public long incr(String key, long delta) {
        if (delta < 0) {
            throw new RuntimeException("递增因子必须大于0");
        }
        return redisTemplate.opsForValue().increment(key, delta);
    }


    /**
     * 递减
     * @param key   键
     * @param delta 要减少几(小于0)
     */
    public long decr(String key, long delta) {
        if (delta < 0) {
            throw new RuntimeException("递减因子必须大于0");
        }
        return redisTemplate.opsForValue().increment(key, -delta);
    }


    // ================================Map=================================

    /**
     * HashGet
     * @param key  键 不能为null
     * @param item 项 不能为null
     */
    public Object hget(String key, String item) {
        return redisTemplate.opsForHash().get(key, item);
    }

    /**
     * 获取hashKey对应的所有键值
     * @param key 键
     * @return 对应的多个键值
     */
    public Map<Object, Object> hmget(String key) {
        return redisTemplate.opsForHash().entries(key);
    }

    /**
     * HashSet
     * @param key 键
     * @param map 对应多个键值
     */
    public boolean hmset(String key, Map<String, Object> map) {
        try {
            redisTemplate.opsForHash().putAll(key, map);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * HashSet 并设置时间
     * @param key  键
     * @param map  对应多个键值
     * @param time 时间(秒)
     * @return true成功 false失败
     */
    public boolean hmset(String key, Map<String, Object> map, long time) {
        try {
            redisTemplate.opsForHash().putAll(key, map);
            if (time > 0) {
                expire(key, time);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 向一张hash表中放入数据,如果不存在将创建
     *
     * @param key   键
     * @param item  项
     * @param value 值
     * @return true 成功 false失败
     */
    public boolean hset(String key, String item, Object value) {
        try {
            redisTemplate.opsForHash().put(key, item, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 向一张hash表中放入数据,如果不存在将创建
     *
     * @param key   键
     * @param item  项
     * @param value 值
     * @param time  时间(秒) 注意:如果已存在的hash表有时间,这里将会替换原有的时间
     * @return true 成功 false失败
     */
    public boolean hset(String key, String item, Object value, long time) {
        try {
            redisTemplate.opsForHash().put(key, item, value);
            if (time > 0) {
                expire(key, time);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 删除hash表中的值
     *
     * @param key  键 不能为null
     * @param item 项 可以使多个 不能为null
     */
    public void hdel(String key, Object... item) {
        redisTemplate.opsForHash().delete(key, item);
    }


    /**
     * 判断hash表中是否有该项的值
     *
     * @param key  键 不能为null
     * @param item 项 不能为null
     * @return true 存在 false不存在
     */
    public boolean hHasKey(String key, String item) {
        return redisTemplate.opsForHash().hasKey(key, item);
    }


    /**
     * hash递增 如果不存在,就会创建一个 并把新增后的值返回
     *
     * @param key  键
     * @param item 项
     * @param by   要增加几(大于0)
     */
    public double hincr(String key, String item, double by) {
        return redisTemplate.opsForHash().increment(key, item, by);
    }


    /**
     * hash递减
     *
     * @param key  键
     * @param item 项
     * @param by   要减少记(小于0)
     */
    public double hdecr(String key, String item, double by) {
        return redisTemplate.opsForHash().increment(key, item, -by);
    }


    // ============================set=============================

    /**
     * 根据key获取Set中的所有值
     * @param key 键
     */
    public Set<Object> sGet(String key) {
        try {
            return redisTemplate.opsForSet().members(key);
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }


    /**
     * 根据value从一个set中查询,是否存在
     *
     * @param key   键
     * @param value 值
     * @return true 存在 false不存在
     */
    public boolean sHasKey(String key, Object value) {
        try {
            return redisTemplate.opsForSet().isMember(key, value);
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 将数据放入set缓存
     *
     * @param key    键
     * @param values 值 可以是多个
     * @return 成功个数
     */
    public long sSet(String key, Object... values) {
        try {
            return redisTemplate.opsForSet().add(key, values);
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }


    /**
     * 将set数据放入缓存
     *
     * @param key    键
     * @param time   时间(秒)
     * @param values 值 可以是多个
     * @return 成功个数
     */
    public long sSetAndTime(String key, long time, Object... values) {
        try {
            Long count = redisTemplate.opsForSet().add(key, values);
            if (time > 0)
                expire(key, time);
            return count;
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }


    /**
     * 获取set缓存的长度
     *
     * @param key 键
     */
    public long sGetSetSize(String key) {
        try {
            return redisTemplate.opsForSet().size(key);
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }


    /**
     * 移除值为value的
     *
     * @param key    键
     * @param values 值 可以是多个
     * @return 移除的个数
     */

    public long setRemove(String key, Object... values) {
        try {
            Long count = redisTemplate.opsForSet().remove(key, values);
            return count;
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }

    // ===============================list=================================

    /**
     * 获取list缓存的内容
     *
     * @param key   键
     * @param start 开始
     * @param end   结束 0 到 -1代表所有值
     */
    public List<Object> lGet(String key, long start, long end) {
        try {
            return redisTemplate.opsForList().range(key, start, end);
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }


    /**
     * 获取list缓存的长度
     *
     * @param key 键
     */
    public long lGetListSize(String key) {
        try {
            return redisTemplate.opsForList().size(key);
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }


    /**
     * 通过索引 获取list中的值
     *
     * @param key   键
     * @param index 索引 index>=0时， 0 表头，1 第二个元素，依次类推；index<0时，-1，表尾，-2倒数第二个元素，依次类推
     */
    public Object lGetIndex(String key, long index) {
        try {
            return redisTemplate.opsForList().index(key, index);
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }


    /**
     * 将list放入缓存
     *
     * @param key   键
     * @param value 值
     */
    public boolean lSet(String key, Object value) {
        try {
            redisTemplate.opsForList().rightPush(key, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 将list放入缓存
     * @param key   键
     * @param value 值
     * @param time  时间(秒)
     */
    public boolean lSet(String key, Object value, long time) {
        try {
            redisTemplate.opsForList().rightPush(key, value);
            if (time > 0)
                expire(key, time);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }

    }


    /**
     * 将list放入缓存
     *
     * @param key   键
     * @param value 值
     * @return
     */
    public boolean lSet(String key, List<Object> value) {
        try {
            redisTemplate.opsForList().rightPushAll(key, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }

    }


    /**
     * 将list放入缓存
     *
     * @param key   键
     * @param value 值
     * @param time  时间(秒)
     * @return
     */
    public boolean lSet(String key, List<Object> value, long time) {
        try {
            redisTemplate.opsForList().rightPushAll(key, value);
            if (time > 0)
                expire(key, time);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 根据索引修改list中的某条数据
     *
     * @param key   键
     * @param index 索引
     * @param value 值
     * @return
     */

    public boolean lUpdateIndex(String key, long index, Object value) {
        try {
            redisTemplate.opsForList().set(key, index, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 移除N个值为value
     *
     * @param key   键
     * @param count 移除多少个
     * @param value 值
     * @return 移除的个数
     */

    public long lRemove(String key, long count, Object value) {
        try {
            Long remove = redisTemplate.opsForList().remove(key, count, value);
            return remove;
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }

    }

}
```





## 4.2 RedisTemplate 基础操作

### 4.2.1 RedisTemplate 操作字符串

```java
    @Test
    void string_test() {
        String [] kv = new String[2];
        kv[0] = "key1";
        kv[1] = "value1";
        redisTemplate.opsForValue().set(kv[0], kv[1]);
        System.out.println("获取 " + kv[0] + " :" + redisTemplate.opsForValue().get(kv[0]));

        // 设置 60s 自动过期
        String key = "today";
        String value = "周六";
        long time = 60;
        redisTemplate.opsForValue().set(key, value, time, TimeUnit.SECONDS);
    }
```



### 4.2.2 redisTemplate 操作 key

```java
    @Test
    void key_test() {
        String key = "zszxz";
        redisTemplate.opsForValue().set(key, "value");
        Boolean exist = redisTemplate.hasKey(key);
        System.out.println(exist);

        //设置剩余时间
        long time = 60;
        redisTemplate.expire(key, time, TimeUnit.SECONDS);

        //获取剩余时间
        Long expire = redisTemplate.getExpire(key, TimeUnit.SECONDS);
        System.out.println(expire);

        //解除 key
        redisTemplate.delete(key);
    }
```



### 4.2.3 redisTemplate 操作 Hash/Map

```java
    @Autowired
    RedisTemplate<String,Object> redisTemplate;
​
    // 放入一个 hash ( key value )
    @Test
    public void test1(){
        String key = "zszxz";
        String item = "name";
        String value = "知识追寻者";
        redisTemplate.opsForHash().put(key, item, value);
    }
​
    // 向hash中存放一个map
    @Test
    public void test2(){
        String key = "feature";
        Map<String, Object> map = new HashMap<>();
        map.put("name", "知识追寻者");
        map.put("age", "18");
        redisTemplate.opsForHash().putAll(key, map);
    }
​
    // 获取一个hash 的 所有key-value
    @Test
    public void test3(){
        String key = "feature";
        Map<Object, Object> entries = redisTemplate.opsForHash().entries(key);
        // {name=知识追寻者, age=18}
        System.out.println(entries);
    }
​
    // 获取一个hash 的 指定key 的value
    @Test
    public void test4(){
        String key = "feature";
        String item = "name";
        Object value = redisTemplate.opsForHash().get(key, item);
        // 知识追寻者
        System.out.println(value);
    }
​
    // 删除指定 hash key 的value
    @Test
    public void test5(){
        String key = "zszxz";
        String item = "name";
        redisTemplate.opsForHash().delete(key, item);
    }
​
    // 是否存在 指定 hash 的key
    @Test
    public void test6(){
        String key = "zszxz";
        String item = "name";
        Boolean exist = redisTemplate.opsForHash().hasKey(key, item);
        // false
        System.out.println(exist);
    }
```



### 4.2.4 redisTemplate 操作 列表

```java
  @Autowired
    RedisTemplate<String,Object> redisTemplate;
​
    @Test
    public void test(){
​
    }
​
    // 列表右推入
    @Test
    public void test1(){
        String key = "zszxz";
        String value = "知识追寻者";
        redisTemplate.opsForList().rightPush(key, value);
    }
​
    // 列表左推入
    @Test
    public void test2(){
        String key = "zszxz";
        String value = "晴雨天";
        redisTemplate.opsForList().leftPush(key, value);
    }
    // 列表左弹出
    @Test
    public void test3(){
        String key = "zszxz";
        Object value = redisTemplate.opsForList().leftPop(key);
        // 晴雨天
        System.out.println(value);
​
    }
    // 列表右弹出
    @Test
    public void test4(){
        String key = "zszxz";
        Object value = redisTemplate.opsForList().rightPop(key);
        // 知识追寻者
        System.out.println(value);
    }
​
    // 将list右推入列表
    @Test
    public void test5(){
        ArrayList<Object> list = new ArrayList<>();
        list.add(1);
        list.add(2);
        list.add(3);
        String key = "number";
        redisTemplate.opsForList().rightPushAll(key, list);
    }
​
    // 修改列表指定索引的值
    @Test
    public void test7(){
        String key = "number";
        int index = 0;
        int value = 666;
        redisTemplate.opsForList().set(key, index, value);
    }
​
    // 获取列表指定索引的值
    @Test
    public void test8(){
        String key = "number";
        int index = 0;
        Object value = redisTemplate.opsForList().index(key, index);
        // 666
        System.out.println(value);
    }
```



### 4.2.5 redisTemplate 操作 set

```java
 @Autowired
    RedisTemplate<String,Object> redisTemplate;
​
    // set 中存储值
    @Test
    public void test1(){
        String key = "zszxz";
        String value1 = "晴雨天";
        String value2 = "公众号：知识追寻者";
        redisTemplate.opsForSet().add(key, value1, value2);
    }
​
    // 从 set 中取值
    @Test
    public void test2(){
        String key = "zszxz";
        Set<Object> members = redisTemplate.opsForSet().members(key);
        // [晴雨天, 公众号：知识追寻者]
        System.out.println(members);
    }
​
    // 判定 set 中是否存在 key-value
    @Test
    public void test3(){
        String key = "zszxz";
        String value = "晴雨天";
        Boolean member = redisTemplate.opsForSet().isMember(key, value);
        // true
        System.out.println(member);
    }
```



## 4.3 SpringBoot 集成 Redis 实现 实体类缓存

### 4.3.1 注解介绍

#### (1) @Cacheable
如果缓存中不存在目标值，则将调用目标方法并将返回的值存入缓存；如果存在，则直接返回缓存中的值，不会执行方法体。即使方法体内进行了数据库的更新操作，也不会执行。

该注解常用参数如下：

- cacheNames/value ：存储方法调用结果的缓存的名称
- key ：缓存数据使用的key，可以用它来指定，key="#param"可以指定参数值，也可以是其他属性

- keyGenerator ：key的生成器，用来自定义key的生成，与key为二选一，不能兼存
- condition：用于使方法缓存有条件，默认为"" ，表示方法结果始终被缓存。conditon="#id>1000"表示id>1000的数据才进行缓存
- unless：用于否决方法缓存，此表达式在方法被调用后计算，因此可以引用方法返回值(result)，默认为"" ，这意味着缓存永远不会被否决。unless = "#result==null"表示除非该方法返回值为null，否则将方法返回值进行缓存
- sync ：是否使用异步模式，默认为false不使用异步



#### (2) @CachePut
如果缓存中先前存在目标值，则更新缓存中的值为该方法的返回值；如果不存在，则将方法的返回值存入缓存。

该注解常用参数同@Cacheable，不过@CachePut没有sync 这个参数



#### (3) @CacheEvict
如果缓存中存在存在目标值，则将其从缓存中删除
该注解常用参数如下：

- cacheNames/value、key、keyGenerator、condition同@Cacheable
- allEntries：如果指定allEntries为true，Spring Cache将忽略指定的key清除缓存中的所有元素，默认情况下为false。
- beforeInvocation：删除缓存操作默认是在对应方法成功执行之后触发的，方法如果因为抛出异常而未能成功返回时也不会触发删除操作。如果指定beforeInvocation为true ，则无论方法结果如何，无论方法是否抛出异常都会导致删除缓存。
  

### 4.3.2 相关实现 - 版本1

> 该方法主要使用了 ==**SpringCache + redis**== 的组合完成实体类的缓存, 主体配置包括 **RedisConfig** 和 **ServiceImpl** 两个类, 其余如 Bean、Mapper、Controller 按照以往的方式配置即可。
>
> 此外, 基础版的实现需要使用 `redisTemplate.opsForValue().set(key, "value");` + `分布式锁` 的方式完成, 该版本为 **Spring** 提供的简化方法。

- **pom.xml**

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
      <modelVersion>4.0.0</modelVersion>
      <groupId>com.example</groupId>
      <artifactId>demo</artifactId>
      <version>0.0.1-SNAPSHOT</version>
      <name>Springboot_redis</name>
      <description>Demo project for Spring Boot</description>
  
      <properties>
          <java.version>1.8</java.version>
          <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
          <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
          <spring-boot.version>2.3.7.RELEASE</spring-boot.version>
      </properties>
  
      <dependencies>
          <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
          <dependency>
              <groupId>mysql</groupId>
              <artifactId>mysql-connector-java</artifactId>
              <version>8.0.29</version>
          </dependency>
          <!-- https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter -->
          <dependency>
              <groupId>org.mybatis.spring.boot</groupId>
              <artifactId>mybatis-spring-boot-starter</artifactId>
              <version>2.2.2</version>
          </dependency>
  
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-data-redis</artifactId>
          </dependency>
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-web</artifactId>
          </dependency>
  
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-devtools</artifactId>
              <scope>runtime</scope>
              <optional>true</optional>
          </dependency>
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-configuration-processor</artifactId>
              <optional>true</optional>
          </dependency>
          <dependency>
              <groupId>org.projectlombok</groupId>
              <artifactId>lombok</artifactId>
              <optional>true</optional>
          </dependency>
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-test</artifactId>
              <scope>test</scope>
              <exclusions>
                  <exclusion>
                      <groupId>org.junit.vintage</groupId>
                      <artifactId>junit-vintage-engine</artifactId>
                  </exclusion>
              </exclusions>
          </dependency>
      </dependencies>
  
      <dependencyManagement>
          <dependencies>
              <dependency>
                  <groupId>org.springframework.boot</groupId>
                  <artifactId>spring-boot-dependencies</artifactId>
                  <version>${spring-boot.version}</version>
                  <type>pom</type>
                  <scope>import</scope>
              </dependency>
          </dependencies>
      </dependencyManagement>
  
      <build>
          <plugins>
              <plugin>
                  <groupId>org.apache.maven.plugins</groupId>
                  <artifactId>maven-compiler-plugin</artifactId>
                  <version>3.8.1</version>
                  <configuration>
                      <source>1.8</source>
                      <target>1.8</target>
                      <encoding>UTF-8</encoding>
                  </configuration>
              </plugin>
              <plugin>
                  <groupId>org.springframework.boot</groupId>
                  <artifactId>spring-boot-maven-plugin</artifactId>
                  <version>2.3.6.RELEASE</version>
                  <configuration>
                      <mainClass>com.example.SpringbootRedisApplication</mainClass>
                  </configuration>
                  <executions>
                      <execution>
                          <id>repackage</id>
                          <goals>
                              <goal>repackage</goal>
                          </goals>
                      </execution>
                  </executions>
              </plugin>
          </plugins>
      </build>
  
  </project>
  ```

- **application.yaml**

  ```java
  server:
    port: 8081
  spring:
    application:
      name: Springboot_redis
    datasource:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/demospace?useSSL=true&useUnicode=true&characterEncoding=UTF8&serverTimezone=Asia/Shanghai
      username: root
      password: yan19991001
    redis:
      host: 127.0.0.1
      port: 6379
  # mybatis
  mybatis:
    type-aliases-package: com.example.bean
    mapper-locations: classpath:mapper/*.xml
  
  logging:
    level:
      com.example: debug
  ```

- **Bean**

  ```
  package com.example.bean;
  
  import lombok.Data;
  import lombok.NoArgsConstructor;
  
  @Data
  @NoArgsConstructor
  public class User {
      int id;
      String name;
      int age;
  
  }
  ```

- **UserServiceImpl**

  ```java
  package com.example.service.impl;
  
  import com.example.bean.User;
  import com.example.mapper.UserMapper;
  import com.example.service.UserService;
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.cache.annotation.CacheEvict;
  import org.springframework.cache.annotation.CachePut;
  import org.springframework.cache.annotation.Cacheable;
  import org.springframework.data.redis.core.RedisTemplate;
  import org.springframework.stereotype.Service;
  
  import java.util.ArrayList;
  import java.util.List;
  
  @Service
  public class UserServiceImpl implements UserService {
      @Autowired
      private UserMapper userMapper;
  
      public RedisTemplate redisTemplate;
  
      @Override
      @CachePut(cacheNames = "user", key = "#user.id")
      public void add(User user) {
          userMapper.add(user);
          System.out.println(user);
      }
  
      @Override
      @Cacheable(cacheNames = "user", key = "#id")
      public User findById(int id) {
          // 如果不存在则查询数据库,并把查询的结果放入缓存中
          User user = userMapper.findById(id);
          System.out.println(user);
          return user;
      }
  
      @Override
      @Cacheable(cacheNames = "user", key = "#id")
      public List<User> findAll() {
          return userMapper.findAll();
      }
  
      // 先执行方法体中的代码，成功执行之后删除缓存
      @CacheEvict(cacheNames = "user", key = "#ids")
      @Override
      public void deleteSelected(String ids) {
          if (ids.contains(",")) {
              String[] split = ids.split(",");
              List<Integer> list = new ArrayList<Integer>();
              for (String s : split) {
                  int i = 0;
                  try {
                      i = Integer.parseInt(s);
                  } catch (NumberFormatException e) {
  
                  }
                  list.add(i);
              }
              for (int id : list) {
                  userMapper.delete(id);
              }
          }else {
              userMapper.delete(Integer.parseInt(ids));
          }
      }
  
      // 如果缓存中先前存在，则更新缓存;如果不存在，则将方法的返回值存入缓存
      @CachePut(cacheNames = "user", key = "#user.id")
      @Override
      public void modify(User user) {
          userMapper.modify(user);
      }
  
  
      @Override
      public List<User> search(User user) {
          return userMapper.search(user);
      }
  }
  ```

- **RedisConfig**

  ```java
  package com.example.config;
  
  import com.fasterxml.jackson.annotation.JsonAutoDetect;
  import com.fasterxml.jackson.annotation.PropertyAccessor;
  import com.fasterxml.jackson.databind.ObjectMapper;
  import com.fasterxml.jackson.databind.jsontype.impl.LaissezFaireSubTypeValidator;
  import org.springframework.cache.CacheManager;
  import org.springframework.cache.annotation.CachingConfigurerSupport;
  import org.springframework.cache.annotation.EnableCaching;
  import org.springframework.context.annotation.Bean;
  import org.springframework.context.annotation.Configuration;
  import org.springframework.data.redis.cache.RedisCacheConfiguration;
  import org.springframework.data.redis.cache.RedisCacheManager;
  import org.springframework.data.redis.connection.RedisConnectionFactory;
  import org.springframework.data.redis.core.RedisTemplate;
  import org.springframework.data.redis.serializer.Jackson2JsonRedisSerializer;
  import org.springframework.data.redis.serializer.RedisSerializationContext;
  import org.springframework.data.redis.serializer.RedisSerializer;
  import org.springframework.data.redis.serializer.StringRedisSerializer;
  
  import java.time.Duration;
  import java.util.Random;
  
  @EnableCaching
  @Configuration
  public class RedisConfig extends CachingConfigurerSupport {
      @Bean
      public CacheManager RedisCacheManager(RedisConnectionFactory factory) {
          RedisSerializer<String> redisSerializer = new StringRedisSerializer();
          Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
          // 解决查询缓存转换异常的问题
          ObjectMapper om = new ObjectMapper();
          om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
  
          /**
           * 新版本中om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL)已经被废弃
           * 建议替换为om.activateDefaultTyping(LaissezFaireSubTypeValidator.instance, ObjectMapper.DefaultTyping.NON_FINAL)
           */
          om.activateDefaultTyping(LaissezFaireSubTypeValidator.instance, ObjectMapper.DefaultTyping.NON_FINAL);
          jackson2JsonRedisSerializer.setObjectMapper(om);
          // 配置序列化解决乱码的问题
          RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
                  // 设置缓存过期时间  为解决缓存雪崩,所以将过期时间加随机值
                  .entryTtl(Duration.ofSeconds(60 * 60 + new Random().nextInt(60 * 10)))
                  // 设置key的序列化方式
                  .serializeKeysWith(RedisSerializationContext.SerializationPair.fromSerializer(redisSerializer))
                  // 设置value的序列化方式
                  .serializeValuesWith(RedisSerializationContext.SerializationPair.fromSerializer(jackson2JsonRedisSerializer));
          // .disableCachingNullValues(); //为防止缓存击穿，所以允许缓存null值
          RedisCacheManager cacheManager = RedisCacheManager.builder(factory)
                  .cacheDefaults(config)
                  // 启用RedisCache以将缓存 put/evict 操作与正在进行的 Spring 管理的事务同步
                  .transactionAware()
                  .build();
          return cacheManager;
      }
  
  
      //使用 自定义的配置 替换 JdkSerializationRedisSerializer， 否则序列化进 redis 会显示为特殊字符；
      @Bean
      public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory connectionFactory) {
          // 创建redisTemplate
          RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();
          redisTemplate.setConnectionFactory(connectionFactory);
          // 使用Jackson2JsonRedisSerialize替换默认序列化
          Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
          ObjectMapper objectMapper = new ObjectMapper();
          objectMapper.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
          objectMapper.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
  
          jackson2JsonRedisSerializer.setObjectMapper(objectMapper);
          // key采用String的序列化方式
          redisTemplate.setKeySerializer(new StringRedisSerializer());
          // value序列化方式采用jackson
          redisTemplate.setValueSerializer(jackson2JsonRedisSerializer);
          // hash的key也采用String的序列化方式
          redisTemplate.setHashKeySerializer(new StringRedisSerializer());
          // hash的value序列化方式采用jackson
          redisTemplate.setHashValueSerializer(jackson2JsonRedisSerializer);
          redisTemplate.afterPropertiesSet();
          return redisTemplate;
      }
  
  }
  ```

- **Controller**

  ```java
  package com.example.controller;
  
  import com.example.bean.User;
  import com.example.service.UserService;
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.stereotype.Controller;
  import org.springframework.web.bind.annotation.*;
  
  import java.util.List;
  
  @Controller
  @RequestMapping("/user")
  public class UserController {
      @Autowired
      private UserService userService;
  
      @RequestMapping("/addUser")
      @ResponseBody
      public String addUser(){
          User user = new User();
          userService.add(user);
          return "ok";
      }
  
      @RequestMapping("/getUser/{id}")
      @ResponseBody
      public User getUser(@PathVariable("id") int id){
          System.out.println("controller:"  + userService.findById(id));
          return userService.findById(id);
      }
  
      @RequestMapping("/loadUsers")
      @ResponseBody
      public List<User> loadUsers(){
          return userService.findAll();
      }
  
      @RequestMapping("/deleteUser/{id}")
      @ResponseBody
      public String deleteUser(@PathVariable("id") int id){
          userService.deleteSelected(Integer.toString(id));
          return Integer.toString(id);
      }
  }
  ```




### 4.3.3 相关实现 - 版本2

> SpringBoot项目使用 `SpringCache`时需要在 `CacheConfig` 或 `主启动类`上添加 `@EnableCaching` 注解告知Spring项目使用了 `Redis缓存` 功能。

- **依赖**

  ```xml
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-data-redis</artifactId>
          </dependency>
  
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-cache</artifactId>
          </dependency>
  ```

  

- **主启动类**

  ```java
  @Slf4j		//开启日志功能
  @SpringBootApplication		//启动类
  @ServletComponentScan		//自动扫描 servlet
  @EnableTransactionManagement	//开启事务, 支持事务ACID特性, 能够回滚, 要么都执行要么都不执行
  @EnableCaching		//开启缓存功能, 搭配 @Chcheput 等注解使用, 记得配置 Redis 服务
  public class ReggieApplication {
      public static void main(String[] args) {
          SpringApplication.run(ReggieApplication.class,args);
          log.info("项目启动成功...");
      }
  }	
  ```

- **配置类**

  ```yaml
    redis:
      host: 127.0.0.1
      port: 6379
    cache:
      redis:
        time-to-live: 1800000 #设置缓存过期时间
  ```

- **使用类**

  ```java
  @RestController
  @RequestMapping("/setmeal")
  @Slf4j
  public class SetmealController {
      @Autowired
      private SetmealService setmealService;
  
      @Autowired
      private SetmealDishService setSetmealDishService;
  
      @Autowired
      private CategoryService categoryService;
  
      @PostMapping
      @CacheEvict(value = "setmealCache", allEntries = true)  //清理 setmeal 缓存下的所有数据
      public R<String> save(@RequestBody SetmealDto setmealDto){
          log.info("套餐信息:{}", setmealDto);
          setmealService.saveWithDish(setmealDto);
          return R.success("套餐保存成功");
      }
  
      @GetMapping("/page")
      public R<Page> page(int page, int pageSize,String name){
          //创建配置对象
          Page<Setmeal> pageinfo = new Page<>(page, pageSize);
          Page<SetmealDto> setmealDtoPage = new Page<>();
          //条件构造器, 套餐名称
          LambdaQueryWrapper<Setmeal> queryWrapper = new LambdaQueryWrapper<>();
          queryWrapper.like(name != null, Setmeal::getName,name);
          queryWrapper.orderByDesc(Setmeal::getUpdateTime);
          setmealService.page(pageinfo, queryWrapper);
  
          //对象拷贝 - 拷贝 apgeinfo 具有的 records 属性
          BeanUtils.copyProperties(pageinfo, setmealDtoPage, "records");
          //深拷贝, 将分类对象拷贝到 setmealdto 中
          List<Setmeal> records = pageinfo.getRecords();
          List<SetmealDto> list = records.stream().map((item) -> {
              SetmealDto setmealDto = new SetmealDto();
              //对象拷贝
              BeanUtils.copyProperties(item, setmealDto);
              Long categoryId = item.getCategoryId();
              //根据分类 id 查询分类对象
              Category category = categoryService.getById(categoryId);
              if(category != null){
                  //获取分类名称
                  String categoryName = category.getName();
                  setmealDto.setCategoryName(categoryName);
              }
              return setmealDto;
          }).collect(Collectors.toList());
  
          setmealDtoPage.setRecords(list);
          return R.success(setmealDtoPage);
      }
  
      /**
       * 删除套餐
       * @param ids
       * @return
       */
      @DeleteMapping
      @CacheEvict(value = "setmealCache", allEntries = true)  //清理 setmeal 缓存下的所有数据
      public R<String> delete(@RequestParam List<Long> ids){
          log.info("删除套餐{}", ids);
          setmealService.removeWithDish(ids);
          return R.success("套餐删除成功");
      }
  
      /**
       * 根据条件查询套餐数据
       * @param setmeal
       * @return
       */
      @GetMapping("/list")
      @Cacheable(value = "setmealCache", key="#setmeal.categoryId + '_' + #setmeal.status")	//cache 名称, key 名称
      public R<List<Setmeal>> list(Setmeal setmeal){
          LambdaQueryWrapper<Setmeal> queryWrapper = new LambdaQueryWrapper<>();
          queryWrapper.eq(setmeal.getCategoryId()!=null,Setmeal::getCategoryId,setmeal.getCategoryId());
          queryWrapper.eq(setmeal.getStatus() != null, Setmeal::getStatus,setmeal.getStatus());
          queryWrapper.orderByDesc(Setmeal::getUpdateTime);
          List<Setmeal> list = setmealService.list(queryWrapper);
          return R.success(list);
      }
  }
  ```

  

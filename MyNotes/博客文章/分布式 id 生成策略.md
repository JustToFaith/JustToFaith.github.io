# 分布式 ID 生成策略

[TOC]

## 为什么要使用分布式 id 生成策略



## 数据库自增

数据库自增通过设置主键的`auto_increment`（自增实现），这种方法比较简单，但是这种方法在分布式应用中会带来 ID 重复的问题。并且单机版的 ID 自增，由于每生成一个 ID 都会访问数据库一次，这会增大数据库的访问压力，无法体现分布式的性能。



## 数据库水平拆分，设置初始值和相同的自增步长

数据库水平拆分，设置初始值和相同的自增，是指在 DB 集群的环境下，将数据库进行水平划分，然后每个数据库设置**不同的初始值**和**相同的步长**，这样来规避 ID 重复的问题。

如图：

![img](https://tva1.sinaimg.cn/large/007S8ZIlly1ggji6d7dunj30r40ajaav.jpg)

假设有三个数据库，为每一个数据库设置一个**初始值**和**步长**，可以通过如下`sql`语句设置：

```sql
set @@auto_increment_offset = 1;     // 设置初始值
set @@auto_increment_increment = 2;  // 设置步长
```

三个数据的初始值分别设置为1、2、3，一般步长设置为数据库的数据，这里数据库数量为3，所以步长也设置为3。

但是这种方法有一个缺点，由于前期已经设置了步长，如果需要扩容就无法进行扩容。解决的方法是在前期设置的时候，可以将步长设置常一点，**预留一些初始值给后续扩容使用**。

缺点：**后期可能会面对无ID初始值可分的窘境，数据库总归是数据库，抗高并发也是有限的**

优点：**DB单点的问题**



## 批量申请自增 ID

批量申请自增 ID 的解决方案可以解决**无 ID 可分配**的问题，它的原理是一次性给对应数据库上分配一批的 id 值进行消费，使用完了之后再来申请。

![img](https://tva1.sinaimg.cn/large/007S8ZIlly1ggjimplvtij30o40aidgf.jpg)

在设计初始阶段可以设计一个有初始值字段，并有步长的字段表，每次要申请批量 ID 的时候，就可以到该表中申请，每次申请后**初始值=上一次的初始值+步长**。

这种方法有一个缺点，不能扛真正意义上的高并发。



## UUID 生成

UUID 的核心思想是使用**机器的网卡、当地时间、一个随机数**来生产 UUID。

我们使用 UUID 的方式只需调用`UUID.randomUUID().toString()`就可以生成，这种方式简单，本地生成，不会消耗网络。

缺点：

1. 不利于存储，16 字节 128 位，通常是以 36 位长度的字符串表示，很多的场景都不合适。
2. UUID 生成的无序的字符串，查询效率低下，没有实际的业务含义，不具备自增特性，所以分布式都不会使用 UUID 作为分布式 ID 使用。

生成 UUID的方式：

1. 版本1 - 基于时间的UUID：主要依赖当前的时间戳及机器mac地址，因此可以保证全球唯一性
   - 优点：能基本保证全球唯一性
   - 缺点：使用了Mac地址，因此会暴露Mac地址和生成时间

2. 版本2 - 分布式安全的UUID：将版本1的时间戳前四位换为POSIX的UID或GID，很少使用
   - 优点：能保证全球唯一性
   - 缺点：很少使用，常用库基本没有实现

3. 版本3 - 基于名字空间的UUID（MD5版）：基于指定的名字空间/名字生成MD5散列值得到，标准不推荐
   - 优点：不同名字空间或名字下的UUID是唯一的；相同名字空间及名字下得到的UUID保持重复。
   - 缺点：MD5碰撞问题，只用于向后兼容，后续不再使用

4. 版本4 - 基于随机数的UUID：基于随机数或伪随机数生成s
   - 优点：实现简单
   - 缺点：重复几率可计算

5. 版本5 - 基于名字空间的UUID（SHA1版）：将版本3的散列算法改为SHA1
   - 优点：不同名字空间或名字下的UUID是唯一的；相同名字空间及名字下得到的UUID保持重复。
   - 缺点：SHA1计算相对耗时

## Redis 的方式

为了解决上面纯关系型数据库生成分布式ID无法抗高并发的问题，可以使用Redis的方式来生成分布式ID。

Redis 本身有`incr`和`increby`这样的自增命令，保证原子性，生成的 ID 也是有序的。

Redis 基于内存操作，性能高效， 不依赖与数据库，数据天然有序，有利于分页和排序。

但是因为加了中间件，需要自己编码实现，工作量增大，增加复杂度。

#### Redis 持久化

- RDB：**RDB是以快照的形式进行持久化，会丢失上一次快照至此时间的数据**
- AOF：**AOF可以设置一秒持久化一次，丢失的数据是秒内的」**，也会存在可能上一次自增后的秒内的ID没有持久化的问题。

#### Redis的生成分布式ID的工具类代码

##### 第一种是使用`RedisAtomicLong` 原子类使用CAS操作来生成ID。

```java
@Service
public class RedisSequenceFactory {
    @Autowired
    RedisTemplate<String, String> redisTemplate;

    public void setSeq(String key, int value, Date expireTime) {
        RedisAtomicLong counter = new RedisAtomicLong(key, redisTemplate.getConnectionFactory());
        counter.set(value);
        counter.expireAt(expireTime);
    }

    public void setSeq(String key, int value, long timeout, TimeUnit unit) {
        RedisAtomicLong counter = new RedisAtomicLong(key, redisTemplate.getConnectionFactory());
        counter.set(value);
        counter.expire(timeout, unit);
    }

    public long generate(String key) {
        RedisAtomicLong counter = new RedisAtomicLong(key, redisTemplate.getConnectionFactory());
        return counter.incrementAndGet();
    }

    public long incr(String key, Date expireTime) {
        RedisAtomicLong counter = new RedisAtomicLong(key, redisTemplate.getConnectionFactory());
        counter.expireAt(expireTime);
        return counter.incrementAndGet();
    }

    public long incr(String key, int increment) {
        RedisAtomicLong counter = new RedisAtomicLong(key, redisTemplate.getConnectionFactory());
        return counter.addAndGet(increment);
    }

    public long incr(String key, int increment, Date expireTime) {
        RedisAtomicLong counter = new RedisAtomicLong(key, redisTemplate.getConnectionFactory());
        counter.expireAt(expireTime);
        return counter.addAndGet(increment);
    }
}

```

##### 第二种是使用`redisTemplate.opsForHash()`和结合`UUID`的方式来生成生成ID。

```java
public Long getSeq(String key,String hashKey,Long delta) throws BusinessException{
        try {
            if (null == delta) {
                delta=1L;
            }
            return redisTemplate.opsForHash().increment(key, hashKey, delta);
        } catch (Exception e) {  // 若是redis宕机就采用uuid的方式
            int first = new Random(10).nextInt(8) + 1;
            int randNo=UUID.randomUUID().toString().hashCode();
            if (randNo < 0) {
                randNo=-randNo;
            }
            return Long.valueOf(first + String.format("%16d", randNo));
        }
    }

```



## 雪花算法

采用64bit作为id生成类型，并且将64bit划分为，如下图的几段。

![img](https://tva1.sinaimg.cn/large/007S8ZIlly1ggjk99kmh4j30m807j75h.jpg)

第一位作为标识位，因为Java中long类型的时代符号的，因为ID位正数，所以第一位位0。

接着的41bit是时间戳，毫秒级位单位，注意这里的时间戳并不是指当前时间的时间戳，而是值之间差（**「当前时间-开始时间」**）。开始时间一般是指ID生成器的开始时间，是由我们程序自己指定的。

接着后面的10bit：包括5位的**「数据中心标识ID（datacenterId）和5位的机器标识ID（workerId）」**，可以最多标识1024个节点（1<<10=1024）。

最的12位是序列号，12位的计数顺序支持每个节点每毫秒差生4096序列号（1<<12=4096）。

雪花算法使用数据中心ID和机器ID作为标识，不会产生ID的重复，并且是在本地生成，不会消耗网络，效率高，有数据显示，每秒能生成26万个ID。



缺点：雪花算法的计算依赖于时间，若是系统的时间回拨，就会产生重复 ID 的情况。

解决方案：若是其前置的时间等于当前的时间，就抛出异常，也可以关闭掉时间回拨。对于回拨时间比较短的，可以等待回拨时间过后再生成ID。

代码实现：

```java
/**
 * 雪花算法
 * @author：黎杜
 */
public class SnowflakeIdWorker {

    /** 开始时间截 */
    private final long twepoch = 1530051700000L;

    /** 机器id的位数 */
    private final long workerIdBits = 5L;

    /** 数据标识id的位数 */
    private final long datacenterIdBits = 5L;

    /** 最大的机器id，结果是31 */
    private final long maxWorkerId = -1L ^ (-1L << workerIdBits);

    /** 最大的数据标识id，结果是31 */
    private final long maxDatacenterId = -1L ^ (-1L << datacenterIdBits);

    /** 序列的位数 */
    private final long sequenceBits = 12L;

    /** 机器ID向左移12位 */
    private final long workerIdShift = sequenceBits;

    /** 数据标识id向左移17位 */
    private final long datacenterIdShift = sequenceBits + workerIdBits;

    /** 时间截向左移22位*/
    private final long timestampLeftShift = sequenceBits + workerIdBits + datacenterIdBits;

    /** 生成序列的掩码 */
    private final long sequenceMask = -1L ^ (-1L << sequenceBits);

    /** 工作机器ID(0~31) */
    private long workerId;

    /** 数据中心ID(0~31) */
    private long datacenterId;

    /** 毫秒内序列(0~4095) */
    private long sequence = 0L;

    /** 上次生成ID的时间截 */
    private long lastTimestamp = -1L;

    /**
     * 构造函数
     * @param workerId 工作ID (0~31)
     * @param datacenterId 数据中心ID (0~31)
     */
    public SnowflakeIdWorker(long workerId, long datacenterId) {
        if (workerId > maxWorkerId || workerId < 0) {
            throw new IllegalArgumentException(String.format("worker Id can't be greater than %d or less than 0", maxWorkerId));
        }
        if (datacenterId > maxDatacenterId || datacenterId < 0) {
            throw new IllegalArgumentException(String.format("datacenter Id can't be greater than %d or less than 0", maxDatacenterId));
        }
        this.workerId = workerId;
        this.datacenterId = datacenterId;
    }

    /**
     * 获得下一个ID (该方法是线程安全的)
     * @return SnowflakeId
     */
    public synchronized long nextId() {
        long timestamp = getCurrentTime();

        //如果当前时间小于上一次生成的时间戳，说明系统时钟回退过就抛出异常
        if (timestamp < lastTimestamp) {
            throw new BusinessionException("回拨的时间为："+lastTimestamp - timestamp);
        }

        //如果是同一时间生成的，则进行毫秒内序列
        if (lastTimestamp == timestamp) {
            sequence = (sequence + 1) & sequenceMask;
            //毫秒内序列溢出
            if (sequence == 0) {
                //获得新的时间戳
                timestamp = tilNextMillis(lastTimestamp);
            }
        } else {  //时间戳改变，毫秒内序列重置
            sequence = 0L;
        }

        //上次生成ID的时间截
        lastTimestamp = timestamp;

        //移位并通过或运算拼到一起组成64位的ID
        return ((timestamp - twepoch) << timestampLeftShift) // 计算时间戳
                | (datacenterId << datacenterIdShift) // 计算数据中心
                | (workerId << workerIdShift) // 计算机器ID
                | sequence; // 序列号
    }

    /**
     *获得新的时间戳
     * @param lastTimestamp 上次生成ID的时间截
     * @return 当前时间戳
     */
    protected long tilNextMillis(long lastTimestamp) {
        long timestamp = getCurrentTime();
        // 若是当前时间等于上一次的1时间就一直阻塞，知道获取到最新的时间（回拨后的时间）
        while (timestamp <= lastTimestamp) {
            timestamp = getCurrentTime();
        }
        return timestamp;
    }

    /**
     * 获取当前时间
     * @return 当前时间(毫秒)
     */
    protected long getCurrentTime() {
        return System.currentTimeMillis();
    }


```



## 百度UidGenerator算法

## 美团 Leef 算法



参考博客：

[分布式id生成策略，我和面试官扯了一个半小时](https://juejin.im/post/5f050a426fb9a07e5d76c6f3?utm_source=gold_browser_extension)


## 介绍
Redis (Remote Dictionary Server) 远程词典服务器，是一个基于内存的键值型 NoSQL 数据库。

**特征**
- 键值型 (key-value)，value 支持多种不同数据结构，功能丰富
- 单线程，每个命令具备原子性
- 低延迟，速度快 (基于内存、IO多路复用、良好的编码)。
- 支持数据持久化
- 支持主从集群、分片集群
- 支持多语言客户端

```shell

redis-cli [options] [commonds]

options:

-h 127.0.0.1: 指定要连接的 redis 节点的 IP 地址，默认是 127.0.0.1
-p 6379: 指定要连接的 redis 节点的端口，默认是 6379
-a 123321: 指定 redis 的访问密码

commonds 就是 Redis 的操作命令，例如：

ping: 与 redis 服务端做心跳测试，服务端正常会返回 pong

```

## 数据结构
Redis 是一个 key-value 的数据库，key 一般是 String 类型，不过 value 的类型多种多样

表格中的前五个是基本类型，后三个是特殊类型。

|  值类型   |          示例           |
| :-------: | :---------------------: |
|  String   |       hello world       |
|   Hash    | {name: "Jack", age: 21} |
|   List    |   [A -> B -> C -> D]    |
|    Set    |        {A, B, C}        |
| SortedSet |     {A:1, B:2, C:3}     |
|    GEO    |   {A: (120.3, 30.5)}    |
|  BitMap   |   0110110101110101011   |
| HyperLog  |   0110110101110101011   |

## 命令

### 通用命令

通用指令是不分数据类型的，都可以使用的指令，常见的有

KEYS：查看符合模板的所有 key，**不建议在生产环境设备上使用** (可以在从节点使用)

DEL：删除一个指定的 key

EXISTS：判断 key 是否存在

EXPIRE：给一个 key 设置有效期，有效期到期时该 key 会被自动删除 `EXPIRE ke 100`

TTL：查看一个 key 的剩余有效期

PERSIST KEY：取消 key 过期时间

SELECT INDEX：切换数据库，默认是第 0 个,共有[0, 15] 个 `SELECT 0`

MOVE key db：从当前数据库将 key 移动到指定 db 库

RANDOMKEY：随机返回一个 key

RENAME key new key：将 key 改名为 newkey `RENAME name newname`

DBSIZE：查看 key 个数 `DBSIZE`

INFO：查看 redis 数据库信息 `INFO`

CONFIG GET *：查看所有 redis 配置信息 `CONFIG GET *`

FLUSHDB：清空当前数据库 `FLUSHDB`

FLUSHALL：清空所有数据库 `FLUSHALL`

```shell
# 查看命令帮助 (以 KEYS 为例)
help KEYS

# 查看所有的 KEY
KEYS *

# 查看所有 a 开头的 key
KEYS a*

# Redis key 对大小写敏感
KEYS A*

# 删除单个
DEL k1

# 删除多个 (返回值：删除的个数)
DEL k1 k2 k3 k4

EXISTS k4 # 0 (不存在)
EXISTS k1 # 1 (存在)

# 设置有效期 (单位：秒),再次设置值 时间会变成新的值
EXPIRE ke 100

# 查看剩余有效期 (-1：永久有效, -2：已被删除)
TTL ke
```

### String 类型

String 类型，也就是字符串类型，是 Redis 中最简单的存储类型。

##### 分类

存储的 value 是字符串，不过根据字符串的格式不同，又可以分为三类：

- string：普通字符串
- int： 整数类型，可以做自增、自减操作
- float： 浮点类型，可以做自增，自减操作

| KEY   | VALUE       |
| ----- | ----------- |
| msg   | hello world |
| num   | 10          |
| score | 92.5        |

不管是哪种格式，底层都是字节数组形式存储，只不过是编码方式不同。字符串类型的最大空间不能超过 512m。

##### 常见命令

String 类型的常见命令有

- SET：添加或者修改已经存在的一个 String 类型的键值对
- GET：根据 key 获取 String 类型的 value
- MSET：批量添加多个 String 类型的键值对
- MGET：根据多个 key 获取多个 String 类型的 value
- INCR：让一个整型的 key 自增1
- DECR：让一个整型的 key 自减 1
- INCRBY：让一个整型的 key 自增并指定步长，例如：`incrby num 2` 让 num 值自增2  (只能操作整型)
- INCRBYFLOAT：让一个浮点类型的数字自增并指定步长 (也可以操作整型)
- SETNX：添加一个 String 类型的键值对，前提是这个 key 不存在，否则不执行
- SETEX：添加一个 String 类型的键值对，并且指定有效期

```shell
SET k1 v1

GET k1

MSET S1 V1 S2 V2 S3 V3

MGET S1 S2 S3

SET N1 100

INCR N1

INCRBY N1 10

# 做自减 (常用)
INCRBY N1 -2

# 自减 1
DECR N1

SET N2 66.5

INCRBYFLOAT N2 0.5

# 不执行 (等价与 SET k1 V11 NX)
SETNX k1 v11

SETNX k6 v6

# 10：10秒有效期 (等价于 SET k7 v7 EX 10)
SETEX k7 10 v7
```

##### key 格式

Redis 的 key 允许有多个单词形成层级结构，多个单词之间用 ':' 隔开 (会自动形成分级存储)，格式如下

`项目名:业务名:类型:id`

这个格式并非固定，可以根据自己的需求来删除或添加词条。

例如我们的项目名称叫 heima，有 user 和 product 两种不同类型的数据，我们可以这样定义 key

- User 相关的 key: `heima:user:1`
- Product 相关的 key: `heima.product:1`

如果 value 是一个 JS 对象，例如一个 user对象，则可以将对象序列化为 JSON 字符串后存储

| KEY             | VALUE                                      |
| --------------- | ------------------------------------------ |
| heima:user:1    | {"id": 1, "name": "jack", "age": 21}       |
| heima:product:1 | {"id": 1, "name": "小米11", "price": 4999} |

##### 常见应用场景

- 缓存 session
- 使用 INCRBY 统计 视频/文章 点击次数
- 用户登陆信息缓存 (侧重于查询，改非常麻烦)

### Hash 类型

Hash 类型，也叫散列，其 value 是一个无序字典，类似于 java 中的 HashMap 结构。

String 结构是将对象序列化为 JSON 字符串后存储，当需要修改对象某个字段时很不方便。

Hash 结构可以将对象中的每个字段单独存储，可以针对单个字段做 CRUD

<table border="1" style="margin: 0 auto; text-align: center;">
  <tr>
    <th rowspan="2">KEY</th>
    <th colspan="2">VALUE</th>
  </tr>
  <tr>
    <td>field</td>
    <td>value</td>
  </tr>
  <tr>
    <td rowspan="2">heima:user:1</td>
    <td>name</td>
    <td>Jack</td>
  </tr>
  <tr>
    <td>age</td>
    <td>21</td>
  </tr>
  <tr>
    <td rowspan="2">heima:user:2</td>
    <td>name</td>
    <td>Rose</td>
  </tr>
  <tr>
    <td>age</td>
    <td>18</td>
  </tr>
</table>

##### 命令

Hash 的常见命令有

- HSET key field value：添加或者修改 hash 类型 key 的 field 的值
- HGET key field：获取一个 hash 类型 key 的 field 的值
- HMSET：批量添加多个 hash 类型 key 的 field 的值
- HMGET：批量获取多个 hash 类型 key 的 field 的值
- HGETALL：获取一个 hash 类型的 key 中的所有的 field 和 value
- HEXISTS key field: 判断 key 对应的 hash 列表 有没有 field 的值
- HDEL key field：删除 key 对应 hash 列表中的 field
- HLEN key： 查看 key 对应的 hash 列表有多长
- HKEYS：获取一个 hash 类型的 key 中的所有的 field
- HVALS：获取一个 hash 类型的 key 中的所有的 value
- HINCRBY：让一个 hash 类型 key 的字段值自增并指定步长
- HSETNX：添加一个 hash 类型的 key 的 field 值，前提是这个 field 不存在，否则不执行

```shell
HSET heima:user:3 name Lucy

HSET heima:user:3 age 21

HMSET heima:user:4 name HanMeiMei age 25 set '女'

HMGET heima:user:4 name age

HGETALL heima:user:4

# 返回 1 存在，0 不存在
HEXISTS heima:user:4 set

HEXISTS heima:user:4 cat

# 返回 1 删除成功，0 不存在
HDEL heima:user:4 set

# 返回 3
HLEN heima:user:4

HKEYS heima:user:4

HVALS heima:user:4

HINCRBY heima:user:4 age 2

HSETNX heima:user:4 color "blue"
```

##### 常见应用场景

- 用户登陆信息缓存 (侧重于改，查询相对麻烦)

### List 类型

Redis 的 List 类型与 Java 中的 LinkedList 类似，可以看做是一个双向链表结构。既可以支持正向检索也可以支持反向检索。

**特征**

- 有序
- 元素可以重复
- 插入和删除快
- 查询速度一般

常用来存储一个有序数据，例如：朋友圈点赞列表，评论列表等。

##### 命令

List 的常见命令有

- LPUSH key element ...：向列表左侧插入一个或多个元素
- LPOP key：移除并返回列表左侧的第一个元素，没有则返回 nil
- RPUSH key element ...：向列表右侧插入一个或多个元素
- RPOP key：移除并返回列表右侧的第一个元素
- LLEN key：获取列表长度
- LINDEX key index：操作 key 集合，获取 索引为 index 位置的数据
- LRANGE key star end：返回一段角标范围内的所有元素 (角标从 0 开始计数)
- BLPOP 和 BRPOP：与 LPOP 和 RPOP 类似，只不过在没有元素时等待指定时间，而不是直接返回 nil

```shell
# 按顺序推进去，1 最先推进去，3 在最左侧
LPUSH users 1 2 3

# 返回 3
LLEN users

# 返回 "2"
LINDEX users 1

# 按顺序推进去，6 最先推进去，6 在最左侧
RPUSH users 6 7 8

# 返回 3 (会删除掉 3)
LPOP users

# 返回 8
RPOP users

# 返回 7 (可以指定取多少个，当前取 1个)
RPOP users 1

# 返回 "1" "6" (不会删除这个数据)
LRANGE users 1 2


```

**如何利用 List 结构模拟一个栈？**

入口和出口在同一边

LPUSH 存，LPOP 取



**如何利用 List 结构模拟一个队列?**

入口和出口不在同一边

LPUSH 存，RPOP 取



**如何利用 List 结构模拟一个阻塞队列？**

入口和出口在不同边

出队时采用 BLPOP 或 BRPOP

##### 常见应用场景

- 用户收藏文章列表 (key: user_favor_article_list  value: [aid1, aid2, aid3, ...])

### Set 类型

Redis 的 Set 结构与 Java 中的 HashSet 类似，可以看做是一个 value 为 null 的 HashMap。因为也是一个 hash 表，因此具备与 HashSet 类似的特征

- 无序
- 元素不可重复
- 查找快
- 支持交集、并集、差集等功能

##### 命令

Set 的常见命令有

- SADD key member ...：向 set 中添加一个或多个元素
- SREM key member ...：移除 set 中的指定元素
- SCARD key：返回 set 中元素的个数
- SISMEMBER key member：判断一个元素是否存在于 set 中
- SMEMBERS key：获取 set 中的所有元素
- SPOP key count：从 key 集合中随机弹出 count 个元素
- SINTER key1 key2 ...：求 key1 和 key2 的交集
- SDIFF key1 key2 ...：求 key1 和 key2 的差集 (返回的事 key1 独有的)
- SUNION key1 key2 ...：求 key1 和 key2 的并集

```shell
# 添加
SADD s1 a b c

# 移除
SREM s1 b

# 是否存在 a 返回 1
SISMEMBER s1 a

# 是否存在 b 返回 0 不存在
SISMEMBER s1 b

# 返回 2
SCARD s1

# 返回 "a" "c"
SMEMBERS s1

# 返回值是随机的有可能是 "a"
SPOP s1 1

SADD s2 a d e f

# 返回 "a"
SINTER s1 s2

# 返回 "c"
SDIFF s1 s2

# 返回 "c" "d" "a" "e" "f" (重复元素只会记录一次)
SUNION s1 s2
```

Set 命令的练习

```shell
将下列数据用 Redis 的 Set 集合来存储
- 张三的好友有：李四、王五、赵六
SADD user1 "李四" "王五" "赵六"

- 李四的好友有：王五、麻子、二狗
SADD user2 "王五" "麻子" "二狗"

利用 Set 的命令实现下列功能
- 计算张三的好友有几人
SCARD user1

- 计算张三和李四有哪些共同好友
SINTER user1 user2

- 查询那些人是张三的好友却不是李四的好友
SDIFF user1 user2

- 查询张三和李四的好友总共有那些人
SUNION user1 user2

- 判断李四是否是张三的好友
SISMEMBER user1 "李四"

- 判断张三是否是李四的好友
SISMEMBER user2 "张三"

- 将李四从张三的好友列表中移除
SREM user1 "李四"

```

##### 常见应用场景

- 去重
- 抽奖 (真 随机抽奖)
  1. 准备一个抽奖池：SADD luckydraw 1 2 3 4 5 6 7 8 9 10 11 12 13
  2. 抽3个 三等奖：SPOP luckydraw 3
  3. 抽2个 二等奖：SPOP luckydraw 2
  4. 抽1个 一等奖：SPOP luckydraw 1

### SortedSet 类型

Redis 的 SortedSet 是一个可排序的 set 集合，也称为 Zset 类型，与 JAVA 中的 TreeSet 有些类似，但底层数据结构却差别很大。SortedSet 中的每一个元素都带有一个 score 属性，可以基于 score 属性对元素排序，底层的实现是一个跳表 (SkipList) 加 hash 表。要求每一个元素都会关联一个 double (score 属性 ) 类型的分数。

添加、删除、查找的复杂度都是 O(1)，集合中最大的成员数是 2 的 32 - 1 次方 (每个集合可以存储 40多亿个成员)

SortedSet 具备一下特性

- 可排序
- 元素不重复
- 查询速度快

因为 SortedSet 的可排序特性，经常被用来实现排行榜这样的功能。

##### 命令

SortedSet 的常见命令有

- ZADD key score member：添加一个或多个元素到 sorted set，如果已经存在则更新其 score 值
- ZREM key member：删除 sorted set 中的一个指定元素
- ZSCORE key member：获取 sorted set  中的指定元素的 score 值
- ZRANK key member：获取 sorted set 中的指定元素的排名
- ZCARD key：获取 sorted set 中的元素个数
- ZCOUNT key min max：统计 score 值在给定范围内的所有元素的个数
- ZINCRBY key increment member：让 sorted set 中的指定元素自增，步长为指定的 increment 值
- ZRANGE key min max：按照 score 排序后，获取指定排名范围内的元素。min 角标从 0 开始
- ZRANGE key start stop：将 key 集合中的元素按分数升序排列返回
- ZRANGEBYSCORE key min max：按照 score 排序后，获取指定 score 范围内的元素
- ZDIFF, ZINTER, ZUNION：求差集、交集、并集。

**注意**： 所有的排名默认都是升序，如果要降序则在命令的 Z 后面添加 REV 即可

SortedSet 命令练习

```shell
将班级的下列学生得分存入 Redis 的 SortedSet 中, 并实现下列功能

Jack 85, Lucy 89, Rose 82, Tom 95, Jerry 78, Amy 92, Miles 76

$ ZADD stus 85 Jack 89 Lucy 82 Rose 95 Tom 78 Jerry 92 Amy 76 Miles

- 删除 Tom 同学
$ ZREM stus Tom

- 获取 Amy 同学的分数
$ ZSCORE stus Amy

- 获取 Rose 同学的排名
$ ZRANK stus Rose # 返回 2 默认是从 0 开始排
$ ZREVRANK stus Rose # 返回 3 使用 REV 降序排

- 查询 80 分以下有几个学生
$ ZCOUNT stus 0 80 # 返回 2

- 给 Amy 同学加2分
$ ZINCRBY stus 2 Amy

- 查出成绩前3名 的同学
$ ZREVRANGE stus 0 2

- 查出成绩倒数后 3名
$ ZRANGE stus 0 2

# 全部排序 （升序排列）
$ ZRANGE stus 0 -1

# 全部排序并显示分数
ZRANGE stus 0 -1 withscores

- 查出成绩 80分以下的所有同学
$ ZRANGEBYSCORE stus 0 80 # "Miles" "Jerry"
```

##### 常见应用场景

- 排行榜：有序集合经典使用场景。例如视频网站需要对用户上传的视频做排行榜，榜单维护可能是多方面：按照时间、按照播放量、按照获得的赞数等。

### Key 设计

Key 设计时，讲究 4个特性

##### 唯一性

常用的方式：使用**缓存数据的主键**作为 key

##### 可读性

可读性是保证Redis 的 key 能做到见名知意

- 普通单值 `employe_info:id1` 员工对象信息
- 表名:主键名:主键值:列名 `employe:id:1:info` 员工对象信息
- 业务模块名:业务逻辑含义:其他:value类型 `employe:base.info:id1:hash` 员工对象信息

##### 灵活性

可以根据业务实际情况使用单一主键 或者联合主键

`post1:reply:A` 评论内容

`post:1:reply:B` 评论内容

##### 时效性

Redis key 一定要设置过期时间。需要根据业务场景，对 key 设置合理的过期时间。在写入 key 时，就要写入过期时间，后续可以按照需要动态设置。

**注意**

- 不设置过期时间，这种 key 为永久 key，会一直占用内存不释放，时间久了，数量一多 就容易达到服务器的内存上线，导致宕机。
- Key 的时效性设置，必须根据业务场景进行评估 合理设置。

### 事务

单个 Redis 命令的执行是原子性的，但 Redis 没有在事务上增加任何维持原子性的机制，所以 Redis 事务的执行并不是原子性的。

Redis 事务可以理解为一个**打包的批量执行脚本**，但批量指令并非原子化的操作，中间某条指令的失败不会导致前面已经做完的指令回滚，也不会造成后续的指令不做。

Redis 事务可以一次执行多个命令，并且带有以下三个重要的保证

- 批量操作在发送 EXEC 命令前被放入队列缓存。
- 收到 EXEC 命令后进入事务执行，事务中任意命令执行失败，其余的命令依然被执行。
- 在事务执行过程，其他客户端提交的命令请求不会插入到事务执行命令序列中。

















































































---

END


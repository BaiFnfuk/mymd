## Redis

### 1. redis的多数据库

redis默认支持16个数据库，对外都是以一个从0开始的递增数字命名，可以通过参数databases来修改默认数据库个数。客户端连接redis服务后会自动选择0号数据库，

可以通过SELECT命令更换数据库。

1. 选择1号数据库

   ```shell
   SELECT 1
   ```

说明：

- Redis不支持自定义数据库名称
- Redis不支持为每个数据库设置访问密码
- Redis的多个数据库之间不是完全隔离的，FLUSHALL命令会清空所有数据库的数据。

2. 清除数据库的数据

   ```shell
   FLUSHALL	#清空所有数据库的所有数据
   FLUSHDB		#清空当前所在数据库的数据
   ```

### 2. Redis的基本语法

#### 2.1 KEYS

获取符合规则的键名列表

```shell
#查询所有键，“*”匹配任意个（包括0）字符
127.0.0.1:6379[1]> KEYS *
1) "name"
#查询包含na_e的键，“？”匹配一个字符
127.0.0.1:6379[1]> KEYS na?e
1) "name"
```

#### 2.2 EXISTS

判断一个键是否存在，如果键存在返回整数类型1，否则返回0

````shell
127.0.0.1:6379[1]> EXISTS name
(integer) 1
127.0.0.1:6379[1]> EXISTS age
(integer) 0
````

#### 2.3 DEL

删除键，可以删除一个或多个键，返回值是删除的键的个数

```shell
127.0.0.1:6379[1]> DEL name
(integer) 1
```

#### 2.4 TYPE

获得键值的数据类型。

返回值可能是string（字符串），hash（散列类型），list（列表类型），set（集合类型），zset（有序集合类型）

```shell
127.0.0.1:6379[1]> TYPE name
string
```

#### 2.5 HELP

HELP 空格 tab 键

### 3. Redis的字符串数据类型(String)

#### 3.1 字符串类型

字符串类型是Redis中最基本的数据类型，它能存储任何形式的字符串，包括二进制数据。可以存储JSON化的对象，字符数组等。一个字符串类型键允许存储的数据最大容量是512MB

#### 3.2 GET, SET

```shell
# 赋值与取值
127.0.0.1:6379[1]> SET name bai
OK
127.0.0.1:6379[1]> get name
"bai"
# 当值不存在时返回空结果
```

#### 3.3 INCR

递增数字

INCR key

当存储的字符串是整数时，Redis提供了一个实用的命令INCR,其作用是让当前键值递增，并返回递增后的值。

```shell
127.0.0.1:6379[1]> INCR age
(integer) 21
127.0.0.1:6379[1]> INCR age
(integer) 22
127.0.0.1:6379[1]> INCR age
(integer) 23
```

#### 3.4 INCRBY

增加指定的整数

INCRBY key increment

```shell
127.0.0.1:6379[1]> INCRBY age 3
(integer) 26
127.0.0.1:6379[1]> INCRBY age 3
(integer) 29
```

#### 3.5 DECR, DECRBY

减少指定的整数

DECR key

DECYBY key decrement

```shell
127.0.0.1:6379[1]> DECR age
(integer) 28
127.0.0.1:6379[1]> DECR age
(integer) 27
127.0.0.1:6379[1]> DECRBY age 3
(integer) 24
127.0.0.1:6379[1]> DECRBY age 3
(integer) 21
```

#### 3.6 APPEND

向尾部追加值

APPEND key value

APPEND的作用是向键值的未尾追加value。如果键不存在则将该键的值设置为value，相当于SET key value。如果值是追加后字符串的总长度。

```shell
127.0.0.1:6379[1]> set str hello
OK
127.0.0.1:6379[1]> APPEND str world!
(integer) 11
```

#### 3.7 STRLEN

获取字符串长度

STRLEN key

STRLEN命令返回键值的长度，如果不存在则返回0。

```shell
127.0.0.1:6379[1]> STRLEN name
(integer) 3
```

#### 3.8 MSET, MGET

同时设置/获取多个键值

MSET key value key value ...

MGET key key key...

```shell
127.0.0.1:6379[1]> MSET test1 1 test2 2 test3 3
OK
127.0.0.1:6379[1]> MGET test1 test2 test3
1) "1"
2) "2"
3) "3"
```

#### 3.9 字符串的基本命令

| 编号 |              命令              |                  描述说明                  |
| :--: | :----------------------------: | :----------------------------------------: |
|  1   |         SET key value          |               设置指定键的值               |
|  2   |            GET key             |               获取指定键的值               |
|  3   |     GETRANGE key start end     |      获取存储在键上的字符串的子字符串      |
|  4   |        GETSET key value        |        设置键的字符串值并返回其旧值        |
|  5   |       GETBIT key offset        |   返回在键处存储的字符串值中偏移处的位值   |
|  6   |       MGET key1 [key2…]        |             获取所有给定键的值             |
|  7   |    SETBIT key offset value     | 存储在键上的字符串值中设置或清除偏移处的位 |
|  8   |    SETEX key seconds value     |          使用键和到期时间来设置值          |
|  9   |        SETNX key value         |         设置键的值，仅当键不存在时         |
|  10  |   SETRANGE key offset value    |  在指定偏移处开始的键处覆盖字符串的一部分  |
|  11  |           STRLEN key           |          获取存储在键中的值的长度          |
|  12  |  MSET key value [key value …]  |          为多个键分别设置它们的值          |
|  13  | MSETNX key value [key value …] |  为多个键分别设置它们的值，仅当键不存在时  |
|  14  | PSETEX key milliseconds value  |     设置键的值和到期时间(以毫秒为单位)     |
|  15  |            INCR key            |             将键的整数值增加1              |
|  16  |      INCRBY key increment      |        将键的整数值按给定的数值增加        |
|  17  |   INCRBYFLOAT key increment    |        将键的浮点值按给定的数值增加        |
|  18  |            DECR key            |              将键的整数值减1               |
|  19  |      DECRBY key decrement      |          按给定数值减少键的整数值          |
|  20  |        APPEND key value        |              将指定值附加到键              |

### 4. 列表类型（List）

Redis列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）
一个列表最多可以包含 232 - 1 个元素 (4294967295, 每个列表超过40亿个元素)。

#### 4.1 LINDEX key index

通过索引获取列表中的元素

```shell
127.0.0.1:6379[1]> LINDEX runoobkey 1
"mongodb"
127.0.0.1:6379[1]> LINDEX runoobkey 0
"mysql"
127.0.0.1:6379[1]> LINDEX runoobkey 2
"redis"
```

#### 4.2 LPOP key

移出并获取列表的第一个元素

```shell
127.0.0.1:6379[1]> LPOP runoobkey
"mysql"
127.0.0.1:6379[1]> LRANGE runoobkey 0 10
1) "mongodb"
2) "redis"
```

#### 4.3 LPUSH key value1 [value2]

将一个或多个值插入到列表头部

```shell
127.0.0.1:6379[1]> LPUSH runoobkey redis
(integer) 1
127.0.0.1:6379[1]> LPUSH runoobkey mongodb
(integer) 2
127.0.0.1:6379[1]> LPUSH runoobkey mysql
(integer) 3
127.0.0.1:6379[1]> LRANGE runoobkey 0 10
1) "mysql"
2) "mongodb"
3) "redis"
```

#### 4.4 LRANGE key start stop

获取列表指定范围内的元素

```shell
127.0.0.1:6379[1]> LRANGE runoobkey 0 10
1) "mysql"
2) "mongodb"
3) "redis"
```

#### 4.5  LSET key index value

通过索引设置列表元素的值

```shell
127.0.0.1:6379[1]> LSET runoobkey 2 sql
OK
127.0.0.1:6379[1]> LRANGE runoobkey 0 10
1) "mysql"
2) "mongodb"
3) "sql"
```

#### 4.6 RPOP key

移除并获取列表最后一个元素

```shell
127.0.0.1:6379[1]> RPOP runoobkey
"sql"
127.0.0.1:6379[1]> LRANGE runoobkey 0 10
1) "mysql"
2) "mongodb"
```

#### 4.7 RPUSH key value1 [value2]

```shell
127.0.0.1:6379[1]> RPUSH runoobkey redis
(integer) 3
127.0.0.1:6379[1]> LRANGE runoobkey 0 10
1) "mysql"
2) "mongodb"
3) "redis"
```

### 5. 集合(Set)

Redis的Set是string类型的无序集合。集合成员是唯一的，这就意味着集合中不能出现重复的数据。
Redis 中 集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)。
集合中最大的成员数为 232 - 1 (4294967295, 每个集合可存储40多亿个成员)。

#### 5.1 SADD key member1 [member2]

向集合添加一个或多个成员

```shell
127.0.0.1:6379[1]> SADD dbs redis mangodb mysql
(integer) 3
127.0.0.1:6379[1]> SRANDMEMBER dbs 3
1) "redis"
2) "mangodb"
3) "mysql"
```

#### 5.2 SCARD key

获取集合的成员数

```shell
127.0.0.1:6379[1]> SCARD dbs
(integer) 3
```

#### 5.3 SISMEMBER key member

判断 member 元素是否是集合 key 的成员

```shell
127.0.0.1:6379[1]> SISMEMBER dbs redis
(integer) 1
127.0.0.1:6379[1]> SISMEMBER dbs sql
(integer) 0
```

#### 5.4 SMEMBERS key

返回集合中的所有成员

```shell
127.0.0.1:6379[1]> SMEMBERS dbs
1) "mangodb"
2) "redis"
3) "mysql"
```

#### 5.5 SPOP key

移除并返回集合中的一个随机元素

```shell
127.0.0.1:6379[1]> SPOP dbs
"mangodb"
```

#### 5.6 SRANDMEMBER key [count]

返回集合中一个或多个随机数

```shell
127.0.0.1:6379[1]> SRANDMEMBER dbs
"mysql"
```

####  5.7 SREM key member1 [member2]

移除集合中一个或多个成员

```shell
127.0.0.1:6379[1]> SREM dbs redis
(integer) 1
```

### 6. 有序集合（sorted set）

Redis 有序集合和集合一样也是string类型元素的集合,且不允许重复的成员。
不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。
有序集合的成员是唯一的,但分数(score)却可以重复。
集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)。 集合中最大的成员数为 232 - 1 (4294967295, 每个集合可存储40多亿个成员)。

#### 6.1 ZADD key score1 member1 [score2 member2]

向有序集合添加一个或多个成员，或者更新已存在成员的分数

```shell
127.0.0.1:6379[1]> ZADD zset 1 a 2 b 3 c 4 d 5 e
(integer) 5
127.0.0.1:6379[1]> ZRANGE zset 0 10
1) "a"
2) "b"
3) "c"
4) "d"
5) "e
```

#### 6.2 ZCARD key

获取有序集合的成员数

```powershell
127.0.0.1:6379[1]> ZCARD zset
(integer) 5
```

#### 6.3 ZINCRBY key increment member

有序集合中对指定成员的分数加上增量 increment

```shell
127.0.0.1:6379[1]> ZINCRBY zset 2 a
"3"
127.0.0.1:6379[1]> ZRANGE zset 0 10
1) "b"
2) "a"
3) "c"
4) "d"
5) "e"
```

#### 6.4 ZRANGE key start stop [WITHSCORES]

```shell
127.0.0.1:6379[1]> ZRANGE zset 0 10
1) "b"
2) "a"
3) "c"
4) "d"
5) "e"
```

#### 6.5 ZRANK key member

```shell
127.0.0.1:6379[1]> ZRANK zset a
(integer) 1
127.0.0.1:6379[1]> ZRANK zset c
(integer) 2
127.0.0.1:6379[1]> ZRANK zset b
(integer) 0
```

#### 6.6 ZREM key member [member …]

```shell
127.0.0.1:6379[1]> ZREM zset d
(integer) 1
127.0.0.1:6379[1]> ZRANGE zset 0 10
1) "b"
2) "a"
3) "c"
4) "e"
```

#### 7. 哈希（Hash）

Redis hash 是一个string类型的field和value的映射表，hash特别适合用于存储对象。
Redis 中每个 hash 可以存储 232 - 1 键值对（40多亿）。

Map<String,Map<String,String>>

#### 7.1 HSET key field value

```shell
127.0.0.1:6379[1]> HSET user name bai age 20 password 123456
(integer) 3
```

#### 7.2 HGET key feld

```shell
127.0.0.1:6379[1]> HGET user name
"bai"
```

#### 7.3 HMSET key field value field value...

```shell
127.0.0.1:6379[1]> HMSET user name chenzp age 27 passwrod 123456
OK
```

#### 7.4 HMGET key field field...

```shell
127.0.0.1:6379[1]> HMGET user name age password
1) "chenzp"
2) "27"
3) "123456"
```

#### 7.5 HGETALL key

```shell
127.0.0.1:6379[1]> HGETALL user
1) "name"
2) "chenzp"
3) "age"
4) "27"
5) "password"
6) "123456"
```

#### 7.6 HEXISTS key field

```shell
127.0.0.1:6379[1]> HEXISTS user name
(integer) 1
```

#### 7.7 HKEYS key

```shell
127.0.0.1:6379[1]> HKEYS user
1) "name"
2) "age"
3) "password"
4) "passwrod"
```

#### 7.8 HVALS key

```shell
127.0.0.1:6379[1]> HVALS user
1) "chenzp"
2) "27"
3) "123456"
4) "123456"
```

#### 7.9 HDEL key field

```shell
127.0.0.1:6379[1]> HDEL user passwrod
(integer) 1
```



### 8. 过期时间

#### 8.1 设置过期时间

```shell
EXPIRE key seconds

127.0.0.1:6379[1]> EXPIRE name 120
(integer) 1
127.0.0.1:6379[1]> TTL name
(integer) 87
```

`TTL返回值：
大于0的数字：剩余生存时间，单位为秒
-1 ： 没有生存时间，永久存储
-2 ： 数据已经被删除`

#### 8.2 清除过期时间

```shell
PERSIST key

127.0.0.1:6379[1]> PERSIST name
(integer) 1
127.0.0.1:6379[1]> TTL name
(integer) -1
```


## redis

### 概述：

1. redis 是一款高性能的 NOSQL 系列的非关系型数据库
2. Redis 是用 C 语言开发的一个开源的高性能键值对(key-value)数据库

### Redis 支持的键值数据类型如下:

- 字符串类型 string
- 哈希类型 hash
- 列表类型 list
- 集合类型 set
- 有序集合类型 sortedset

### redis的应用场景：

- 缓存(数据查询、短连接、新闻内容、商品内容等等)
- 聊天室的在线好友列表
- 任务队列。(秒杀、抢购、12306 等等)
- 应用排行榜
- 网站访问统计
- 数据过期处理(可以精确到毫秒)
- 分布式集群架构中的 session 分离

### Mac系统下安装redis：

- 网址:https://redis.io/download，下载 stable 版本，稳定版本。
- 将解压后文件夹放到/usr/local
- 切换到redis目录
- 进入 src 目录下面编译 redis：sudo make。编译成功后会有一个提示: It's a good idea to run 'make test'
- 编译 test：sudo make test。成功后提示: All tests passed without errors
- 安装：sudo make install。成功后提示: It's a good idea to run 'make test'!
- 重开一个终端窗口运行redis-server命令

### redis的命令操作：

1. redis的数据结构：redis 存储的是:key,value 格式的数据，其中 key 都是字符串，value有5种不同的数据结构

   - 字符串类型 string：

     - 存储: set key value
        127.0.0.1:6379> set username zhangsan

     - 获取: get key 

       127.0.0.1:6379> get username

     - 删除: del key

       127.0.0.1:6379> del username

   - 哈希类型 hash : map 格式

     - 存储: hset key field value
        127.0.0.1:6379> hset myhash username lisi

     - 获取: hget key field: 获取指定的 field 对应的值

       127.0.0.1:6379> hget myhash username

       hgetall key:获取所有的 field 和 value

       127.0.0.1:6379> hgetall myhash

     - 删除: hdel key field

       127.0.0.1:6379> hdel myhash username

   - 列表类型 list : linkedlist 格式。支持重复元素

     - 添加:

       ➢  lpush key value: 将元素加入列表左表

       ➢  rpush key value:将元素加入列表右边

     - 获取：range key start end :范围获取

       127.0.0.1:6379> lrange myList 0 -1（全部获取）

     - 删除:

       ➢  lpop key: 删除列表最左边的元素，并将元素返回

       ➢  rpop key: 删除列表最右边的元素，并将元素返回

   - 集合类型 set : 不允许重复元素

     - 存储:sadd key value

     - 获取:smembers key

       获取 set 集合中所有元素

     - 删除:srem key value

       删除 set 集合中的某个元素

   - 有序集合类型 sortedset:不允许重复元素，且元素有顺序

     - 存储:zadd key score value

       127.0.0.1:6379> zadd mysort 60 zhangsan

       127.0.0.1:6379> zadd mysort 80 wangwu

       127.0.0.1:6379> zadd mysort 50 lisi

     - 获取:zrange key start end [withscores]

       127.0.0.1:6379> zrange mysort 0 -1
        1) "lisi"
        2) "zhangsan"
        3) "wangwu"

       <!--withscores-->

       127.0.0.1:6379> zrange mysort 0 -1 withscores

        1) "zhangsan"
        2) "60"
        3) "wangwu"

        4) "80"
        5) "lisi"
        6) "500"

     - 删除:zrem key value

       127.0.0.1:6379> zrem mysort lisi

   ### 持久化

   - redis 是一个内存数据库，当 redis 服务器重启，获取电脑重启，数据会丢失，我们可 以将 redis 内存中的数据持久化保存到硬盘的文件中。

   - redis的持久化机制：

     - RDB

       默认方式，不需要进行配置，默认就使用这种机制
       在一定的间隔时间中，检测 key 的变化情况，然后持久化数据

     - AOF 

       日志记录的方式，可以记录每一条命令的操作。可以每一次命令操作后，持久化数据

   
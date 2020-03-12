
——————————————————————————————————————
redis.conf 配置项说明如下：
1. Redis默认不是以守护进程的方式运行，可以通过该配置项修改，使用yes启用守护进程
    daemonize no
2. 当Redis以守护进程方式运行时，Redis默认会把pid写入/var/run/redis.pid文件，可以通过pidfile指定
    pidfile /var/run/redis.pid
3. 指定Redis监听端口，默认端口为6379，作者在自己的一篇博文中解释了为什么选用6379作为默认端口，因为6379在手机按键上MERZ对应的号码，而MERZ取自意大利歌女Alessia Merz的名字
    port 6379
4. 绑定的主机地址
    bind 127.0.0.1
5.当 客户端闲置多长时间后关闭连接，如果指定为0，表示关闭该功能
    timeout 300
6. 指定日志记录级别，Redis总共支持四个级别：debug、verbose、notice、warning，默认为verbose
    loglevel verbose
7. 日志记录方式，默认为标准输出，如果配置Redis为守护进程方式运行，而这里又配置为日志记录方式为标准输出，则日志将会发送给/dev/null
    logfile stdout
8. 设置数据库的数量，默认数据库为0，可以使用SELECT <dbid>命令在连接上指定数据库id
    databases 16
9. 指定在多长时间内，有多少次更新操作，就将数据同步到数据文件，可以多个条件配合
    save <seconds> <changes>
    Redis默认配置文件中提供了三个条件：
    save 900 1
    save 300 10
    save 60 10000
    分别表示900秒（15分钟）内有1个更改，300秒（5分钟）内有10个更改以及60秒内有10000个更改。
 
10. 指定存储至本地数据库时是否压缩数据，默认为yes，Redis采用LZF压缩，如果为了节省CPU时间，可以关闭该选项，但会导致数据库文件变的巨大
    rdbcompression yes
11. 指定本地数据库文件名，默认值为dump.rdb
    dbfilename dump.rdb
12. 指定本地数据库存放目录
    dir ./
13. 设置当本机为slav服务时，设置master服务的IP地址及端口，在Redis启动时，它会自动从master进行数据同步
    slaveof <masterip> <masterport>
14. 当master服务设置了密码保护时，slav服务连接master的密码
    masterauth <master-password>
15. 设置Redis连接密码，如果配置了连接密码，客户端在连接Redis时需要通过AUTH <password>命令提供密码，默认关闭
    requirepass foobared
16. 设置同一时间最大客户端连接数，默认无限制，Redis可以同时打开的客户端连接数为Redis进程可以打开的最大文件描述符数，如果设置 maxclients 0，表示不作限制。当客户端连接数到达限制时，Redis会关闭新的连接并向客户端返回max number of clients reached错误信息
    maxclients 128
17. 指定Redis最大内存限制，Redis在启动时会把数据加载到内存中，达到最大内存后，Redis会先尝试清除已到期或即将到期的Key，当此方法处理 后，仍然到达最大内存设置，将无法再进行写入操作，但仍然可以进行读取操作。Redis新的vm机制，会把Key存放内存，Value会存放在swap区
    maxmemory <bytes>
18. 指定是否在每次更新操作后进行日志记录，Redis在默认情况下是异步的把数据写入磁盘，如果不开启，可能会在断电时导致一段时间内的数据丢失。因为 redis本身同步数据文件是按上面save条件来同步的，所以有的数据会在一段时间内只存在于内存中。默认为no
    appendonly no
19. 指定更新日志文件名，默认为appendonly.aof
     appendfilename appendonly.aof
20. 指定更新日志条件，共有3个可选值： 
    no：表示等操作系统进行数据缓存同步到磁盘（快） 
    always：表示每次更新操作后手动调用fsync()将数据写到磁盘（慢，安全） 
    everysec：表示每秒同步一次（折衷，默认值）
    appendfsync everysec
 
21. 指定是否启用虚拟内存机制，默认值为no，简单的介绍一下，VM机制将数据分页存放，由Redis将访问量较少的页即冷数据swap到磁盘上，访问多的页面由磁盘自动换出到内存中（在后面的文章我会仔细分析Redis的VM机制）
     vm-enabled no
22. 虚拟内存文件路径，默认值为/tmp/redis.swap，不可多个Redis实例共享
     vm-swap-file /tmp/redis.swap
23. 将所有大于vm-max-memory的数据存入虚拟内存,无论vm-max-memory设置多小,所有索引数据都是内存存储的(Redis的索引数据 就是keys),也就是说,当vm-max-memory设置为0的时候,其实是所有value都存在于磁盘。默认值为0
     vm-max-memory 0
24. Redis swap文件分成了很多的page，一个对象可以保存在多个page上面，但一个page上不能被多个对象共享，vm-page-size是要根据存储的 数据大小来设定的，作者建议如果存储很多小对象，page大小最好设置为32或者64bytes；如果存储很大大对象，则可以使用更大的page，如果不 确定，就使用默认值
     vm-page-size 32
25. 设置swap文件中的page数量，由于页表（一种表示页面空闲或使用的bitmap）是在放在内存中的，，在磁盘上每8个pages将消耗1byte的内存。
     vm-pages 134217728
26. 设置访问swap文件的线程数,最好不要超过机器的核数,如果设置为0,那么所有对swap文件的操作都是串行的，可能会造成比较长时间的延迟。默认值为4
     vm-max-threads 4
27. 设置在向客户端应答时，是否把较小的包合并为一个包发送，默认为开启
    glueoutputbuf yes
28. 指定在超过一定的数量或者最大的元素超过某一临界值时，采用一种特殊的哈希算法
    hash-max-zipmap-entries 64
    hash-max-zipmap-value 512
29. 指定是否激活重置哈希，默认为开启（后面在介绍Redis的哈希算法时具体介绍）
    activerehashing yes
30. 指定包含其它的配置文件，可以在同一主机上多个Redis实例之间使用同一份配置文件，而同时各个实例又拥有自己的特定配置文件
    include /path/to/local.conf
————————————————————————————————————————

# Redis 数据类型
Redis 支持五种数据类型：String（字符串），hash（哈希），list（列表），set（集合），zset（sorted set：有序集合）。

## String（字符串）  
string 是 redis 最基本的类型，可以理解成语 Memcacaed 一样的类型，一个Key 对应一个 value 。
string 类型是二进制安全的，意思是 redis 的 string 可以包含任何数据，比如 jpg 图片或者序列化的对象。
string类型是 redis 最基本的数据类型， 一个最大能存储 `512MB`.

例子：
```shell
set name "redis" #设置key value  注意：一个最大能存储512MB
get name #根据 key 获取 value
```  

## hash（哈希）
Redis hash 是一个键名对集合。
Redis hash 是一个 String 类型的 field 和 value 的映射表，hash 特别适合存储对象。

例子：
```shell
hmset user:1 name "zhangsan" sex "男" age "15" #key 为 user:1 的对象
hget user:1 name # 获取 user:1 的name
hgetall user:1  # 获取user:1 整个对象
```   

## List（列表）
Redis 列表是简单的字符串列表，按照插入顺序排序，你可以添加一个元素到列表的头部（左边）或者尾部（右边）

例子：
```shell 
lpush runoob redis #添加第一个
lpush runoob java #添加第二个
lpush runoob vue #添加第三个
leange ruboob 0 10 #获取 0 到 10 个
```

## Set（集合）`sadd key member`
Redis 的 Set 是 String 类型的无序集合。
集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O（1）
`sadd` 命令
添加一个 String 元素到 key 对应的set 集合中， 成功返回1，如果元素已经在集合中，返回0，Key对应的 Set 不存在返回错误

例子：
```shell
sadd arr redis #添加第一个 返回1
sadd arr java  #添加第二个 返回1
sadd arr javascript #添加第三个 返回1
sadd arr java #重复添加 返回0

smembers arr #获取集合所有
```

## zset(Sorted Set:有序集合)
Redis zset 和 set 一样也是 String 类型元素的集合，且不允许重复的成员。
不同是的每个元素都会关联一个 Double 类型的分数，redis 正是通过分数来为集合中的成员进行从小到大的排序。
zset 的成员是唯一的，但分数（score）却可以重复。

例子：
```shell
sadd arr 0 redis #添加第一个 0 是索引可以重复 返回1
sadd arr 0 java  #添加第二个 0 是索引可以重复 返回1
sadd arr 0 javascript #添加第三个 0 是索引可以重复 返回1
sadd arr 0 java #重复添加 0 是索引可以重复 返回0 

zrangebyscore arr 0 1000
zrange arr 0 1000
```

______________________________________________
# Redis 教程
Redis 命令用于 redis 服务商执行操作。
要在 Redis 服务商执行命令需要一个redis 客户端，Redis 客户端在我们安装的 Redis 的安装包中。  
##Redis 客户端的基本语法为：
```shell
redis-server #启动redis服务 
redis-cli  #链接本地 redis 服务
redis-cli -h host -p port -a password #链接远程 redis 服务
ping    #检测 redis 是否启动  返回 pong 已启动
```  

# Redis 对 redis 的操作命令
```shell
dbsize  # 查询当前库有多少键值
flushall # 清除当前库  ***需谨慎***
```

# Redis 键（Key）
Redis 键命令用于管理 Redis 的键
  
```shell
set servce:name "anan"
set name "anan"  # 设置键值

del name # 删除键值

get server:name 
get name # 获取键值

exists name # 查看键是否存在

expire name seconds #为给定的键设置过期时间

```

# Redis HyperLogLog
Redis 在2.8.9 版本中添加了 HyperLogLog 结构。
Redis HyperLogLog 是用来做技术统计的算法，其有点是:在输入元素的数量或体积非常非常大时，计算技术所需要的空间总是固定的，并且很小的。

在 Redis 里面 每个 HyperLogLog 键值话费 12KB 内存，就可以计算解禁 2^64 个不同元素的基数，这和计算技术时，元素越多消耗内存就越多的集合形成鲜明对比。

但是，因为 HyperLogLog 只会根据输入元素来计算技术，而不会存储输入元素本身，是有意 HyperLogLog 不能像集合那样，返回输入的各个元素

HaperLogLog 命令
```shell
pfadd arr java # 添加制定元素到 HyperLogLog
pfcount key # 返回非定 HyperLogLog 的技术估算值。
pfmerge distkey sourcekey ... # 将多个HyperLogLog 合并为一个 HyperLogLog
```





<!-- 
## 启动redis 服务  
```shell
redis-server
```  
## 查看redis是否启动   
```shell 
redis-cli
```  


##  `redis config` 命令  
#### 获取配置 
```shell
config get *  #查看所有配置
config get [config name] #指定查看配置信息   
```   
#### 修改配置
```shell  
config set [config name] [new config name]
``` -->

















# Redis Command

## 启动 Redis
```
# 启动 redis 服务
redis-server

# 打开一个 Redis 链接
redis-cli

# 给定主机和端口启动
redis-cli -h 127.0.0.1 -p 6379

```


## String
``` 
# 设置键值
set key value

#获取指定键的值
get key

# 批量设置键值
mset key value [key1 vlaue1...]

# 批量设置键值（仅当他们不存在时候）
msetnx key value [key1 value1...]

# 批量获取指定键的值
mget key [key1...]

# 给指定的键设置新值，并返回旧值
getset key value

# 获取键上的子串
getrange key start end 

# 设置键值得到期时间(毫秒)
psetex key timeout value
```  

## 哈希
```
# 判断是否存在的哈希
hexists key value

# 设置哈希
hset key field value

# 当字段不存在时才设置字段的值
hsetnx key field value

# 多字段设置哈希
hmset value field value [field1 value1...]

# 删除哈希字段
hdel key value

# 查看哈希(返回所有field和value)
hgetall key

# 查看单个key的value
hget key field

# 查看多有field(返回所有字段)
hkeys key

# 获取所有value(返回所有值)
hvals key

# 返回key的field的个数
hlen key

# 返回指定字段的值
hmget key field [field1...]

```

## 列表
***有序，不唯一，左右两边都可以插入和弹出 长度达40亿***  

```
# 左边插入
lpush key value [value1...]

# 右边插入
rpush key value [value1...]

# 中间插入(指定之前还是只有，指定一个value的位置)
linsert key before|after value newValue

# 左边弹出(弹出最左边的值)
lpop key 

# 右边弹出(弹出最右边的值)
rpop key

# 遍历删除(count>0从左往右遍历，count<0从右往左遍历，count=0删除所有，count要删除的个数，value要删除的值)
lrem key count value

# 剪切列表
ltrim key start end

# 修改
lset key index newValue

# 查看
lrange key start end

# 获取指定位置的value
lindex key index

# 获取列表长度
llen key
```

## 集合
***无序，唯一，长达40亿 ***
#### 内操作
```
# 添加(如果结果存在则添加失败)
sadd key element 

# 删除(将集合中的element溢出)
srem key element 

# 判断 element 是否存在
sismember key element

# 从集合中随机挑选出 N 个元素
srandmember key n
```  

#### 集合间操作
```
# 差集
sdiff key1 key2

# 并集
sinter key1 key2

# 交集
sunion key1 key2
```

# 有序集合
***有序 不唯一***  

```
## 添加
zadd key score element

# 增加分数
zincrby key incresocre element

# 返回元素的分数
zscore key element 

# 返回元素的总个数
zcard key

# 返回升序的排名名次
zfrank key element

# 查看区间
zrange key start end [withscores]

# 查询所有
zrange key 0 -1 withscores 

# 根据 score 获取区间
zrangebyscore key min max withscore

# 统计分数
zcount key minScore maxScore

# 删除
zrem key element [element1...]

# 根据排名删除
zremrangebyrand key start end

# 按照分数删除
zremrangebycount key minScore maxScore

```


## 键值管理
#### 单个
```
# 返回key的类型
type key

# 返回 key 的实际数据类型
object encoding key 

# 删除
del key

# 判断key 是否存在
extsis key

# 强制重命名
rename key newKeyName

# 设置过期时间
expire key timeout

# 查看过期时间
ttl key

# 去除过期时间
persist key
```

#### 所有
```
# 返回所有键(支持正则)
keys *

# 扫描
scan
```

## 服务器数据
```
# 返回所有 key 数量
dbsize

# 选择切换数据库(默认16个数据库 0-15 用户隔离数据)
select [0 - 15]

# 清除当前数据库
flushdb

# 清除所有数据库
flushall
```

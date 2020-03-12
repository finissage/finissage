
# mysql 教程
## DDL
**数据库定义语言** 关键字 `create`

```sql
-- 登录数据库
mysql -uroot -proot -h localhost -P 3306
-- -u 用户名
-- -p 密码
-- -h ip
-- -P 端口号

-- 查看当前数据库版本
status 
select version();

-- 查看所有数据库
show databases;

-- 查看数据库所有表
show tables;

-- 查看当前使用的数据库
select database();

-- 查看数据库字符集
show variables like 'character%';

-- 查看当前登录用户
select user();

-- 使用数据库
use databaseName;

-- 修改数据库字符集
set names utf8;

-- 修改mysql 访问权限
grant all privileges on 数据库名称.表名 to '登录名'@'ip' with grant option
grant all privileges on *.* to 'root'@'%' with grant option

-- 删除数据库
drop database `demo`;

-- drop 
drop table `user_table`; -- 删除表

-- 创建一个数据库(如果不存在)
create database if not exists `test_db` default charset utf8;

-- 创建一个表(如果不存在)
create table if not exists `user_table`(
	`user_id` int(11) not null auto_increment primary key comment 'id',
	`user_name` varchar(20) not null unique comment '用户名',
	`user_age` int(11) comment '年龄'
)engine=innodb default charset utf8;
```

#### 表结构 （table）

```sql
-- 添加一个字段(放在 id 后面)
alter table tablename add `file_name` varchar(20) not null comment '文件名称' after `id`;

-- 添加多个字段
alter table `tablename` add(
	`file_name` varchar(200) not null defualt '' comment '添加第一个列',
	`file_path` varchar(200) not null defualt '' comment '添加第二个列'
)

-- 修改字段名称
alter table `tablename` rename column `file_name` to `fileName`;

-- 修改字段类型及描述
alter table `tablename` modify `file_name` varchar(20) not null default '' comment '修改类的描述及类型';

-- 删除一个列
alter table `tablename` drop `file_name`;

```

#### 索引（index）
```sql
-- 查询表中所有索引
show index from `tables`;

-- 穿件一个字段的索引
create index `indexName` on `tableName` (username);

-- 创建一个联合索引
create index `indexName` on `tableName`(`username`,`create_time`);

-- 添加一个唯一索引
alter unique index `indexName` on `tablename`(`username`);	

-- 删除表中的索引
drop index `indexname` on `tablename`

```

#### 锁（lock）
```sql
-- 
```

## DML
数据库操纵语言  关键字 `insert`、 `delete`、 `update`
```sql
-- 添加
insert into `user_table` vlaues(null, '张三', 19); -- 添加张三 id为自增
insert into `user_table`(`user_name`, `user_age`) 
values ('李四', 20), 
('王五', 21); -- 批量添加制定列

-- 删除表数据 
delete from `user_table` -- 清空表数据(逐行)
delete from `user_table` where id = 1; -- 删除id=1的数据

-- 清空表数据
truncate table `user_table`; -- 清空表数据
delete from 'user_table'; -- 

-- 修改
update table `user_table` set `user_name` = '赵六' where id = 1; -- 将id为1的名称修改为赵六
update table `user_table` set `user_name` = '赵六', `user_age` = 21 where id = 1; -- 将id的名称修改为赵六，年龄修改为21	

```

## DCL
数据库控制语言 关键字 `grant`、 `remove`

## DQL
数据库查询语言 关键字 `select`

## 数据库备份与恢复
```shell
# 在没有登录数据库的操作，-p 不要显示输入密码 备份前会有输入密码提示
mysqldump -uroot -p databaseName > filePath
mysqldump -uroot -p demo > /demo.sql  # 备份demo 的数据库到 跟路径下的demo.sql文件

mysqldump --not-date -uroot -p databaseName > filePath
mysqldump --not-date -uroot -p demo > /demo.sql # 备份demo 数据库结构到文件

mysqldump -uroot -p databaseName --default-character-set=字符集 > /filePath
mysqldump -uroot -p demo --default-character-set=utf8 > /demo.sql

# 恢复数据库(没有登录数据库操作)
# 登录数据库后操作
use demo; # 登录数据库
source /demo.sql # 从文件回复数据库
```
```sql
-- 数据导出
select * into outfile 'path' from tableName;
-- 导入
load data infile 'path' into table tableName
```

## mysql 事务
`mysql` 事务主要是用于处理操作量大，复杂度搞得数据，比如说，在人员管理胸中。你删除一个人员。你即需要删除人员的基本资料，也要删除和该人员相关的信息，如信箱，文章等等，这样，这些数据  

### 事务特性
***原子性:*** 一个事务中的操作要么全部提交，要么全部回滚  
***一致性:*** 在一个事务执行结束后，数据库的完整性没有破坏  
***隔离性:*** 数据库多线程并发情况下对数据读写的能力，mysql有四种隔离级别(session级别)  
* 读未提交（read uncommitted）
* 读提交（read committed） 
* 可重复读（repeatbale read）
* 串行化(serializable)  

***持久性:*** 事务结束后对数据的改变是永久性的改变

|事务隔离级别|脏读|不可重复读|幻读|
|:--|:--|:--|:--|
|读未提交(read-uncommitted)|√|√|√|
|不可重复读(read-commited)|×|√|√|
|可重复读(repeatable-read)|×|×|√|
|串行化(serializabla)|×|×|×|

```sql
-- mysql 默认隔离级别是可重复读 修改隔离级别 读提交
set session transaction isolation livel read commited;

-- 开启事务
begin
start transaction

-- 事务保存点 （添加一个事务保存点）
savepoint `savepoint_name` 

-- 提交事务
commit

-- 回滚事务
rollback

-- 回滚事务到保存点
rollback to sacepoint `sacepoint_name`

-- 开启自动提交事务
set transaction = 1;

-- 关闭自动提交事务
set transaction = 0;

-- 设置数据库隔离级别

```

## Sql 优化

1. 全值匹配
2. 最佳左前缀法则
	让索引不失效的策略 
3. 不要再索引列上做任何操作(函数)
4. 范围条件放最后
	范围条件之后的条件会失效
5. 尽量使用覆盖索引
6. 不等于要慎用
7. null/not对索引的影响
	根据字段是否非空会有针对性的影响
8. like 查询要当心
9. 字符类型加引号
10. or改union


## MysqlSlap (mysql压测工具)
> 1. mysqlslap 是 mysql 的 5.1.4 版本开始官方提供的压力测试工具  
> 2. 创建 schema、table、test data；  
> 3. 运行负责测试，可以使用多个并发客户端连接；  
> 4. 测试环境清理（删除创建的数据、表等，断开连接）  

### mysqlslap 参数
|参数|作用
|:--|:--|
|--create-schema=name| 置顶测试的数据库名，默认是mysqlslap
|--engine=name|指定测试表所使用的存储引擎，可置顶多个|
|--concurrency=N|模拟N个客户端并发执行，可置顶多个值|
|--number-of-queries=N|总测试次数(并发客户数X没客户查询次数),比如并发是10，总次数是100，那么10个客户端个执行10次)|
|--iterations=N|迭代执行次数，既重复的次数(相同的测试进行N次,求一个平均值)，整个步骤重复的次数，包括准备数据，测试load，清理|
|--commit|执行N条后提交一次|
|--auto-generate-sql,-a|自动生成测试和表，表示用`mysqlslap`工具自己生成的sql 脚本来测试并发压力。|
|auto-generate-sql-load-type=name|测试语句的类型。代表要测试的环境是读操作还是写操作还是两者混合的，取值包括:`read`,`write`,`key`,`udpate`,`or mixed 默认`.|
|--auto=generate-sql-add-auto-increment|对生成的表自动添加auto_increment列|
|--number-char-cols=name|自动生成的测试表中包含N个字符类型的列,默认是1|
|--number-int-cols=name|自动生成的测试表中包含N个数字类型的列,默认是1| 
|--debug-info|打印内存和CPU信息|

```sql
mysqlslap -uroot -proot --concurrency=1000 --iterations 10 -a --auto-generate-sql-add-autoincrement --engine=inndb --number-of-queries=1000
```

## 存储引擎

### myisam  
- 特性：
	* 并发与锁级别-表级锁
	* 支持全文检索
	* 支持数据压缩
- 场景：
	* 非事务性应用（数据仓库，报表，日志数据）
	* 只读类型
	* 空间类应用（空间函数，坐标）  

### innodb
> mysql 5.5及以后版本默认存储引擎
> 5.6 以前默认为 系统表空间   
- 系统表空间和独立表空间
	* 系统表空间无法简单的收缩文件大小
	* 独立表空间可以通过 optimize table 收缩系统文件
	* 系统表空间会产生 IO 瓶颈
	* 独立表空间可以同时向多个文件刷新数据
	* ***建议：ioondb使用独立表空间 ***  
- 特性：
	* innodb 是一种事务性存储引擎
	* 完全支持事务的 ACID 特性
	* redo log 和 undo log
	* innodb 支持行锁（并发程度更高）
- 场景：
	* innodb 适用于大多数 OLTP 应用

### myisam 对比 innodb
|对比项|myisam|innodb|
|:--|:--|:--|
|主外键|不支持|支持|
|事务|不支持|支持|
|行锁|表锁，即使操作一条数据也会锁整张表，不适合高并发的操作|行锁，操作值锁操作的行适合高并发操作|
|缓存|值缓存索引，不缓存真是数据|缓存索引和真是数据，对内存要求高|
|表空间|小|大|
|关注点|性能|事务|
|默认安装|yes|yes|

## 锁
> 1. 锁是计算机协调多个进程或线程并发访问某一资源的机制  
> 2. 在数据库中，数据也是一种共许多用户共享的资源，如何保证数据并发访问的一致性、有效性、是多有数据库必须解决的一个问题，锁冲突也是影响数据库并发访问性能的一个重要因素  
> 3. 锁对数据库而言闲的尤其重要，也更复杂。

### innodb 行锁
#### 共享锁 （读锁）  
	当一个事务对某几行上读锁时，允许其他事物对着几行进行读操作，但不允许起进行写操作，也不运行其他事物给这几行上排它锁，但允许上读锁。
```sql
-- 共享锁 lock in share mode
select * from tableName where id = 1 lock in share mode;
```

#### 排它锁 （写锁）
	当一个事物对某几行上写锁时，不允许其他事物写，但允许读，更不允许其他事物给这几行上任何锁，包括写锁。
```sql
-- 排它锁 for update 
select * from tableName where id = 1 for update;
```

- 注意：
	* 两个事物不能锁同一个索引，
	* insert delete update 再说会务中都会自动默认加上排它锁
	* 行锁必须有索引才能实现，否则自动锁表，那么就不是行锁了。


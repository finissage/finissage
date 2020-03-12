# mysql 执行计划

> mysql 使用 explain + `sql` 语句查看执行计划，该执行计划不一定完成正确但是可以参考

```sql
-- 查看执行计划
explain select * from `table_name` where `id` = 1;
```

## select_type
|select_type| 说明|
|:--|:--|
|simple|简单查询|
|primary|最外层查询|
|subquery|映射子查询|
|derived|子查询
|union|联合查询|
|union result|使用联合的结果|

## table 
正在访问的表名

## type
|type|说明|
|:--|:--
|all|全表扫描
|index|全索引扫描
|range|对索引列进行范围查找
|index_merge|合并索引，使用多个列索引搜索
|ref|根据索引查找一个或多个值
|eq_ref|搜索时用到`primary key` 或 `unique` 类型
|const|常量，表最多有一个匹配行，因为仅有一行，在进行的列值可被优化器剩余部分认为是常数，`const` 表很快，因为他们只读取一次
|system|系统，表仅有一行(=系统表)。这是 `const` 连接类型的一个特例

> 性能：`all` < `index` < `range` < `index_merge` < `ref_or_null` < `ref` < `eq_ref` < `const` < `system`
> 性能在 `range` 之下基本都可以进行调优

## possible_keys 
可能使用的索引

## key 
真实使用的

## key_len
Mysql 中使用索引字节长度

## rows
mysql 预估为了找到所需的行而要读取的行数

## extra
|extra|说明
|:--|:--
|using index|此值表示`mysql`姜使用覆盖索引，以避免访问表。
|using where|`mysql` 姜存储引擎检索行后再进行过滤，许多where 条件里设计索引中的列，当（并且如果）它读取索引时，就能被存储引擎检验，因此不是所有带where自居的查询都会显示`using where`， 有时using where`的出现就是一个暗示:查询可受益于不同的索引。
|using temporary|`mysql`对查询结果排序是会使用临时表。
|using filesort| `mysql`回怼结果使用一个外部索引排序，二不是按索引次序从表中读取行，`mysql`有两种文件排序算法，这两种排序方式都可以在内存或磁盘上完成，`explain`不是告诉你`mysql`将使用那种文件排序，也不会告诉你排序会在内存里还是在磁盘上完成
|range checked for each record(index map: N)|没有好用的索引，新的索引将在链接的每一行上中心估算，N是显示在`possible_keys`列中的索引的位图，并且是沉余的

## limit
`limit` 匹配后就不会继续进行扫描
# RabbitMQ CTL (rabbitadmin)

|功能|操作|
|:---:|:---:|
|exchange（消息交换机）|[定义](#定义exchange) [查询](#查询exchange) [删除](#删除exchange)|  
|queue（消息队列）|[定义](#定义queue)  [查询](#查询queue)  [删除](#删除queue) |
|binding（绑定）|[定义](#定义binding)  [查询](#查询binding) [删除](#删除binding)|
|vhost（虚拟主机）|[定义](#定义vhost) [查询](#查询vhost) [删除](#删除vhost)|
|user（用户）| [定义](#定义user)[查询](#查询user) [删除](#删除user)|
|queue （消息）|[发布消息](#发布一条消息) [订阅消息](#消费一条消息)|

## 定义exchange

```shell
rabbitmqadmin declare exchange name=test type=direct
```
格式：`declare exchange name=... type=... [auto_delete=... internal=... durable=... arguments=...]`


**options：**
- `name`：给交换机定义一个名称
- `type`：
  - `direct`：直接将消息推送`routing key`指定的`queue`上（默认类型）
  - `topic`：根据消息路由密钥和用于将队列绑定到交换机的模式之间的匹配，将消息交换到一个或多个队列（`*`代表匹配单个单词，`#`匹配零个或多个单词）  
  - `fanout`：将消息发给所有已绑定的交换机上
  - `headers`：标题交换被设计用于在多个属性上进行路由，这些属性比路由密钥更容易表示为消息头。标题交换忽略路由密钥属性。相反，用于路由的属性取自`headers`属性。如果头的值等于绑定时指定的值，则认为消息被匹配
- `auto_delete`： 当所有队列完使用后，交换机是否自动删除
- `internal`：如果交易是内部交易，则为真，即不能由客户直接发布
- `durable`：Rabbitmq服务重启后`exchange`是否依旧存在
- `arguments`：交换机的附加键/值参数。

## 查询exchange

``` shell
rabbitmqadmin list exchanges
```

格式：`list exchanges [column...]`

**columns：**
- `name`：名称
- `type`：类型

# 删除exchange

```shell
rabbitmqadmin delete exchange name=test
```

格式：`delete exchange name=...`

**options：**
- `name`：要删除的`exchange`名称

## 定义queue
*定义一个`queue`成功后，系统会自动定义一个`binding`，`source`指向的是系统默认的`exchange`，名称为空字符串，`type`是`direct`*
```shell
rabbitmqadmin declare queue name=test  
```
格式：
`declare queue name=queuename [node=... auto_delete=... durable=.... arguments=....]`

**options：**
- `name`：给`queue`指定一个名称
- `auto_delete`：是否自动删除
- `durable`：重新启动 RabbitMQ 服务队列是否依旧存在
- `arguments`：队列的附加键/值参数。

## 查询queue

```shell
rabbitmtadmin list queues
```
格式：`list queues [column... ]`

**columns：**
- `name`：名称
- `messages`：信息

## 删除queue

```shell
rabbitmtadmin delete queue name=test
```

格式：`delete queue name=...`

**options：**
- `name`：删除`queue`名称

---

## 定义binding

```shell
rabbitmqadmin declare binding source=test destination=test
```

格式：`declare binding source=... destination=... [arguments=... routing_key=... destination_type=...]`

**options：**
- `source`：目的地队列或交换
- `destination`：目的地
- `arguments`：绑定的附加键/值参数。
- `routing_key`：路由密钥
- `destination_type`：指定绑定目的的类型（可以为：`queue`，`exchange`）

## 查询binding

```shell
rabbitmqadmin list bindings
```

格式：`list bindings [column...]`  
**columns：**
- `source`：源（交换机）
- `destination`：目标（交换机/消息序列）
- `routing_key`：路由密钥

## 删除binding

```shell
rabbitmqadmin delete binding source=test destination_type=queue destination=1111 properties_key=1111
```
格式：`delete binding source=... destination_type=... destination=... properties_key=...`

**options：**
- `source`：交换机的名称
- `destination_type`：目的地的类型（type：`exchange`，`queue`）
- `destination`：目的地的名称
- `properties_key`：经测试，`properties_key`对应的就是 定义`binding`时指定的`routing_key`（非官方定义个人理解及测试）

--- 


## 定义vhost

```shell
rabbitmqadmin declare vhost name=test
```

格式：`declare vhost name=vhostname [tracing=...]`

**options：**
- `name`：给`vhost`指定一个名称
- `tracing`：*暂未找到使用方法*

## 查询vhost

```shell
rabbitmqadmin list vhosts
```

格式：`list vhosts [column...]`

**columns：**
- `name`：名称
- `messages`：信息

## 删除vhost

```shell
rabbitmqadmin delete vhost name=test
```

格式：`delete vhost name=....`

**options：**
- `name`：要删除的`vhost`名称

--- 

## 定义user

```shell
rabbitmqadmin declare user name=zhangsan password=123456 tags=tags
```
格式：`declare user name=... password=... tags=...`

**options：**
- `name`：用户名
- `password`：密码
- `tags`：标签

## 查询user

```shell
rabbitmqadmin list users
```

格式：`list users [column...]`

**columns：**
- `name`：名称
- `hashing_algorithm`：散列算法
- `password_hash`：哈希密码
- `tags`：标签

## 删除user

```shell
rabbitmqadmin delete user name=test
```
格式：`delete user name=...`

**options：**
- `name`： 指定要删除的用户名名称

--- 

## 发布一条消息

```shell
rabbitmqadmin publish routing_key=1111 payload="hello world"
```
格式：`publish routing_key=...  [exchange=... payload=... payload_encoding=... properties=...]`

**options：**
- `roting_key`：用于消息绑定到列队的密钥消息上的路由密钥英语器注册的队列的路由密钥想匹配，最多255个字节
- `exchange`：消息交换机的名称 如果不指定 将走默认的`exchange`名称是一个空字符串
- `payload`：消息内容
- `payload_encoding`：消息内容编码格式
- `properties`：暂无查找结果

## 消费一条消息

```shell
rabbitmqadmin get queue=1111 requeue=true
```
格式：`get queue=... [count=... requeue=... payload_file=... encoding=...]`

**options：**
- `queue`：消息列表名称
- `count`：检索多个消息
- `requeue`：当消费者死亡，消息无法被确认，如果true消息被重新排列到消息队列， RabbitMQ 会将此消息重新推送给其他订阅此消息的消费者，如果false 将不重新排列到消息队列，直接从内存删除
- `payload_file`：如果在 get 上指定`payload_file` ，则会显示有效载荷。如果在`get`上指定了`payload_file`， 则不能设置`count`
- `encoding`：获得消息的编码格式

> *引用官方地址：* https://www.rabbitmq.com/tutorials/amqp-concepts.html

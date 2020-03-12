# `RabbitMQ`
[什么是`RabbiqMQ`?](#什么是RabbitMQ?)  
[`RabbitMQ`能做什么？](#RabbitMQ能做什么)  
[`AMQP`协议和`RabbitMQ`](#AMQP协议和RabbitMQ)  
[用户](#用户)  
[`Exchange`交换器](#Exchange交换器)  
[过期时间（`TTL`）](#过期时间)  
[消息确认](#消息确认)  
[持久化](#持久化)  
[死信队列](#死信队列)  
[延迟队列](#延迟队列)  
[`SpringBoot`整合`RabbitMQ`](#SpringBoot整合RabbitMQ)

## 什么是`RabbitMQ`？
`RabbitMQ` 是基于 `AMQP` 协议的一种消息中间件，使用 `Erlang` 语言开发的。有这原生 `Socket` 一样的速度，如命 `rabbit`（兔子）;

## `RabbitMQ`能做什么？
* 异步： 将消息写入到队列，非必要的业务逻辑已异步的方式来处理
* 解耦：`A`系统要调用`B` `C`系统，如果将来需要调用`D`系统，使用`mq` `A`系统不需要做更改，订阅就能解决问题
* 削峰：秒杀场景大量请求直接写入队列中， 队列满了之后直接熔断请求

## `RabbitMQ`架构
`Producer`、`Exchange`、`Binding`、`Queue`、`Consumer`之间的关系
![Producer、Exchange、Binding、Queue、Consumer关系图](../images/rabbitmq_arc1.jpg)  

`Routing Key`、`Binding Key`、`Exchange Type`的关系
![`Routing Key`、`Binding Key`、`Exchange Type`](../images/rabbitmq_arc2.png)


## `AMQP`协议和`RabbitMQ`
* `Server`: 接受客户端的连接，实现 `AMQP` 实体服务  
* `Connection`: 连接，应用程序与 `Server` 的网络连接，`TCP`连接。  
* `Channel`： 信道，消息读写操作在信道中进行，客户端可以建立多个信道，每个信道代表一个会话任务。  
* `Message`： 消息，应用程序和服务器之间传送的数据，消息可以非常简单，也可以很复杂。有 `Properties` 和 `Body` 组成。    
    * `Properties`：为外包装，可以对消息进行修饰，比如消息的优先级、延迟高等特性；  
    * `Body`：就是消息体内容  
* `Virtual Host`：虚拟主机，用于逻辑隔离。一个虚拟主机里边可以有若干个 `Exchange` 和 `Queue`，同一个虚拟主机里边不能有相同的名称和 `Excange` 或 `Queue`。  
* [`Exchange`](#Exchange交换器)：交换机，接受消息，按照路由规则将消息路由到一个或多个队列，如果路由不到，或者返回给生产者，或者直接丢弃，`RabbitMQ` 常用的交换机类型有 `direci`、`topic`、`fanout`、`headers`四种，后面详细介绍。  
* `Binding`：绑定，交换器和消息队列之间的虚拟链接， 绑定中可以包含一个或多个 `RoutingKey`。  
* `RoutingKey`： 路由键，身缠这将消息发送给交换器的时候，会发送一个 `RoutingKey`，用来指定路由规则，这样交换器就知道吧消息发送到那个队列。路由键通常为一个“.”分割的字符串，例如“`com`.`name`”。  
* `Queue`： 消息队列，用来保存消息，共消费者消费。 



|概念|详解|  
|:--:|:--:|  
|Exchange|消息交换机,他指定消息按照什么规则,路由到那个队列|  
|Queue|消息队列,每个消息最终都会被投入到一个或多个队列|  
|Binding|绑定,他的作用就是把 `Exchange` 和 `Qeueu`按照路由规则绑定在一起|  
|Routing Key|路由关键字，`Exchange`根据这个关键字进行消息投递|  
|Vhost|虚拟主机，可以开设多个 `Vhost`,用作不同用户的权限分离|  
|Producer|消息生产者,就是消息投递的程序|
|Consumer|消息消费者，就是接受消息的程序|
|Channel|消息通道，在客户端的每个链接里，可以建立多个 `channel`，每个`channel`代表一个会话任务|  

## 用户
1. `adminnistrator`(超级管理员)  
    可登录管理控制台，可查看所有的信息，并且可以对用户，策略进行操作。  
2. `monitoring`（监控者）  
    可登录管理控制台，同事可以查看 `rabbitmq` 节点的相关信息（进程数，内存使用情况，  磁盘使用情况等。。）  
3. `policymaker`（策略制定者）  
    可登录管理控制台，同时可以对`policy` 进行管理，但是无法查看节点的相关信息  
4. `managerment`（普通管理者）  
    进可登录管理控制台，无法查看节点信息，也无法对策略进行管理。  
5. 其他  
    无法登录管理控制台，通常就是普通的生产者与消费者  

## `Exchange`交换器  
`RabbitMQ` 常用的交换器有 `direct`，`topic`，`fanout`， `headers` 四种。  

1. `Direct Exchange`  
    该类型的交换器将所有发送到该交换器的消息被转发到 `RoutingKey` 指定的队列中，也就是说路由到 `BindingKey` 和 `RoutingKey` 完全匹配的队列中。  
2. `Topic Exchange`   
    该类型的交换器将所有发送到 `Topic`，`Exchange` 的消息被转发到所有 `RoutingKey` 中指定的 `Topic` 的队列上面  
3.  `Fanout Exchange`  
    该类型不处理路由键，会吧所有发送到交换器的消息路由到所有绑定的队列中。有点是转发消息最快，性能最好。  
4. `Handers Exchange`  
    该类型的交换器不以来由路由规则来路由消息，二是根据消息内容中的 `Headers` 属性进行匹配。 `Headers` 类型交换器性能差，在实际中并不常用。  

## 过期时间
**`Time` `To` `Live`， 也就是生存时间，是一条消息在队列中的最大存活时间。单位是毫秒**  
`RabbitMQ` 可以对消息和队列设置 `TTL`  
1.  消息过期时间  
`RabbitMQ` 支持设置消息的过期时间，在消息发送的时候可以进行指定，每条消息的过期时间可以不同。  
2.  队列过期时间
`RabbitMQ` 支持设置队列的过期时间，从消息如队列开始计算，知道超过了队列的超时时间配置，那么消息会变成死信，自动清除。  
如果两种方式一起使用，则过期时间已两者中较小的那个数值为准。  
也可以不设置 `TTL`， 不设置表示消息不会过期。如果设置为``0``,则表示除非此时可以直接将消息投递到消费者，否则该消息将被立即丢弃。  

## 消息确认
为了保证消息从队列可靠的到达消费者，`RabbitMQ` 提供了消息确认机制。消费者订阅队列的时候，可以指定 `autoAck` 参数， 
1. 当 `autoAck` 为 `true` 的时候，`RabbitMQ` 采用自动确认模式，`RabbitMQ` 自动把发送出去的消息设置了确认，然后从内存或硬盘中删除，二不管消费者是否真正的消费到了这个消息。
2. 当 `autoAck` 为 `false` 的时候，`RabbitMQ` 会等待消费者回复的确认信号，收到确认信号之后从内存或磁盘中删除消息。

***消息确认机制是 `RabbitMQ` 消息可靠性的基础，只要设置 `autoAck` 参数为 `false`，消费者就有足够的时间处理消息，不用担心处理消息的过长中消费者进程关掉后消息丢失的问题。***   

## 持久化
消息的可靠性是 `RabbitMQ` 的一大特色，持久化可以防止在异常情况下丢失数据。`RabbitMQ` 的持久化分为三个部分，`交换器持久化`，`队列持久化`和消息`持久化`.  
### 交换器持久化
交换器持久化可以通过在声明队列时将 `durable` 参数设置为 `true`。如果交换器不设置持久化，那么在 `RabbitMQ` 服务重启后，相关的交换器元数据会丢失，不过消息不会丢失，只是不能将消息发送到这个交换器了。

### 队列持久化
队列的持久化能保证起本身的元数据不会因一场情况为丢失，但是不能保证内部锁存储的消息不会丢失。要确保消息不会丢失，需要将其设置为持久化，队列的持久化可以通过在声明队列时将 `durable` 参数设置为 `true`

### 消息持久化
设置所有消息持久化，这样必要会影响 `RabbitMQ` 的性能，因为磁盘的写入速度比内存的写入要慢得多，对可靠性不那么高的消息可以不蚕蛹持久化处理已提高整体的吞吐量。

***设置了队列和消息的持久化，当 `RabbitMQ` 服务重启后，消息依然存在，如果只设置队列或消息持久化，重启后消息都会消失。*** 

## 死信队列

## 延迟队列
一般的队列，消息一旦进入队列就会被消费者立即消费。延迟队列就是进入该对的消息会被消费者延迟消费，延迟队列中存储的对象是延迟消息，“延迟消息”是指当消息被发送以后，等待特定的时间后，消费者才能拿到这个消息进行消费。

延迟队列用于需要延迟的场景，最常见的使用场景是：订单，用户下单后30 分钟未支付，那么订单自动取消。除了延迟消费，延迟队列的典型应用场景还有延迟重试，比如消费者从队列中消费消息失败了，可以延迟一段时间进行重试。

# `SpringBoot`整合`RabbitMQ`  
## 配置  
#### 组件整合  
```xml
<!-- rabbitmq 依赖 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```  

#### 配置 RabbitMQ 链接信息，包含主机，端口，用户名和密码  
```yml
spring:
    rabbitmq:
        host: localhost
        port: 5672
        username: admin
        password: admin
```   
## 实现生产者与消费者  
#### 生产者
生产者用来生产消息进行发送， 需要用到 `RabbitTemplate`，`RabbitTemplate`是发送消息的管件类，`convertAndSeand`方法可以指定消息发送的交换器、路由键、消息内容等。  
```java
@Component
public class Producer{
    @Autowired
    public RabbitTemplate rabbitTemplate;

    public void produce() {
        String ms = new Date() + "hello world";
        System.out.println(ms);
        rabbitmqTemplate.converAndSeand("rabbitmq_queue", message);
    }
}

```

#### 消费者
消费者消费生产者发送的消息。实现消费者主要用到 `@RabbitListener`。`@RabbitListener`是一个功能强大的注解，这个注解里边可以注解配置`@QueueBinding`、`@Queue`、`@Exchange`直接通过这个组合注解一次性搞定多个交换机、绑定、路由、并且配置坚挺功能等。   
1. 在 RabbitMQ 控制面板创建好队列，使用 `@RabbitListener` 监听队列。  
```java
@RabbitListener(queues = "test_queue")
```

2. 使用 `@RabbitListener` 自动创建队列。
```java
@RabbitListener(queuesToDeclare = "test_queue")
```

3. 使用 `@RabbitListener` 自动创建队列，并对 `Exchange`和`Queue`进行绑定。
```java
@RabbitListener(bindings = @QueueBinding(value = "test_queue"), key = "test_key", exchange = @Exchange("test_exchange"))
```
``` java
@Component
public class Consumer{
    @RabbitHandler
    @RabbitListener(queuesToDeclare = @Queue("test_queue"))
    public void process(String message) {
        System.out.println("消费者消费消息： " + message);
    }
}
```


## RabbitmqAdmin

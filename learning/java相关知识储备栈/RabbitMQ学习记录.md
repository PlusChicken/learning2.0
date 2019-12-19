# RabbitMQ学习记录

生产者和消费者

MQ消息队列：

消息队列(Message Queue,简称MQ)，队列，FIFO先入先出

主要用途:不同进程Process/线程Thread之间通信

RabbitMQ是一个开源的,在AMQP基础上完整的,可复用的企业消息系统。

开发语言:Erlang(向并发的编程语言)

AMQP是消息队列的一个协议

用户名为中文解决办法：

https://blog.csdn.net/leoma2012/article/details/97636859

https://blog.csdn.net/thewaiting/article/details/80379294

路径为中文怎么办

1.  管理员运行cmd然后打开RabbitMQ安装目录
2. rabbitmq-service.bat remove
3. set RABBITMQ_BASE=D:\rabbitmq_server\data
4. rabbitmq-service.bat install
5. rabbitmq-plugins enable rabbitmq_management
6. 去服务管理器启动服务

对于RabbitMQ来说,除了生产者、消息队列、消费者这三个基本模块以外,还添加了一个模块,即交换机(Exchange).它使得生产者和消息队列之间产生了隔离,生产者将消息发送给交换机,而交换机则根据调度策略把相应的消息转发给对应的消息队列。

![rabbitmq](..\picture\rabbitmq.png)

（一下平均分类的输出，输出顺序不具备规律）

一对一：

~~~~java
2019-09-03 10:50:05.606  INFO 42484 --- [nio-8080-exec-3] com.demo2.component.MsgProducer          : 简单消息发送时间：Tue Sep 03 10:50:05 CST 2019
2019-09-03 10:50:05.611  INFO 42484 --- [nio-8080-exec-3] com.demo2.aspect.HttpAspect              : response=success
2019-09-03 10:50:05.621  INFO 42484 --- [ntContainer#1-1] com.demo2.component.MsgReceiver2         : 接收消息2：简单消息发送
~~~~

一对多：

~~~~java
2019-09-03 10:56:59.713  INFO 42484 --- [ntContainer#0-3] com.demo2.component.MsgReceiver1         : 接收消息1：简单消息发送
2019-09-03 10:56:59.714  INFO 42484 --- [ntContainer#1-3] com.demo2.component.MsgReceiver2         : 接收消息2：简单消息发送
2019-09-03 10:56:59.714  INFO 42484 --- [ntContainer#0-3] com.demo2.component.MsgReceiver1         : 接收消息1：简单消息发送
2019-09-03 10:56:59.714  INFO 42484 --- [ntContainer#1-3] com.demo2.component.MsgReceiver2         : 接收消息2：简单消息发送
2019-09-03 10:56:59.714  INFO 42484 --- [ntContainer#0-3] com.demo2.component.MsgReceiver1         : 接收消息1：简单消息发送
2019-09-03 10:56:59.714  INFO 42484 --- [ntContainer#1-3] com.demo2.component.MsgReceiver2         : 接收消息2：简单消息发送
2019-09-03 10:56:59.715  INFO 42484 --- [ntContainer#0-3] com.demo2.component.MsgReceiver1         : 接收消息1：简单消息发送
2019-09-03 10:56:59.715  INFO 42484 --- [ntContainer#1-3] com.demo2.component.MsgReceiver2         : 接收消息2：简单消息发送
2019-09-03 10:56:59.715  INFO 42484 --- [ntContainer#0-3] com.demo2.component.MsgReceiver1         : 接收消息1：简单消息发送
2019-09-03 10:56:59.715  INFO 42484 --- [ntContainer#1-3] com.demo2.component.MsgReceiver2         : 接收消息2：简单消息发送
~~~~

多对多：就是平均分配

多对一：。。。



#### Rabbitmq中Exchange的四种模式

fanout>direct>>topic                        11:10:6

1 fanout

Fanout exchange是最基本的交换机类型，它所能做的事情非常简单——广播消息。扇形交换机会把能接收到的消息全部发送给绑定在自己身上的队列。因为广播不需要"思考"，所以扇形交换机处理消息的速度也是所有的交换机类型里面最快的。

实现demo

2 direct

Direct Exchange-处理路由键。需要将一个队列绑定到交换机上，要求该消息与一个特定的路由键完全匹配。这是一个完整的匹配。如果一个队列绑定到该交换机上要求路由键"dog"，则只有被标记为"dog"的消息才被转发,不会转发dog.puppy,也不会转发dog.guard,只会转发dog.目的是把特定的消息广播给特定的队列.

任何发送到Direct Exchange的消息都会被转发到RouteKey中指定的Queue.

​	a.一般情况可以使用rabbitMQ自带的Exchange:""(该Exchange的名字为空字符串,下文称其为defaultExchange)

​	b.这种模式下不需要将Exchange进行任何绑定(binding)操作

​	c.消息传递时需要一个"RouteKey",可以简单的理解为要发送到的队列名字

​	d.如果vhost中不存在RouteKey中指定的队列名,则该消息会被抛弃

实现demo

3 topic

Topic exchange它是一种支持正则匹配的exchange，发送到topic exchange上的消息需要携带指定的规则的routing_key，主题交换机会根据这个规则将数据发送到对应的(多个)队列上。该exchange的routing_key需要有一定的规则，交换机和队列的binding_key需要采用*.#. *......的格式，每个部分用.分开，其中： *表示一个单词，#表示任意数量(零个或多个)单词。

实现demo

4 header:header模式在实际使用中较少

它是忽略routing_key的一种路由方式。路由器和交换机路由的规则是通过Headers信息来交换的，这个有点像HTTP的Headers。将一个exchange声明成Headers exchange，绑定一个队列的时候，定义一个Hash的数据结构，消息发送的时候会携带一组hash数据结构的信息，当Hash的内容匹配上的时候，消息就会被写入队列。绑定exchange和队列的时候，Hash结构中要求携带一个键"x-match",这个键的Value可以是any或者all，这代表信息携带的Hash是需要全部匹配(all)，还是仅匹配一个键(any)就可以了。相比direct exchange，首部交换机的优势是匹配的队则不被限定为字符串(String).




















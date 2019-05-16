[原网址](https://www.rabbitmq.com/tutorials/tutorial-four-spring-amqp.html)

个人理解：direct exchange 和 fanout exchange的不同之处在于过滤。


## Direct exchange
直接交换机
前面教程我们的消息系统广播所有消息到消费者。我们想扩展它通过允许基颜色类型的消息过滤器。举个例子，我们想要一个程序接收到的错误日志消息写到硬盘，而不浪费硬盘空间在警告和信息日志上。
我们使用的生产者消费者（fanout交换机）模式不能给我们提供这样的灵活性-它只能无脑的广播。
我们将会使用**直接交换机**代替，它背后的路由算法很简单-一个消息路由关键字和它所要去的队列绑定的关键字匹配。

为了说明它，考虑如下设置

![picture](https://www.rabbitmq.com/img/tutorials/direct-exchange.png)
在这个设置中，我们看到**直接交换机** **X**又两个队列绑定在它身上。第一个队列的绑定关键字是orange,第二个又两个绑定，一个是black，另一个是green。
在这样设置中一个带有orange关键字的消息发布到交换机将会被路由到Q1队列，带有路由关键字black和green的消息会被发送到队列Q2.其它所有的消息会被丢弃。
## 多绑定

![多绑定](https://www.rabbitmq.com/img/tutorials/direct-exchange-multiple.png)
给多个队列绑定同一个绑定关键字是合法的。在我们的例子中我们可以增加一个绑定在X和Q1使用关键字**black**.如此，直接交换机的行为像**fanout**将广播消息到所有匹配的队列中。一个关键字为black的消息将会被传递到Q1和Q2.

## 发布消息
我们将会使用这种模型在我们的路由系统中，我们将会使用直接交换机替代**fanout**来发送消息。我们提供颜色来当作路由关键字。那种方式使接收程序可以通过颜色来选择想要接受的消息(或者订阅).让我们先关注于发送消息。

## 代码部分省略




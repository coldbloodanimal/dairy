[原文网址](https://www.rabbitmq.com/tutorials/tutorial-one-spring-amqp.html "第一部分")
## 1.hello world!
RabbitMQ是一个消息中间件，他接受和转发消息。你可以把它当作邮局：当你把邮件放到邮箱时，你确信邮寄员最终会把你的信件传递给收件人。类似的，RabbitMQ是一个邮箱，邮局和邮寄员。
RabbitMQ 和邮局不同之处在于它不是处理报纸，而是接收，存储和转发二进制数据块-消息。
RabbitMQ，发送消息使用的一些行业术语。
* **生产**没有别的意思，只是发送。一个程序发送消息的是一个生产者。

![producer](https://www.rabbitmq.com/img/tutorials/producer.png)
* **队列**是一个居住在RabbitMQ中的邮箱，尽管消息流过RabbitMQ和你的应用，它们只能被存储在一个队列中。一个队列只会被主机内存和硬盘限制。它本质上是一个大的消息缓冲器。许多生产者可以发送消息到一个队列，许多消费则可以尝试接收来自一个队列里的数据。
我们是这样表示一个队列的：

![queue](https://www.rabbitmq.com/img/tutorials/queue.png)
* **消费**和接收有相似的意思。一个消费者是一个程序大部分时候等待接收消息：

![消费](https://www.rabbitmq.com/img/tutorials/consumer.png)

注意生产者，消费者，和队列(原文这里是broker，我猜测应该broker的核心是队列)不需要住在一起；事实上大多数应用中它们也没有。一个应用可以即是生产者又是消费者。
"Hello World"
（使用 Spring 先进的消息队列协议（Advanced Message Queuing Protocol (AMQP)))
这部分教程我们会写两个程序使用spring-amqp库;一个生产者发送一个单条消息，消费者接收消息并打印它们。我们会掩饰一些spring amqp api细节，专注于简单的事情来开始。这是一个"Hello World"的消息。
下图。"P"是生产者"C"是消费者。中间的盒子是队列-一个RabbitMQ为消费者保持消息缓冲区。

![生产者消费](https://www.rabbitmq.com/img/tutorials/python-one.png)

    The Spring AMQP 框架
    RabbitMQ支持多种协议。这个教程使用AMQP 0-9-1，开源,多用途的消息协议。有大量支持RabbitMQ的客户端在不同的语言中。
我们将使用springboot来引导和配置 spring amqp项目。我们选择maven来构建项目，当然也可以使用gradle。

代码部分略

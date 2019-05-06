## 1.hello world!
RabbitMQ是一个消息中间件，他接受和转发消息。你可以把它当作邮局：当你把邮件放到邮箱时，你确信邮寄员最终会把你的信件传递给收件人。类似的，RabbitMQ是一个邮箱，邮局和邮寄员。
RabbitMQ 和邮局不同之处在于它不是处理报纸，而是接收，存储和转发二进制数据块-消息。
RabbitMQ，发送消息使用的一些行业术语。
* **生产**没有别的意思，只是发送。一个程序发送消息的是一个生产者。

![producer](https://www.rabbitmq.com/img/tutorials/producer.png)
* **队列**

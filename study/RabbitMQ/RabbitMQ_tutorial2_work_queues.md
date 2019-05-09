# 工作队列
**前提**
这个教程假设你已经安装了RabbitMQ且运行在localhost标准端口5672.万一你使用不同端口.端口或认证，连接设置需要调整
**那里可以找到帮助呢**
如果你在学习教程过程中遇到问题，你们可以去通过邮件[联系](https://groups.google.com/forum/#!forum/rabbitmq-users)
![](https://www.rabbitmq.com/img/tutorials/python-two.png)
在第一个教程中，我们写了程序去发送结合接受消息从一个有名字的队列中。在这里我们会创建一个工作队列用来分配耗时任务在多个工人之间。

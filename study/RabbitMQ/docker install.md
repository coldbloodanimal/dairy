docker run -d --hostname rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3.7.14-management

如上会创建一个默认账号/密码 guest/guest

docker run -d --hostname rabbitmq -e RABBITMQ_DEFAULT_USER=root -e RABBITMQ_DEFAULT_PASS=root -p 5672:5672 -p 15672:15672 rabbitmq:3.7.14-management

指定账号密码 root/root

localhost:15672

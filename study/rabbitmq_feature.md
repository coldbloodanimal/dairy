### RabbitMQ 是最广泛的被部署的开源的消息中间件(message broker)。
broker 有经纪人的意思，经纪人是一个中间人，所以此处翻译未中间件
##### RabbitMQ 特性

1. 异步发消息
   1. 支持多种消息协议
   2. 消息队列
   3. 分发确认，什么鬼
   4. 灵活的路由到队列(什么鬼)
   5. 多种交换类型
2. 开发体验
   1.部署通过各种工具，如docker，开发跨越语言，发消息通过喜爱的语言如java,.net,php.
3.分布式部署
   1.集群部署为了高可用，高吞吐；
   2.通过多个可用的区域、地带的结帮(把多个区域的机器连接起来，i guess)
4.企业级和云
   1.可插拔的鉴定，认证
   2.支持TLS(传输层安全性协议（英语：Transport Layer Security，缩写作TLS）)和LDAP(LDAP是轻量目录访问协议，英文全称是Lightweight Directory Access Protocol，一般都简称为LDAP)
   3.轻量和容易的部署在公有和私有云。
5. 工具和插件
   1.多种多样的工具和插件支持持续集成,operational metrics(操作度量，什么鬼)，和集成到其它企业级系统。灵活的插件方式扩展RabbitMQ功能
6. 管理和监控
   http应用接口，命令黄工具和图形化界面用来管理和监控RabbitMQ.

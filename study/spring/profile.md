[profile](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-profiles.html)
spring profile 可以分割你的应用配置让它们只在特定环境下可用。任何@Component或者@Configuration被@Profile来限制什么时候被加载。如下

	@Configuration
	@Profile("production")
	public class ProductionConfiguration {

		// ...

你可以使用spring.profiles.active 环境属性来指定哪些配置文件被使用。你可以指定属性通过任何方式（前面章节（spring里面的）描述的方式）。比如，你可以使用
	spring.profiles.active=dev,hsqldb
你也可以是指定它在命令行同时用下面的开关
	--spring.profiles.active=dev,hsqldb
。

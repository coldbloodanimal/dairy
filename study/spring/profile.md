[profile](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-profiles.html)
spring profile 可以分割你的应用配置让它们只在特定环境下可用。任何@Component或者@Configuration被@Profile来限制什么时候被加载。如下
    @Configuration
    @Profile("production")
    public class ProductionConfiguration {

	    // ...

    }

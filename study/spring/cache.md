#spring 缓存

添加maven依赖，缓存时基于redis的，所以也要又redis

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-cache</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
        
添加注解,位置同SpringBootApplication

        @EnableCaching
        @SpringBootApplication

#核心的三个注解
1. @Cacheable 方法可缓存:查询缓存，如果没有缓存，则进入方法体内
2. @CacheEvict 缓存被清除:推测场景：数据删除时候用
3. @CachePut 缓存被更新 ：推测场景：增加修改的时候使用

注解的属性cacheNames 被个人理解为分组或者包
如果注解不设置key，会默认把方法参数作为key,
指定个性化key，使用key="#kv.id"，对象的属性或者直接属性"#id"
如下

    @CachePut(cacheNames = "redis",key="#kv.id")
    @RequestMapping(value="/cache/{id}",method= RequestMethod.PUT)
    public KeyValueModel setValue(@RequestBody KeyValueModel kv){
        return kv;
    }
    
超时可以使用spring.cache.redis.time-to-live=60000 
以秒为单位

弊端:
* 超时设置的不灵活
* redis中key 的格式为redis::key有点奇怪
有空再详细看看
如上thank you



#核心的三个注解
@Cacheable 方法可缓存:一次查询之后，下次就会使用该缓存，而不会进入方法内部
@CacheEvict 缓存被清除:推测场景：数据删除时候用
@CachePut 缓存被更新 ：推测场景：增加修改的时候使用

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
thank you



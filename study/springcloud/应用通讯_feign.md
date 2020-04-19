订单服务order调用商品服务product，使用feign时，需要写一个product的客户端，类似于jdbc对于mysql，如下
    @FeignClient("product")
    public interface StoreClient {
        @RequestMapping(method = RequestMethod.GET, value = "/stores")
        List<Store> getStores();
        
        @RequestMapping(method = RequestMethod.POST, value = "/stores/{storeId}", consumes = "application/json")
        Store update(@PathVariable("storeId") Long storeId, Store store);
    }
    
**FeignClient("prodcut")** 中的product私理解为服务名称,接口中方法的注解的值，就是服务product对外提供的接口。

根据product通过注册中心，可以拿到提供该服务的地址列表，加上这些接口就是完整的可访问的url了，

使用这些客户端的时候，需要添加注解，如@EnableFeignClients，见下面代码

  @SpringCloudApplication
  @EnableFeignClients
  public class Application {
      public static void main(String[] args) {
          SpringApplication.run(Application.class, args);
      }
  }

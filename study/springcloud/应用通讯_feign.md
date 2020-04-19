## feign
订单服务"order"调用商品服务"product"，使用feign时，需要写一个product的客户端，类似于jdbc对于mysql，如下
    @FeignClient("product")
    public interface StoreClient {
        @RequestMapping(method = RequestMethod.GET, value = "/stores")
        List<Store> getStores();
        
        @RequestMapping(method = RequestMethod.POST, value = "/stores/{storeId}", consumes = "application/json")
        Store update(@PathVariable("storeId") Long storeId, Store store);
    }
    
**FeignClient("prodcut")** 中的product私理解为服务名称,根据product通过注册中心，可以拿到提供该服务的地址列表，加上方法注释，可以获得完整的服务器url，所以我们看起来是调用方法，背后转化成了url.这些都是feign做的，

零外一些服务使用这些客户端的时候，需要添加注解，如@EnableFeignClients，见下面代码

  @SpringCloudApplication
  @EnableFeignClients
  public class Application {
      public static void main(String[] args) {
          SpringApplication.run(Application.class, args);
      }
  }

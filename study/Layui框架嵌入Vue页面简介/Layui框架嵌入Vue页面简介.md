[TOC]

# 目的

我们的目的是在我们Layui框架的项目中，部分功能页面使用Vue开发。

![](images\aim.png)

# 当前场景

我们的项目是前端使用Layui框架的前后端不分离的web项目。

![](images\前后端不分离.jpg)

前端Layui框架

![](images\Layui框架.jpg)

# 我们的迁移策略

1. 使用Vue框架开发Vue页面。

   ![](images\vue_first.png)

2. **每开发完成一个Vue页面，将它放置在原来的Layui框架中**(技术升级及成本等原因)。

   ![](images\aim.png)

3. 待到时机成熟时（Vue页面达到一定比率，或者公司决策），再统一使用Vue框架。

![](images\vue_third.png)

## 我们遇到的问题

- 如何将Vue页面嵌入到Layui框架中
- 嵌入Layui框架的Vue页面，能否平滑的切回到Vue框架中

## 容器

此处容器指的是可以让事物生存的环境。如：java web项目生存在tomcat下，人类生活在陆地上。

问：每个事物都有自己的生存环境，如果更换环境，那要如何生存。比如人想要去海里生存怎么能做到呢？

答：潜水服。我们需要一种特殊的容器，它可以容纳事物，也可以生存在更换后的环境。姑且称它为虚拟环境或者适配器（有待商榷）。

![](images\air_container.jpg)

我们假设大海是Layui框架，海绵宝宝和派大星是两个功能页面，珊迪是Vue功能页面，我们只要给珊迪开发一个潜水服。

## 如何解决我们的问题

我们需要一个Vue功能页面容器（或者叫做适配器，珊迪的潜水服）

![](images\container.png)

Vue页面容器的两个作用：

- Vue功能页面能在它里面正常运行
- 这个容器可以在Layui中打开

简单说就是使得原Layui框架可以打开Vue页面

![](images\final.png)

### Vue功能页面容器的实现

Vue框架就是Vue页面的容器（我们通过浏览器可以打开功能页面），我们要做的是在**不修改Vue功能页面的前提下**(因为之后还要迁回到Vue框架)，对Vue框架的修改以及裁剪

修改及裁剪内容：

修改的内容：身份认证、授权等

裁剪的内容：左侧菜单列表、websocket通信支持等

### 这个容器可以在Layui中打开

Layui可以通过iframe访问页面，所以只要适配器可以通过浏览器打开即可。

# Vue框架的修改及裁剪

![](images\edward.jpg)

## 修改：

### 身份认证：

修改方式：我们的web项目本身使用cookie存储权限信息的，我们沿用了cookie存储替代了Vue框架中地身份认证方式。

优点：原有项目改动小。每一个在Vue页面中中访问后端的请求都会自动携带cookie。

缺点：跨域问题，Vue前端项目和我们的web项目只能部署在同一台服务器。cookie的同源策略不在乎协议、端口（有断章取义嫌疑，[原文](https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy#%E8%B7%A8%E6%BA%90%E6%95%B0%E6%8D%AE%E5%AD%98%E5%82%A8%E8%AE%BF%E9%97%AE)）。当前如果我们只有少量Vue开发页面，暂时没有必要单独申请服务器。

规划：待到使用Vue框架时候，我们可以修改身份认证方式。

### 授权：

修改方式：当前我们采用的框架是支持一定授权的，暂未修改。

## 裁剪的内容

### 左侧菜单列表、websocket通信支持：

修改方式：我们采用的框架中，有属性设置是否展示菜单，websocket功能也依据此属性控制。将它设置为关闭即可。

优点：简单

缺点：框架中一些没必要的功能可能还在，只是看不到。

# 请求的调用方式

我们的web项目和Vue项目是单独发布的，虽然我们将Vue页面嵌入到了Layui框架中，但是Vue的访问地址可能不是同源的。

一般情况下，我们可能会使用tomcat这两个项目，此时它们是同源的。

![](images\request.png)

# 其它，待续……

## 跨域

由于VUE前端是单独发布的，所以服务器要允许http协议和websocket协议跨域。

### 跨域简介

`跨源资源共享` ([CORS](https://developer.mozilla.org/zh-CN/docs/Glossary/CORS))（或通俗地译为跨域资源共享）是一种基于 [HTTP](https://developer.mozilla.org/zh-CN/docs/Glossary/HTTP) 头的机制，该机制通过允许服务器标示除了它自己以外的其它 [origin](https://developer.mozilla.org/zh-CN/docs/Glossary/Origin)（域，协议和端口），使得浏览器允许这些 origin 访问加载自己的资源。

## 验证方式

### 两个前端如何如何鉴权？

cookie共享

最终系统部署后，我们有两个前端地址，一个是原来的地址，一个是vue发布后的地址。

原地址用户登录后，会将token(sessionId)存储在cookie中，vue如果获取这个token呢?

- 使用非cookie方式
- ：将vue和原系统发布在同一个服务器即可.

同源定义：如果两个 URL 的 [protocol](https://developer.mozilla.org/zh-CN/docs/Glossary/Protocol)、[port (en-US)](https://developer.mozilla.org/en-US/docs/Glossary/Port) (如果有指定的话) 和 [host](https://developer.mozilla.org/zh-CN/docs/Glossary/Host) 都相同的话，则这两个 URL 是*同源*。


# nginx简介

官网：https://nginx.org/

[TOC]



## nginx安装

yum install nginx

## nginx常用命令

| 名称             | 命令                   | 备注                                |
| ---------------- | ---------------------- | ----------------------------------- |
| 查看版本         | nginx -v               |                                     |
| 查找安装路径     | find / -name nginx     | 配置文件一般在/etc/nginx/nginx.conf |
| 启动             | systemctl start nginx  |                                     |
| 查看状态         | systemctl status nginx |                                     |
| 重新加载配置文件 | nginx -s reload        | 修改配置文件后，运行它，立刻生效    |
| 快速关闭         | nginx -s stop          |                                     |
| 优雅的关闭       | nginx -s quit          |                                     |

## nginx部分指令解释

root：设置请求根目录. 如下配置，请求/fptxl.html 的返回文件为/data/fptxl/fptxl.html。

> ```
> location /fptxl.html {
>     root /data/fptxl;
> }
> ```



## nginx静态内容服务

web服务一个重要任务就是提供文件服务，图片，html页面

我们举个例子：

​	我们创建一个静态页面，里面有一张图片，这需要我们在配置文件中设置http中server中的location

1. linux下文件结构

   ```
   [root@VM-0-5-centos data]# pwd
   /data
   [root@VM-0-5-centos data]# tree
   .
   └── fptxl
       ├── fptxl.html
       └── images
           └── rich_woman.jpg
   ```

   

2. 静态文件代码

   [静态内容代码](https://github.com/coldbloodanimal/fptxl.git)

3. nginx配置

   nginx.conf 文件的http-->server->location配置

   ```
   	# 前缀"/"会匹配所有请求，如果有多个location匹配一个请求，则选择前缀最长的
   	location / {
   		#每个请求加上root的地址，就是文件在服务器的真实地址
   		root /data/fptxl;
   		#默认打开网页
   		index fptxl.html
   	}
   	
   ```

   每次更改配置文件后，记得重新加载配置文件

   ```
   nginx -s reload
   ```

   ## nginx简单代理服务

   nginx一个常用功能是当作代理服务器，接受请求，传给被代理服务器，获取响应，返回给客户端。

   ```
   +------------+                +------------+                +----------------+
   |            | +------------> |            | +------------> |                |
   | http client|                |proxy server|                | proxied server |
   |            | <------------+ |            | <------------+ |                |
   +------------+                +------------+                +----------------+
      browser                        nginx                          tomcat
   ```
   
   我们举个例子：配置一个简单的[代理服务](##nginx简单代理服务)用于处理后端请求，[nginx静态内容服务](##nginx静态内容服务)用于处理前端请求，这可以作为一种前后端分离配置方案。
   
   
   
   1.nginx下文件结构
   
   



问题：前端请代理中的后端请求地址，为什么不直接使用后端服务器的地址？

参考答案：后端负载均衡，

## nginx部分指令解释

root：请求的根目录，一个完整的请求是root+uri，如果当前指令中没有root,


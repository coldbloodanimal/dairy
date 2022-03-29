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

## nginx静态内容服务

web服务一个重要任务就是提供文件服务，图片，html页面

我们举个例子：

​	我们创建一个静态页面，里面有一张图片，这需要我们在配置文件中设置http中server中的location

- 项目文件结构

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

   

- 静态文件代码

   [静态内容代码](https://github.com/coldbloodanimal/fptxl.git)

- nginx配置

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

   **nginx发布静态资源，最终的访问路径是root+location**

   每次更改配置文件后，记得重新加载配置文件
   
   ```
   nginx -s reload
   ```

## nginx简单代理服务(前后端分离例子)

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

   1. 代理服务nginx配置

      nginx.conf 文件的http-->server->location配置

      		location /fptxl/ {
      			#每个请求都会加上root对应的地址，就是文件在服务器的真实地址
      			proxy_pass http://localhost:8080/;
      		}

      **proxy_pass**:一个请求如果匹配了location，匹配location的部分会被替换成proxy_pass,比如一个公网请求http://fptxl.com/fptxl/actuator/env，"/fptxl/actuator/env"中匹配前缀"/fptxl/"的部分会被替换为http://localhost:8080/，最终地址为：“http://localhost:8080/”+“actuator/env”

      **通过含有proxy_pass的location，可以将多个不同ip:port的服务映射成同一个ip:port下不同路径的服务**

      占用8080端口的是我们在服务器运行的一个后端服务，用于查看后端服务

   2. 静态内容配置

      本例子中可以直接使用上方[nginx静态内容服务](##nginx静态内容服务)

      静态文件位置：

      ```
      [root@VM-0-5-centos data]# tree
      .
      ├── fptxl
      │   ├── fetch.html
      │   ├── fptxl.html
      ```
   
      fetch.html内容 [访问地址](http://fptxl.com/fetch.html)

      ```
      <button>get instance port</button>
      <script>
          const btn = document.querySelector('button');
          btn.addEventListener('click', () => {
          	//https访问的时候此处会报错，应当使用
          	//var url=window.location.protocol+"//fptxl.com/fptxl/actuator/env";
              var promise=fetch('http://fptxl.com/fptxl/actuator/env').then(response => response.json())
                  .then(data => {
                      var port=data["propertySources"][0]["properties"]["local.server.port"]["value"];
                      let pElem = document.createElement('p');
                      pElem.textContent = '内部服务端口为：'+port;
                      document.body.appendChild(pElem);
                  })
          });
      </script>
      ```

      nginx完整配置
      
      ```
      		# 前缀"/"会匹配所有请求，如果有多个location匹配一个请求，则选择前缀最长的
      		location / {
      			#每个请求都会加上root对应的地址，就是文件在服务器的真是地址
      			root /data/fptxl;
      			#默认打开网页
      			index fptxl.html;
      		}
      		
      		# 如果有多个location匹配一个请求，则选择前缀最长的
      		location /fptxl/ {
      			#每个请求都会加上root对应的地址，就是文件在服务器的真是地址
      			proxy_pass http://localhost:18080/;
      		}
      ```

## 负载均衡

### 简介

负载均衡通过多个应用实例来实现优化资源利用，最大化吞吐量，提高容错。

nginx可以提供有效的HTTP负载均衡来分发信息到多个web应用的应用服务器中来提高性能、吞吐量、可靠性。

### 负载均衡的机制

nginx提供以下几种负载均衡机制

轮询：以轮询的方式依次将请求调度不同的服务器，即每次调度执行i = (i + 1) mod n，并选出第i台服务器。

最少连接：下一个请求会被指派给连接数最少的服务器

ip哈希：使用基于客户端ip地址的哈希函数来决定使用哪台服务器提供服务

### 默认的负载均衡配置（轮询）

最简单的负载均衡配置：

```
http {
	#名称尽量不要使用特殊字符，避免使用了nginx关键字
	upstream backend {  #服务器集群名字   
		server    localhost:8080;#服务器配置    
		server    localhost:18080;#服务器配置  
    }

    server {
        listen 80;

		location /fptxl/ {
            proxy_pass http://backend/;
        }
    }
}
```

负载均衡默认使用**轮询**，nginx反向代理可以给HTTP, HTTPS, FastCGI, uwsgi, SCGI, memcached, and gRPC提供负载均衡。

### 最小连接数负载均衡

当有些任务耗时较长时，比较适合用最少连接数的方式。

黑话：当有些人任务过多时，给他少派发一些工作。

```
http {
	#名称尽量不要使用特殊字符，避免使用了nginx关键字
	upstream backend {  #服务器集群名字  
    	least_conn;
		server    localhost:8080;#服务器配置    
		server    localhost:18080;#服务器配置  
    }
}
```

### 会话持久化（ip-hash）

轮询和最小连接数会使得后续的请求被派发给不同的服务，ip-hash使用客户的ip地址作为哈希的key值来决定那个服务器提供服务，如果服务器没有做会话共享，可以使用这种方式。

```
#名称尽量不要使用特殊字符，避免使用了nginx关键字
upstream backend {  #服务器集群名字  
	ip_hash;
	server    localhost:8080;#服务器配置    
	server    localhost:18080;#服务器配置  
}
```

### 权重负载均衡

权重可以使用在轮询、最小连接数、ip-hash中

```
upstream backend {  #服务器集群名字  
	server    localhost:8080 weight=3; #服务器配置    
	server    localhost:18080;#服务器配置  
}
```

这个配置，当有四个请求的时候，第一个服务器处理3个，第二个处理1个

## 配置https服务

### 生成https证书方式

[点我获得](https://segmentfault.com/a/1190000004976222) 生成https证书方式的详细信息  

```
sudo mkdir /etc/nginx/ssl
sudo openssl req -x509 -nodes -days 36500 -newkey rsa:2048 -keyout /etc/nginx/ssl/nginx.key -out /etc/nginx/ssl/nginx.crt
```

### 配置https服务

ssl参数必须启用，证书和私匙必须指定，配置如下

```
    # HTTPS server
    #
    server {
        listen       443 ssl;
        ssl_certificate /etc/nginx/ssl/nginx.crt;
        ssl_certificate_key /etc/nginx/ssl/nginx.key;

		ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;
		ssl_ciphers         HIGH:!aNULL:!MD5;

		# 前缀"/"会匹配所有请求，如果有多个location匹配一个请求，则选择前缀最长的
		location / {
			#每个请求都会加上root对应的地址，就是文件在服务器的真是地址
			root /data/fptxl;
			#默认打开网页
			index fptxl.html;
		}
		
		location /fptxl/ {
            proxy_pass http://backend/;
        }
    }
```

**注意**：编码时候，尽量不要指定协议头，可以从通过js（window.location.protocol）中获取，减少http转化为https时协议不一致引起的异常。

## websocket配置

HTTP 1.1 支持将HTTP/1.1协议转化为 WebSocket

nginx配置如下

```
http {
    map $http_upgrade $connection_upgrade {
        default upgrade;
        ''      close;
    }

    server {
        location /chat/ {
            proxy_pass http://chat/;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection $connection_upgrade;
            proxy_read_timeout 6000;
            proxy_set_header Origin "";
        }
    }
```

## websocket的wss配置

wss与ws的区别在于一些ssl的配置，与http和https的区别一致

```
http {
    map $http_upgrade $connection_upgrade {
        default upgrade;
        ''      close;
    }

    server {
        listen       8443 ssl;
        ssl_certificate /etc/nginx/ssl/nginx.crt;
        ssl_certificate_key /etc/nginx/ssl/nginx.key;
		ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;
		ssl_ciphers         HIGH:!aNULL:!MD5;
    
        location /chat/ {
            proxy_pass http://chat/;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection $connection_upgrade;
            proxy_read_timeout 6000;
            proxy_set_header Origin "";
        }
    }
```

wss和https是可以共享443端口的。

这里不共享一个端口是因为wss的项目和https的项目是两个项目，静态文件地址不能合并在一起

## nginx部分指令解释

**location**:nginx选择location为请求提供服务，优先使用正则，其次使用最长前缀。

**root**：请求的根目录，一个完整的请求是root+uri，如果当前指令中没有root,则会使用上下文的root. 如下配置，请求/fptxl.html 的返回文件为/data/fptxl/fptxl.html。

> ```
> location / {
>  root /data/fptxl;
> }
> ```

请求"/i/top.gif"的响应是文件"/data/w3/i/top.gif",如果location没有root指令，则会使用server中配置的root指令

**proxy_pass**:设置被代理服务的协议、地址和uri，地址可以是域名或者ip和一个可选的端口

## 演示中的nginx.conf

```

#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;
	
	#websocket 配置时使用
	map $http_upgrade $connection_upgrade {
        default upgrade;
        ''      close;
    }

	#名称尽量不要使用特殊字符，避免使用了nginx关键字
	upstream backend {  #服务器集群名字  
		server    localhost:8080; #服务器配置    
		server    localhost:18080;#服务器配置  
	}
	
	#名称尽量不要使用特殊字符，避免使用了nginx关键字
	upstream talk {  #服务器集群名字  
		server    localhost:8090; #服务器配置    
		server    localhost:8091;#服务器配置  
	}
	
    server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;
		
		# 前缀"/"会匹配所有请求，如果有多个location匹配一个请求，则选择前缀最长的
		location / {
			#每个请求加上root的地址，就是文件在服务器的真实地址
			root /data/fptxl;
			#默认打开网页
			index fptxl.html;
		}
		
		location /fptxl/ {
            proxy_pass http://backend/;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }
	
	
	server {
        listen       8088;
        server_name  localhost;
		
		location / {
            proxy_pass http://talk;
			proxy_http_version 1.1;
			proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection $connection_upgrade;
			proxy_set_header Origin "";
        }

    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    server {
        listen       443 ssl;
        ssl_certificate /etc/nginx/ssl/nginx.crt;
        ssl_certificate_key /etc/nginx/ssl/nginx.key;

		ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;
		ssl_ciphers         HIGH:!aNULL:!MD5;

		# 前缀"/"会匹配所有请求，如果有多个location匹配一个请求，则选择前缀最长的
		location / {
			#每个请求加上root的地址，就是文件在服务器的真实地址
			root /data/fptxl;
			#默认打开网页
			index fptxl.html;
		}

		location /fptxl/ {
            proxy_pass http://backend/;
        }
		
    }
	
	server {
        listen       8443 ssl;
        ssl_certificate /etc/nginx/ssl/nginx.crt;
        ssl_certificate_key /etc/nginx/ssl/nginx.key;

		ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;
		ssl_ciphers         HIGH:!aNULL:!MD5;

		location / {
            proxy_pass http://talk;
			proxy_http_version 1.1;
			proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection $connection_upgrade;
			proxy_set_header Origin "";
        }
		
    }

}

```

配置文件的服务器为[www.fptxl.com](http://www.fptxl.com),可以直接访问，但由于是测试服务器，随时会失效

## 其它

问题：前端请代理中的后端请求地址，为什么不直接使用后端服务器的地址？参考答案：后端负载均衡。








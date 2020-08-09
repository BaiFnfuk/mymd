### Nginx

### 1. 什么是nginx

> nginx是一个高性能的HTTP和反向代理服务器，特点是占用内存少，并发能力强。支持最高50000个并发

### 2. 反向代理

#### 2.1 正向代理

正向代理：如果把局域网外的Internet想像成一个巨大的资源库，则局域网中的客户端要访问Internet,则需要通过代理服务器来访问，这种代理服务就称为正向代理

![image-20200806151848248](C:\Users\baifn\AppData\Roaming\Typora\typora-user-images\image-20200806151848248.png)

#### 2.2 反向代理

反向代理：客户端对代理是无感知的，因为客户端不需要配置任何配置就可以访问，我们只需要将请求发送到反向代理服务器，由反向代理服务器去选择目标服务器获取数据后，在返回给客户端，此时反向代理服务器和目标服务器对外就是一个服务，暴露的是代理服务器地址，隐藏了真实服务器ip 地址

![image-20200806152725286](C:\Users\baifn\AppData\Roaming\Typora\typora-user-images\image-20200806152725286.png)

### 3. 负载均衡

​		客户端发送多个请求到服务器，服务器处理请求，有一些可能要与数据库进行交互，服务器处理完毕后，再将结果返回给客户端。在并发请求相对较少的情况下是比较适合的，但随着信息数量的不断增长，访问和数据量的飞速增长，以及系统业务的复杂度增加，这种架构会造成服务器相应客户端的请求日益缓慢，并发量特别大的时候，还容易造成服务器直接崩溃。

​		这时集群的概念产生了，单个服务器解决不了，我们增加服务器数量，然后将请求分发到各个服务器上，将原先请求集中到单个服务器的情况，改为将请求分发到多个服务器上，将负载分发到不同的服务器，也就是我们所说的负载均衡。

![image-20200806160527071](C:\Users\baifn\AppData\Roaming\Typora\typora-user-images\image-20200806160527071.png)

### 4. 动静分离

为了加快网站的解析速度，可以把动态网页和静态页面由不同的服务器来解析，加快解析速度 ，降低原来单个服务器的压力

![image-20200806161712113](C:\Users\baifn\AppData\Roaming\Typora\typora-user-images\image-20200806161712113.png)

###  5.nginx安装

1. 官网下载nginx压缩包

2. 安装相关依赖

   - openssl
   - pcre
   - zlib

   ```shell
   yum -y install gcc zlib zlib-devel pcre-devel openssl openssl-devel
   ```

3. 解压nginx到/usr/local/目录下，其他目录也可以

4. 进入目录执行

   ```shell
   ./configure
   make
   make install
   ```

5. 启动nginx，进入/usr/local/nginx/sbin目录

   ```shell
   ./nginx
   ```

### 6. nginx常用命令

启动nginx

`./nginx`

关闭nginx

`./nginx -s stop`

重新加载nginx

`./nginx -s reload`

### 7. nginx的配置文件

nginx的配置文件是/usr/local/nginx/conf/nginx.conf

配置文件分了三部分

全局块：

从配置文件开始到events块之间的内容，主要会设置一些影响nginx服务器整体运行的配置指令，主要包括配置运行Nginx服务器的用户（组），允许生成的work process数，进程PID存放路径，日志存放路径和类型以及配置文件的引入等。

`worker_processes  1;`nginx服务器并发处理服务的关键配置，worker processes值越大，可以支持的并发处理量也越多，但是会受到硬件，软件等设备的制约



events块：

events块涉及的指令主要影响nginx服务器与用户的网络连接，常用的设置包括是否开启对多 work process下的网络连接进行序列化，是否允许同时接收多个网络连接，选择那种驱动模型来处理连接请求，每个work process可以同时支持的最大连接数等

`worker_connections  1024;`表示每个work process支持最大连接数为1024

这部分的配置对nginx的性能影响较大，在实际中应该灵活配置



http块：

这是nginx中配置最频繁的部分，代理，缓存和日志定义等绝大多数功能和第三方模块的配置都在这里，需要注意的是，http块也可以包括http全局块，server块。

1. http全局块
2. server块

### 8. 反向代理实现

```shell
 server {
          listen       80;
          server_name  192.168.1.102;
  
          location / {
              root   html;
              proxy_pass   http://127.0.0.1:8080;
              index  index.html index.htm;
          }
}
```

访问http://192.168.1.102访问tomcat

多端口反向代理：

启动两个tomcat一个端口8080，一个端口8081

分别在两个tomcat里创建tomcat0,tomcat1两个目录，里面随便写一个html文件

配置nginx

```shell
server {
    listen       80;
    server_name  192.168.1.102;
 
    location / {
        root   html;
        index  index.html index.htm;
    }
 
    location ~ /tomcat0/ {
        proxy_pass   http://192.168.1.102:8080;
    }
    location ~ /tomcat1/ {
        proxy_pass   http://192.168.1.102:8081;
	}
}
```

访问http://192.168.1.102/tomcat0和http://192.168.1.102/tomcat1

### 9. 负载均衡实现

配置文件：

```shell
http{
	upstream myserver {
		server 192.168.1.102:8080 weight=1 fail_timeout=3;
		server 192.168.1.102:8081 weight=1 fail_timeout=3;
	}
	
	server{
		listen	80;
		server_name  192.168.1.102;
		location / {
			proxy_pass   http://myserver;
		}
	}
}

```

负载均衡策略：
| 方式				|				|
| ------------------ | --------------- |
| 轮询               | 默认方式        |
| weight             | 权重方式        |
| ip_hash（session一致性） | 依据ip分配方式  |
| least_conn         | 最少连接方式    |
| fair（第三方）     | 响应时间方式    |
| url_hash（第三方） | 依据URL分配方式 |

### 10. 动静分离实现

```shell
location /www/ {
	root /static/;
	index index.html index.jsp
}

location /image/ {
	root /static/;
	autoindex on; // 列出目录内空
}

[root@localhost static]# ls
image  www
```

### 11. 高可用集群

![image-20200807173957141](C:\Users\baifn\AppData\Roaming\Typora\typora-user-images\image-20200807173957141.png)

环境搭建：

1. 两台nginx服务器
2. keepalived ` yum -y install keepalived`
3. 需要虚拟ip


## DOCKER

### 安装Docker

- 删除本机原有的docker

  ```shell
  sudo yum remove docker \
                    docker-client \
                    docker-client-latest \
                    docker-common \
                    docker-latest \
                    docker-latest-logrotate \
                    docker-logrotate \
                    docker-engine
  ```

- 安装工具库

  ```shell
  sudo yum install -y yum-utils
  sudo yum-config-manager \
      --add-repo \
      http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
  ```

- 安装

  ```shell
  sudo yum install docker-ce docker-ce-cli containerd.io
  ```

- 启动docker

  ```shell
  sudo systemctl start docker
  ```

### 基本命令

#### docker images

查看安装的镜像

```shell
Usage:	docker images [OPTIONS] [REPOSITORY[:TAG]]

List images

Options:
  -a, --all             # 列出所有安装的镜像
  -f, --filter filter   # 设置过滤条件
  -q, --quiet           # 查看镜像的ID
  
docker images -a		# 列出所有安装的镜像
docker images -aq		# 列出所有安装的镜像的ID
```

#### docker pull

安装镜像

```shell
Usage:	docker pull [OPTIONS] NAME[:TAG|@DIGEST]

Pull an image or a repository from a registry

Options:
  -a, --all-tags                # 下载所有版本的镜像
  
docker pull mysql:5.7			# 下载5.7版本的mysql镜像
```

#### docker search

搜索镜像

```shell
Usage:	docker search [OPTIONS] TERM

Search the Docker Hub for images

Options:
  -f, --filter filter   # 设置过滤条件
 
docker search mysql -f STARS=3000      # 搜索收藏数在3000以上的mysql镜像
```

#### docker rmi

删除镜像

```shell
Usage:	docker rmi [OPTIONS] IMAGE [IMAGE...]

Remove one or more images

docker rmi mysql(镜像ID也可以)	# 删除mysql镜像
docker rmi -f $(docker images -aq) # 删除所有镜像
```

#### docker run

创建并启动一个容器

```shell
Usage:	docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

Run a command in a new container

# 参数说明
--name="Name"  容器名字，用来区分容器
-d			   后台方式运行
-it			   使用交互方式进行，进入容器查看内容
-p(小写)	  指定容器的端口 -p 8080:8080
	-p 主机端口：容器端口（常用）
	-p 容器端口
-P(大写)		  随机容器端口

docker run centos -it /bin/bash		#进入centos容器
```

#### docker ps

列出所有运行的容器

```shell
docker ps	# 列出所有正在运行的容器
docker ps -a  # 列出所有运行过的容器，包括已经停止的容器
docker ps -q  # 只显示容器的ID 

exit	# 直接停止容器并退出
ctrl + p + q # 退出容器但不停止容器
```

#### docker start

启动一个容器

```shell
docker start 容器ID	# 启动当前容器
docker restart 容器ID	# 重启当前容器
docker kill	容器ID	# 强制停止当前容器
```

#### docker stop

停止容器

```shell
docker stop 容器ID
```

#### docker rm

删除容器

```shell
docker rm 容器ID		# 删除容器, 不能删除正在运行的容器，要强制删除要加上-f
docker rm -f (docker ps -aq)	# 删除全部容器
```

#### docker stats

查看docker cpu状态

```shell
docker stats
```



### 常用的其他命令

#### 后台启动容器

```shell
docker run -d centos
# 问题docker ps发现centos停止了
# 常见的坑，docker容器使用后台运行，就必须要有一个前台进程，docker发现没有应用，就会自动停止
```

#### 查看日志

```shell
docker logs -tf --tail num 容器ID # num 要显示的日志条数
```

#### 查看容器中的进程信息

```shell
docker top 容器ID
```

#### 查看容器的元数据

```shell
docker inspect 容器ID
```

#### 进入当前正在运行的容器

```shell
# 方式1
docker exec -it 容器ID bashshell
docker exec -it 79d60719b6a1 /bin/bash

# 方式2
docker attach 容器ID bashshell
docker attach 79d60719b6a1 /bin/bash

# docker exec 进入容器后开启一个新和终端，可以在里面操作（常用）
# docker attach 进入容器正在执行的终端，不会启动新的终端
```

#### 从容器内拷贝文件到主机上

```shell
docker cp 容器ID:容器路径 目的主机路径

docker cp 79d60719b6a1:/home/test.java /home
```

### centos7 开放端口

```shell
# 开放端口
firewall-cmd --zone=public --add-port=5672/tcp --permanent   # 开放5672端口
# 禁止端口
firewall-cmd --zone=public --remove-port=5672/tcp --permanent  #关闭5672端口
firewall-cmd --reload   # 配置立即生效
# 查看防火墙所有开放的端口
firewall-cmd --zone=public --list-ports

```

### 安装nginx

````shell
docker search nginx
docker pull nginx
docker run -d --name nginx01 -p 3344:80 nginx
````

### 安装tomcat

```shell
docker search tomcat
docker pull tomcat:9.0
docker run -d --name tomcat01 -p 8080:8080 tomcat:9.0

# 安装的只是最小的环境，需要进入到容器把webapps.dist目录里的内容复制到webapps目录下，要不然不能访问到tomcat的首页
```

### 安装elasticsearch

```shell
docker search elasticsearch
docker pull elasticsearch
docker run -d --name elasticsearch01 -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch

# 跑elasticsearch会非常卡，这个程序对内存需要非常大	阿里云的服务器1核2G不够跑es
# 需要加上内存限制， 修改配置文件， -e 环境配置修改
docker run -d --name elasticsearch02 -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xmx512m" elasticsearch

```

### 可视化

- portainer（先用这个）
- Rancher（CI/CD再用）

#### 什么是 portainer？

Docker图形化界面管理工具！提供一个后台面板供我们操作！

安装命令：

```shell
docker run -d -p 8088:9000 \
--restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
```

访问测试：

```shell
localhost:8088
用户名：admin
密码： fnfuk2020
```

#### 提交容器

```shell
docker commit 提交容器成为一个新的副本
docker commit -m="提交的描述信息" -a="作者" 容器ID 目标镜像名：[TAG]
```

### 容器数据卷

#### 什么是容器数据卷

如果数据都在容器中，那么我们容器删除，数据就会丢失。

容器之间有一个数据共享的技术。Docker容器中产生的数据，同步到本地这就是卷技术。目录的挂载，将我们容器内的目录挂载到linux上面。

**总结一句话：容器的持久化和同步操作！容器间也是可以数据共享的。**

#### 使用数据卷

```shell
# 方式1：直接使用命令来挂载 -v
# 方式2：采用DockerFile
docker run -it -v 主机目录：容器内目录

docker run -it -v /home/test:/home centos /bin/bash
```

#### 数据卷容器

多个mysql同步数据，通过--volumes-from 容器名 的方式进行数据同步

```shell
docker run -it --name 容器名2 --volumes-from 容器名1 镜像名
```



### DockerFile

#### 什么是DockerFile

DockFile就是用来构建docker镜像的构建文件。命令脚本。通过脚本可以生成镜像

```shell
# 创建一个dockfile文件，名字可以随意，建议 Dockerfile
# 文件中的内容 指令都是大写 参数
FROM centos # 基于centos
VOLUME ["VOLUME01", "VOLUME02"] # 匿名挂载的两个目录
CMD echo "----end----"
CMD /bin/bash

# 这里的每个命令，就是镜像的一层
```

构建步骤：

1. 编写一个dockerfile文件
2. docker build构建成为一个镜像
3. docker run 运行镜像
4. docker push发布到dockerhub或阿里云 

#### DockFile构建过程

基础知识：

1. 每个保留关键 字（指令）都必须是大写字母
2. 执行从上到下顺序执行
3. #表示注释
4. 每一个指令都会创建提交一个新的镜像层，并提交

DockerFile: 构建文件，定义一切步骤，源代码

DockerImages: 通过DockerFile构建生成的镜像，最终发布和运行的产品。

Docker容器： 容器就是镜像运行起来提供服务

#### DockerFile的指令

```shell
FROM	#基础镜像，一切从这里开始构建
MAINTAINER # 镜像是谁写的，姓名+邮箱
RUN		# 镜像构建的时候需要运行的命令
ADD		# 步骤，tomcat镜像，这个tomcat压缩包！添加内容
WORKDIR # 镜像的工作目录
VOLUME  # 挂载目录
EXPOSE	# 暴露端口
CMD		# 指定这个容器启动的时候要运行的命令,不可以追加命令
ENTRYPOINT	# 指定这个容器启动的时候要运行的命令，可以追加命令
ONBUILD	# 当构建一个被继承 DockerFile这个时候就会运行ONBUILD的指令。触发指令
COPY	# 类似ADD，将我们的文件拷贝到镜像中
ENV		# 构建的时候设置环境变量

```

#### 实战测试

1. 编写DockerFile文件

   ```shell
   FROM centos
   
   MAINTAINER Bai 15950140@qq.com
    
   ENV MYPATH /usr/local
   WORKDIR $MYPATH
   
   RUN yum -y install vim
   RUN yum -y install net-tools
   EXPOSE 80
   
   CMD echo $MYPATH
   CMD echo "----end---"
   CMD /bin/bash
   ```

2. 构建镜像

   ```shell
   docker build -f ./mydockerfile -t mycentos:0.1 .
   # 最后的点不要漏了
   ```

   

### Docker 网络

#### 自定义模式

查看所有docker网络

```shell
docker network ls
```

网络模式

brdge: 桥接模式 docker(默认)

none: 不配置网络

host：和宿主机共享网络

container: 容器网络连通！（用得少，局限大）

测试：

```shell
# 处定义网络
# --driver bridge 桥接
# --subnet 192.168.0.0/16 子网地址
# --gateway 192.168.0.1 网关
docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet

# 使用自定义网络
docker run -d -p 8080:8080 --name tomcat01 --net mynet tomcat
```

使用自定义网络容器可以互相ping

#### 网络连通

容器与网络连接,两个docker间的通信

```shell
docker network connect 网络名 容器名

docker network connect mynet tomcat01
```



### 安装MySQL

思考：MySQL的数据持久化问题！

```shell
docker pull mysql
# 运行时需要做数据挂载
# 安装启动mysql, 需要配置密码
docker run -d -p 3306:3306 -v /root/mysql/conf:/etc/mysql/conf.d -v /root/mysql/data/:/var/lib/mysql/ -e MYSQL_ROOT_PASSWORD=root --name mysql01 mysql
```

#### 具名挂载，匿名挂载

```shell
docker run -v 卷名：容器路径	# 具名挂载 会在/var/lib/docker/volumes目录下生成相应的卷名目录
docker run -v 容器路径		  # 匿名挂载
docker run -v 主机路径：容器路径	# 两个路径都是绝对路径
```

### redis集群

分片+高可用+负载均衡

```shell
# 创建redis网络
docker network create redis --subnet 172.38.0.0/16

# 创建redis的挂载配置文件和数据保存目录
for port in 1 2 3 4 5 6 \
do \
mkdir /root/redis/node-${port}/conf
touch /root/redis/node-${port}/conf/redis.conf
cat << EOF > /root/redis/node-${port}/conf/redis.conf
  port 6379
  bind 0.0.0.0
  cluster-enabled yes
  cluster-config-file nodes.conf
  cluster-node-timeout 5000
  cluster-announce-ip 172.38.0.16
  cluster-announce-port 6379
  cluster-announce-bus-port 16379
  appendonly yes
EOF
done

# 启动redis
docker run -d -p 6371:6379 -p 16371:16379 --name redis-1 \
-v /root/redis/node-1/data:/data \
-v /root/redis/node-1/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.11 redis:6.0 redis-server /etc/redis/redis.conf

docker run -d -p 6372:6379 -p 16372:16379 --name redis-2 \
-v /root/redis/node-2/data:/data \
-v /root/redis/node-2/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.12 redis:6.0 redis-server /etc/redis/redis.conf

docker run -d -p 6373:6379 -p 16373:16379 --name redis-3 \
-v /root/redis/node-3/data:/data \
-v /root/redis/node-3/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.13 redis:6.0 redis-server /etc/redis/redis.conf

docker run -d -p 6374:6379 -p 16374:16379 --name redis-4 \
-v /root/redis/node-4/data:/data \
-v /root/redis/node-4/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.14 redis:6.0 redis-server /etc/redis/redis.conf

docker run -d -p 6375:6379 -p 16375:16379 --name redis-5 \
-v /root/redis/node-5/data:/data \
-v /root/redis/node-5/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.15 redis:6.0 redis-server /etc/redis/redis.conf

docker run -d -p 6376:6379 -p 16376:16379 --name redis-6 \
-v /root/redis/node-6/data:/data \
-v /root/redis/node-6/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.16 redis:6.0 redis-server /etc/redis/redis.conf

# 进入redis
docker exec -it redis-1 /bin/bash
# 创建redis集群
redis-cli --cluster create 172.38.0.11:6379 172.38.0.12:6379 172.38.0.13:6379 172.38.0.14:6379 172.38.0.15:6379 172.38.0.16:6379 --cluster-replicas 1

# 连接集群
redis-cli -c
```


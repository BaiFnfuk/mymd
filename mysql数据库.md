## MySQL

### 1. CentOS7 MySQL安装

- 下载[MySQL5.7](https://downloads.mysql.com/archives/community/ )**RPM Bundle**压缩包。

- 将压缩包放到centOS7的/usr/local/src目录下，其他目录也可以。

- 一般centOS7默认安装了mariadb，把它卸载

  ```bash
   rpm -qa|grep mariadb
   rpm -e --nodeps mariadb-libs-5.5.65-1.el7.x86_64
  ```

- 安装mysql5.7所需要的依赖

  ```bash
  yum install libaio
  yum install perl
  yum install net-tools
  ```

- 解压mysql5.7压缩包

  ```bash
  tar -xvf mysql-5.7.29-1.el7.x86_64.rpm-bundle.tar
  ```

- 安装mysql5.7

  ```bash
  rpm -ivh mysql-community-common-5.7.29-1.el7.x86_64.rpm
  rpm -ivh mysql-community-libs-5.7.29-1.el7.x86_64.rpm
  rpm -ivh mysql-community-client-5.7.29-1.el7.x86_64.rpm 
  rpm -ivh mysql-community-server-5.7.29-1.el7.x86_64.rpm
  ```

- 启动mysqld服务

  ```bash
  systemctl start mysqld
  ```

- 修改密码

  ```bash
  # 查看临时密码
  => grep password /var/log/mysqld.log
  2020-07-10T02:28:52.187635Z 1 [Note] A temporary password is generated for root@localhost: Z;oZ>IJ9ct/1
  
  # 用临时密码登录mysql
  mysql -uroot –p
  
  # 修改新密码
  # 若想修改成简单的密码，如：123456这种
  set global validate_password_policy=0;
  set global validate_password_length=4;
  ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';
  ```

- 开启远程连接，如navicat等软件连接mysql

  ```bash
  GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;
  ```

- 配置mysql的配置文件my.cnf

  ```bash
  # 在[mysqld]下面配置几行
  port=3306			# 端口配置
  lower_case_table_names=1       #配置表名不区分大小写 1：不区分大小写 0：区分大小写  这行必须配置 默认表名是区分大小写的，不利于开发
  character-set-server=utf8          #设置为默认编码为utf8
  init_connect='SET NAMES utf8'
  max_connections=1024             #设置最大连接数
  
  vim /etc/my.cnf
  
  ```

- 开放端口

  ```bash
  # 开放端口
  firewall-cmd --zone=public --add-port=3306/tcp --permanent   # 开放5672端口
  # 禁止端口
  firewall-cmd --zone=public --remove-port=3306/tcp --permanent  #关闭5672端口
  firewall-cmd --reload   # 配置立即生效
  # 查看防火墙所有开放的端口
  firewall-cmd --zone=public --list-ports
  ```

### 2. 连接数据库

1. 连接数据库

   ```sql
   mysql -u root -p -- -u:用户名 -p:密码
   ```

2. 数据库语言

   - DDl 数据库定义语言
   - DML 数据库操作语言
   - DQL 数据库查询语言
   - DCL 数据库控制语言

### 3. 操作数据库

#### 3.1 数据库(了解)

1. 创建数据库

   ```sql
   CREATE DATABASE IF NOT EXISTS westos charset=utf8; --IF NOT EXISTS可选
   ```

2. 删除数据库

   ```sql
   DROP DATABASE IF NOT EXISTS westos;
   ```

3. 使用数据库

   ```sql
   USE `westos`; --此处的`是tal键上的那个键。如果你的表名是一个特殊字符，就需要带``
   ```

4. 查看数据库

   ```sql
   SHOW DATABASES;
   ```

#### 3.2 数据库的列类型

> 数值

- tinyint	          十分小的数据               1个字节
- smallint	        较小的数据                   2个字节
- mediumint       中等大小的数据           3个字节
- `int	   标准整数	       4个字节 (常用)  对应java的int`
- big                     较大的数据                   8个字节                   对应java的long
- float                   浮点数                           4个字节                   对应java的float
- double                浮点数                           8个字节 （精度问题） 对应java的double
- decimal              字符串形式的浮点数 ，金融计算的时候，一般是使用decimal

> 字符串

- char                   字符串固定大小0~255
- `varchar   可变字符串0~65535（常用）对应java的String` 
- tinytext              微型文本 2^8-1 
- `text      文本中2^16-1    保存大文本`

> 时间日期

- date         YYYY-MM-DD,日期格式
- time         HH:mm:ss,日期格式
- `datetime    YYYY-MM-DD  HH:mm:ss最常用的时间格式`
- `timestamp  时间戳，1970.1.1到现在的毫秒数，也较为常用`
- year         年份表示

> null

- 没有值，未知
- `注意，不要使用null进行运算，结果为null`

#### 3.3 数据库的字段属性（重点）

**unsinged:**

- 无符号的整数
- 声明了该列不能为负数

**zerofill:**

- 0填充的
- 不足的位数，使用0来填充，int(3), 5, 005

**AUTO_INCREMENT:**

- 通常理解为自增，自动在上一条记录的基础上+1（默认）
- 通常用来设计唯一的主键，必须是整数类型
- 可以自定义自增的起始值和步长

**NULL / NOT NULL:**

- 假设设置为not null，如果不给它赋值，就会报错
- 如果不填写值 ，默认就是null

**default:**

- 设置默认的值
- 如果不指定该列的值，就用默认值

### 4. 表操作

每一个表，都必须存在以下五个字段，未来做项目用的，表示一个记录存在的意义

- id 主键
- version 乐观锁
- is_delete 伪删除
- gmt_create 创建时间
- gmt-update 修改时间

#### 4.1 创建数据库表（重点）

```sql
CREATE TABLE IF NOT EXISTS `student`(
	`id` INT(10) NOT NULL AUTO_INCREMENT COMMENT '学号',
	`name` VARCHAR(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',
	`pwd` VARCHAR(20) NOT NULL DEFAULT '123456' COMMENT '密码',
	`sex` VARCHAR(2) NOT NULL DEFAULT '女' COMMENT '性别',
	`birthday` DATETIME DEFAULT NULL COMMENT '出生日期',
	`address` VARCHAR(100) DEFAULT NULL COMMENT '家庭地址',
	`email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱',
	PRIMARY KEY(`id`)
)ENGINE=INNODB DEFAULT CHARSET=utf8;
-- 字段名都是用的`符号，tab键上面
```

格式：

```sql
CREATE TABLE `表名`(
	字段名 类型 属性,
    字段名 类型 属性,
    字段名 类型 属性,
    字段名 类型 属性
);
```

#### 4.2 数据表的类型

- INNODB ~默认使用
- MYISAM

|            | MYISAM |   INNODB    |
| :--------: | :----: | :---------: |
|  事务支持  | 不支持 |    支持     |
| 数据行锁定 | 不支持 |    支持     |
|  外键约束  | 不支持 |    支持     |
|  全文索引  |  支持  |   不支持    |
| 表空间大小 |  较小  | 较大，约2倍 |

常规使用操作：

- MYISAM 节约空间，速度较快
- INNODB 安全性高，事务处理，多表多用户操作

> 在物理空间存在的位置

所有数据都存在var/lib/mysql目录下，一个文件夹代表一个数据库

MySQL引擎在物理文件上的区别

- InnoDB 在数据库表中只有一个*.frm文件，以及上级目录 下的ibdata1文件
- MySAM对应文件
  - *.frm	表结构的定义文件
  - *.MYD  数据文件（data）
  - *.MYI    索引文件

> 设置数据库表的字符集编码

方法1：

```sql
-- 建表时指定
charset=utf8
```

方法2：

```bash
#在/etc/my.cnf文件中添加，不推荐
character-set-server=utf8
```

默认的编码是不支持中文的。

### 5. MySQL数据管理

#### 5.1 外键（了解）

> 方式1

```sql
CREATE TABLE IF NOT EXISTS `grade`(
	`gradeid` int(10) NOT NULL AUTO_INCREMENT COMMENT '年级id',
	`gradename` VARCHAR(50) NOT NULL COMMENT'年级名称',
	PRIMARY KEY(`gradeid`)
)ENGINE=INNODB DEFAULT CHARSET=utf8;


CREATE TABLE IF NOT EXISTS `student` (
  `id` int(10) NOT NULL AUTO_INCREMENT COMMENT '学号',
  `name` varchar(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',
  `pwd` varchar(20) NOT NULL DEFAULT '123456' COMMENT '密码',
  `sex` varchar(2) NOT NULL DEFAULT '女' COMMENT '性别',
  `birthday` datetime DEFAULT NULL COMMENT '出生日期',
  `address` varchar(100) DEFAULT NULL COMMENT '家庭地址',
  `email` varchar(50) DEFAULT NULL COMMENT '邮箱',
  `gradeid` int(10) NOT NULL COMMENT '学生年级',
   PRIMARY KEY (`id`),
   KEY `FK_gradeid` (`gradeid`),
   CONSTRAINT `FK_gradeid` FOREIGN KEY (`gradeid`) REFERENCES `grade` (`gradeid`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

> 方式2

```sql
-- 创建表的时候没有外键关系
ALTER TABLE `student` ADD CONSTRAINT `FK_gradeid` FOREING KEY (`gradeid`) REFERENCES `grade` (`gradeid`);
```

不建议使用，避免数据库过多造成困扰，了解就可以

`最全实践：`

- 数据库就是单纯的表，只用来存数据，只有行（数据）和列（字段）
- 我们想使用多张表的数据，想使用外键（程序去实现）

#### 5.2 DML语言（全部记住）

DML语言：数据操作语言

- insert
- update
- delete

`delete和truncate`

- 相同点：都能删除数据，都不会删除表结构
- 不同：
  - truncate重新设置自增列，计数器会归零
  - truncate不会影响事务
  - delete计数器不会归零

#### 5.3 DQL查询数据（最重点）

DQL语言：数据查询语言

- 所有的查询操作都用它 select
- 简单的查询，复杂的查询它都能做
- `数据库中最核心的语言，最重要的语言`

#### 5.4 去重查询

```sql
select distinct studentNo from result;  
```

#### 5.5 联表查询

|    操作    |                    描述                     |
| :--------: | :-----------------------------------------: |
| inner join |      如果表中至少有一个匹配，就返回行       |
| left join  | 会从左表中返回所有的值，即使右表中没有匹配  |
| right join | 会从右表中返回所有的值 ，即使左表中没有匹配 |

`on与where`

- join on连接查询
- where 等值查询

#### 5.6 自连接

自己的表和自己的表连接。`核心，一张表拆为两张一样的表即可。`

父类：

| categoryid | categoryName |
| :--------: | :----------: |
|     2      |   信息技术   |
|     3      |   软件开发   |
|     5      |   美术技术   |

子类：

| pid  | categoryid | categoryName |
| :--: | :--------: | :----------: |
|  3   |     4      |    数据库    |
|  2   |     8      |   办公信息   |
|  3   |     6      |   web开发    |
|  5   |     7      |   美术设计   |

操作：查询父类对应的子类关系

|   父类   |   子类   |
| :------: | :------: |
| 信息技术 | 办公信息 |
| 软件开发 |  数据库  |
| 软件开发 | web开发  |
| 美术设计 |  ps技术  |

### 6. 事务

#### 6.1 什么是事务

要么都成功，要么都失败

将一组sql放在一个批次中去执行

> 事务原则：ACID原子性，一致性，隔离性，持久性     问题： 脏读，幻读，不可重复读

**原子性**

要么都成功，要么都失败

**一致性**

事务前后的数据完整性要保证一致。

**持久性**

事务一旦提交则不可逆，被持久化到数据库中。

**隔离性**

事务的隔离性是多个用户并发访问数据库时，数据库为每一个用户开启事务，不能被其他事务的操作数据所干扰。事务之间互相隔离

>隔离所导致的一些问题

**脏读**

指一个事务读取了另外一个事务未提交的数据

**不可重复读**

在一个事务内读取表中的某一行数据，多次读取结果不同。这个不一定是错误，只是某些场合不对

**虚读（幻读）**

是指在一个事务内读取到了别的事务插入的数据，也就是多了一条数据，导致前后读取不一致。

`mysql默认是开启事务自动提交的`

### 7. 索引

> MySQL官方对索引的定义为：索引（index）是帮助MySQL高效获取数据的数据结构。提取句子主干，就可以得到索引的本质：索引就是数据结构

#### 7.1 索引的分类

- 主键索引（PRIMARY KEY）
  - 唯一标识，主键不可重复，只能有一个列作为主键
- 唯一索引 （UNIQUE KEY）
  - 避免重复的列出现，唯一索引可以重复，多个列都可以标识为唯一索引
- 常规索引 （KEY/INDEX）
  - 默认的，index或key关键字来设置
- 全文索引  （FullText）
  - 在特定的数据库引擎下才有，MylSAM

#### 7.2 索引原则

- 索引不是越多越好
- 不要对经常变动的数据加索引
- 小数据量的表不需要加索引
- 索引一般加在常用来查询的字段上

### 8. 备份

为什么要备份：

- 保证重要的数据不丢失
- 数据转移

MySQL数据库备份的方式

- 直接拷贝物理文件
- 在可视化工具中手动导出
- 使用命令行导出 mysqldump

### 9. 规范数据库设计

#### 9.1 为什么需要设计

当数据库比较复杂的时候，我们就需要设计了

糟糕的数据库设计：

- 数据冗余，浪费空间
- 数据库插入和删除都会麻烦，异常
- 程序性能差

良好的数据库设计：

- 节省内存空间
- 保证数据库的完整性
- 方便我们开发系统

软件开发中，关于数据库的设计

- 分析需求：分析业务和需要处理的数据库的需求
- 概要设计：设计关系图E-R图



设计数据库的步骤：（个人博客）

- 收集信息，分析需求
  - 用户表（用户登录注销，用户个人信息，写博客，创建分类）
  - 分类表（文章分类，谁创建的）
  - 文章表（文章的信息）
  - 友链表 （友链信息）
  - 自定义表 （系统信息，某个关键的字，或者一些主字段） key:value
- 标识实体 （把需求落实）
- 标识实体之间的关系
  - user->blog 写博客
  - user->category 创建分类
  - user->user 关注
  - 

#### 9.2 三大范式

为什么需要数据规范化？

- 信息重复
- 更新异常
- 插入异常
  - 无法正常显示信息
- 删除异常
  - 丢失有效数据

> 三大范式

**第一范式**

- 要求数据库表的每一列都是不可分割的原子数据项

**第二范式**

- 前提满足第一范式
- 每张表只描述一件事情

**第三范式**

- 前提满足第一第二范式
- 确保数据表中的每一列数据都和主键直接相关，而不能间接相关。



规范数据库的设计

**规范性和性能的问题**

关联查询的表不得超过三张表

- 考虑商业化的需求，（成本，用户体验）数据库的性能更加重要
- 在规范性能的问题的时候，需要适当的考虑一下规范性
- 故意给某些表增加一些冗余的字段（从多表查询中变为单查询）
- 故意增加一些计算列（从大数据量降低为小数据量的查询：索引）

### 10. JDBC

#### 10.1 数据库驱动

为了简化开发人员对数据库的操作，提供了一个java操作数据库的规范，俗称JDBC

相关包：

- java.sql
- javax.sql
- mysql-connector-java-5.1.47.jar

步骤总结：

- 加载驱动
- 连接数据库 DriverManager
- 获得执行sql的对象 Statement， PreparedStatement
  - statement.executeQuery(); 查询操作返回ResultSet
  - statement.execute();执行任何SQL语句
  - statement.executeUpdate(); 更新，插入，删除，都是这个，返回受影响的行数
- 获得返回的结果 ResultSet
- 释放连接

`SQL注入`

sql存在漏洞，会被攻击导致数据泄露。SQl会被拼接

#### 10.2 PreparedStatement和Statement

- PreparedStatement防止sql注入的本质，把传递的参数当做字符

  
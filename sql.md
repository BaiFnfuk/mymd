## SQL

### 数据库操作

1. 查看所有数据库

   ```mysql
   show databases;
   ```

2. 查看当前使用的数据库

   ```mysql
   select database();
   ```

3. 创建数据库

   ```mysql
   create database 数据库名 charset=utf8;
   ```

4. 删除数据库

   ```mysql
   drop database 数据库名;
   ```

5. 使用数据库

   ```mysql
   use 数据库名;
   ```

6. 查看数据库中所有表

   ```mysql
   show tables;
   ```

   

### 表操作

1. 查看表结构

   ```mysql
   desc 表名;
   ```

2. 创建表结构的语法

   ```mysql
   create table 表名（
   	字段名 数据类型 可选条件
   ）;
   
   demo:
   create table classes(
   	id int unsigned auto_increment primary key not null,
       name varchar(10) 
       number varchar(20)
   );
   
   create table students(
       id int unsigned primary key auto_increment not null,
       name varchar(20) default '',
       age tinyint unsigned default 0,
       height decimal(5,2),
       gender enum('男','女','人妖','保密'),
       cls_id int unsigned default 0
   );
   ```

3. 修改表-添加字段

   ```mysql
   alter table 表名 add 列名 类型
   
   demo:
   alter table students add birthday datetime; 
   ```

4. 修改表-修改字段-生命名版

   ```mysql
   alter table 表名 change 原名 新名 类型及约束;
   
   demo:
   alter table students change birthday birth datetime not null;
   ```

5. 修改表-修改字段-不生命名版

   ```mysql
   alter table 表名 modify 列名 类型及约束;
   
   demo:
   alter table students modify birtd date not null;
   ```

6. 删除表

   ```mysql
   drop table 表名;
   
   demo：
   drop table students;
   ```

7. 查看表的创建

   ```mysql
   show create table 表名;
   
   demo:
   show create table students;查询基本使用
   ```

### 查询基本使用

1. 查询所有列

   ```mysql
   select * from 表名;
   demo：
   select * from classes;
   ```

2. 查询指定列

   ```mysql
   select 列名， 列名... from 表名;
   demo:
   select id, name from classess;
   ```

### 插入

说明：主键列是自动增长，但是在全列插入时需要占位，通常使用空值(0或者null) ; 字段默认值 default 来占位，插入成功后以实际数据为准

1. 全列插入：值的顺序与表结构字段的顺序完全一一对应

   ````mysql
   insert into 表名values(值1， 值2...);
   demo:
   insert into students values(0,’郭靖‘,1,'蒙古','2020-6-28');
   ````

2. 部分插入:值的顺序与给出的列顺序对应

   ```mysql
   insert into 表名(列1， 列2...) values(值1, 值2...); 
   demo:
   insert into students(name,hometown,birthday) values('黄蓉','桃花岛','2020-6-28');
   ```

3. 全列多行插入

   ```mysql
   insert into 表名 values(...),(...),(...);
   demo:
   insert into classes values(1,'python1'),(1,'python2');
   ```

4. 部分列多行插入

   ```mysql
   insert into 表名(列1, 列2...) values (值1, 值2...);
   demo:
   insert into students(name) values('杨康'),('杨过'),('小龙女');
   ```

### 修改

```mysql
update 表名 set 列1 = 值1， 列2 = 值2... where 条件;
demo:
update students set gender=0, hometown='北京' where id = 5;
```

### 删除

```mysql
delete from 表名 where 条件;
demo:
delete from students where id=5;
```

### as关键字

1. 使用as给字段起别名

   ```mysql
   select id as 序号, name as 名字, gender as 性别 from students;
   ```

2. 可以通过as给表起别名

   ```mysql
   select s.id, s.name, s.gender from students as s;
   ```

### 条件语句查询

where后面支持多种运算符，进行条件的处理

- 比较运算符
- 逻辑运算符
- 模糊查询
- 范围查询
- 空判断

#### 比较运算符

- 等于：=
- 大于：>
- 大于等于：>=
- 小于：<
- 小于等于：<=
- 不等于：!= 或 <>

1. 查询编号大于3的学生

   ```mysql
   select * from students where id > 3;
   ```

2. 查询编号不大于4的学生

   ```mysql
   select * from students where id <= 4;
   ```

3. 查询姓名不是“黄蓉”的学生

   ```mysql
   select * from students where name != "黄蓉";
   ```

4. 查询没有被删除的学生

   ```mysql
   select * from students where is_delete=0;
   ```

#### 逻辑运算符

- and
- or
- not

1. 查询编号大于3的女同学

   ```mysql
   select * from students where id > 3 and gender=0;
   ```

2. 查询编号小于4或没被删除的学生

   ```mysql
   select * from students where id <=4 or is_delete = 0;
   ```

3. 查询编号不在是3， 8， 10， 15的学生

   ```mysql
   select * from students where id not in(3， 8， 10， 15);
   ```

#### 模糊查询

- like
  %表示任意多个任意字符
  _表示一个任意字符

1. 查询姓黄并且名是一个字的学生

   ```mysql
   select * from students where name like "黄_";
   ```

2. 查询姓黄的学生

   ```mysql
   select * from students where name like "黄%";
   ```

#### 范围查询

范围查询分为连续范围查询和非连续范围查询

- in 表示在一个非连续的范围内

1. 查询编号是1或3或8的学生

   ```mysql
   select * from students where id in (1, 3, 8);
   ```

2. between … and …表示在一个连续的范围内

   ```mysql
   select * from students where id between 3 and 8;
   ```

3. 查询编号是3至8的男生

   ```mysql
   select * from students where (id between 3 and 8) and gender=1;
   ```

#### 空判断

判断为空

1. 查询没有填写身高的学生

   ```mysql
   select * from students where height is null;
   
   注意：
   1. null 与 ''是不同的
   2. is null 判断为空
   3. is not null 判断为非空
   ```

2. 查询填写了身高的学生

   ```mysql
   select * from students where height is not null;
   ```

3. 查询填写了身高的男生

   ```mysql
   select * from students where height is not null and gender=1;
   ```

   `优先级
   优先级由高到低的顺序为：小括号，not，比较运算符，逻辑运算符
   and比or先运算，如果同时出现并希望先算or，需要结合()使用`

### 排序

排序查询语法

```mysql
select * from 表名 order by 列1 asc|desc [, 列2 asc|desc,...];
```

`语法说明:
将行数据按照列1进行排序，如果某些行 列1 的值相同时，则按照 列2 排序，以此类推
asc从小到大排列，即升序
desc从大到小排序，即降序
默认按照列值从小到大排列（即asc关键字）`

1. 查询未删除男生信息，按学号降序

   ```mysql
   select * from students where is_delete=0 order by id desc;
   ```

2. 查询未删除学生信息，按名称升序

   ```mysql
   select * from students where is_delete=0 order by name;
   ```

3. 显示所有的学生信息，先按照年龄从大–>小排序，当年龄相同时 按照身从高–>矮排序

   ```mysql
   select * from students order by age desc, height desc;
   ```

### 分页

```mysql
select * from 表名 limit start=0,count;
```

`说明
从start开始，获取count条数据
start默认值为0
也就是当用户需要获取数据的前n条的时候可以直接写上 xxx limit n;`

`分布公式： limit( (curPage-1)*pageSize, pageSize)`

1. 查询前3行男生信息

   ```mysql
   select * from students where gender=1 limit 0, 3;
   ```

### 聚合函数

#### 总数

count(*) 表示计算总行数，括号中写星与列名，结果是相同的

1. 查询学生总数

   ```mysql
   select count(*) from students;
   ```

#### 最大值

max(列) 表示求此列的最大值

1. 查询女生的编号最大值

    ```mysql
    select max(id) from students where gender=1;
    ```

#### 最小值

min(列) 表示求此列的最小值

1. 查询未删除的学生最小编号

   ```mysql
   select min(id) from students where is_delete=0;
   ```

#### 求和

sum(列) 表示求此列的和

1. 查询男生的总年龄

   ```mysql
   select sum(age) from students where gender=1;
   ```

2. 平均年龄

   ````mysql
   select sum(age)/count(*) from students;
   ````

####  平均值

avg(列) 表示求此列的平均值

1. 查询未删除女生的编号平均值

   ```mysql
   select avg(id) from students where gender=1 and is_delete=0;
   ```

### 分组

```mysql
select gender from students group by gender;
```

`group by + group_concat()`

group_concat(字段名)根据分组结果，使用group_concat()来放置每一个分组中某字段的集合

```mysql
select gender, group_concat(name) from students group by gender;
```

`group by + having`

having 条件表达式：用来过滤分组结果
having作用和where类似，但having只能用于group by 而where是用来过滤表数据

```mysql
select gender, count(*) from students group by gender having count(*)>2;
```

### 连接查询语法

```mysql
select * from 表1 inner|left|right join 表2 on 表1.列 运算符 表2.列;
```

1. 使用内连接查询班级表与学生表

   ```mysql
   select * from students inner join classes on students.cls_id = classes.id;
   ```

2. 使用左连接查询班级表与学生表

   ````mysql
   select * from students as s left join classes as c on s.cls_id = c.id;
   ````

3. 使用右连接查询班级表与学生表

   ```mysql
   select * from students as s right join classes as c on s.cls_id = c.id;
   ```

4. 查询学生姓名及班级名称

   ```mysql
   select s.name, c.name from students as s inner join classes as c on s.clss_id = c.id;
   ```

### 子查询

`在一个 select 语句中,嵌入了另外一个 select 语句, 那么被嵌入的 select 语句称之为子查询语句,外部那个select语句则称为主查询.`

主查询和子查询的关系

- 子查询是嵌入到主查询中
- 子查询是辅助主查询的,要么充当条件,要么充当数据源
- 子查询是可以独立存在的语句,是一条完整的 select 语句

#### 标量子查询

1. 查询班级学生平均年龄,查询大于平均年龄的学生

   ````mysql
   select * from students where age > (select avg(age) from students);
   ````

#### 列级子查询

1. 查询还有学生在班的所有班级名字，找出学生表中所有的班级 id，找出班级表中对应的名字

   ```mysql
   select name from classes where id in (select cls_id from students);
   ```

#### 行级子查询

1. 查找班级年龄最大,身高最高的学生

   ```mysql
   select * from students where (height, age) = (select max(height), max(age) from students);
   ```

   




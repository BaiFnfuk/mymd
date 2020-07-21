## Mybatis

### 1. mybatis基础使用

#### 1.1 mybatis核心配置文件

db.properties

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://192.168.1.102/mybatis?useUnicode=true&characterEncoding=utf8&useSSL=false
username=root
password=fnfuk2020
```

mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <properties resource="db.properties"/>

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="com/bai/dao/UserMapper.xml"/>
    </mappers>
</configuration>
```

#### 1.2 编写工具类

```java
public class MybatisUtils {
    private static SqlSessionFactory sqlSessionFactory = null;

    static {
        try {
            String resource = "org/mybatis/example/mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static SqlSession getSqlSession(){
         return sqlSessionFactory.openSession();
    }
}
```

#### 1.3 编写接口和实体类

```java
public interface UserDao {

    List<User> getUserList();
}
```

```java
public class User implements Serializable {
    private Integer id;
    private String name;
    private String pwd;

    public User() {
    }

    public User(Integer id, String name, String pwd) {
        this.id = id;
        this.name = name;
        this.pwd = pwd;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPwd() {
        return pwd;
    }

    public void setPwd(String pwd) {
        this.pwd = pwd;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", pwd='" + pwd + '\'' +
                '}';
    }
}
```



#### 1.4 编写接口的xml文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace：绑定一个对应的Dao/Mapper接口-->
<mapper namespace="com.bai.dao.user.UserDao">
    <!--id:接口的方法名 resultType：结果返回类型-->
    <select id="getUserList" resultType="com.bai.entity.User">
		select * from user;
    </select>
</mapper>
```

#### 1.5 测试方法

```java
@Test
    public void getUserList(){
        SqlSession session = null;
        try {
            session = MybatisUtils.getSqlSession();
            // 方式一：推荐
            UserDao userDao = session.getMapper(UserDao.class);
            List<User> list = userDao.getUserList();

            // 方式二： 不推荐使用
            //List<User> list = session.selectList("com.bai.dao.user.UserDao.getUserList");
            for (User user : list) {
                System.out.println(user);
            }
        }finally {
            session.close();
        }

    }
```

### 2. 增删改查

#### 2.1 查

```java
User getUserById(int id);

<select id="getUserById" parameterType="int" resultType="com.bai.entity.User">
    	// 取值时{}里的名称要与参数名一致
        select * from user where id = #{id};
</select>
    
 @Test
public void getUserById(){
   SqlSession session = MybatisUtils.getSqlSession();
   UserDao userDao = session.getMapper(UserDao.class);
   User user = userDao.getUserById(1);
   System.out.println(user);
   session.close();
}
```

#### 2.2 增

`增删改`需要提交事务

```java
int insertUser(User user);

<insert id="insertUser" parameterType="com.bai.entity.User">
    // 取值时{}里的名称要与user的属性名一致
	insert into user(name, pwd) values(#{name}, #{pwd});
</insert>
    
@Test
public void insertUser(){
   SqlSession session = MybatisUtils.getSqlSession();
   UserDao userDao = session.getMapper(UserDao.class);
   User user = new User();
   user.setName("赵六");
   user.setPwd("123456");
   int i = userDao.insertUser(user);
   session.commit();
   System.out.println(i);
   session.close();
}
```

#### 2.3 删

`增删改`需要提交事务

```java
int deleteUserById(int id);

<delete id="deleteUserById" parameterType="int">
	delete from user where id = #{id};
</delete>
    
 @Test
public void deleteUserById(){
   SqlSession session = MybatisUtils.getSqlSession();
   UserDao userDao = session.getMapper(UserDao.class);
   int i = userDao.deleteUserById(5);
   session.commit();
   System.out.println(i);
   session.close();
}
```

#### 2.4 改

`增删改`需要提交事务

```java
int updateUser(User user);

<update id="updateUser" parameterType="com.bai.entity.User">
     // 取值时{}里的名称要与user的属性名一致
	update user set name = #{name}, pwd = #{pwd} where id = #{id};
</update>
    
 @Test
public void updateUserById(){
    SqlSession session = MybatisUtils.getSqlSession();
    UserDao userDao = session.getMapper(UserDao.class);
    User user = userDao.getUserById(1);
    user.setName("田七");
    user.setPwd("123456789");
    int i = userDao.updateUser(user);
    session.commit();
    System.out.println(i);
    session.close();
}
```

#### 2.5 万能Map

```java
int addUserByMap(Map<String, Object> map);

<insert id="addUserByMap" parameterType="map">
    // 取值时{}里的名称要与map的键名一致
	insert into user(name, pwd) VALUES (#{name}, #{pwd});
</insert>
```

当方法要传递多个参数时，可以用Map或者注解。

#### 2.6 模糊查询

```java
List<User> getUserLike(String name);

// 两种方式，方式1会有sql注入问题，推荐方式2
<select id="getUserLike" resultType="com.bai.entity.User" parameterType="string">
	select * from user where name like '%${name}%';
</select>
<select id="getUserLike" resultType="com.bai.entity.User" parameterType="string">
	select * from user where name like concat('%', #{name}, '%');
</select>
    
    
@Test
public void getUserLike(){
   	SqlSession session = MybatisUtils.getSqlSession();
   	UserDao userDao = session.getMapper(UserDao.class);
   	List<User> users = userDao.getUserLike("李");
   	for (User user : users) {
   	    System.out.println(user);
   	}
   	session.close();
}
```

模糊查询时要注意sql注入的问题

- 在java代码执行的时候，传递通配符%%

  ```java
  List<User> users = userDao.getUserLike("%李%");
  ```

- 在sql拼接中使用通配符。 

  ```xml
  select * from user where name like '%${name}%';
  ```

### 3. 配置解析

#### 3.1 核心配置文件mybatis-config.xml

```
configuration（配置）
properties（属性）
settings（设置）
typeAliases（类型别名）
typeHandlers（类型处理器）
objectFactory（对象工厂）
plugins（插件）
environments（环境配置）
environment（环境变量）
transactionManager（事务管理器）
dataSource（数据源）
databaseIdProvider（数据库厂商标识）
mappers（映射器）
```

#### 3.2 环境配置（environments）

MyBatis可以配置成适应多种环境，但每个SqlSessionFactory实例只能选择一种环境

MyBatis默认的事务管理器是JDBC，连接池：POOLED

#### 3.3 属性（properties）

我们可以通过properties属性来实现引用配置文件

这些属性可以在外部进行配置，并可以进行动态替换。你既可以在典型的 Java 属性文件中配置这些属性，也可以在 properties 元素的子元素中设置。

db.properties

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost/mybatis?useUnicode=true&characterEncoding=utf8&useSSL=false
username=root
password=fnfuk2020
```

mybatis-config.xml

```xml
<!--引入db.properties文件--> 
<properties resource="db.properties"/>
```

#### 3.4 别名

类型别名可为 Java 类型设置一个缩写名字。 它仅用于 XML 配置，意在降低冗余的全限定类名书写。

```xml
<!--一个类起一个别名-->
<typeAliases>
  <typeAlias alias="User" type="com.bai.entity.User"/>
</typeAliases>

<!--可以指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean
在没有注解的情况下，会使用 Bean 的首字母小写的非限定类名来作为它的别名
-->
<typeAliases>
  <package name="com.bai"/>
</typeAliases>
```

在实体类比较少的时候，使用第一种方式，实体类比较多的时候，使用第二种。

第一种可以自己起别名，第二种则不行，如果非要改，需要在实体上增加注解@Alias（"name"）

#### 3.5 设置

`mapUnderscoreToCamelCase `：是否开启驼峰命名自动映射，即从经典数据库列名 A_COLUMN 映射到经典 Java 属性名 aColumn。

`cacheEnabled `：全局性地开启或关闭所有映射器配置文件中已配置的任何缓存。

`lazyLoadingEnabled` ： 延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。 特定关联关系中可通过设置 fetchType属性来覆盖该项的开关状态。

`logImpl` : 指定 MyBatis 所用日志的具体实现，未指定时将自动查找。

#### 3.6 映射器（mappers）

方式一：（推荐使用）

```xml
<!-- 使用相对于类路径的资源引用 -->
<mappers>
  <mapper resource="org/mybatis/builder/AuthorMapper.xml"/>
  <mapper resource="org/mybatis/builder/BlogMapper.xml"/>
  <mapper resource="org/mybatis/builder/PostMapper.xml"/>
</mappers>
```

方式二：

```xml
<!-- 使用映射器接口实现类的完全限定类名 -->
<mappers>
  <mapper class="org.mybatis.builder.AuthorMapper"/>
  <mapper class="org.mybatis.builder.BlogMapper"/>
  <mapper class="org.mybatis.builder.PostMapper"/>
</mappers>
```

方式二的注意点：

- 接口和他的Mapper配置文件必须同名
- 接口和他的Mapper配置文件必须在同一个包下

### 4. ResultMap（解决属性名和字段名不一致的问题）

数年库的字段名和实体类的属性名不一致时

- sql语句起别名

- 使用resultmap

  ```xml
  <!--column:数据库列名 property:实体类属性-->
  <resultMap id="user" type="User" >
  	<id property="id" column="id" javaType="int"/>
  	<result property="name" column="name" javaType="string"/>
  	<result property="password" column="pwd" javaType="string"/>
  </resultMap>
  
  <select id="getUserList" resultMap="user">
  	select * from user;
  </select>
  ```

### 5. 日志

#### 5.1 日志工厂

如果一个数据库操作，出现了异常，我们需要排错，日志就是最好的助手

#### 5.2 log4j

- 导包
- 编写配置文件

### 6. 分页

- 减少数据的处理量

  ```xml
  select * from user limint #{currPage}, #{pageSize};
  ```

### 7. 注解

```java
@Select("select * from user where id = #{id}")
User getUserById(int id);

@Insert("insert into user(name, pwd) values(#{name}, #{pwd})")
int insertUser(User user);

@Delete("delete from user where id = #{id}")
int deleteUserById(int id);

@Update(" update user set name = #{name}, pwd = #{pwd} where id = #{id}")
int updateUser(User user);
```

- 注解在接口上实现
- 需要核心配置文件中绑定接口

#### 7.1 多参数方法@Param

当方法有多个参数时，参数前面必须加上@Param注解

```java
User getUser(@Param int id, @Param String name);
```



#### 7.2 #{} 和#{}的区别

- #{}能最大程度防止sql注入，而${}不行
- ${}是将参数直接替换，#{}则是解析为点位符

### 8. 多对一

```java
List<Student> getStudentList1();
Teacher getTeacherById(int id);

List<Student> getStudentList2();
```

```xml
 <!--方式1： 子查询的方式   -->
<resultMap id="studentMap1" type="Student">
    <id column="sid" property="id" javaType="int"/>
    <result column="sname" property="name" javaType="string"/>
    <association column="tid" property="teacher" javaType="Teacher" select="getTeacherById"/>
</resultMap>

<select id="getStudentList1" resultMap="studentMap2">
	select * from student;
</select>

<select id="getTeacherById" resultType="Teacher">
	select * from teacher where id = #{id};
</select>

<!--方式2： 按照结果嵌套处理 -->
<resultMap id="studentMap2" type="Student">
    <id column="sid" property="id" javaType="int"/>
    <result column="sname" property="name" javaType="string"/>
    <association property="teacher" javaType="Teacher">
        <id column="tid" property="id" javaType="int"/>
        <result column="tName" property="name" javaType="string"/>
    </association>
</resultMap>

<select id="getStudentList2" resultMap="studentMap2">
	select s.id sid, s.name sname,s.tid tid, t.name tname from student s, teacher t where s.tid = t.id;
</select>


```

两种resultMap的写法都可以，按照自己喜好使用。



### 9. 一对多

```java
Teacher getTeacher1(@Param("id") int id);
Teacher getTeacher2(@Param("id") int id);
Student getStudentByTid(@Param("tid") int tid);
```

```xml
<!--方式1 嵌套子处理-->
<resultMap id="teacherMap1" type="Teacher">
     <id column="id" property="id" javaType="int"/>
     <result column="name" property="name" javaType="string"/>
     <collection property="studentList" javaType="ArrayList" ofType="Student"  select="getStudentByTid" column="id"/>
</resultMap>

<select id="getTeacher1" resultMap="teacherMap1">
    select * from teacher where id = #{id};
</select>
<select id="getStudentByTid" resultType="Student">
    select * from student where tid = #{tid};
</select>

<!--方式2 ：按照结果处理-->
<resultMap id="teacherMap2" type="Teacher">
    <id column="tid" property="id" javaType="int"/>
    <result column="tname" property="name" javaType="string"/>
    <collection property="studentList" ofType="Student">
        <id column="sid" property="id" javaType="int"/>
        <result column="sname" property="name" javaType="string"/>
        <result column="stid" property="tid" javaType="int"/>
    </collection>
</resultMap>

<select id="getTeacher2" resultMap="teacherMap2">
	select t.id tid, t.name tname, s.id sid, s.name sname, s.tid stid from teacher t, student s where s.tid = t.id AND t.id = #{id};
</select>
```

小结：

- 关联：association （多对一）
- 集合：collection (一对多)
- javaType & ofTye
  - javaType 用来指定实体类中属性的类型
  - ofType 用来指定映射到List或者集合中的pojo类型，泛型中的约束类型

### 10. 动态sql

动态SQL就是根据不同的条件生成不同的SQL语句

#### 10.1 if

where 标签解决后面需要 1=1的问题

```xml
<select id="getBlogLike" parameterType="map" resultType="blog">
        select * from blog
        <where>
            <if test="author != null">
                AND author like concat('%', #{author}, '%')
            </if>
            <if test="title != null">
                AND title like concat('%', #{title}, '%')
            </if>
        </where>
</select>
```

#### 10.2 choose,when, otherwise

动态标签都配合<where>标签用

```xml
<select id="queryBlogChoose" resultType="blog" parameterType="map">
    select * from blog
    <where>
        <choose>
            <when test="title != null">
                title like concat('%', #{title}, '%')
            </when>
            <when test="author != null">
                AND author like concat('%', #{author}, '%')
            </when>
            <otherwise>
                AND views = #{views}
            </otherwise>
        </choose>
    </where>
</select>
```

#### 10.3 set

```xml
<update id="updateBlog" parameterType="map">
    update blog
    <set>
        <if test="title != null">
            title = #{title},
        </if>
        <if test="author != null">
            author = #{author},
        </if>
        <if test="views != null">
            views = #{views},
        </if>
    </set>
    where id = #{id}
</update>
```

#### 10.4 foreach

```xml
<select id="queryBlogByIds" resultType="blog" parameterType="list">
    select * from blog where id in
    	<!--collection="list"： 集合的类型， list, Array, 如果是map是map的键 -->
        <foreach collection="list" open="(" close=")" item="id" separator=",">
            #{id}
        </foreach>
</select>
```

### 11. 缓存

#### 11.1 一级缓存（mybatis默认开启）

- 一级缓存也叫本地缓存
  -  与数据库同一次会话期间查询到的数据会放在本地缓存 中
  - 以后如果需要获取相同的数据，直接从缓存中拿，没必要再去查询数据库
- 一级缓存当有增删改的操作时一级缓存会被清空
- 手动清除缓存 session.clearCache(); // 清除缓存

#### 11.2 二级缓存

- 二级缓存也叫全局缓存，一级缓存作用域太低，所以诞生了二级缓存
- 基于namespace级别的缓存，一个名称空间，对应一个二级缓存
- 工作机制
  - 一个会话查询一条数据，这个数据就会被放到当前会话的一级缓存中;
  - 如果当前会话关闭了，这个会话对应的一级缓存就没了，但是我们想要的是，会话关闭了，一级缓存中的数据被保存到二级缓存中
  - 新的会话查询信息，就可以从二级缓存中获取内容
  - 不同的mapper查出来的数据会放在自己对应的缓存map中



- 二级缓存的开启步骤

  - mybatis-config.xml文件中打开全局缓存

    ```xml
    <setting name="cacheEnable" value="true"/>
    ```

  - 在对应的mapper.xml文件中添加<cache/>标签

    ```xml
    <cache/>
    ```

  - 只要开启了二级缓存，在同一个Mapper下就有效
  - 所有的数据都会先放到一级缓存 中，
  - 只有当会话提交或者关闭的时候，才会提交到二级缓存中






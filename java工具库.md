## java工具库

### lombok

- 什么是lombok

  ​		Lombok项目是一个Java库，它会自动插入编辑器和构建工具中，Lombok提供了一组有用的注解，用来消除Java类中的大量样板代码。仅五个字符(@Data)就可以替换数百行代码从而产生干净，简洁且易于维护的Java类。

  **通俗解释：lombok快速开发工具，提供一组java相关注解，通过注解用来更快生成java对象中我们想要的相关方法（get, set, tostring...）等一系列方法**

#### 使用lombok

1. 引入依赖

```maven
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.12</version>
    <scope>provided</scope>
</dependency>
```

2. 使用提供注解

```java
/**
	@Data 注解
	自动给对象提供，GET, SET， TOString， hashCode, equals 等方法
**/ 
@Data
public class User{
    private String id;
    private String name;
    private String age;
    private Date bir;
}
```

#### lombok原理

在编译阶段插入相关函数

#### lombok一组注解

##### 1. @Data

- @DATA 用在类上，生成对象中的GET, SET, TOString, HashCode,equals等相关方法

- 具体用法

  ``` java
  @Data
  public class User{
      private String id;
      private String name;
      private String age;
      private Date bir;
  }
  ```

##### 2. @Getter and @Setter

- 用在类上， 只生成对应的GET,SET方法

- 具体用法

  ```java
  @Getter
  @Setter
  public class User{
      private String id;
      private String name;
      private String age;
      private Date bir;
  }
  ```

##### 3. ToString

- 用在类上，生成tostring方法

- 具体用法

  ```java
  @ToString
  public class User{
      private String id;
      private String name;
      private String age;
      private Date bir;
  }
  ```

##### 4. @AllArgsConstructor And @NoArgsConstructor

- 用在类上，生成全参构造函数和无参构造函数，这两个注解一般同时使用

- 具体用法

  ```java
  @AllArgsConstructor
  @NoArgsConstructor
  public class User{
      private String id;
      private String name;
      private String age;
      private Date bir;
  }
  ```

##### 5. @Accessors

- 用在类上，给类中的set方法开启链式调用，value属性指定 是否开启set方法的链式调用，true开启，false不开启

- 具体用法

  ```java
  @Accessors(chain = true)
  public class User{
      private String id;
      private String name;
      private String age;
      private Date bir;
  }
  
  User user = new User();
  user.setId("1").setName("xiaochen").setAge("20").setBir(new Date());
  ```

#### idea中安装lombok

- idea安装lombok插件才能使用lombok的注解。



### 日志

#### Slf4j

##### @Slf4j

- 用在类上，用来快速定义一个日志变量

- 原理， 在对应类上加入这个注解相当于在这个类中声明一个日志对象

  ```java
  private Logger log = LoggerFactory.getLogger(this.getClass());
  ```

- 具体用法

  ```java
  @Slf4j
  @Controller
  @RequestMapping("/user")
  public class UserController{
      
      @RequestMapping("/findAll")
      public String findAll(){
          log.info("进入findAll()方法");
          // 占位用法
          log.info("进入[{}]方法"， “findAll”);
          return "index";
      }
  }
  ```

  


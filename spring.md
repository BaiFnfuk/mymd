## Spring5

### 1. IOC(控制反转)

将类的创建交给容器，程序本身不创建对象，当要用到这个对象时从容器中拿。解耦。

applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd">
    
    <!-- 开启注解 -->
    <context:annotation-config />
    
    <!-- 属性方法 -->
    <bean class="com.bai.entity.User" id="user">
        <property name="id" value="1"/>
        <property name="name" value="spring"/>
    </bean>
    <!-- 构造方法的方式 -->
    <bean id="user1" class="com.bai.entity.User">
        <constructor-arg name="id" value="2"/>
        <constructor-arg name="name" value="呵呵"/>
    </bean>
</beans>
```

测试方法：

```java
public static void main(String[] args) {
    ApplicationContext context = new ClassPathXmlApplicationContext("application.xml");
    User user = (User) context.getBean("user");
    System.out.println(user);
}
```

总结： 在配置文件加载的时候，容器中管理的对象就已经初始化了

#### 1.1 别名

给bean起一个别名

```xml
<!--给id为user的bean起一个hehe的别名-->
<alias name="user" alias="hehe"/>
```

#### 1.2 Bean的配置

```xml
<!--
 id: bean的唯一标识符
 class: bean对象所对应的全限类名
 name : 也是别名，与alias作用一样，但是name可以一次取多个别名，逗号分隔

-->
<bean class="com.bai.entity.User" id="user">
        <property name="id" value="1"/>
        <property name="name" value="spring"/>
 </bean>
```

#### 1.3 import

一般用于团队开发，他可以将多个配置文件导入合并成一个配置文件

```xml
<import resource="applicationContext-dao.xml"/>
```

### 2. 依赖注入（DI)

#### 2.1 构造器注入

```xml
<bean class="com.bai.dao.UserDaoImpl" id="userDao"/>
<!-- 类UserServiceImpl的构造器需要一个UserDao对象 -->
<bean class="com.bai.service.UserServiceImpl" id="userService">
    <constructor-arg name="userDao" ref="userDao"/>
</bean>
```

#### 2.2 set方法注入

- 依赖注入：set注入
  - 依赖： bean对象 的创建依赖于容器
  - 注入：bean对象中的所有属性由容器注入

```java
public class Student {

    private String name;
    private Address address;
    private String[] books;
    private List<String> hobby;
    private Map<String, String> card;
    private Set<String> games;
    private Properties info;
    private String wife;
    
    // getter/setter...
}
```

```xml
<bean id="address" class="com.bai.entity.Address"/>

<bean id="student" class="com.bai.entity.Student">
    <!-- 普通值注入 -->
    <property name="name" value="哈哈"/>
    <!-- 对象注入 ref-->
    <property name="address" ref="address"/>
    <!-- 数组注入 -->
    <property name="books">
		<array>
		    <value>西游记</value>
		    <value>三国演义</value>
		    <value>水浒传</value>
		    <value>红楼梦</value>
		</array>
    </property>
    <!-- list注入 -->
    <property name="hobby">
        <list>
            <value>听歌</value>
            <value>看电影</value>
            <value>看书</value>
        </list>
    </property>
    <!-- map注入 -->
    <property name="card">
       <map>
           <entry key="身份证" value="111111111"/>
           <entry key="银行卡" value="222222222"/>
           <entry key="学生卡" value="333333333"/>
       </map>
    </property>
    <!-- set注入 -->
    <property name="games">
       	<set>
       	    <value>LOL</value>
       	    <value>COC</value>
       	    <value>WOW</value>
       	</set>
    </property>
    <!-- null值注入 -->
    <property name="wife">
		<null/>
    </property>
    <!-- properties注入 -->
    <property name="info">
        <props>
            <prop key="学号">123</prop>
            <prop key="姓名">小明</prop>
            <prop key="性别">男</prop>
        </props>
    </property>
</bean>
```

#### 2.3 其他方法注入

p命名空间和c命名空间注入

需要添加：

xmlns:p="http://www.springframework.org/schema/p"

xmlns:c="http://www.springframework.org/schema/c"

p是property的意思，c是constructor的意思

```xml
<bean id="user" class="com.bai.entity.User" p:id="1" p:name="小明"/>
<bean id="user1" class="com.bai.entity.User" c:id="2" c:name="小红"/>
```

注意：c命名空间的bean需要有相应的构造器和无参构造器

### 3. Bean的作用域

1. singieton 单例模式(spring的默认就是单例模式)

   ```xml
   <bean id="address" class="com.bai.entity.Address" scope="singleton"/>
   ```

2. prototype 原型模式，每次从容器中get的时候，都会产生一个新对象

   ```xml
   <bean id="address" class="com.bai.entity.Address" scope="prototype"/>
   ```

3. session,request在web开发中使用

### 4. Bean的自动装配

- 自动装配，是spring满足bean依赖一种方式
- spring会在上下文中自动寻找，并自动给bean装配

在spring中有三种装配的方式

1. 在xml中显式的配置
2. 在java中显式配置
3. 隐式的自动装配(byName, byType)（重要）

#### 4.1 ByName自动装配

```xml
<bean id="dog" class="com.bai.entity.Dog"/>

<bean id="cat" class="com.bai.entity.Cat"/>
<!--
 byName:  会自动在容器上下文中查找，和自己对象的set方法后面的值对应的bean
这种试必须保证bean的id名跟对象的属性名一致
-->
<bean id="people" class="com.bai.entity.People" autowire="byName">
    <property name="name" value="小明"/>
</bean>
```

```java
public class People {

    private Cat cat;
    private Dog dog;

    private String name;
 
    // getter/setter
}
```

#### 4.2 ByType

```xml
<bean id="dog" class="com.bai.entity.Dog"/>

<bean id="cat" class="com.bai.entity.Cat"/>
<!--
byType : 会自动在容器上下文中查找，和自己对象属性类型相同的bean
这种方式必须保证类型全局唯一，不能有多个同类型的bean
-->
<bean id="people" class="com.bai.entity.People" autowire="byType">
    <property name="name" value="小明"/>
</bean>
```

```java
public class People {

    private Cat cat;
    private Dog dog;

    private String name;
 
    // getter/setter
}
```

#### 4.3 注解

1. 要导入context约束

2. 在applicationContext.xml文中开启注解`<context:annotation-config />`

@Autowired

```java
@Autowired
private Cat cat;
@Autowired
private Dog dog;
```

@Autowired是通过反射机制实现，set方法可以省略。使用该注解的前提是这个自动装配的属性在容器中存在，先通过byType注入，要有多个同类型的bean则通过byName注入

```xml
@Qualifier(value = "cat")
private Cat cat;
```

通过Qualifier显式地指定注入那个bean

### 5. 使用注解开发

1. 在spring4之后要使用注解开发，必须要保证aop的包导入
2. 增加注解支持<context:annotation-config />

3. 指定扫描包，<context:component-scan base-package="com.bai.entity"/>

```xml
<context:annotation-config />
<context:component-scan base-package="com.bai"/>
```

```java 
/* @Component-将类交给spring管理，相当于<bean id="cat" class="com.bai.entity.Cat">  
   @Value-给属性赋值，相当于<property name="name" value="猫"/>
*/
@Component
public class Cat {
    
    @Value("猫")
    private String name;
    
    public void shout(){
        System.out.println("miao~");
    }
}
```

@Component有几个衍生的注解，在web开发中，会按照mvc三层架构分层

- dao - @Repository
- service - @Service
- controller - @Controller

这四个注解功能是一样的，都是代表将某个类注册到Spring中。

### 6. 使用java的方式配置spring

使用java方式配置spring就不需要再使用xml文件

```java
/*
 @Configuration - 这个类被spring接管了，注册到了容器中，代表这是一个配置类
 @ComponentScan - 扫描包路径
*/
@Configuration
@ComponentScan("com.bai")
public class MyConfig {
    /*
    	@Bean - 方法的返回值注册到容器中，id就是方法名
    */
    @Bean()
    public User getUser(){
        return new User("小明");
    }
}

@Test
public void test(){
	ApplicationContext context = new AnnotationConfigApplicationContext(MyConfig.class);
	User user = context.getBean("user", User.class);
	System.out.println(user.getName());
}
```

### 7. 代理模式

#### 7.1 静态代理

角色分析：

- 抽象对象 ： 一般会使用接口或者抽象类来解决
- 真实角色： 被代理的角色
- 代理角色：代理真实角色，代理真实角色后，我们一般会做一些附属操作
- 客户： 访问代理对象的角色

代理模式的好处：

- 可以使真实角色的操作更加纯粹，不用去关注一些公共的业务
- 公共业务交给代理，实现了业务的分工
- 公共业务发生扩展的时候，方便集中管理

缺点：

- 一个真实角色就会产生一个代理角色;代码量会翻倍，开发效率变低

步骤：

1. 接口

   ```java
   public interface Rent {
   
       void rent();
   }
   ```

2. 真实角色

   ```java
   public class Landlord implements Rent{
   
       @Override
       public void rent() {
           System.out.println("房东：出租房子！");
       }
   }
   ```

3. 代理角色

   ```java
   public class Proxy implements Rent{
   
       private Landlord landlord;
   
       public Proxy() {
       }
   
       public Proxy(Landlord landlord) {
           this.landlord = landlord;
       }
   
       @Override
       public void rent() {
           showings();
           landlord.rent();
           singAContract();
           charge();
       }
   
       public void showings(){
           System.out.println("中介：带房客看房！");
       }
   
       public void charge(){
           System.out.println("中介：收点中介费！");
       }
   
       public void singAContract(){
           System.out.println("中介：签订租赁合同");
       }
   ```

4. 客户端访问代理角色

   ```java
   public class Tenant{
   
       public static void main(String[] args) {
           // 房东要租房子
           Landlord landlord = new Landlord();
           // 代理，中介帮房东租房子。代理一般会有一些附属操作
           Proxy proxy = new Proxy(landlord);
           // 直接找中介租房即可
           proxy.rent();
       }
   ```

#### 7.2 动态代理

- 动态代理和静态代理角色一样
- 动态代理的代理类是动态生成的，不是我们直接写好的
- 动态代理分为两大类：基于接口的动态代理，基于类的动态代理
  - 基于接口---JDK动态代理
  - 基于类：cglib
  - java字节码实现：javassist

需要了解两个类：Proxy 代理， InvocationHandler 调用代理处理

动态代理的好外：

- 可以使真实角色的操作更加纯粹，不用去关注一些公共的业务
- 公共业务交给代理，实现了业务的分工
- 公共业务发生扩展的时候，方便集中管理
- 一个动态代理类代理的是一个接口，一般就是对应的一类业务
-  一个动态代理类可以代理多个类，只要现了同一接口即可

动态代理：

```java
public class ProxyInvocationHandler implements InvocationHandler {

    private Object target;

    // 被代理对象
    public void setTarget(Object target) {
        this.target = target;
    }

    // 生成代理对象
    public Object getProxy(){
      return Proxy.newProxyInstance(this.getClass().getClassLoader(),
                target.getClass().getInterfaces(), this);
    }

    // 处理代理实例：并返回结果
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        // 动态代理的本质，就是通过反射来实现
        Object result = method.invoke(target, args);
        return result;
    }
}
```

接口：

```java
public interface UserService {
    void add();
    void delete();
    void update();
    void query();
}
```

被代理类：

```java
public class UserServiceImpl implements UserService {
    @Override
    public void add() {
        System.out.println("增加一个用户");
    }

    @Override
    public void delete() {
        System.out.println("删除一个用户");
    }

    @Override
    public void update() {
        System.out.println("更新一个用户");
    }

    @Override
    public void query() {
        System.out.println("查询一个用户");
    }

}
```

测试：

```java
 @Test
public void Test(){
    UserService userService = new UserServiceImpl();
    ProxyInvocationHandler invocationHandler = new ProxyInvocationHandler();
    invocationHandler.setTarget(userService);
    UserService proxy = (UserService) invocationHandler.getProxy();
    proxy.add();
}
```

### 8. AOP(面向切面编程)

AOP(Aspect Oriented Programming)，面向切面编程，通过预编译的方式和运行期动态代理实现程序功能的统一维护的一种技术。

#### 8.1 AOP在Spring中的作用

提供声明式事务，允许用户自定义切面

- 横切关注点，跨越应用程序多个模块的方法或功能。即是与我们业务逻辑无关，但是我们需要关注的部分，就是横切关注点。如日志，安全，缓存，事务等等。。。
- 切面（Aspect）:横切关注占，被模块化的特殊对象，即它是一个类
- 通知（Advice）:切面必须完成的工作，即它是类中的一个方法
- 目标（Traget）：被通知对象
- 代理（Proxy）：向目标对象应用通知之后创建的对象
- 切入点（PointCut）:切面通知执行的“地点”的定义
- 连接点（JointPoint）:与切入点匹配的执行点

#### 8.2 使用Spring实现AOP

`需要导入一个依赖包`

```xml
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.5</version>
</dependency>
```

方式一：使用Spring接口（spring api接口实现）

```xml
<!--注册bean-->
<bean id="userService" class="com.bai.service.UserServiceImpl"/>
<bean id="beforeLog" class="com.bai.log.BeforeLog"/>
<bean id="afterLog" class="com.bai.log.AfterLog"/>
<!-- 方式一：使用原生spring api接口 -->
<!--配置aop:需要导入aop约束-->
<aop:config>
    <!--切入点：pointcut expression：表达式，execution（“要执行的位置！* * * *”）
	execution（）里的内容为 返回值 类路径 方法 参数列表 * com.bai.service.*.*(..)
	* com.bai.service.*.*(..) 不限返回值 service包下的所有类 所有方法 不限参数类型
-->
	<aop:pointcut id="pointcut" expression="execution(* com.bai.service.UserServiceImpl.*(..))"/>
    <aop:advisor advice-ref="beforeLog" pointcut-ref="pointcut"/>
    <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>
</aop:config>
```

advice:

```java
public class BeforeLog implements MethodBeforeAdvice {

    /* method: 要执行的目标对象的方法
     args: 参数
     target: 目标对象
     */
    @Override
    public void before(Method method, Object[] args, Object target) throws Throwable {
        System.out.println(target.getClass().getName() + "的" + method.getName() + "被执行了");
    }
}
```

```java
public class AfterLog implements AfterReturningAdvice {

    @Override
    public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
        System.out.println(target.getClass().getName() + "的" + method.getName() + "被执行了,返回结果：" + returnValue);
    }
}
```



```java
public class UserServiceImpl implements UserService {

    @Override
    public void add() {
        System.out.println("增加了一个用户");
    }

    @Override
    public void delete() {
        System.out.println("删除一个用户");
    }

    @Override
    public void update() {
        System.out.println("更新一个用户");
    }

    @Override
    public void query() {
        System.out.println("查询一个用户");
    }
}
```

测试：

```java
 @Test
public void test(){
	ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
	UserService userService = (UserService) context.getBean("userService");
	userService.add();
}
```

方式2：自定义类（自定义切面）

```java
public class MyPointCut {

    public void before(){
        System.out.println("=======方法执行前=====");
    }

    public void after(){
        System.out.println("=========方法执行后======");
    }
}
```

```xml

<bean id="myPointCut" class="com.bai.log.MyPointCut"/>
<aop:config>
    <!-- 自定义切面 -->
	<aop:aspect ref="myPointCut">
		<aop:pointcut id="pointcut" expression="execution(* com.bai.service.*.*(..))"/>
		<aop:before method="before" pointcut-ref="pointcut"/>
		<aop:after method="after" pointcut-ref="pointcut"/>
	</aop:aspect>
</aop:config>
```

#### 8.3 注解实现AOP

1. 开启注解支持

	```xml
<aop:aspectj-autoproxy/>
	```
	
2. 编写切面

   ```java
   @Aspect  //标记这个类是一个切面
   @Component
   public class AnnotationPointCut {
   
       @Before("execution(* com.bai.service.*.*(..))") // 标记这是一个通知
       public void before(){
           System.out.println("方法执行前");
       }
   
       @After("execution(* com.bai.service.*.*(..))")
       public void after(){
           System.out.println("方法执行后");
       }
   
       // 在环绕增强中，我们可以给定一个参数，代表我们要获取处理切入的点
       @Around("execution(* com.bai.service.*.*(..))")
       public void around(ProceedingJoinPoint joinPoint) throws Throwable {
           System.out.println("环绕前");
           // 执行方法
           joinPoint.proceed();
           System.out.println("环绕后");
       }
   
       @AfterThrowing("execution(* com.bai.service.*.*(..))")
       public void afterThrowing(){
           System.out.println("抛出异常后");
       }
   
       @AfterReturning("execution(* com.bai.service.*.*(..))")
       public void afterReturn(){
           System.out.println("方法返回后");
       }
   }
   ```

### 9. Spring整合Mybatis

1. 导包

   ```xml
   <dependency>
       <groupId>org.mybatis</groupId>
       <artifactId>mybatis-spring</artifactId>
       <version>2.0.5</version>
   </dependency>
   ```
   
2. 配置文件

   ```xml
   <context:property-placeholder location="classpath:db.properties"/>
   
   
   <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
           <property name="driverClassName" value="${jdbc.driver}"/>
           <property name="url" value="${jdbc.url}"/>
           <property name="username" value="${jdbc.username}"/>
           <property name="password" value="${jdbc.password}"/>
   </bean>
   
   <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
       <property name="dataSource" ref="dataSource"/>
       <property name="configLocation" value="classpath:mybatis-config.xml"/>
       <property name="mapperLocations" value="classpath:com/bai/mapper/*Mapper.xml"/>
   </bean>
   <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
       <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
       <property name="basePackage" value="com.bai.mapper"/>
   </bean>
   <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
       <constructor-arg name="sqlSessionFactory" ref="sqlSessionFactory"/>
   </bean>
   ```

### 10. spring中的事务

- 声明式事务： AOP

  开启容器的事务处理功能

  ```xml
  <!--  配置声明式事务  -->
      <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
          <property name="dataSource" ref="dataSource"/>
      </bean>
      <!--  配置事务的类：  -->
      <tx:advice id="txAdvice" transaction-manager="transactionManager">
          <!--  配置那些方法需要事务  -->
          <tx:attributes>
              <tx:method name="add*" propagation="REQUIRED"/>
              <tx:method name="delete*" propagation="REQUIRED"/>
              <tx:method name="update*" propagation="REQUIRED"/>
              <tx:method name="query*" read-only="true" />
              <tx:method name="*" propagation="REQUIRED"/>
          </tx:attributes>
      </tx:advice>
  
      <!--  配置事务的切入  -->
      <aop:config>
          <aop:pointcut id="txPointcut" expression="execution(* com.bai.mapper.*.*(..))"/>
          <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointcut"/>
      </aop:config>
  ```

  ```java
  @Override
      public List<Users> queryUser() {
          Users users = new Users(7, "小明", "123456");
          usersMapper.addUsers(users);
          usersMapper.deleteUsers(7); //此处的sql语句是错误的，事务回滚
          return usersMapper.queryUser();
      }
  ```

  `注意：要在同一个方法里，事务才生效`

- 注解使用事务

  1. 打开事务注解支持

     ```xml
     <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
         <property name="dataSource" ref="dataSource"/>
     </bean>
     <!-- 事务注解支持 -->
     <tx:annotation-driven transaction-manager="transactionManager"/>
     ```

  2. 在要进行事务的方法上使用@Transactional注解

     ```java
     @Transactional
     @Override
     public List<Users> queryUser() {
         Users users = new Users(7, "小明", "123456");
         usersMapper.addUsers(users);
         usersMapper.deleteUsers(7); //此处的sql语句是错误的，事务回滚
         return usersMapper.queryUser();
     }
     ```

- 编程式事务： 需要在代码中，进行事务(不推荐使用)


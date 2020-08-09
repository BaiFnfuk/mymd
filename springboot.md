### 1. 原理初探

- pom.xml

  - spring-boot-dependencies :核心依赖在父工程中
  - 引入springboot依赖的时候，不需要指定版本，因为父工程已经指定

- 启动器

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
  </dependency>
  ```

  - 比如spring-boot-starter-web，就会帮我们导入自动导入web环境的所有依赖！
  - springboot会将所有的功能场景都打包成一个个的启动器

- 主程序

  - @SpringBootApplication 标注这个类是一个springboot的应用

    ```xml
    @SpringBootApplication包含下面两个注解
    
    @SpringBootConfiguration :springboot的配置
    	@Configuration: spring配置类
    	@Component	： 说明这也是一个spring的组件	
    @EnableAutoConfiguration :自动配置
    	@AutoConfigurationPackage ： 自配置包
    		@Import(AutoConfigurationPackages.Registrar.class)： 自动配置‘包注册’
    	@Import(AutoConfigurationImportSelector.class)： 自动配置导入选择
    ```

### 2. 给对象赋值

通过@ConfigurationProperties注解可以在yml文件中给对象属性赋值

```java
@ConfigurationProperties("person")
public class Person {
    private String name;
    private Integer age;
    private Boolean happy;
    private Date birthday;
    private Map<String, Object> maps;
    private List<Object> lists;
    private Dog dog;
}
```

```yaml
# 给对象赋值
person:
  name: 小明
  age: 3
  habby: true
  birthday : 2020/7/23
  maps : {k1: v1, k2: v2}
  lists:
    - code
    - music
  dog:
    name: 旺财
    id: 3
```

### 3. 多环境配置

创建application.yml, application-dev.yml文件，在application.yml文件下加上下面的内容，选择加载application-dev.yml配置文件

```yml
spring:
  profiles:
    active: dev
```

### 4. web开发

#### 4.1 静态资源导入

- 在Springboot,我们可以使用以下方式处理静态资源
  - META-INF/resources/webjars  http://localhost:8080/webjars/ 访问
  - classpath:public, classpath:static, classpath:resources   http://localhost:8080/ 访问

- 优先级： resources > static(默认) > public

```java
private static final String[] CLASSPATH_RESOURCE_LOCATIONS = { "classpath:/META-INF/resources/",
      "classpath:/resources/", "classpath:/static/", "classpath:/public/" };
```

#### 4.2 定制首页

themplates目录下的所有页面，只能通过controller来跳转，需要模板引擎(thymeleaf)的支持

自定义图标，将favicon.ico图片放到static目录，springboot会自动将这图片做为图标

```java
// 扩展springMVC
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {

    // 视图跳转
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/").setViewName("index");
        registry.addViewController("/index.html").setViewName("index");
        registry.addViewController("/main.html").setViewName("dashboard");
    }

     //自定义国际化
    @Bean("localeResolver")
    public LocaleResolver addLocaleResolver(){
        return new MyLocaleResolver();
    }

    //自定义拦截器
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoginHandlerInterceptor())
                .addPathPatterns("/**")
                .excludePathPatterns("/", "/index.html", "/login");
    }
}
```

#### 4.3 thymeleaf

需要导入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

将html页面放到themplates目录下，通过controller跳转到页面

html文件需要引入命名约束

```html
<html lang="en" xmlns:th="http://www.thymeleaf.org">
```

所有的html元素都可以被thymeleaf替换接管： th:属性名

所有的静态资源都需要使用thymeleaf接管;

#### 4.4 thymeleaf语法

1. ${}取值

   ```html
   <span th:text="${msg}"></span>
   ```

2. @{}链接

   ```html
   <a th:href="@{/index}">首页</a>
   ```

3. 循环

   ```html
   <ul>
      <li th:each="str:${list}" th:text="${str}"></li>
   </ul>
   ```

#### 4.5 国际化

1. 在resources目录下创建i18n目录

2. 创建login.properties， login_zh_CN.properties， login_en_US.properties文件

3. login.properties为默认的语言，其他的为可切换的语言

4. 三个文件内容

   ```properties
   login.btn=登录
   login.password=密码
   login.remember=记住我
   login.tip=请登录
   login.username=用户名
   ```

   ```properties
   login.btn=Sgin in
   login.password=Password
   login.remember=Remember me
   login.tip=please sign in
   login.username=Username
   ```

5. 在springboot配置文件中配置文件的位置

   ```yaml
   spring:
     messages:
       basename: i18n/login
   ```

6. 在页面中国际化用#{}来取得相应的内容

   ```html
   <h1 th:text="#{login.tip}">Please sign in</h1>
   ```

7. 自定义LocaleResolver

   ```java
   @Configuration("localeResolver")
   public class MyLocaleResolver implements LocaleResolver {
   
       //解析请求
       @Override
       public Locale resolveLocale(HttpServletRequest request) {
           String lang = request.getParameter("lang");
           // 默认语言
           Locale locale = Locale.getDefault();
           // 如果请求的连接带了国际化的参数
           if (!StringUtils.isEmpty(lang)){
               // zh_CN 分隔成国家和地区
               String[] split = lang.split("_");
               locale = new Locale(split[0], split[1]);
           }
           return locale;
       }
   
       @Override
       public void setLocale(HttpServletRequest request, HttpServletResponse response, Locale locale) {
   
       }
   }
   ```

   ```html
   <a class="btn btn-sm" th:href="@{/index.html(lang='zh_CN')}">中文</a>
   <a class="btn btn-sm" th:href="@{/index.html(lang='en_US')}">English</a>
   ```

#### 4.6 拦截器

```java
public class LoginHandlerInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        HttpSession session = request.getSession();
        String token = (String) session.getAttribute("USER_SESSION");
        if (StringUtils.isEmpty(token)) {
            request.setAttribute("msg", "没有权限，请先登录！");
            request.getRequestDispatcher("/index.html").forward(request, response);
            return false;
        }
        return true;
    }
}
```

#### 4.7 404等错误处理

在templates目录下创建error目录 ，把404.html等错误页面放到目录就行。

1. 后台模板：x-admin

### 5. Data

#### 5.1  jdbc使用

导包：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>

<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.48</version>
</dependency>
```

配置数据库：

```yaml
datasource:
  driver-class-name: com.mysql.jdbc.Driver
  username: root
  password: 123456
  url: jdbc:mysql://localhost:3306/mybatis?useUnicode=true&characterEncoding=utf8&useSSL=false&serverTimezone=UTC
```

测试：

```java
 @Autowired
    private DataSource dataSource;
    @Autowired
    private JdbcTemplate jdbcTemplate;

    @Test
    void contextLoads() throws SQLException {
        Connection connection = dataSource.getConnection();
        String sql = "select * from user";
        List<Map<String, Object>> maps = jdbcTemplate.queryForList(sql);
        for (Map<String, Object> map : maps) {
            System.out.println(map.get("name"));
        }
        connection.close();
    }
```

#### 5.2 Druid

导包：

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.1.22</version>
</dependency>
```

编写配置文件：

```yaml
datasource:
  driver-class-name: com.mysql.jdbc.Driver
  username: root
  password: 123456
  url: jdbc:mysql://localhost:3306/mybatis?useUnicode=true&characterEncoding=utf8&useSSL=false&serverTimezone=UTC
  type: com.alibaba.druid.pool.DruidDataSource
  druid:
    # 初始化大小，最小，最大
    initial-size: 5
    min-idle: 5
    max-active: 20
    # 配置获取连接等待超时的时间
    max-wait: 60000
    # 配置间隔多久才进行一次检测，检测需要关闭的空闲连接，单位是毫秒
    time-between-eviction-runs-millis: 60000
    # 配置一个连接在池中最小生存的时间，单位是毫秒
    min-evictable-idle-time-millis: 300000
    validation-query: SELECT 1 FROM DUAL
    test-while-idle: true
    test-on-borrow: false
    test-on-return: false
    # 打开PSCache，并且指定每个连接上PSCache的大小
    pool-prepared-statements: true
    max-pool-prepared-statement-per-connection-size: 20

  # 配置监控统计拦截的filters, stat: 监控统计， log4j: 日志记录， wall:防御sql注入
  # log4j需要导包，要不然会报错
  #  <dependency>
  #     <groupId>org.slf4j</groupId>
  #     <artifactId>slf4j-log4j12</artifactId>
  #  </dependency>
    filters: stat, wall, log4j
  # useGlobalDataSourceStat: true
    use-global-data-source-stat: true
  # 通过connectProperties属性来打开mergeSql功能；慢SQL记录
    connection-properties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
```

```java

@Configuration
public class DriudConfig {
    @ConfigurationProperties(prefix = "spring.datasource")
    @Bean
    public DruidDataSource druidDataSource(){
        return new DruidDataSource();
    }
    /**
     * 配置监控服务器
     *
     * @return 返回监控注册的servlet对象
     * 监控页面： http://localhost/druid/index
     */
    @Bean
    public ServletRegistrationBean statViewServlet() {
        ServletRegistrationBean servletRegistrationBean = new ServletRegistrationBean(new StatViewServlet(), "/druid/*");
        // 添加IP白名单
        servletRegistrationBean.addInitParameter("allow", "127.0.0.1");
        // 添加IP黑名单，当白名单和黑名单重复时，黑名单优先级更高
//        servletRegistrationBean.addInitParameter("deny", "192.168.25.123");
        // 添加控制台管理用户
        servletRegistrationBean.addInitParameter("loginUsername", "admin");
        servletRegistrationBean.addInitParameter("loginPassword", "admin");
        // 是否能够重置数据
        servletRegistrationBean.addInitParameter("resetEnable", "false");
        return servletRegistrationBean;
    }

    /**
     * 配置服务过滤器
     *
     * @return 返回过滤器配置对象
     */
    @Bean
    public FilterRegistrationBean statFilter() {
        FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean(new WebStatFilter());
        // 添加过滤规则
        filterRegistrationBean.addUrlPatterns("/*");
        // 忽略过滤格式
        filterRegistrationBean.addInitParameter("exclusions", "*.js,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*,");
        return filterRegistrationBean;
    }

}

```

#### 5.3 mybatis

导包：

```xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.3</version>
</dependency>
```

编写XXMapper.java和XXMapper.xml文件即可。

在启动类上用@MapperScan("com.bai.mapper")注解 或者是每一个Mapper类上使用@Mapper注解进行mapper注册

### 6. springSecurity

- 权限
  - 功能权限
  - 访问权限
  - 菜单权限

导包：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

@EnableWebSercuity开启WebSecurity模式

```java
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    // 授权
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        // 首页所有人可以访问，但是功能页只有对应权限的人才能访问
        // 请求授权的规则
        http.authorizeRequests().antMatchers("/").permitAll()
                .antMatchers("/level1/**").hasRole("vip1")
                .antMatchers("/level2/**").hasRole("vip2")
                .antMatchers("/level3/**").hasRole("vip3");
        // 没有权限默认会到登录页面，需要开启登录的页面
        http.formLogin();
        // 开户注销功能
        http.logout();
    }

    // 认证 BCryptPasswordEncoder加密方法
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
                .withUser("admin").password(new BCryptPasswordEncoder().encode("123456")).roles("vip2", "vip3")
                .and()
                .withUser("root").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1", "vip2", "vip3")
                .and()
                .withUser("guest").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1");
    }
}
```

### 7. shiro

1. 导包

   ```xml
   <dependency>
       <groupId>org.apache.shiro</groupId>
       <artifactId>shiro-spring-boot-web-starter</artifactId>
       <version>1.5.3</version>
   </dependency>
   ```

2. 编写realm

   ```java
   public class LoginRealm extends AuthorizingRealm {
   	//授权
       @Override
       protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principal) {
           return null;
       }
   	// 认证
       @Override
       protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
           return null;
       }
   }
   ```

3. 编写config类

   ```java
   public class ShrioConfig {
   
   
       // ShiroFilterFactoryBean
       // DefaultWebSecurityManager
       // 创建 realm对象 需要自定义
   
       @Bean("realm")
       public Realm getRealm(){
           return new LoginRealm();
       }
   
   
       @Bean
       public ShiroFilterChainDefinition  shiroFilterChainDefinition(){
           ShiroFilterChainDefinition chainDefinition = new DefaultShiroFilterChainDefinition();
           return chainDefinition;
       }
   
       @Bean("securityManager")
       public DefaultWebSecurityManager getDefaultWebSecurityManager(Realm realm){
           DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
           // 关联realm
           securityManager.setRealm(realm);
           return securityManager;
       }
   
       @Bean("shiroFilterFactoryBean")
       public ShiroFilterFactoryBean getShiroFilterFactoryBean(DefaultWebSecurityManager securityManager,
                                                               ShiroFilterChainDefinition shiroFilterChainDefinition){
           ShiroFilterFactoryBean shiroFilterFactoryBean = new ShiroFilterFactoryBean();
           // 设置安全管理器
           shiroFilterFactoryBean.setSecurityManager(securityManager);
           /* 添加shiro的内置过滤器
           *  anon ：无需认证就可以访问
           *  authc: 必需认证才能访问
           *  user: 必须拥有 记住我 功能才能访问
           *  perms: 拥有对某个资源的权限才能访问
           * */
   
           Map<String, String> filterChainMap = shiroFilterChainDefinition().getFilterChainMap();
           filterChainMap.put("/user/**", "authc");
           shiroFilterFactoryBean.setFilterChainDefinitionMap(filterChainMap);
           shiroFilterFactoryBean.setLoginUrl("/tologin");
           return shiroFilterFactoryBean;
       }
   
   }
   ```

4. 测试

   ```java
   @RequestMapping("/login")
   public String login(String username, String password, Model model){
       // 获取当前用户
       Subject subject = SecurityUtils.getSubject();
       // 封装用户的登录数据
       UsernamePasswordToken token = new UsernamePasswordToken(username, password);
       token.setRememberMe(true);
       // 登录
       try{
           subject.login(token);
           return "redirect:/";
       }catch (UnknownAccountException e){
           log.error("用户名错误！");
           e.printStackTrace();
           model.addAttribute("info", "用户名错误！");
           return "forward:/tologin";
       }catch (IncorrectCredentialsException e){
           log.error("密码错误！！");
           e.printStackTrace();
           model.addAttribute("info", "密码错误！！");
           return "forward:/tologin";
       }
   }
   ```

   ```java
   public class LoginRealm extends AuthorizingRealm {
   
       // 授权
       @Override
       protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principal) {
           return null;
       }
   
       // 认证
       @Override
       protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
   
           // 用户名，密码从数据库中取
           String username = "admin";
           String password = "123456";
           UsernamePasswordToken userToken = (UsernamePasswordToken) token;
           if (!userToken.getUsername().equals(username)){
               return null; //抛出异常
           }
           // 密码认证，shiro完成
           AuthenticationInfo authenticationInfo = new SimpleAuthenticationInfo("", password, "");
           return authenticationInfo;
       }
   }
   ```

### 8. Swagger

在项目中使用Swagger

导包：

```xml
<dependency>
     <groupId>com.spring4all</groupId>
     <artifactId>swagger-spring-boot-starter</artifactId>
     <version>1.9.1.RELEASE</version>
</dependency>
```

配置文件编写：

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {

    @Bean
    public Docket docket(Environment environment) {

        // 只有测试环境才打开swagger
        Profiles profiles = Profiles.of("dev", "test");
        boolean flag = environment.acceptsProfiles(profiles);

        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                /* 指定要扫描的包
                *  basePackage()：指定要扫描的包
                *  any(): 扫描全部
                *  none():不扫描
                *  withClassAnnotation():扫描类上的注解
                *  withMethodAnnotation(): 扫描方法上的注解
                * */
                .enable(flag)
            	// 设置分组
                .groupName("Bai")
                .select()
                .apis(RequestHandlerSelectors.basePackage("cn.zpchen.controller"))
                // 过滤，只扫描路径带有/hello的请求
                //.paths(PathSelectors.ant("/hello/**"))
                .build();
    }
    
    public ApiInfo apiInfo() {
        ApiInfoBuilder builder = new ApiInfoBuilder();
        builder.description("Swagger api 接口文档")
                .title("swagger接口文档")
                .version("v1.0")
                .contact(new Contact("Bai", "http://www.baidu.com", "11111@qq.com"))
                .license("Apache 2.0").licenseUrl("http://www.apache.org/licenses/LICENSE-2.0")
                .extensions(new ArrayList<>());
        return builder.build();
    }
}
```

设置多个分组，配置多个Doctet分组就好

#### 8.1 swagger2注解

```java
@ApiModel("用户实体类")
public class User implements Serializable {

    @ApiModelProperty("用户ID")
    private Integer userId;
    @ApiModelProperty("用户名")
    private String username;
    @ApiModelProperty("用户密码")
    private String password;
}
```

```java
@Api(tags = "用户操作")
@Controller
public class HelloController {

    @GetMapping("/hello")
    @ResponseBody
    public String hello() {
        return "Hello World!";
    }

    @ApiOperation(value = "获取用户信息", notes = "userId(用户id)不能空")
    @GetMapping("/user")
    @ResponseBody
    public User user(
            @ApiParam("用户id") Integer userId,
            @ApiParam("用户名") String username,
            @ApiParam("用户密码") String password) {
        User user = new User();
        user.setUserId(userId);
        user.setUsername(username);
        user.setPassword(password);
        return user;
    }

    @ApiOperation(value = "测试结果封装", notes = "")
    @GetMapping("/result")
    @ResponseBody
    public Result<Object> getResult() {
        return Result.success("请求成功");
    }
}
```

### 9. 异步任务

1. 在程序入口类上开启异步注解

```java
@EnableAsync
@SpringBootApplication
public class SwaggerStudyApplication {

    public static void main(String[] args) {
        SpringApplication.run(SwaggerStudyApplication.class, args);
    }

}
```

2. 在需要耗时的方法上加上@Async

```java
@Async // 告诉spring这是一个异步的方法
public void sendMail(){
    try {
        Thread.sleep(3000);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
    System.out.println("======>数据正在处理。。。。");
}
```

发送邮件实现

导包

```xml
 <dependency>
    <groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```

```java
 @Autowired
    private JavaMailSender mailSender;

    @Value("${spring.mail.username}")
    private String from;

    @Async // 告诉spring这是一个异步的方法
    public void sendMail(String to, String subject, String content){
        System.out.println("======>正在发送。。。。");
        SimpleMailMessage message = new SimpleMailMessage();
        message.setFrom(from);
        message.setTo(to);
        message.setSubject(subject);
        message.setText(content);
        mailSender.send(message);
    }
```

```yaml
spring:
  mail:
    host: smtp.163.com
    username: 邮箱地址
    password: 授权码
    default-encoding: UTF-8
```

```java
 @ApiOperation("发送邮件")
    @GetMapping("/sendMail")
    @ResponseBody
    public Result<Object> sendMail() {
        String to = "邮件地址";
        String subject = "发送邮件功能测试";
        String content = "邮件内容，测试内容";
        asyncService.sendMail(to, subject, content);
        return Result.success("发送成功");
    }
```

### 10. 定时任务
```
TaskScheduler	//任务高度者
TaskExecutor	//任务执行者
@EnableScheduling // 开启定时功能的注解
@Scheduled		 // 什么时候执行
cron表达式
```

```java
/**
 * 在一个特定的时间执行这个方法
 * cron = "0 * * * * 0-7" 秒 分 时 日 月 周几
 */
@Scheduled(cron = "0 * * * * 0-7")
public void scheduled(){
    System.out.println("Hello");
}
```

### 11 整合redis

导包：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

```java
@Configuration
public class RedisConfig {

    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory){
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(redisConnectionFactory);

        // 序列化配置
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.activateDefaultTyping(LaissezFaireSubTypeValidator.instance, ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
        // String的序列化
        StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();

        // key采用String的序列化方式
        template.setKeySerializer(stringRedisSerializer);
        // hash的key也采用String的序列化方式
        template.setHashKeySerializer(stringRedisSerializer);
        // value序列化方式采用jackson
        template.setValueSerializer(jackson2JsonRedisSerializer);
        // hash的value序列化方式采用jackson
        template.setHashKeySerializer(jackson2JsonRedisSerializer);
        template.afterPropertiesSet();

        return template;
    }
}
```

### 12. 分布式

#### 12.1 Dubbo + zookeeper

1. 创建两个模块：dubbo-provider, dubbo-consumer
2.  安装zookeeper, 官网下载解压，进入conf目录复制一份zoo.cfg配置文件，并添加admin.serverPort=8180，因为zookeeper开启会占用8080端口。进入bin目录，执行`./zkServer.sh start`
3. 由于dubbo 2.7.7并没有集成dubbo-admin，所以要自己下载编译打包。github上可以下载，编译前找到application.properties文件将zookeeper的地址修改成zookeeper所在的服务器地址，编译打包时最好跳过测试，因为测试过程会报错。最后得到dubbo-admin-server-0.2.0-SNAPSHOT.jar就是dubbo-admi

导包：

```xml
<dependency>
    <groupId>org.apache.dubbo</groupId>
    <artifactId>dubbo-spring-boot-starter</artifactId>
    <version>2.7.7</version>
</dependency>

<dependency>
    <groupId>com.github.sgroschupf</groupId>
    <artifactId>zkclient</artifactId>
    <version>0.1</version>
    <exclusions>
        <exclusion>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
        </exclusion>
        <exclusion>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
        </exclusion>
    </exclusions>
</dependency>

<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-framework</artifactId>
    <version>5.1.0</version>
    <exclusions>
        <exclusion>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
        </exclusion>
    </exclusions>
</dependency>

<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-recipes</artifactId>
    <version>5.1.0</version>
</dependency>

<dependency>
    <groupId>org.apache.zookeeper</groupId>
    <artifactId>zookeeper</artifactId>
    <version>3.6.1</version>

    <exclusions>
        <exclusion>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
        </exclusion>
        <exclusion>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
        </exclusion>
        <exclusion>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

服务编写：

dubbo-provider模块里创建一个服务

```java
public interface TicketService {

    String getTicket();
}

@Service("ticketService")
@DubboService //注册为Dubbo服务
public class TicketServiceImpl implements TicketService{

    @Override
    public String getTicket() {
        return "《Java从入门到放弃》";
    }
}
```

配置dubbo + zookeeper:

```properties
server.port=8081
# 设置服务注册的地址
dubbo.application.name= dubbo-provider-server
dubbo.registry.address= zookeeper://localhost:2181
dubbo.metadata-report.address= zookeeper://localhost:2181
dubbo.scan.base-packages= cn.zpchen.service
# dubbo2.7.7超时时间设置长点，要不然会连接不上zookeeper
dubbo.registry.timeout= 20000
```

消费都编写：

在dubbo-consumer里编写

```java
public interface UserService {

    String buyTicket();
}

// 这个接口的路径要跟提供者的接口路径一致
public interface TicketService {
    String getTicket();
}

@Service("userService")
@DubboService
public class UserServiceImpl implements UserService{

    @DubboReference // 调用服务 
    private TicketService ticketService;

    @Override
    public String buyTicket() {
        String ticket = ticketService.getTicket();
        return ticket;
    }
}
```

配置文件配置：

```properties
server.port=8082

# 设置取服务的地址
dubbo.application.name= dubbo-consumer
dubbo.registry.address= zookeeper://localhost:2181
dubbo.metadata-report.address= zookeeper://localhost:2181
dubbo.registry.timeout= 20000
```


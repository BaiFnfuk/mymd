### 1. SpringMVC的基本配置

1. wel.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
   
       <!--  配置DispatchServlet: 这个是SpringMVC的核心;请求分发器，前端控制器  -->
       <servlet>
           <servlet-name>springMVC</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <!--  DispatchServlet要绑定Spring的配置文件  -->
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:springmvc-servlet.xml</param-value>
           </init-param>
           <!--  启动级别：1 -->
           <load-on-startup>1</load-on-startup>
       </servlet>
       <!--  在springMVC中， /与/*的区别
               / ：只匹配所有的请求，不会去匹配*.jsp这种带jsp后缀的请求
               /* ：匹配所有的请求，包括*.jsp页面
        -->
       <servlet-mapping>
           <servlet-name>springMVC</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
       <!-- 解决乱码问题 -->
       <filter>
           <filter-name>encoding</filter-name>
           <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
           <init-param>
               <param-name>encoding</param-name>
               <param-value>UTF-8</param-value>
           </init-param>
       </filter>
       <filter-mapping>
           <filter-name>encoding</filter-name>
           <url-pattern>/*</url-pattern>
       </filter-mapping>
   </web-app>
   ```

2. 原始方式springmvc-servlet.xml（一般不使用这种方式）

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <!--  处理器遇映射器  -->
       <bean  class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
       <!--  处理器适配器  -->
       <bean  class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
       <!--  视图解析器  -->
       <bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
           <!--  前缀   -->
           <property name="prefix" value="/WEB-INF/jsp/"/>
           <!--  后缀   -->
           <property name="suffix" value=".jsp"/>
       </bean>
   
   </beans>
   ```

   注解方式

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:mvc="http://www.springframework.org/schema/mvc"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
   
       <context:component-scan base-package="com.bai.*"/>
       <!--  让springMVC不处理静态资源  .cc .js .html .mp3 .mp4...-->
       <mvc:default-servlet-handler/>
       <!--  支持MVC注解驱动  -->
       <mvc:annotation-driven/>
   
       <bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
           <property name="prefix" value="/WEB-INF/jsp/"/>
           <property name="suffix" value=".jsp"/>
       </bean>
   </beans>
   ```

3. controller

   ```java
   /*
    @Controller：代表这个类被spring接管
    被这个注解的类中所有方法，如果返回值是Spring,并且有具体页面可以跳转，那么就会被视图解析器解析;
    
    @RequestMapping: 用于映射url到控制器类或一个特定的处理程序方法，可用于类或方法上。用于类上，表示类中所有响应请求的方法都是以该地址作为父路径。
   */
   @Controller
   @RequestMapping("/view")
   public class ViewController {
   
       @RequestMapping("/hello")
       public String helloView(Model model){
           model.addAttribute("msg", "Hello SpringMVC");
           return "hello";  
       }
   }
   ```
   
   给前端返回数据可以用ModelAndView, Mdel或ModelMap对象。

### 2.  RestFul风格

RestFul就是一个资源定位及资源操作的风格，不是标准也不是协议，只是一种风格。基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存等机制

例如：http://localhost:8080/user/1 其中1就是要查询的用用户的id,比传统的id=1更简洁

使用RestFul操作资源：可以通过不同的请求方式来实现不同的效果！如下：请求地址一样，但是功能可以不同！

- http://loacalhost:8080/item/1 查询，GET
- http://loacalhost:8080/item/1 新增，POST
- http://loacalhost:8080/item/1 更新，PUT
- http://loacalhost:8080/item/1 删除，DELETE

```java
/*
	@GetMapping  对应GET请求
	@PostMapping 对应POST请求
	@DeleteMapping 对应DELETE请求
	@PutMapping	对应PUT请求
	@PathVariable 参数的值从url上取
*/
@RequestMapping(value = "/restFul/{a}/{b}")
public String restFul(@PathVariable int a, @PathVariable int b, Model model){
    int c = a + b;
    model.addAttribute("msg" , c);
    return "hello";
}
```

### 3. 转发，重定向

```java
@Controller
public class ForwardAndRedirectController {
    
    // 转发
    @GetMapping("/forward")
    public String forward(){
        return "forward:/WEB-INF/jsp/hello.jsp";
    }

    // 重定向
    @GetMapping("/redirect")
    public String redirect(){
        return "redirect:/view/hello";
    }
}
```

转发和重定向：在有配置视图解析器和没有配置视图解析器时有点区别。

没有配置视图解析器：

- 转发： 需要返回"forward:文件全路径"

  ```java
  // 转发
  @GetMapping("/forward")
  public String forward(){
      return "forward:/WEB-INF/jsp/hello.jsp";
  }
  ```

- 返回"redirect:文件的路径"，如果文件在WEB-INF目录下，刚需要返回访问该文件的请求路径如"redirect:/view/hello"

  ```java
  // 重定向
  @GetMapping("/redirect")
  public String redirect(){
      // return "redirect:/index.jsp";
      return "redirect:/view/hello";
  }
  ```

有配置视图解析器：

- 转发：直接返回文件名,不带后缀

  ```java
  @GetMapping("/forward")
  public String forward(){
  	return "hello";
  }
  ```

- 重定向: 返回"redirect:文件的路径"，如果文件在WEB-INF目录下，刚需要返回访问该文件的请求路径如"redirect:/view/hello"

  ```java
  // 重定向
  @GetMapping("/redirect")
  public String redirect(){
      // return "redirect:/index.jsp";
      return "redirect:/view/hello";
  }
  ```

### 4. 数据处理

当前端的参数名与后端方法的参数名一样的时候,前端的参数会直接赋值到后端方法相同名字的参数。

```java
/* 访问http://localhost:8080/spring/name?name='小时' */
@GetMapping("/name")
public String getParam(String name, Model model){
    //1. 接收前端参数
    System.out.println(name);   // 这里会输出小明
    // 2. 将返回的结果传递给前端
    model.addAttribute("msg", name);
    return "hello";
}
```

当前端的参数名与后端方法的参数名不一样的时候,可以在方法的参数名前加上@RequestParam注解。

```java
/*
  访问 http://localhost:8080/spring/name?userName="小明"
*/
@GetMapping("/name")
public String getParam(@RequestParam("userName") String name, Model model){
    //1. 接收前端参数
    System.out.println(name);	 // 这里会输出小明
    // 2. 将返回的结果传递给前端
    model.addAttribute("msg", name);
    return "hello";
}
```

建议从前端接收数据的参数前都加上@RequestParam注解

前端传过来的数据是一个类的时候，必须保证属性名和类的属性名要一致，不然会接收不到。

```java
@PostMapping("/user")
public String getParamPost(User user, Model model){
    System.out.println(user);
    model.addAttribute("msg", user);
    return "hello";
}
```

```java
public class User {
    private Integer id;
    private String name;
    private String password;
    
   // getter/setter...
}
```

```html
<form action="${pageContext.request.contextPath}/user" method="post">
    <p>用户id:<input type="number" name="id"></p>
    <p>用户名:<input type="text" name="name"></p>
    <p>密&nbsp;码:<input type="password" name="password"></p>
    <p><input type="submit" value="提交"></p>
</form>
```

### 5. 乱码问题

在web.xml文件添加Spring的一个过滤器

```xml
<filter>
    <filter-name>encoding</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>encoding</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

### 6. JSON

如果要控制器的方法返回JSON字符串，在方法上面加上@ResponseBody注解即可

```java
@RequestMapping("/json")
@ResponseBody // 加上这个注解，就不会走视图解析器，会直接返回一个字符串
public String json(User user){
    ObjectMapper mapper = new ObjectMapper();
    String json = null;
    try {
        json = mapper.writeValueAsString(user);  //jackson工具
    } catch (JsonProcessingException e) {
        e.printStackTrace();
    }
    return json;
}
```

除了在方法上加上@ResponseBody外，还可以在类上使用@RestController注解，这个注解声明了这个类的所有返回String的方法都是直接返回字符串，不走视图解析器

```java
@RestController
public class JsonController {
    
}
```

解决json中文乱码问题

```xml
<mvc:annotation-driven>
    <mvc:message-converters>
        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
            <constructor-arg value="UTF-8"/>
        </bean>
        <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
            <property name="objectMapper">
                <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                    <property name="failOnEmptyBeans" value="false"/>
                </bean>
            </property>
        </bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```

### 7. 拦截器

过滤器与拦截器的区别：拦截器是AOP思想的具体应用

过滤器：servlet规范中的一部分，任何java web工程都可以使用，在url-pattern中配置了/*之后可以对所有要访问的资源进行拦截

拦截器：

- 拦截器是SpringMVC框架自己的，只有使用SPringMVC框架的工程才能使用
- 拦截器只会拦截访问的控制器（controller）方法，如果访问的是jsp/html/css/image/js是不会拦截的

#### 7.1 自定义拦截器

实现HandlerInterceptor接口,主要是实现preHandle方法

```java
public class MyInterceptor implements HandlerInterceptor {

    // return true;执行下一个拦截器，return false: 拦截
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("============处理前==========");
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("===========处理后============");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("============清理==============");
    }
}
```

```xml
<mvc:interceptors>
    <mvc:interceptor>
        <!-- /**包括这个请求下面的所有的请求 -->
        <mvc:mapping path="/**"/>
        <bean class="com.bai.config.MyInterceptor"/>
    </mvc:interceptor>
</mvc:interceptors>
```

### 8. 文件上传下载

导包:

```xml
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.4</version>
</dependency>

<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.6</version>
</dependency>
```

文件上传：

```xml
<!-- 文件上传配置   -->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <!-- 请求的编码格式，必须和jsp的pageEncoding一致，以便正确读取表单内容，默认为ISO-8859-1 -->
    <property name="defaultEncoding" value="utf-8"/>
    <!--  上传文件大小上限，单位为字节（10485760=10M）-->
    <property name="maxUploadSize" value="10485760"/>
    <property name="maxInMemorySize" value="40960"/>
</bean>
```

```java
 @RequestMapping("/upload")
    public String uploadFile(@RequestParam("file") CommonsMultipartFile file, HttpServletRequest request) throws IOException {
        // 上传路径保存设置
        String path = request.getSession().getServletContext().getRealPath("/upload");
        System.out.println(path);
        File realPath = new File(path);
        if (!realPath.exists()){
            realPath.mkdir();
        }
        // 上传文件地址
        System.out.println("上传文件保存地址");
        // 通过CommonsMultipartFile的方法直接写文件
        file.transferTo(new File(realPath + "/" + file.getOriginalFilename()));
        return "redirect:/index.jsp";
    }
```

```jsp
<form action="${pageContext.request.contextPath}/upload" method="post" enctype="multipart/form-data">
  选择文件 :<input type="file" name="file"><br>
  <input type="submit" value="提交">
</form>
```

文件下载：

```java
@RequestMapping("/download")
public String downloadFile(HttpServletRequest request, HttpServletResponse response) throws IOException {
    // 要下载的文件地址
    String path = request.getSession().getServletContext().getRealPath("/upload");
    String fileName = "1.jpg";

    // 1. 设置response响应头
    response.reset(); //设置页面不缓存，清空buffer
    response.setCharacterEncoding("utf-8");
    response.setContentType("multipart/form-data"); // 二进制传输数据
    // 设置响应头
    response.setHeader("Content-Disposition", "attachment;fileName="
            + URLEncoder.encode(fileName,"UTF-8"));

    File file = new File(path,fileName);

    // 2. 读取文件--输入流
    InputStream inputStream = new FileInputStream(file);
    // 3. 写出文件--输出流
    OutputStream outputStream = response.getOutputStream();

    byte[] buffer = new byte[1024];
    int index=0;
    // 4.执行写出操作
    while ((index = inputStream.read(buffer)) != -1){
        outputStream.write(buffer, 0, index);
        outputStream.flush();
    }
    outputStream.close();
    inputStream.close();
    return null;
}
```
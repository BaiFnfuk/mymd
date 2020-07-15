## javaWeb

### 1. Tomcat

1. 安装tomcat

   - 到官网下载[tomcat](https://tomcat.apache.org/download-90.cgi)的压缩包
   - 解压到/usr/local目录下，其他目录也可以
   - 启动或关闭。tomcat的bin目录下startup.sh脚本启动tomcat，shutdown.sh脚本关闭tomcat。
   - 打开浏览器访问http://ipaddress:8080，如果能显示tomcat的欢迎页面则正常

2. server.xml文件

   - tomcat的主要配置文件

   - 修改端口

     ```xml
     <Connector port="8080" protocol="HTTP/1.1"
                    connectionTimeout="20000"
                    redirectPort="8443" />
     ```

     - tomcat的默认端口号为8080
     - mysql的默认端口号为3306
     - http的默认端口号为80
     - https的默认端口号为443

   - 修改主机名

     ```xml
     <Host name="localhost"  appBase="webapps"
                 unpackWARs="true" autoDeploy="true">
     ```

     - 默认的主机名为：localhost，修改这个主机名要到修改主机的host文件
     - 默认网站应用存放的位置为：webapps

   **`高难度面试题：`**
   
   请你谈谈网站是如何进行访问的！
   
   1. 输入一个域名;回车
   
   2. 检查本机的hosts配置文件下有没有这个域名映射
   
      1. 有，直接返回对应的ip地址，这个地址中，有我们需要访问的web程序则可以直接访问
   
         ```xml
         127.0.0.1	localhost
         ```
   
      2. 没有，去DNS服务器查找，找到的话就返回，找不到就返回找不到
   
3. 发布一个web网站

   1. 将自己写的网站放到tomcat指定的web应用的文件夹（webapps）下，就可以访问了

   网站应该有的结构

   ```
   --webapps: Tomcat服务器的web目录
   	--ROOT
   	--bai :网站的目录名
   		--WEB-NF
   			--classses：java程序
   			--lib :web应用所依赖的jar包
   			--web.xml:网站配置文件
   		--index.html :默认的首页
   		--static
   			--css
   				--style.css
   			--js
   			--img
   		--...
   ```

### 2. Http

#### 2.1 什么是Http

http（超文本传输协议）是一个简单的请求-响应协议，它通常运行在TCP之上。

- 文本：html, 字符串，。。。
- 超文本：图片，音乐，视频，定位，地图。。。
- 端口80

https: 安全的

- 端口443

#### 2.2 Http请求

- 客户端---发请求---服务器

  请求：

  ```java
  Request URL: https://www.baidu.com/ 	//请求地址
  Request Method: GET						//get方法/post方法
  Status Code: 200 OK						//状态码
  Remote Address: 14.215.177.38:443		//百度的服务器地址
  ```

  1. 请求行

     - 请求行中的请求方式：GET
     - 请求方式：`Get,Post`,Head,Delete,Put,Tract...
       - get: 请求能够携带的参数比较少，大小有限制，会在浏览器的地址栏显示数据内容，不安全，但高效
       - post:请求能够携带的参数没有限制，大小没有限制，不会在浏览器的地址栏显示数据内容，安全，但不高效

  2. 消息头

     ```java
     Accept:告诉浏览器，它所支持的数据类型
     Accept-Encoding：支持那种编码格式	GBK	UTF-8	GB2312	ISO8859-1
     Accept-Language:告诉浏览器它的语言环境
     Connection:告诉浏览器请求完成是断开还是保持连接
     Cache-Control:缓存控制
     HOST:主机
     。。。
     ```

     

#### 2.3 Http响应

- 服务器---响应---客户端

  响应：

  ```java
  Cache-Control: private					//缓存控制
  Connection: keep-alive					//保持连接
  Content-Encoding: gzip					//编码
  Content-Type: text/html;charset=utf-8	//类型
  ```

  1. 响应体

     ```java
     Accept:告诉浏览器，它所支持的数据类型
     Accept-Encoding：支持那种编码格式	GBK	UTF-8	GB2312	ISO8859-1
     Accept-Language:告诉浏览器它的语言环境
     Connection:告诉浏览器请求完成是断开还是保持连接
     Cache-Control:缓存控制
     HOST:主机
     Refrush：告诉客户端多久刷新一次
     Location:让网页重新定位
     ```

  2. 响应状态码

     200：请求响应成功

     3xx：请求重定向

     - 重定向：重新定位到另外一个url,请求数据不会保留
     - 转发：将请求转到另一个url，请求数据会保留

     4xx：找不到资源 404

     - 资源不存在

     5xx:服务器代码错误 500 502（网关错误）

     `常见面试题：`

     当你的浏览器中地址栏输入地址并按下回车的一瞬间到网页能够展示出来，经历了什么？


### 3. Maven

1. 安装Maven

   - 到官网下载[Maven](http://maven.apache.org/download.cgi)压缩包
   - 解压maven压缩包，添加M2_HOME(bin目录路径)和MAVEN_HOME（maven根目录路径）环境变量

2. settings.xml

   - 配置阿里云仓库镜像

   ```xml
   <mirrors>
   	<mirror>
       	<id>nexus-aliyun</id>
       	<mirrorOf>*,!jeecg,!jeecg-snapshots</mirrorOf>
       	<name>Nexus aliyun</name>
       	<url>http://maven.aliyun.com/nexus/content/groups/public</url>
   	</mirror>
   </mirrors>
   ```

   - 创建一个本地仓库

   ```xml
   <localRepository>D:\respository</localRepository>
   ```

#### 3.1 一个完整的maven web应用的目录结构

![image-20200711224028852](C:\Users\baifn\AppData\Roaming\Typora\typora-user-images\image-20200711224028852.png)



   #### 3.2 pom.xml

1. 打包时把配置文件一起打包

```xml
<!-- 在build中配置resources,来防止我们资源导出失败问题-->
    <resources>
      <resource>
        <directory>src/main/resources</directory>
        <includes>
          <include>**/*.properties</include>
          <include>**/*.xml</include>
        </includes>
        <filtering>true</filtering>
      </resource>
      <resource>
        <directory>src/main/java</directory>
        <includes>
          <include>**/*.properties</include>
          <include>**/*.xml</include>
        </includes>
        <filtering>true</filtering>
      </resource>
    </resources>
```

#### 3.3 web.xml

- idea创建的web.xml文件的文件头是2.3版本的，太老了要修改成tomcat里的文件头

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                        http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
           version="4.0"
           metadata-complete="true">
    
  </web-app>
  ```

### 4. Servlet

#### 4.1 HelloServlet

1. 构建一个普通的Maven项目，删除里面的src目录，以后就在这个项目里面建立Module，这个空的工程就是主工程

2. 在主工程中创建一个子模块
3. maven环境优化
   - 修改web.xml为最新的
   - 将maven的结构搭建完整
4. 编写一个servlet程序
   - 编写一个普通类
   - 实现servlet接口，直接继承HttpServlet

#### 4.2 Mapping问题

1. 一个servlet可以指定多个
2. 程序的默认请求路径为`/*`
3. *前面不能加项目映射的路径：如`/hello/ *bai是不行的`
4. 一个servlet可以指定通用映射路径
5. 可以指定一些后缀或者前缀
6. 优先级问题
   - 指定了固有的映射路径优先级最高，如果没有则会走默认的处理请求;

#### 4.3 ServletContext

web容器在启动的时候，它会为每个web程序都创建一个对应的ServletContext对象，它代表了当前的web应用。

- 共享数据

  在这个Servlet中保存的数据，可以在另一个Servlet中读取

- 获取初始化参数

  ```xml
  <context-param>
  	<param-name>url</param-name>
      <param-value>jdbc:mysql://localhost:3306/school</param-value>
  </context-param>
  ```

  ```java
  ServletContext context = this.getServletContext();
  String contextPath = context.getContextPath();
  System.out.println(contextPath);
  String url = context.getInitParameter("url");
  System.out.println(url);
  ```

- 转发

  ```java
  request.getRequestDispatcher( "/hello").forward(request, response);
  ```

  - 转发浏览器地址栏上的地址不会发生变化，request和response数据会保留

- 读取资源文件

  Properties类

  ```java
  ServletContext context = this.getServletContext();
  InputStream is = context.getResourceAsStream("/WEB-INF/classes/db.properties");
  Properties properties = new Properties();
  properties.load(is);
  String url = properties.getProperty("url");
  String username = properties.getProperty("username");
  String password = properties.getProperty("password");
  resp.getWriter().write("userName:" + username + " password: " + password);
  ```

### 5. HttpServletRequest

web服务器接收到客户端的http请求，针对这个请求，分别创建一个代表请的HttpServletRequest对旬，代表响应的一个HttpServletResponse;

- 如果要获取客户端请求过来的参数，找HttpServletRequest
- 如果要给客户端响应一些信息找HttpServletResponse

后台接收乱码问题

```java
request.setCharacterEncoding("utf-8");
```



### 6. HttpServletResponse

1. 简单分类

   负责向浏览器发送数据的方法

   ```java
   ServletOutputStream outputStream = resp.getOutputStream();
   PrintWriter writer = resp.getWriter();
   ```

   负责向浏览器发送响应头的方法

2. 常见应用

   1. 向浏览器输出消息

   2. 下载文件

      - 要获取下载文件的路径

      - 下载文件的名字是啥

      - 设置想办法让浏览器能够支持下载我们需要的东西
      - 获取下载文件的输入流
      - 创建缓冲区
      - 获取OutputStream对象
      - 将FileOutputStream流写入到buffer缓冲区
      -  使用OutputStream将缓冲区中的数据输出到客户端

3. 重定向

   ```java
   resp.sendRedirect(req.getContextPath()+ "/image");
   ```

   常见场景：

   - 用户登录

   `面试题：请你聊聊重写向和转发的区别`

   - 相同点

     页面都会实现跳转

   - 不同点

     请求转发的时候，url不会产生变化

     重定向的时候，url地址栏会发生变化

### 7. Session,Cookie

#### 7.1 会话

**会话**：用户打开了浏览器，点击了很多超链接，访问多个web资源，关闭浏览器，这个过程可以称之为会话 

**有状态会话**：服务端给客户端一个cookie，客户端下次访问服务端时带上cookie

#### 7.2 保存会话的两种技术

**cookie**

- 客户端技术 （响应，请求）
- 一般会保存在本地的用户目录下

一个网站cookie是否存在上限！

- 一个cookie只能保存一个信息
- 一个web站点可以给浏览器发送多个cookie，最多存放20个cookie
- cookie大小有限制4kb
- 浏览器上限300个cookie

删除cookie：

- 不设置有效期，关闭浏览器，自动失效
- 设置有效期为0

`中文乱码`

问题最快解决办法URLEncoder.encode()编码，URLDecoder.decode()解码

**session**

- 服务器技术，利用这个技术，可以保存用户的会话信息。可以把信息或者数据放到session中 

常见：网站登录之后，你下次访问就不用再登录了

#### 7.3 Session（重点）

什么是session：

- 服务器会给每一个用户或者浏览器创建一个Session对象
- 一个Session独占一个浏览器，只要浏览器没有关闭，这个Session就存在
- 用户登录之后，整个网站它都可以访问！->保存用户信息，保存购物车的信息

session和cookie

- cookie是把用户的数据写给用户的浏览器，浏览器保存（可以保存多个）
- session把用户的数据写到用户独占session中，服务器端保存（保存重要的信息，减少服务器资源的浪费）
- session对象由服务器创建

使用场景：

- 保存一个登录用户的信息
- 购物车信息
- 在整个网站中经常会使用的数据，我们将它保存在Session中

### 8. JSP

#### 8.1 什么是JSP

java server pages : java服务器端页面，也和Servlet一样，用于动态web技术

最大的特点：

- 写jsp就像在写HTML
- 区别
  - HTML只给用户提供静态的数据
  - JSP页面中可以嵌入JAVA代码，为用户提供动态数据

#### 8.2 JSP原理

本质就是一个Servlet。

jsp内置对象

- pageContext 页面上下文对象   类型 javax.servlet.jsp.PageContext 作用域 Page
- request 请求对象　　         类型 javax.servlet.ServletRequest 作用域 Request
- response 响应对象　　       类型 javax.servlet.SrvletResponse 作用域 Page
- session 会话对象　　         类型 javax.servlet.http.HttpSession 作用域 Session
- application 应用程序对象　    类型 javax.servlet.ServletContext 作用域 Application
- out 输出对象　　            类型 javax.servlet.jsp.JspWriter 作用域 Page
- config 配置对象　　          类型 javax.servlet.ServletConfig 作用域 Page
- exception 例外对象　　　　   类型 javax.lang.Throwable 作用域 page
- page 页面对象　　　　       类型javax.lang.Object 作用域 Page

作用域：

application>session>pageContext>request

#### 8.3 JSP基础语法

**jsp表达式**

`<%=变量或表达式%>`

```jsp
<%--
作用：用来将程序的输出，输出到客户端
<%=变量或表达式%>
--%>
<%= new Date()%>
```

`<% java代码 %>`

```jsp
<%
    int sum = 0;
    for (int i = 1; i <= 100; i++) {
        sum = sum + i;
    }
    out.println("<p>sum=" + sum + "</p>");
%>
```

`<%! %> jsp声明`

```jsp
<%! 
	static{
    	Sysout.out.println("Loading Servlet!");
	}
	private void fun(){
        Sysout.out.println("进入了方法fun");
    }
%>
```

jsp声明会被编译到jsp生成的java类中（全局变量）！其他的会被生成到_jspService()（局部变量）方法中。

#### 8.4 jsp指令

`<%@page %>`：设置页面的一些信息，如错误页面等

`<%@include file=""%>`：包含一个页面,一般不用这个

`<%@import %>`：导入java包

#### 8.5 jsp标签，JSTL标签，EL表达式

`EL表达式：${}`

- 获取数据
- 执行运算
- 获取web开发的常用对象

```xml
 <!-- el表达式 -->
        <dependency>
            <groupId>javax.servlet.jsp.jstl</groupId>
            <artifactId>jstl-api</artifactId>
            <version>1.2</version>
            <exclusions>
                <exclusion>
                    <groupId>javax.servlet.jsp</groupId>
                    <artifactId>jsp-api</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>javax.servlet</groupId>
                    <artifactId>servlet-api</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <!--jstl标签库-->
        <dependency>
            <groupId>org.apache.taglibs</groupId>
            <artifactId>taglibs-standard-impl</artifactId>
            <version>1.2.5</version>
        </dependency>
```

` jsp标签`

1. 转发

   ```jsp
   <jsp:forward page="index.jsp">
       <jsp:param name="userName" value="bai"/>
       <jsp:param name="password" value="123456"/>
   </jsp:forward>
   
   <%--http://localhost:8080/index.jsp?userName=bai&passwrod=123456 --%>
   
   用户名：<%=request.getParameter("userName")%>
   用户密码：<%=request.getParameter("password")%>
   ```

2. 包含其他页面

   ```jsp
   <jsp:include page=""></jsp:include>
   ```

`JSTL表达式`

JSTL标签库的使用就是为了弥补HTML标签的不足，它自定义许多标签，可以供我们使用，标签的功能和java代码一样

- 核心标签（掌握部分）

  ```jsp
  <%-- 引入jstl标签核心库 --%>
  <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
  ```

  `<c:if />`

  ```jsp
  <c:if test="${param.userName} == 'admin'" var="isadmin">
      <c:out value="管理员欢迎你">
      </c:out>
  </c:if>
  ```

  `<c:set var='key' value='value'/>`

  `<c:choose><c:when>`

  ```jsp
  <c:set var="score" value="85"/>
  
  <c:choose>
      <c:when test="${score>=90}">
          你的成绩为优秀
      </c:when>
      <c:when test="${score>=80}">
          你的成绩一般
      </c:when>
      <c:when test="${score>=70}">
          你的成绩良好
      </c:when>
      <c:when test="${score<=60}">
          你的成绩不合格
      </c:when>
  </c:choose>
  ```

  `<c:forEach>`

  ```jsp
  <%
      ArrayList<String> people = new ArrayList<>();
      people.add("张三");
      people.add("李四");
      people.add("王五");
      people.add("赵六");
      people.add("钱七");
      request.setAttribute("list", people);
  %>
  <ul>
      <%-- var:每次遍历出来的变量 items:要遍历的对象  varstatus:遍历的状态--%>
     <ul>
      	<c:forEach var="people" items="${list}" varStatus="statu">
          <li>${statu.index}:${people}</li><br>
      	</c:forEach>
  	</ul>
  </ul>
  ```

### 9. JavaBean

实体类

JavaBean有特定的写法

- 必须有一个无参构造
- 属性必须私有化
- 必须有对应的get/set方法

一般用来和数据库的字段做映射ORM;

ORM：对象关系映射

- 表-->类

- 字段-->属性

- 行记录-->对象 

  |  id  | name  | age  | address |
  | :--: | :---: | :--: | :-----: |
  |  1   | bai01 |  3   |  广东   |
  |  2   | bai02 |  4   |  广东   |
  |  3   | bai03 |  5   |  广东   |

  ```java
  class People{
      private int id;
      private String name;
      private int age;
      private String address;
    
      ...
  } 
  
  
  ```

### 10. MVC三层架构

什么是MVC: Model	View	Controller 模型，视图，控制器

Model

- 业务处理：业务逻辑（Service）
- 数据持久层：CRUD (Dao)

View

- 展示数据
- 提供链接发起servlet请求

Controller (Servlet)

- 接收用户的请求：（req:请求参数，Session信息）
- 交给业务层处理
- 控制视图的跳转

### 11. Filter

拦截请求，过滤信息

```java
public class CharacterEncodingFilter implements Filter {

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        request.setCharacterEncoding("UTF-8");
        response.setCharacterEncoding("UTF-8");
        response.setContentType("text/html;charset=UTF-8");
        System.out.println("filter执行前");
        // 放行，不放行程序会卡在这里
        chain.doFilter(request, response);  
        System.out.println("filter执行后");
    }

    @Override
    public void destroy() {

    }
}

```

```xml
<filter>
	<filter-name>characterEncoding</filter-name>
    <filter-class>com.bai.filter.CharacterEncodingFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>characterEncoding</filter-name>
    <url-pattern>/*</url-pattern>  //全路径过滤，尽量不要这样写
</filter-mapping>
```

### 12. 监听器(已经很少用)

```java
public class OnlineCountListener implements HttpSessionListener {

    @Override
    public void sessionCreated(HttpSessionEvent se) {
        HttpSession session = se.getSession();
        ServletContext context = session.getServletContext();
        Integer onlineCount = (Integer) context.getAttribute("OnlineCount");
        if (onlineCount == null){
            onlineCount = 1;
        }else {
            onlineCount = onlineCount + 1;
        }

        context.setAttribute("OnlineCount", onlineCount);

    }

    @Override
    public void sessionDestroyed(HttpSessionEvent se) {
        HttpSession session = se.getSession();
        ServletContext context = session.getServletContext();
        Integer onlineCount = (Integer) session.getAttribute("OnlineCount");
        if (onlineCount == null){
            onlineCount = 0;
        }else {
            onlineCount = onlineCount - 1;
        }
        context.setAttribute("OnlineCount", onlineCount);
    }
}
```

```xml
<listener>
	<listener-class>com.bai.listener.OnlineCountListener</listener-class>
</listener>
```














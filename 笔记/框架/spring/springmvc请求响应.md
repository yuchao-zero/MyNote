

## springmvc

### 服务器三层架构的表现层：springmvc

### 概念：

MVC 全名是 Model View Controller，是模型(model)-视图(view)-控制器(controller) 的缩写， 是一种用于设计创建 Web 应用程序表现层的模式。

model即javabean的对象，封装数据

view即jsp或html技术，展示数据

controller即控制器，servlet等，处理请求

### 作用：

浏览器 ------> 发送请求，请求参数------>表现层

表现层------>响应结果------>浏览器

<font color=red>springmvc的核心即请求和响应</font>

### springmvc开发流程：

1. 搭建开发环境

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   
   <project xmlns="http://maven.apache.org/POM/4.0.0"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
     <modelVersion>4.0.0</modelVersion>
   
     <groupId>club.banyuan</groupId>
     <artifactId>springmvc</artifactId>
     <version>1.0-SNAPSHOT</version>
     <packaging>war</packaging>
   
     <name>springmvc Maven Webapp</name>
     <!-- FIXME change it to the project's website -->
     <url>http://www.example.com</url>
   
     <properties>
       <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
       <maven.compiler.source>1.8</maven.compiler.source>
       <maven.compiler.target>1.8</maven.compiler.target>
       <!--    版本锁定-->
       <spring.version>5.0.2.RELEASE</spring.version>
     </properties>
   
     <dependencies>
       <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
         <version>4.11</version>
         <scope>test</scope>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-core</artifactId>
         <version>${spring.version}</version>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-beans</artifactId>
         <version>${spring.version}</version>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-context</artifactId>
         <version>${spring.version}</version>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-web</artifactId>
         <version>${spring.version}</version>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-webmvc</artifactId>
         <version>${spring.version}</version>
       </dependency>
       <dependency>
         <groupId>javax.servlet</groupId>
         <artifactId>javax.servlet-api</artifactId>
         <version>4.0.0</version>
         <scope>provided</scope>
       </dependency>
       <dependency>
         <groupId>javax.servlet.jsp</groupId>
         <artifactId>javax.servlet.jsp-api</artifactId>
         <version>2.3.3</version>
         <scope>provided</scope>
       </dependency>
     </dependencies>
   
     <build>
       <finalName>mvnWeb</finalName>
       <plugins>
         <plugin>
           <groupId>org.apache.tomcat.maven</groupId>
           <artifactId>tomcat7-maven-plugin</artifactId>
           <version>2.2</version>
           <configuration>
             <port>8080</port>
             <path>/</path>
           </configuration>
         </plugin>
       </plugins>
     </build>
   </project>
   
   
   ```

2. 在web.xml文件中配置springmvc的核心控制器

   ```
   <!DOCTYPE web-app PUBLIC
     "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
     "http://java.sun.com/dtd/web-app_2_3.dtd" >
   
   <web-app>
     <display-name>Archetype Created Web Application</display-name>
     <!-- 配置 spring mvc 的核心控制器 -->
     <servlet>
       <servlet-name>SpringMVCDispatcherServlet</servlet-name>
       <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
       <!-- 配置初始化参数，用于读取 SpringMVC 的配置文件 -->
       <init-param>
         <param-name>contextConfigLocation</param-name>
         <param-value>classpath:SpringMVC.xml</param-value>
       </init-param>
       <!-- 配置 servlet 的对象的创建时间点：应用加载时创建。
         取值只能是非 0 正整数，表示启动顺序 -->
         <!-- 启动服务器，创建该-->
       <load-on-startup>1</load-on-startup>
     </servlet>
     <servlet-mapping>
       <servlet-name>SpringMVCDispatcherServlet</servlet-name>
       <url-pattern>/</url-pattern>
     </servlet-mapping>
   </web-app>
   
   ```

3. 在resources目录下创建springmvc.xml配置文件

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
     xmlns:mvc="http://www.springframework.org/schema/mvc"
     xmlns:context="http://www.springframework.org/schema/context"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://www.springframework.org/schema/beans
              http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/mvc
           http://www.springframework.org/schema/mvc/spring-mvc.xsd
            http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd">
     <!-- 配置创建 spring 容器要扫描的包 -->
     <context:component-scan base-package="club.banyuan"></context:component-scan>
     
     <!-- 配置视图解析器 -->
     <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
       <property name="prefix" value="/WEB-INF/pages/"></property>
       <property name="suffix" value=".jsp"></property>
     </bean>
   </beans>
   
   ```

### 解析springmvc的工作流程：

![image-20200804235607591](/Users/edz/Library/Application Support/typora-user-images/image-20200804235607591.png)

1. 启动服务器，加载配置文件：
   - DispatcherServlet对象创建
   - Springmvc.xml被加载
   - HelloController创建成对象
2. 发送请求，后台处理请求
3. 返回数据

### mvc:annotation-driven说明：

在 SpringMVC 的各个组件中，处理器映射器、处理器适配器、视图解析器称为SpringMVC 的三大组件。使用<mvc:annotation-driven> 自动加载 RequestMappingHandlerMapping(处理映射器)和 RequestMappingHandlerAdapter ( 处 理 适 配 器 ) ， 可 用 在 SpringMVC.xml 配 置 文 件 中 使 用 <mvc:annotation-driven>替代注解处理器和适配器的配置。

### RequestMapping注解：

1. 注解置于类上方，用于区分某一模块

   ```
   @Controller
   @RequestMapping("/user")
   public class HelloController {
   
     @RequestMapping("/hello")
     public String hello(){
       System.out.println("hello ----- controller");
       return "hello";
     }
   
     @RequestMapping("/success")
     public String success(){
       System.out.println("success");
       return "success";
     }
   }
   ```

2. Index.jsp页面请求路径需要加上模块路径

   ```
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   <h3>入门程序</h3>
   <a href="user/hello">hello ------ controller</a>
   <a href="user/success">success</a>
   </body>
   </html>
   
   ```

3. 属性：

   - value:用于指定请求的 URL。它和 path 属性的作用是一样的。

   - method:用于指定请求的方式。

     ```
     /**
     * 保存账户
     * @return
     */ @RequestMapping(value="/saveAccount",method=RequestMethod.POST)
     public String saveAccount() { System.out.println("保存了账户"); return "success";
     }
     
     ```

     Jsp:

     ```
     <!-- 请求方式的示例 -->
     <a href="account/saveAccount">保存账户，get 请求</a> <br/> <form action="account/saveAccount" method="post"> <input type="submit" value=" 保存账户， post
     请求 "> </form>
     ```

   - params:用于指定限制请求参数的条件。它支持简单的表达式。要求请求参数的 key 和value 必须和 配置的一模一样。

     ```
     /**
     * 删除账户 * @return
     */
     @RequestMapping(value="/removeAccount",params={"accountName","money>10 0"})
     public String removeAccount() { System.out.println("删除了账户"); return "success";
     }
     ```

     Jsp:

     ```
     <!-- 请求参数的示例 -->
     <a href="account/removeAccount?accountName=aaa&money>100">删除账户，金 额 100</a>
     <br/>
     <a href="account/removeAccount?accountName=aaa&money>150">删除账户，金 额 150</a>
     ```

   - headers:用于指定限制请求消息头的条件。

     <font color=red>注意: 以上四个属性只要出现 2 个或以上时，他们的关系是与的关系。</font>

### 解决中文乱码的方案：

在web.xml文件中配置过滤器

```
<!--  配置解决中文乱码的过滤器-->
  <filter>
    <filter-name>characterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>characterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
```

### 静态资源配置：

```
在springmvc.xml配置文件中配置
<!--
        静态资源，不需要经过controller
        location : 静态资源的在服务器上的物理路径
        mapping ：浏览器访问静态资源的请求路径
    -->
  <mvc:resources mapping="images/**" location="images/"/>
```



### 解决EL表达式在jsp页面无法显示的方案：

```
<!-- 在jsp页面头部添加isELIgnored="false"属性-->
<%@ page isELIgnored="false" contentType="text/html;charset=UTF-8" language="java" %>
```

### 自定义类型转换器：

1. 在utils包路径下创建工具类StringToDateConverter.java

   ```
   public class StringToDateConverter implements Converter<String, Date> {
     
     @Override
     public Date convert(String s) {
       if (s == null) {
         throw new RuntimeException("不能为空");
       }
       DateFormat df = new SimpleDateFormat("yyyy-MM-dd");
   //    字符串转换日期
       try {
         return df.parse(s);
       } catch (Exception e) {
         throw new RuntimeException("数据转换失败");
       }
     }
   }
   ```

2. 在配置文件springmvc.xml中配置类型转换器

   ```
   <!-- 配置类型转换器工厂 -->
     <bean id="converterService"
       class="org.springframework.context.support.ConversionServiceFactoryBean"> <!-- 给工厂注入一个新的类型转换器 -->
       <property name="converters">
         <array>
           <!-- 配置自定义类型转换器 -->
           <bean class="club.banyuan.utils.StringToDateConverter"></bean>
         </array>
       </property>
     </bean>
     <mvc:annotation-driven conversion-service="converterService"/>
   ```

### springmvc中的常用注解：

1. RequestParam

   - 作用：把请求中指定名称的参数给控制器中的形参赋值。避免参数名称不一致导致后端拿不到数据。

   - 属性：

     - value：请求参数中的名称。
     - required：请求参数中是否必须提供此参数。默认值:true。表示必须提供，如果不提供将报错。

   - 使用案例：

     - jsp代码：

       ```
       <a href="test/testRequestParam?name=test1" >测试RequestParam</a>
       ```

     - 控制器代码：

       ```
       @Controller
       @RequestMapping("/test")
       public class TestAnnotation {
       
         @RequestMapping("/testRequestParam")
         public String testRequestParam(@RequestParam("name")String username){
           System.out.println(username);
           return "hello";
         }
       }
       ```

   - 解决中文乱码的方案：

     - 修改pom.xml文件下的tomcat插件

       ```
       <build>
           <finalName>mvnWeb</finalName>
           <plugins>
             <plugin>
               <groupId>org.apache.tomcat.maven</groupId>
               <artifactId>tomcat7-maven-plugin</artifactId>
               <version>2.2</version>
               <configuration>
                 <port>8080</port>
                 <path>/</path>
                 <!-- 添加编码配置-->
                 <uriEncoding>UTF-8</uriEncoding>
               </configuration>
             </plugin>
           </plugins>
         </build>
       ```

     - 

2. RequestBody

   - 作用：用于获取请求体内容。直接使用得到是 key=value&key=value...结构的数据。<font color=red> get 请求方式不适用</font>。

   - 属性：

     - required:是否必须有请求体。默认值是：true。当取值为 true 时，get 请求方式会报错。如果取值 为 false，get 请求得到是 null。

   - 解决中文乱码问题的方案

     - 在web.xml文件中配置过滤器

       ```
       <!--  配置解决中文乱码的过滤器-->
         <filter>
           <filter-name>characterEncodingFilter</filter-name>
           <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
           <init-param>
             <param-name>encoding</param-name>
             <param-value>UTF-8</param-value>
           </init-param>
           <init-param>
             <param-name>forceEncoding</param-name>
             <param-value>true</param-value>
           </init-param>
         </filter>
         <filter-mapping>
           <filter-name>characterEncodingFilter</filter-name>
           <url-pattern>/*</url-pattern>
         </filter-mapping>
       ```

     - 控制器java文件中要进行转码

       ```
       @RequestMapping(value = "/testRequestBody", method = RequestMethod.POST, produces = "text/html;charset=UTF-8")
         public String testRequestBody(@RequestBody String body) throws UnsupportedEncodingException {
       // 转码  
           System.out.println(URLDecoder.decode(body, "utf-8"));
           return "hello";
         }
       ```

3. Pathvariable

   - 作用：用于绑定 url 中的占位符。例如：请求 url 中/testPathVariable/{id}，这个{id}就是 url 占位符。 url 支持占位符是 spring3.0 之后加入的。是 springmvc 支持 rest 风格 URL 的一个重要标志。

   - 属性：

     - value：用于指定 url 中占位符名称。
     - required：是否必须提供占位符。

   - 使用案例：

     - 控制器java代码

       ```
       @RequestMapping("/testPathVariable/{id}")
         public String testPathVariable(@PathVariable(value = "id") Integer id) {
           System.out.println(id);
           return "hello";
         }
       ```

     - jsp页面

       ```
       <%--PathVariable注解--%>
       <a href="test/testPathVariable/20" >测试PathVariable</a>
       ```

   - REST风格URL

     - REST(英文:Representational State Transfer，简称 REST)描述了一个架构样式的网络系统， 比如 web 应用程序。

     - restful的示例：

       /account/1  HTTP GET : 得到 id = 1 的 account

       /account/1  HTTP DELETE: 删除 id = 1 的 account

       /account/1  HTTP PUT: 更新 id = 1 的 account

       /account      HTTP POST: 新增 account

     - 基于 **HiddentHttpMethodFilter** 的示例

       - 作用：由于浏览器 form 表单只支持 GET 与 POST 请求，而 DELETE、PUT 等 method 并不支持，Spring3.0 添加了一个过滤器，可以将浏览器请求改为指定的请求方式，发送给我们的控制器方法，使得支持 GET、POST、PUT 与 DELETE 请求。

       - 使用方法：

         1. 在web.xml中配置过滤器

            ```
            <filter>
                    <filter-name>hiddenHttpMethodFilter</filter-name>
                    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
                </filter>
            <filter-mapping>
                    <filter-name>hiddenHttpMethodFilter</filter-name>
                    <url-pattern>/*</url-pattern>
                </filter-mapping>
            ```

         2. 请求方式必须为Post请求

            ```
            <%--测试REST风格URL--%>
            <!-- 保存 -->
            <form action="test/testRestPOST" method="post">
                用户名称:<input type="text" name="username"><br/>
                <input type="hidden" name="_method" value="POST">
                <input type="submit" value=" 保存 ">
            </form>
            <hr/>
            <!-- 更新 -->
            <form action="test/testRestPUT/1" method="post">
                用户名称:<input type="text" name="username"><br/>
                <input type="hidden" name="_method" value="PUT">
                <input type="submit" value=" 更新 ">
            </form>
            <hr/>
            <!-- 删除 -->
            <form action="test/testRestDELETE/1" method="post">
                <input type="hidden" name="_method" value="DELETE">
                <input type="submit" value=" 删除 ">
            </form>
            <hr/>
            <!-- 查询一个 -->
            <form action="test/testRestGET/1" method="post">
                <input type="hidden" name="_method" value="GET">
                <input type="submit" value=" 查询 ">
            </form>
            ```

         3. 控制器java代码

            ```
            /**
               * post请求 保存
               *
               * @param user
               * @return
               */
              @RequestMapping(value = "/testRestPOST", method = RequestMethod.POST, produces = "text/html;charset=UTF-8")
              public String testRestPOST(User user) throws UnsupportedEncodingException {
                System.out.println("rest post" + URLDecoder.decode(user.toString(), "utf-8"));
                return "hello";
              }
            
              /**
               * post请求 更新
               *
               * @param id
               * @param user
               * @return
               */
              @RequestMapping(value = "/testRestPUT/{id}", method = RequestMethod.PUT)
              public String testRestfulURLPUT(@PathVariable("id") Integer id, User user) {
                System.out.println("rest put " + id + "," + user);
                return "hello";
              }
            
              /**
               * post请求 删除
               *
               * @param id
               * @return
               */
              @RequestMapping(value = "/testRestDELETE/{id}", method = RequestMethod.DELETE)
              public String testRestfulURLDELETE(@PathVariable("id") Integer id) {
                System.out.println("rest delete " + id);
                return "hello";
              }
            
              /**
               * post请求 查询
               *
               * @param id
               * @return
               */
              @RequestMapping(value = "/testRestGET/{id}", method = RequestMethod.GET)
              public String testRestfulURLGET(@PathVariable("id") Integer id) {
                System.out.println("rest get " + id);
                return "hello";
              }
            ```

4. RequestHeader

   - 作用：用于获取请求消息头。

   - 属性：

     - value：提供消息头名称
     - required：是否必须有此消息头 <font color=blue>注: 在实际开发中一般不怎么用</font>。

   - 使用实例：

     - jsp页面

       ```
       <!-- RequestHeader 注解 -->
       <a href="test/testRequestHeader">获取请求消息头</a>
       ```

     - 控制器java代码

       ```
       @RequestMapping("/testRequestHeader")
         public String testRequestHeader(@RequestHeader(value = "Accept-Language",
             required = false) String requestHeader) {
           System.out.println(requestHeader);
           return "hello";
         }
       ```

5. CookieValue

   - 作用：用于把指定 cookie 名称的值传入控制器方法参数。

   - 属性：

     - value：指定cookie的名称
     - required：是否必须有此cookie

   - 使用实例：

     - jsp页面：

       ```
       <!-- CookieValue 注解 -->
       <a href="test/testCookieValue">绑定cookie的值</a><br/>
       ```

     - 控制器java代码

       ```
       @RequestMapping("/testCookieValue")
         public String testCookieValue(
             @CookieValue(value = "JSESSIONID", required = false) String cookieValue) {
           System.out.println(cookieValue);
           return "hello";
         }
       ```

6. ModelAttribute

   - 作用：该注解是 SpringMVC4.3 版本以后新加入的。它可以用于修饰方法和参数。 出现在方法上，表示当前方法会在控制器的方法执行之前，先执行。它可以修饰没有返回值的方法，也可以修饰有具体返回值的方法。 出现在参数上，获取指定的数据给参数赋值。

   - 属性：value:用于获取数据的 key。key 可以是 POJO 的属性名称，也可以是 map 结构的 key。

   - 应用场景：当表单提交数据不是完整的实体类数据时，保证没有提交数据的字段使用数据库对象原来的数据。

   - 使用实例1：基于pojo属性的基本使用

     - jsp页面：

       ```
       <!-- ModelAttribute 注解的基本使用 -->
       <a href="test/testModelAttribute?username=test">测试modelAttribute</a>
       ```

     - 控制器java代码

       ```
       @ModelAttribute
         public void showModel(User user) {
           System.out.println("执行了 showModel 方法" + user.getUsername());
         }
         @RequestMapping("/testModelAttribute")
         public String testModelAttribute(User user) {
           System.out.println("执行了控制器的方法" + user.getUsername());
           return "hello";
         }
       ```

    - 使用实例2：基于 Map 的应用场景示例1：ModelAttribute 修饰方法带返回值

      <font color=red>当表单提交数据不是完整的实体类数据时，保证没有提交数据的字段使用数据库对象原来的数据。</font>

       - jsp页面：

         ```
         <!-- 修改用户信息 -->
         <form action="test/updateUser" method="post">
             用户名称:<input type="text" name="username"><br/>
             用户年龄:<input type="text" name="age"><br/>
             <input type="submit" value=" 保存 ">
         </form>
         ```

      - 控制器Java代码

        ```
        @ModelAttribute
          public User showModel(String username) { //模拟去数据库查询
            User abc = findUserByName(username);
            System.out.println("执行了 showModel 方法" + abc);
            return abc;
          }
        
          @RequestMapping("/updateUser")
          public String testModelAttribute2(User user) {
            System.out.println("控制器中处理请求的方法:修改用户:" + user);
            return "hello";
          }
        
          private User findUserByName(String username) {
            User user = new User();
            user.setUsername(username);
            user.setAge(19);
            user.setPassword("123456");
            return user;
          }
        ```

7. SessionAttribute

   - 作用：用于多次执行控制器方法间的参数共享。

   - 属性：

     - value：用于指定存入的属性名称
     - type：用于指定存入的数据类型。

     

   - 使用实例：

     - jsp页面

       ```
     <!-- SessionAttribute 注解的使用 -->
       <a href="SessionAttribute/testPut">存入 SessionAttribute</a> <hr/>
       <a href="SessionAttribute/testGet">取出 SessionAttribute</a> <hr/>
       <a href="SessionAttribute/testClean">清除 SessionAttribute</a>
       ```
       
     - 控制器Java代码
     
       ```
       package club.banyuan.controller;
       
       import org.springframework.stereotype.Controller;
       import org.springframework.ui.Model;
       import org.springframework.ui.ModelMap;
       import org.springframework.web.bind.annotation.RequestMapping;
       import org.springframework.web.bind.annotation.SessionAttributes;
       import org.springframework.web.bind.support.SessionStatus;
       
       /**
        * @author :yu
        * @description : TODO
        * @date :2020/8/6 16:41
        */
       @Controller
       @RequestMapping("/SessionAttribute")
       @SessionAttributes(value = {"username", "password"}, types = {Integer.class})
       public class SessionAttributeController {
       
         /**
          * 把数据存入 SessionAttribute
          *
          * @param model
          * @return Model 是 spring 提供的一个接口，该接口有一个实现类 ExtendedModelMap 该类继承了 ModelMap，而 ModelMap 就是
          * LinkedHashMap 子类
          */
         @RequestMapping("/testPut")
         public String testPut(Model model) {
           model.addAttribute("username", "泰斯特");
           model.addAttribute("password", "123456");
           model.addAttribute("age", 31);
       //跳转之前将数据保存到 username、password和age中，
       // 因为注解@SessionAttribute中有这几个参数
           return "hello";
         }
       
         @RequestMapping("/testGet")
         public String testGet(ModelMap model) {
           System.out.println(model.get("username") + ";" +
               model.get("password") + ";" + model.get("age"));
           return "hello";
         }
       
         @RequestMapping("/testClean")
         public String complete(SessionStatus sessionStatus) {
           sessionStatus.setComplete();
           return "hello";
         }
       }
       
       
       
       ```
     
       
     
     
     
     

​     




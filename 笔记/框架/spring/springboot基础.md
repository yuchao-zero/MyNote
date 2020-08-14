## springboot基础

### 简介：

1. spring优点：

   Spring 是 Java 企业版(Java Enterprise Edition，JEE，也称 J2EE)的轻量级代替品。 无需开发重量级的 Enterprise JavaBean(EJB)，Spring 为企业级 Java 开发提供了一种 相对简单的方法，通过依赖注入和面向切面编程，用简单 的 Java 对象(Plain Old Java Object，POJO)实现了 EJB 的功能。

2. spring缺点：

   虽然 Spring 的组件代码是轻量级的，但它的配置却是重量级的。一开始，Spring 用 XML 配置，而且是很多 XML 配 置。Spring 2.5 引入了基于注解的组件扫描，这消除了大量针 对应用程序自身组件的显式 XML 配置。Spring 3.0 引入 了基于 Java 的配置，这是一种 类型安全的可重构配置方式，可以代替 XML。 所有这些配置都代表了开发时的损耗。因为在思考 Spring 特性配置和解决业务问题之间需 要进行思维切换，所以编 写配置挤占了编写应用程序逻辑的时间。和所有框架一样，Spring 实用，但与此同时它要求的回报也不少。 除此之外，项目的依赖管理也是一件耗时耗力的事情。在环境搭建时，需要分析要导入哪些 库的坐标，而且还需要 分析导入与之有依赖关系的其他库的坐标，一旦选错了依赖的版本， 随之而来的不兼容问题就会严重阻碍项目的开 发进度。

3. springboot：

   1. 用于解决spring的缺点
   2. springboot提供了一种快速使用 Spring 的方式

4. springboot的核心功能：

   1. 起步依赖：

      起步依赖本质上是一个 Maven 项目对象模型(Project Object Model，POM)，定义了对其他库的传递依赖，这些东西加在一起即支持某项功能。 简单的说，起步依赖就是将具备某种功能的坐标打包到一起，并提供一些默认的功能。

   2. 自动配置：

      Spring Boot 的自动配置是一个运行时(更准确地说，是应用程序启动时)的过程，考虑了众多因素，才决定 Spring 配置应该用哪个，不该用哪个。该过程是 Spring 自动完成的。
      
### 入门案例：

1. 创建maven工程

2. 添加springboot的<font color=red>起步依赖</font>

   SpringBoot 要求，项目要继承 SpringBoot 的起步依赖 spring-boot-starter-parent:

   ```
   <parent> 
   <groupId>org.springframework.boot</groupId> 
   <artifactId>spring-boot-starter-parent</artifactId> 
   <version>2.0.1.RELEASE</version>
   </parent>
   ```

   SpringBoot 要集成 SpringMVC 进行 Controller 的开发，所以项目要导入 web 的启动依赖

   ```
       <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
   
   ```

3. 编写springboot的引导类

   要通过 SpringBoot 提供的引导类起步 SpringBoot 才可以进行访问

   ```
   package club.banyuan;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   @SpringBootApplication
   public class MySpringBootApplication {
   public static void main(String[] args) { SpringApplication.run(MySpringBootApplication.class);
   } }
   ```

4. 编写controller

   在引导类 MySpringBootApplication 同级包或者子级包中创建 QuickStartController

   ```
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestMapping; import org.springframework.web.bind.annotation.ResponseBody;
   @Controller
   public class QuickStartController {
   @RequestMapping("/quick") @ResponseBody
   public String quick(){
   return "springboot 访问成功!";
   } }
   ```

5. 执行 SpringBoot 起步类的主方法

### 代码解析：

1. springboot代码解析：

   - @SpringBootApplication:标注 SpringBoot 的启动类，该注解具备多种功能
   - SpringApplication.run(MySpringBootApplication.class) 代表运行 SpringBoot 的 启动类，参数为 SpringBoot 启动类的字节码对象

2. springboot工程热部署

   ```
   <!--热部署配置--> 
   <dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-devtools</artifactId> 
   </dependency>
   ```

3. spring-boot-start-parent

   内置依赖spring-boot-starter-dependencies，一部分坐标 的版本、依赖管理、插件管 理已经定义好，所以我们的 SpringBoot 工程继承 spring-boot-starter-parent 后已经具备版本锁定等配置了。所以起步依赖的作用就是进行依赖的传递。

4. spring-boot-starter-web

   spring-boot-starter-web 就是将 web 开发要使用的 spring-web、spring-webmvc 等 坐标进行了“打包”，这样我们的工程只要引入 spring-boot-starter-web 起步依赖的 坐 标就可以进行 web 开发了，同样体现了依赖传递的作用。

### springboot的配置文件

1.  <font color=red>SpringBoot 默认会从 Resources 目录下加载 application.properties 或 application.yml (application.yaml)文件</fon>
2. YML 文件格式是 YAML (YAML Aint Markup Language)编写的文件格式，YAML 是一种 直观的能够被电脑识别的的数 据数据序列化格式，并且容易被人类阅读，容易和脚本语言 交互的，可以被支持 YAML 库的不同的编程语言程序导 入，比如: C/C++, Ruby, Python, Java, Perl, C#, PHP 等。YML 文件是以数据为核心的，比传统的 xml 方式更加简 洁。 YML 文件的扩展名可以使用.yml 或者.yaml。

### 配置文件与配置类的属性映射方式

1. 使用注解@value映射

   application.yml 配置如下:

   ```
   person:
   	name: zhangsan 
   	age: 18
   ```

   实体 Bean 代码如下:

   ```
   @Controller
   public class QuickStartController {
   
   @Value("${person.name}") 
   private String name; 
   
   @Value("${person.age}") 
   private Integer age;
   
   @RequestMapping("/quick") 
   @ResponseBody
   public String quick(){
   return "springboot 访问成功! name="+name+",age="+age; }
   }
   ```

2. 使用注解@ConfigurationProperties 映射

   实体 Bean 代码如下:

   ```
   @Controller 
   @ConfigurationProperties(prefix = "person") 
   public class QuickStartController {
   
   private String name; 
   private Integer age;
   
   @RequestMapping("/quick") 
   @ResponseBody 
   public String quick(){ 
   return "springboot 访问成功! name="+name+",age="+age; 
   }
   public void setName(String name) { 
   this.name = name; 
   } 
   public void setAge(Integer age) { 
   this.age = age; 
   } 
   }
   ```

   <font color=red>注意:使用@ConfigurationProperties 方式可以进行配置文件与实体字段的自动映射，但 需要字段必须提供 set 方 法才可以，而使用@Value 注解修饰的字段不需要提供 set 方法</font>

### springboot整个mybatis

1. 添加 Mybatis 的起步依赖

   ```
   <dependency>
         <groupId>org.mybatis.spring.boot</groupId>
         <artifactId>mybatis-spring-boot-starter</artifactId>
         <version>1.1.1</version>
       </dependency>
   ```

2. 添加数据库驱动坐标

   ```
   <dependency>
         <groupId>mysql</groupId>
         <artifactId>mysql-connector-java</artifactId>
         <version>8.0.20</version>
       </dependency>
   ```

3. 在 application.properties 中添加数据量的连接信息

   ```
   spring:
     datasource:
       driverClassName: com.mysql.cj.jdbc.Driver
       url: jdbc:mysql://localhost:3306/shoppingstreet?&useSSL=false&serverTimezone=UTC
       username: root
       password: ych417130
   ```

4. 创建表单

5. 创建实体类

6. 编写持久层接口

7. 配置mapper映射文件

8. 在配置文件中添加mybatis信息

   ```
   mybatis:
   #spring 集成 Mybatis 环境
   #pojo 别名扫描包
     type-aliases-package: club.banyuan.entity
     #加载 Mybatis 映射文件
     mapper-locations: classpath:club/banyuan/mapper/*Mapper.xml
   ```

9. 编写表现层控制器代码

### 补充：

1. springboot自动集成了Tomcat依赖，默认端口号8080

​    

​      
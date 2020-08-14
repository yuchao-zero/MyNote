### 单一模块构建：

1. 创建maven项目

2. 配置pom.xml文件

   ```
   <packaging>war</packaging>
   
       <properties>
           <junit.version>4.12</junit.version>
           <spring.version>5.2.7.RELEASE</spring.version>
           <mybatis.version>3.5.5</mybatis.version>
           <mybatis.spring.version>2.0.5</mybatis.spring.version>
           <!--
           <mysql.version>8.0.20</mysql.version>
           -->
           <mysql.version>8.0.19</mysql.version>
           <slf4j.version>1.6.4</slf4j.version>
           <jstl.version>1.2</jstl.version>
           <servlet-api.version>2.5</servlet-api.version>
           <jsp-api.version>2.0</jsp-api.version>
           <c3p0.version>0.9.5.2</c3p0.version>
       </properties>
   
       <dependencyManagement>
           <dependencies>
               <!-- 单元测试 -->
               <dependency>
                   <groupId>junit</groupId>
                   <artifactId>junit</artifactId>
                   <version>${junit.version}</version>
                   <scope>test</scope>
               </dependency>
               <!-- 日志处理 -->
               <dependency>
                   <groupId>org.slf4j</groupId>
                   <artifactId>slf4j-log4j12</artifactId>
                   <version>${slf4j.version}</version>
               </dependency>
               <!-- Mybatis -->
               <dependency>
                   <groupId>org.mybatis</groupId>
                   <artifactId>mybatis</artifactId>
                   <version>${mybatis.version}</version>
               </dependency>
               <dependency>
                   <groupId>org.mybatis</groupId>
                   <artifactId>mybatis-spring</artifactId>
                   <version>${mybatis.spring.version}</version>
               </dependency>
               <!-- MySql -->
               <dependency>
                   <groupId>mysql</groupId>
                   <artifactId>mysql-connector-java</artifactId>
                   <version>${mysql.version}</version>
               </dependency>
               <!-- 数据库连接池 -->
               <dependency>
                   <groupId>com.mchange</groupId>
                   <artifactId>c3p0</artifactId>
                   <version>${c3p0.version}</version>
               </dependency>
   
               <!-- Spring -->
               <dependency>
                   <groupId>org.springframework</groupId>
                   <artifactId>spring-context</artifactId>
                   <version>${spring.version}</version>
               </dependency>
               <dependency>
                   <groupId>org.springframework</groupId>
                   <artifactId>spring-beans</artifactId>
                   <version>${spring.version}</version>
               </dependency>
               <dependency>
                   <groupId>org.springframework</groupId>
                   <artifactId>spring-webmvc</artifactId>
                   <version>${spring.version}</version>
               </dependency>
               <dependency>
                   <groupId>org.springframework</groupId>
                   <artifactId>spring-jdbc</artifactId>
                   <version>${spring.version}</version>
               </dependency>
               <dependency>
                   <groupId>org.springframework</groupId>
                   <artifactId>spring-aspects</artifactId>
                   <version>${spring.version}</version>
               </dependency>
               <dependency>
                   <groupId>org.springframework</groupId>
                   <artifactId>spring-context-support</artifactId>
                   <version>${spring.version}</version>
               </dependency>
               <dependency>
                   <groupId>org.springframework</groupId>
                   <artifactId>spring-test</artifactId>
                   <version>${spring.version}</version>
               </dependency>
               <!-- JSP相关 -->
               <dependency>
                   <groupId>jstl</groupId>
                   <artifactId>jstl</artifactId>
                   <version>${jstl.version}</version>
               </dependency>
               <dependency>
                   <groupId>javax.servlet</groupId>
                   <artifactId>servlet-api</artifactId>
                   <version>${servlet-api.version}</version>
                   <scope>provided</scope>
               </dependency>
               <dependency>
                   <groupId>javax.servlet</groupId>
                   <artifactId>jsp-api</artifactId>
                   <version>${jsp-api.version}</version>
                   <scope>provided</scope>
               </dependency>
           </dependencies>
       </dependencyManagement>
   
       <dependencies>
           <!-- Spring -->
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-context</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-beans</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-webmvc</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-jdbc</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-aspects</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-context-support</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-test</artifactId>
           </dependency>
           <!-- JSP相关 -->
           <dependency>
               <groupId>jstl</groupId>
               <artifactId>jstl</artifactId>
           </dependency>
           <dependency>
               <groupId>javax.servlet</groupId>
               <artifactId>servlet-api</artifactId>
               <scope>provided</scope>
           </dependency>
           <dependency>
               <groupId>javax.servlet</groupId>
               <artifactId>jsp-api</artifactId>
               <scope>provided</scope>
           </dependency>
           <dependency>
               <groupId>com.mchange</groupId>
               <artifactId>c3p0</artifactId>
           </dependency>
           <!-- 单元测试 -->
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>${junit.version}</version>
               <scope>test</scope>
           </dependency>
           <!-- 日志处理 -->
           <dependency>
               <groupId>org.slf4j</groupId>
               <artifactId>slf4j-log4j12</artifactId>
           </dependency>
           <!-- Mybatis -->
           <dependency>
               <groupId>org.mybatis</groupId>
               <artifactId>mybatis</artifactId>
           </dependency>
           <dependency>
               <groupId>org.mybatis</groupId>
               <artifactId>mybatis-spring</artifactId>
           </dependency>
           <!-- MySql -->
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
           </dependency>
       </dependencies>
   
       <build>
           <plugins>
               <plugin>
                   <groupId>org.apache.tomcat.maven</groupId>
                   <artifactId>tomcat7-maven-plugin</artifactId>
                   <configuration>
                       <path>/</path>
                       <port>8080</port>
                   </configuration>
               </plugin>
           </plugins>
           <resources>
               <resource>
                   <directory>src/main/java</directory>
                   <includes>
                       <include>**/*.xml</include>
                   </includes>
               </resource>
               <resource>
                   <directory>src/main/resources</directory>
                   <includes>
                       <include>**/*.xml</include>
                   </includes>
               </resource>
           </resources>
       </build>
   ```

3. 编写web.xml

   在 main 下创建 webapp 文件夹在 webapp 下创建 WEB-INF在WEB-INF下添加 web.xml

   ```
   <!--  中文乱码过滤器-->
     <filter>
       <filter-name>characterEncoding</filter-name>
       <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
       <init-param>
         <param-name>encoding</param-name>
         <param-value>utf-8</param-value>
       </init-param>
     </filter>
     <filter-mapping>
       <filter-name>characterEncoding</filter-name>
       <url-pattern>/*</url-pattern>
     </filter-mapping>
   
     <!-- 配置 spring mvc 的核心控制器 -->
     <servlet>
       <servlet-name>SpringMVCDispatcherServlet</servlet-name>
       <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
       <!-- 配置初始化参数，用于读取 SpringMVC 的配置文件 -->
       <init-param>
         <param-name>contextConfigLocation</param-name>
         <param-value>classpath:applicationContext.xml</param-value>
       </init-param>
       <!-- 配置 servlet 的对象的创建时间点：应用加载时创建。
         取值只能是非 0 正整数，表示启动顺序 -->
       <load-on-startup>1</load-on-startup>
     </servlet>
     <servlet-mapping>
       <servlet-name>SpringMVCDispatcherServlet</servlet-name>
       <url-pattern>/</url-pattern>
     </servlet-mapping>
   ```

4. 编写实体类

   ```
   package club.banyuan.entity;
   
   public class User {
   
     private Integer id;
     private String loginName;
     private String userName;
     private String password;
     private Integer sex;
     private String email;
     private String mobile;
   
     public User() {
     }
   
     public User(Integer id, String loginName, String userName, String password, Integer sex,
         String email, String mobile) {
       this.id = id;
       this.loginName = loginName;
       this.userName = userName;
       this.password = password;
       this.sex = sex;
       this.email = email;
       this.mobile = mobile;
     }
   
     public Integer getId() {
       return id;
     }
   
     public void setId(Integer id) {
       this.id = id;
     }
   
     public String getLoginName() {
       return loginName;
     }
   
     public void setLoginName(String loginName) {
       this.loginName = loginName;
     }
   
     public String getUserName() {
       return userName;
     }
   
     public void setUserName(String userName) {
       this.userName = userName;
     }
   
     public String getPassword() {
       return password;
     }
   
     public void setPassword(String password) {
       this.password = password;
     }
   
     public Integer getSex() {
       return sex;
     }
   
     public void setSex(Integer sex) {
       this.sex = sex;
     }
   
     public String getEmail() {
       return email;
     }
   
     public void setEmail(String email) {
       this.email = email;
     }
   
     public String getMobile() {
       return mobile;
     }
   
     public void setMobile(String mobile) {
       this.mobile = mobile;
     }
   }
   
   ```

5. 编写持久层接口

   ```
   package club.banyuan.dao;
   
   import club.banyuan.entity.User;
   
   import java.util.List;
   
   public interface UserDao {
   
     public List<User> getAll();
   }
   
   ```

6. 添加mapper配置文件

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE mapper     PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
     "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="club.banyuan.dao.UserDao">
     <select id="getAll" resultType="club.banyuan.entity.User">
           select * from user;
       </select>
   </mapper>
   ```

7. 编写业务层service接口及其实现类

   接口：

   ```
   package club.banyuan.service;
   
   import club.banyuan.entity.User;
   
   import java.util.List;
   
   public interface UserService {
   
     public List<User> getAll();
   }
   
   ```

   实现类：

   ```
   package club.banyuan.service.impl;
   
   import club.banyuan.dao.UserDao;
   import club.banyuan.entity.User;
   import club.banyuan.service.UserService;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.stereotype.Service;
   
   import java.util.List;
   
   @Service
   public class UserServiceImpl implements UserService {
   
     @Autowired
     private UserDao userDao;
   
     public List<User> getAll() {
       return userDao.getAll();
     }
   }
   
   ```

8. 编写表现层控制器

   ```
   package club.banyuan.controller;
   
   import club.banyuan.entity.User;
   import club.banyuan.service.UserService;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.stereotype.Controller;
   import org.springframework.ui.Model;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.servlet.ModelAndView;
   
   import javax.servlet.http.HttpServletRequest;
   import java.util.Collection;
   import java.util.List;
   import java.util.Map;
   
   @Controller
   public class UserController {
   
     @Autowired
     private UserService userService;
   
     @RequestMapping("/allUser")
     public String getAll(Model model) {
       List<User> userList = userService.getAll();
       model.addAttribute("userList", userList);
       return "list";
     }
   
   }
   
   ```

9. 添加jsp页面

   ```
   <%--
     Created by IntelliJ IDEA.
     User: edz
     Date: 2020/7/17
     Time: 4:53 下午
     To change this template use File | Settings | File Templates.
   --%>
   <%@ page isELIgnored="false" contentType="text/html;charset=UTF-8" language="java" %>
   <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   <table>
       <c:forEach items="${requestScope.userList}" var="user">
           <tr>
               <td>${user.loginName}</td>
               <td>
                   <c:if test="${user.sex == 1}">男</c:if>
                   <c:if test="${user.sex == 0}">女</c:if>
               </td>
               <td>
                       ${user.email}
               </td>
           </tr>
       </c:forEach>
   </table>
   </body>
   </html>
   
   ```

10. 在resources目录下添加配置xml配置文件

    1. 持久层applicationContext-dao.xml文件

       ```
       <?xml version="1.0" encoding="UTF-8"?>
       <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
                  http://www.springframework.org/schema/beans/spring-beans.xsd">
       
         <!-- 数据库连接池 -->
         <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
       
           <property name="driverClass" value="com.mysql.cj.jdbc.Driver"></property>
           <property name="jdbcUrl"
             value="jdbc:mysql://localhost:3306/shoppingstreet?useSSL=false&amp;serverTimezone=UTC"></property>
           <property name="user" value="root"></property>
           <property name="password" value="ych417130"></property>
           <!--连接池启动的时候默认创建的连接数量 -->
           <property name="initialPoolSize" value="3"></property>
           <!--连接池最多可以管理的连接对象个数 -->
           <property name="maxPoolSize" value="100"></property>
           <!--连接池中最多能够管理的statement对象 -->
           <property name="maxStatements" value="1000"></property>
           <!--一旦连接池中现有的连接数量不够，每次增长的连接数目：5 ，但是连接池中的连接数量 -->
           <!--最多不可超过maxPoolSize中设置的连接数目 -->
           <property name="acquireIncrement" value="5"></property>
         </bean>
         <!-- SqlSessionFactory -->
         <!-- 让spring管理sqlsessionfactory 使用mybatis和spring整合包中的 -->
         <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
           <!-- 数据库连接池 -->
           <property name="dataSource" ref="dataSource"/>
           <property name="typeAliasesPackage" value="club.banyuan.entity"/>
           <!-- 加载mybatis的全局配置文件 -->
           <property name="mapperLocations" value="classpath*:club/banyuan/dao/**/*.xml"/>
         </bean>
       
         <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
           <!--指定dao接口的位置 -->
           <property name="basePackage" value="club.banyuan.dao"/>
           <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"></property>
         </bean>
         
         <!-- 配置一个事务管理器 -->
         <bean id="transactionManager"
           class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
           <!-- 注入 DataSource -->
           <property name="dataSource" ref="dataSource"></property>
         </bean>
       
         <!-- 事务的配置 -->
         <tx:advice id="txAdvice" transaction-manager="transactionManager">
           <tx:attributes>
             <tx:method name="*" read-only="false" propagation="REQUIRED"/>
             <tx:method name="find*" read-only="true" propagation="SUPPORTS"/>
             <tx:method name="get*" read-only="true" propagation="SUPPORTS"/>
           </tx:attributes>
         </tx:advice>
       
         <!-- 配置 aop -->
         <aop:config>
           <!-- 配置切入点表达式 -->
           <aop:pointcut expression="execution(* club.banyuan.service.impl.*.*(..))" id="pt1"/>
           <!-- 在 aop:config标签内部：建立事务的通知和切入点表达式的关系 -->
           <aop:advisor advice-ref="txAdvice" pointcut-ref="pt1"/>
         </aop:config>
       
       </beans>
       
       
       ```

    2. 表现层applicationContext-mvc.xml文件

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
       
         <!--
             静态资源，不需要经过controller
             location : 静态资源的在服务器上的物理路径
             mapping ：浏览器访问静态资源的请求路径
         -->
         <!--    <mvc:resources location="images/" mapping="images/**"/>-->
         <!--    <mvc:resources location="css/" mapping="css/**"/>-->
         <!--    <mvc:resources location="js/" mapping="js/**"/>-->
       
         <mvc:annotation-driven/>
       </beans>
       
       ```

    3. 配置applicationContext.xml

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
       
         <import resource="classpath:applicationContext-mvc.xml"/>
         <import resource="classpath:applicationContext-dao.xml"/>
       
       </beans>
       
       ```

11. 项目路径图

    ![image-20200810161441958](/Users/edz/Library/Application Support/typora-user-images/image-20200810161441958.png)

### 分模块构建：

1. 继承和聚合：

   1. 继承：继承是为了消除重复，如果将 dao、service、web 分开创建独立的工程则每个工程的 pom.xml 文件中的内容存在重复，比如:设置编译版本、锁定 spring 的版本的等，可以将这些重复的 配置提取出来在父工程的 pom.xml 中定义。
   2. 聚合：项目开发通常是分组分模块开发，每个模块开发完成要运行整个工程需要将每个模块聚合在 一起运行，比如:dao、service、web 三个工程最终会打一个独立的 war 运行。

2. 案例：

   1. 创建maven-parent父模块

      定义pom.xml：在父工程的 pom.xml 中抽取一些重复的配置的，比如:锁定 jar 包的版本、设置编译版本等。 打包方式设置为 pom

      ```
      <?xml version="1.0" encoding="UTF-8"?>
      <project xmlns="http://maven.apache.org/POM/4.0.0"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>
      
        <groupId>club.banyuan</groupId>
        <artifactId>ssm2</artifactId>
        <version>1.0-SNAPSHOT</version>
        <properties>
          <junit.version>4.12</junit.version>
          <spring.version>5.2.7.RELEASE</spring.version>
          <mybatis.version>3.5.5</mybatis.version>
          <mybatis.spring.version>2.0.5</mybatis.spring.version>
          <mysql.version>8.0.20</mysql.version>
          <slf4j.version>1.6.4</slf4j.version>
          <jstl.version>1.2</jstl.version>
          <servlet-api.version>2.5</servlet-api.version>
          <jsp-api.version>2.0</jsp-api.version>
          <c3p0.version>0.9.5.2</c3p0.version>
        </properties>
        <dependencyManagement>
          <dependencies>
            <!-- 单元测试 -->
            <dependency>
              <groupId>junit</groupId>
              <artifactId>junit</artifactId>
              <version>${junit.version}</version>
              <scope>test</scope>
            </dependency>
            <!-- 日志处理 -->
            <dependency>
              <groupId>org.slf4j</groupId>
              <artifactId>slf4j-log4j12</artifactId>
              <version>${slf4j.version}</version>
            </dependency>
            <!-- Mybatis -->
            <dependency>
              <groupId>org.mybatis</groupId>
              <artifactId>mybatis</artifactId>
              <version>${mybatis.version}</version>
            </dependency>
            <dependency>
              <groupId>org.mybatis</groupId>
              <artifactId>mybatis-spring</artifactId>
              <version>${mybatis.spring.version}</version>
            </dependency>
            <!-- MySql -->
            <dependency>
              <groupId>mysql</groupId>
              <artifactId>mysql-connector-java</artifactId>
              <version>${mysql.version}</version>
            </dependency>
            <!-- 数据库连接池 -->
            <dependency>
              <groupId>com.mchange</groupId>
              <artifactId>c3p0</artifactId>
              <version>${c3p0.version}</version>
            </dependency>
      
            <!-- Spring -->
            <dependency>
              <groupId>org.springframework</groupId>
              <artifactId>spring-context</artifactId>
              <version>${spring.version}</version>
            </dependency>
            <dependency>
              <groupId>org.springframework</groupId>
              <artifactId>spring-beans</artifactId>
              <version>${spring.version}</version>
            </dependency>
            <dependency>
              <groupId>org.springframework</groupId>
              <artifactId>spring-webmvc</artifactId>
              <version>${spring.version}</version>
            </dependency>
            <dependency>
              <groupId>org.springframework</groupId>
              <artifactId>spring-jdbc</artifactId>
              <version>${spring.version}</version>
            </dependency>
            <dependency>
              <groupId>org.springframework</groupId>
              <artifactId>spring-aspects</artifactId>
              <version>${spring.version}</version>
            </dependency>
            <dependency>
              <groupId>org.springframework</groupId>
              <artifactId>spring-context-support</artifactId>
              <version>${spring.version}</version>
            </dependency>
            <dependency>
              <groupId>org.springframework</groupId>
              <artifactId>spring-test</artifactId>
              <version>${spring.version}</version>
            </dependency>
            <!-- JSP相关 -->
            <dependency>
              <groupId>jstl</groupId>
              <artifactId>jstl</artifactId>
              <version>${jstl.version}</version>
            </dependency>
            <dependency>
              <groupId>javax.servlet</groupId>
              <artifactId>servlet-api</artifactId>
              <version>${servlet-api.version}</version>
              <scope>provided</scope>
            </dependency>
            <dependency>
              <groupId>javax.servlet</groupId>
              <artifactId>jsp-api</artifactId>
              <version>${jsp-api.version}</version>
              <scope>provided</scope>
            </dependency>
          </dependencies>
        </dependencyManagement>
      
        <dependencies>
          <!-- Spring -->
          <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
          </dependency>
          <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
          </dependency>
          <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
          </dependency>
          <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
          </dependency>
          <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
          </dependency>
          <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context-support</artifactId>
          </dependency>
          <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
          </dependency>
          <!-- JSP相关 -->
          <dependency>
            <groupId>jstl</groupId>
            <artifactId>jstl</artifactId>
          </dependency>
          <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <scope>provided</scope>
          </dependency>
          <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jsp-api</artifactId>
            <scope>provided</scope>
          </dependency>
          <dependency>
            <groupId>com.mchange</groupId>
            <artifactId>c3p0</artifactId>
          </dependency>
          <!-- 单元测试 -->
          <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>${junit.version}</version>
            <scope>test</scope>
          </dependency>
          <!-- 日志处理 -->
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
          </dependency>
          <!-- Mybatis -->
          <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
          </dependency>
          <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
          </dependency>
          <!-- MySql -->
          <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
          </dependency>
        </dependencies>
      
        <build>
          <plugins>
            <plugin>
              <groupId>org.apache.tomcat.maven</groupId>
              <artifactId>tomcat7-maven-plugin</artifactId>
              <configuration>
                <path>/</path>
                <port>8080</port>
              </configuration>
            </plugin>
          </plugins>
        </build>
      ```

   2. ssm_dao子模块:

      在父工程上右击创建 maven 模块，填写模块名称 ssm_dao 打包方式是 jar

      配置pom.xml

      ```
      <?xml version="1.0" encoding="UTF-8"?>
      <project xmlns="http://maven.apache.org/POM/4.0.0"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
        <parent>
          <artifactId>ssm2</artifactId>
          <groupId>club.banyuan</groupId>
          <version>1.0-SNAPSHOT</version>
        </parent>
        <modelVersion>4.0.0</modelVersion>
      
        <artifactId>ssm2-dao</artifactId>
        <packaging>jar</packaging>
      
        <build>
          <resources>
            <resource>
              <directory>src/main/java</directory>
              <includes>
                <include>**/*.xml</include>
              </includes>
            </resource>
            <resource>
              <directory>src/main/resources</directory>
              <includes>
                <include>**/*.xml</include>
              </includes>
            </resource>
          </resources>
        </build>
      
      
      </project>
      ```

   3. ssm_service子模块:

      定义 pom.xml：

      ```
      <?xml version="1.0" encoding="UTF-8"?>
      <project xmlns="http://maven.apache.org/POM/4.0.0"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
        <parent>
          <artifactId>ssm2</artifactId>
          <groupId>club.banyuan</groupId>
          <version>1.0-SNAPSHOT</version>
        </parent>
        <modelVersion>4.0.0</modelVersion>
      
        <artifactId>ssm2-service</artifactId>
        <packaging>jar</packaging>
      
        <dependencies>
          <dependency>
            <groupId>club.banyuan</groupId>
            <artifactId>ssm2-dao</artifactId>
            <version>1.0-SNAPSHOT</version>
          </dependency>
        </dependencies>
      
      </project>
      ```

   4. ssm_web子模块：

      定义 pom.xml：

      ```
      <?xml version="1.0" encoding="UTF-8"?>
      <project xmlns="http://maven.apache.org/POM/4.0.0"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
        <parent>
          <artifactId>ssm2</artifactId>
          <groupId>club.banyuan</groupId>
          <version>1.0-SNAPSHOT</version>
        </parent>
        <modelVersion>4.0.0</modelVersion>
      
        <artifactId>ssm2-web</artifactId>
        <packaging>war</packaging>
      
        <dependencies>
          <dependency>
            <groupId>club.banyuan</groupId>
            <artifactId>ssm2-service</artifactId>
            <version>1.0-SNAPSHOT</version>
          </dependency>
        </dependencies>
      
      </project>
      ```

   5. 将父工程发布至仓库

      ```
      <modules>
          <module>ssm2-dao</module>
          <module>ssm2-service</module>
          <module>ssm2-web</module>
        </modules>
        <packaging>pom</packaging>
      ```

      ```
      <?xml version="1.0" encoding="UTF-8"?>
      <project xmlns="http://maven.apache.org/POM/4.0.0"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>
      
        <groupId>club.banyuan</groupId>
        <artifactId>ssm2</artifactId>
        <version>1.0-SNAPSHOT</version>
        <modules>
          <module>ssm2-dao</module>
          <module>ssm2-service</module>
          <module>ssm2-web</module>
        </modules>
        <packaging>pom</packaging>
      
        <properties>
          <junit.version>4.12</junit.version>
          <spring.version>5.2.7.RELEASE</spring.version>
          <mybatis.version>3.5.5</mybatis.version>
          <mybatis.spring.version>2.0.5</mybatis.spring.version>
          <mysql.version>8.0.20</mysql.version>
          <slf4j.version>1.6.4</slf4j.version>
          <jstl.version>1.2</jstl.version>
          <servlet-api.version>2.5</servlet-api.version>
          <jsp-api.version>2.0</jsp-api.version>
          <c3p0.version>0.9.5.2</c3p0.version>
        </properties>
        <dependencyManagement>
          <dependencies>
            <!-- 单元测试 -->
            <dependency>
              <groupId>junit</groupId>
              <artifactId>junit</artifactId>
              <version>${junit.version}</version>
              <scope>test</scope>
            </dependency>
            <!-- 日志处理 -->
            <dependency>
              <groupId>org.slf4j</groupId>
              <artifactId>slf4j-log4j12</artifactId>
              <version>${slf4j.version}</version>
            </dependency>
            <!-- Mybatis -->
            <dependency>
              <groupId>org.mybatis</groupId>
              <artifactId>mybatis</artifactId>
              <version>${mybatis.version}</version>
            </dependency>
            <dependency>
              <groupId>org.mybatis</groupId>
              <artifactId>mybatis-spring</artifactId>
              <version>${mybatis.spring.version}</version>
            </dependency>
            <!-- MySql -->
            <dependency>
              <groupId>mysql</groupId>
              <artifactId>mysql-connector-java</artifactId>
              <version>${mysql.version}</version>
            </dependency>
            <!-- 数据库连接池 -->
            <dependency>
              <groupId>com.mchange</groupId>
              <artifactId>c3p0</artifactId>
              <version>${c3p0.version}</version>
            </dependency>
      
            <!-- Spring -->
            <dependency>
              <groupId>org.springframework</groupId>
              <artifactId>spring-context</artifactId>
              <version>${spring.version}</version>
            </dependency>
            <dependency>
              <groupId>org.springframework</groupId>
              <artifactId>spring-beans</artifactId>
              <version>${spring.version}</version>
            </dependency>
            <dependency>
              <groupId>org.springframework</groupId>
              <artifactId>spring-webmvc</artifactId>
              <version>${spring.version}</version>
            </dependency>
            <dependency>
              <groupId>org.springframework</groupId>
              <artifactId>spring-jdbc</artifactId>
              <version>${spring.version}</version>
            </dependency>
            <dependency>
              <groupId>org.springframework</groupId>
              <artifactId>spring-aspects</artifactId>
              <version>${spring.version}</version>
            </dependency>
            <dependency>
              <groupId>org.springframework</groupId>
              <artifactId>spring-context-support</artifactId>
              <version>${spring.version}</version>
            </dependency>
            <dependency>
              <groupId>org.springframework</groupId>
              <artifactId>spring-test</artifactId>
              <version>${spring.version}</version>
            </dependency>
            <!-- JSP相关 -->
            <dependency>
              <groupId>jstl</groupId>
              <artifactId>jstl</artifactId>
              <version>${jstl.version}</version>
            </dependency>
            <dependency>
              <groupId>javax.servlet</groupId>
              <artifactId>servlet-api</artifactId>
              <version>${servlet-api.version}</version>
              <scope>provided</scope>
            </dependency>
            <dependency>
              <groupId>javax.servlet</groupId>
              <artifactId>jsp-api</artifactId>
              <version>${jsp-api.version}</version>
              <scope>provided</scope>
            </dependency>
          </dependencies>
        </dependencyManagement>
      
        <dependencies>
          <!-- Spring -->
          <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
          </dependency>
          <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
          </dependency>
          <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
          </dependency>
          <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
          </dependency>
          <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
          </dependency>
          <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context-support</artifactId>
          </dependency>
          <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
          </dependency>
          <!-- JSP相关 -->
          <dependency>
            <groupId>jstl</groupId>
            <artifactId>jstl</artifactId>
          </dependency>
          <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <scope>provided</scope>
          </dependency>
          <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jsp-api</artifactId>
            <scope>provided</scope>
          </dependency>
          <dependency>
            <groupId>com.mchange</groupId>
            <artifactId>c3p0</artifactId>
          </dependency>
          <!-- 单元测试 -->
          <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>${junit.version}</version>
            <scope>test</scope>
          </dependency>
          <!-- 日志处理 -->
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
          </dependency>
          <!-- Mybatis -->
          <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
          </dependency>
          <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
          </dependency>
          <!-- MySql -->
          <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
          </dependency>
        </dependencies>
      
        <build>
          <plugins>
            <plugin>
              <groupId>org.apache.tomcat.maven</groupId>
              <artifactId>tomcat7-maven-plugin</artifactId>
              <configuration>
                <path>/</path>
                <port>8080</port>
              </configuration>
            </plugin>
          </plugins>
        </build>
      
      </project>
      ```

   6. 项目路径：

      ![image-20200810165711278](/Users/edz/Library/Application Support/typora-user-images/image-20200810165711278.png)

      ![image-20200810165740486](/Users/edz/Library/Application Support/typora-user-images/image-20200810165740486.png)

      


### 简介：

1. 官网：https://projects.spring.io/spring-security/ SpringSecurity
2. 强大的且容易定制的实现认证与授权的基于 Spring 开发的框架

### 功能：

1. Authentication:认证，就是用户登录。
2. Authorization:授权，判断用户拥有什么权限，可以访问什么资源。
3. 安全防护，防止跨站请求，session 攻击等
4. 非常容易结合 SpringMVC 进行使用

### springSecurity环境（流程）：

1. 创建javaWeb工程，导入jar包（SSMAndSecurity文件）

2. 配置web.xml文件

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
     version="4.0">
   
     <!-- SpringSecurity过滤器链  -->
   <!--  <filter>-->
   <!--    <filter-name>springSecurityFilterChain</filter-name>-->
   <!--    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>-->
   <!--  </filter>-->
   <!--  <filter-mapping>-->
   <!--    <filter-name>springSecurityFilterChain</filter-name>-->
   <!--    <url-pattern>/*</url-pattern>-->
   <!--  </filter-mapping>-->
   
     <!-- 启动Spring  -->
     <listener>
       <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
     </listener>
     <context-param>
       <param-name>contextConfigLocation</param-name>
       <param-value>
         classpath:applicationContext.xml
        <!-- classpath:spring-security.xml -->
       </param-value>
     </context-param>
   
     <!--启动SpringMVC-->
     <servlet>
       <servlet-name>DispatcherServlet</servlet-name>
       <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
       <init-param>
         <param-name>contextConfigLocation</param-name>
         <param-value>classpath:springmvc.xml</param-value>
       </init-param>
       <!-- 服务器启动加载Servlet-->
       <load-on-startup>1</load-on-startup>
     </servlet>
     <servlet-mapping>
       <servlet-name>DispatcherServlet</servlet-name>
       <url-pattern>/</url-pattern>
     </servlet-mapping>
   
   </web-app>
   ```

   

3. src目录下创建applicationContext.xml和springmvc.xml配置文件

   applicationContext.xml

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xmlns:context="http://www.springframework.org/schema/context"
     xmlns:aop="http://www.springframework.org/schema/aop"
     xsi:schemaLocation="http://www.springframework.org/schema/beans
   	http://www.springframework.org/schema/beans/spring-beans.xsd
   	http://www.springframework.org/schema/context
   	http://www.springframework.org/schema/context/spring-context.xsd
   	http://www.springframework.org/schema/aop
   	http://www.springframework.org/schema/aop/spring-aop.xsd">
   
   </beans>
   
   ```

   springmvc.xml

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
     xmlns:mvc="http://www.springframework.org/schema/mvc"
     xmlns:contenxt="http://www.springframework.org/schema/context"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="
           http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/mvc
           http://www.springframework.org/schema/mvc/spring-mvc.xsd
           http://www.springframework.org/schema/context
           http://www.springframework.org/schema/context/spring-context.xsd">
   </beans>
   ```

   

4. web.xml文件中配置SpringSecurity 过滤器

   打开web.xml配置文件中的filter标签和classpath

5. src目录下创建spring-security.xml配置文件

   ```
   <beans xmlns="http://www.springframework.org/schema/beans"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xmlns:security="http://www.springframework.org/schema/security"
     xsi:schemaLocation="http://www.springframework.org/schema/beans
               http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
               http://www.springframework.org/schema/security
               http://www.springframework.org/schema/security/spring-security-5.3.xsd">
               
               </beans>
   ```

   

### HttpBasic 方式的权限实现：

1. 编写src/club/banyuan/controller

2. 配置springmvc.xml

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
     xmlns:mvc="http://www.springframework.org/schema/mvc"
     xmlns:contenxt="http://www.springframework.org/schema/context"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="
           http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/mvc
           http://www.springframework.org/schema/mvc/spring-mvc.xsd
           http://www.springframework.org/schema/context
           http://www.springframework.org/schema/context/spring-context.xsd">
   
     <!-- 扫描Controller类-->
     <contenxt:component-scan base-package="club.banyuan"/>
   
     <!--注解方式处理器映射器和处理器适配器 -->
     <mvc:annotation-driven></mvc:annotation-driven>
   
     <!--视图解析器-->
     <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
       <!--前缀 -->
       <property name="prefix" value="/WEB-INF/jsp/"/>
       <!-- 后缀-->
       <property name="suffix" value=".jsp"/>
     </bean>
   
   
   </beans>
   
   ```

   

3. 补充页面

   ![image-20200728103744865](/Users/edz/Library/Application Support/typora-user-images/image-20200728103744865.png)

4. 配置spring-security.xml

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xmlns:security="http://www.springframework.org/schema/security"
     xsi:schemaLocation="http://www.springframework.org/schema/beans
               http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
               http://www.springframework.org/schema/security
               http://www.springframework.org/schema/security/spring-security-5.3.xsd">
   
     <!-- <security:http>: spring过滤器链配置：
                  1）需要拦截什么资源
                  2）什么资源什么角色权限
                  3）定义认证方式：HttpBasic，FormLogin（*）
                  4）定义登录页面，定义登录请求地址，定义错误处理方式
              -->
   
     <security:http>
       <!--
          pattern: 需要拦截资源
          access: 拦截方式
                  isFullyAuthenticated(): 该资源需要认证才可以访问
      -->
       <security:intercept-url pattern="/**" access="isFullyAuthenticated()"/>
   
       <!-- security:http-basic: 使用HttpBasic方式进行登录（认证） -->
       <security:http-basic/>
     </security:http>
   
     <!--
        security:authentication-manager： 认证管理器
             1）认证信息提供方式（账户名，密码，当前用户权限）
     -->
     <security:authentication-manager>
       <security:authentication-provider>
         <security:user-service>
           <security:user name="eric" password="{noop}123456" authorities="ROLE_USER"/>
           <security:user name="jack" password="{noop}123456" authorities="ROLE_ADMIN"/>
         </security:user-service>
       </security:authentication-provider>
     </security:authentication-manager>
   
   </beans>
   
   
   ```

   

### FormLogin 方法的权限实现(*)：

1. 使用配置实现用户权限访问控制:配置spring-security.xml文件

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xmlns:security="http://www.springframework.org/schema/security"
     xsi:schemaLocation="http://www.springframework.org/schema/beans
               http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
               http://www.springframework.org/schema/security
               http://www.springframework.org/schema/security/spring-security-5.3.xsd">
   
     <!-- <security:http>: spring过滤器链配置：
                  1）需要拦截什么资源
                  2）什么资源什么角色权限
                  3）定义认证方式：HttpBasic，FormLogin（*）
                  4）定义登录页面，定义登录请求地址，定义错误处理方式
              -->
   
     <security:http>
       <!--
           pattern: 需要拦截资源
           access: 拦截方式
                   isFullyAuthenticated(): 该资源需要认证才可以访问
                   isAnonymous():只有匿名用户才可以访问（如果登录用户就无法访问）
                   permitAll():允许所有人（匿名和登录用户）方法
       -->
       <security:intercept-url pattern="/product/index" access="permitAll()"/>
       <security:intercept-url pattern="/userLogin" access="permitAll()"/>
   
       <security:intercept-url pattern="/product/add" access="hasRole('ROLE_USER')"/>
       <security:intercept-url pattern="/product/update" access="hasRole('ROLE_USER')"/>
       <security:intercept-url pattern="/product/list" access="hasRole('ROLE_ADMIN')"/>
       <security:intercept-url pattern="/product/delete" access="hasRole('ROLE_ADMIN')"/>
   
   
       <security:intercept-url pattern="/**" access="isFullyAuthenticated()"/>
   
   
       <!-- security:http-basic: 使用HttpBasic方式进行登录（认证） -->
       <!--        <security:http-basic/>-->
       <security:form-login login-page="/userLogin"
         login-processing-url="/securityLogin"
         default-target-url="/product/index"/>
   
       <!-- 自定义权限不足处理 -->
       <security:access-denied-handler error-page="/error"/>
   
       <!-- 关闭Spring Security CSRF机制 -->
       <security:csrf disabled="true"/>
     </security:http>
   
     <!--
        security:authentication-manager： 认证管理器
             1）认证信息提供方式（账户名，密码，当前用户权限）
     -->
     <security:authentication-manager>
       <security:authentication-provider>
         <security:user-service>
           <security:user name="eric" password="{noop}123456" authorities="ROLE_USER"/>
           <security:user name="jack" password="{noop}123456" authorities="ROLE_ADMIN"/>
         </security:user-service>
       </security:authentication-provider>
     </security:authentication-manager>
   
   </beans>
   
   ```

2. SpringSecurity 执行原理

   总拦截器先拿到之前所有拦截器的执行结果，若是有问题就抛出异常

3. 自定义登录页面

   ```
   <%--
     Created by IntelliJ IDEA.
     User: edz
     Date: 2020/7/28
     Time: 10:34 上午
     To change this template use File | Settings | File Templates.
   --%>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   <form action="${pageContext.request.contextPath}/securityLogin" method="post">
       用户名:<input type="text" name="username"/><br/>
       用户名:<input type="password" name="password"/><br/>
       <input type="submit" value="登录"/>
   </form>
   
   </body>
   </html>
   
   ```

4. 自定义登录请求

   ```
   package club.banyuan.controller;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestMapping;
   
   @Controller
   public class MainController {
   
     @RequestMapping("/userLogin")
     public String login() {
       return "/login";
     }
   
     @RequestMapping("/error")
     public String error() {
       return "/error";
     }
   
   }
   
   ```

5. 添加错误页面/WEB-INF/jsp/error.jsp

   ```
   <%--
     Created by IntelliJ IDEA.
     User: edz
     Date: 2020/7/28
     Time: 10:48 上午
     To change this template use File | Settings | File Templates.
   --%>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   亲，你没有权限访问该资源！
   </body>
   </html>
   
   ```

6. 自定义 UserDetailService 类实现用户权限 访问控制

   ```
   package club.banyuan.security;
   
   import org.springframework.security.core.authority.AuthorityUtils;
   import org.springframework.security.core.userdetails.User;
   import org.springframework.security.core.userdetails.UserDetails;
   import org.springframework.security.core.userdetails.UserDetailsService;
   import org.springframework.security.core.userdetails.UsernameNotFoundException;
   
   public class MyUserDetailService implements UserDetailsService {
   
     /**
      * loadUserByUsername: 读取用户信息
      *
      * @param
      * @return
      * @throws UsernameNotFoundException
      */
     @Override
     public UserDetails loadUserByUsername(String s) throws UsernameNotFoundException {
       User user = new User("test", "{noop}123456",
           AuthorityUtils.commaSeparatedStringToAuthorityList("ROLE_USER"));
       return user;
     }
   }
   //其中 User 类就是 UserDetail 实现类，用于封装数据库账户信息
   ```

7. 自定义登录成功与失败处理逻辑

   ```
   package club.banyuan.security;
   
   import com.fasterxml.jackson.databind.ObjectMapper;
   import org.springframework.security.core.Authentication;
   import org.springframework.security.web.authentication.AuthenticationSuccessHandler;
   
   import javax.servlet.ServletException;
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   import java.io.IOException;
   import java.util.HashMap;
   import java.util.Map;
   
   public class MyAuthenticationSuccessHandler implements AuthenticationSuccessHandler {
   
     ObjectMapper objectMapper = new ObjectMapper();
   
     @Override
     public void onAuthenticationSuccess(HttpServletRequest httpServletRequest,
         HttpServletResponse httpServletResponse, Authentication authentication)
         throws IOException, ServletException {
       //返回json字符串给前端
       Map result = new HashMap();
       result.put("success", true);
   
       String json = objectMapper.writeValueAsString(result);
       httpServletResponse.setContentType("text/json;charset=utf-8");
       httpServletResponse.getWriter().write(json);
   
     }
   }
   
   ```

   ```
   package club.banyuan.security;
   
   import com.fasterxml.jackson.databind.ObjectMapper;
   import org.springframework.security.core.Authentication;
   import org.springframework.security.core.AuthenticationException;
   import org.springframework.security.web.authentication.AuthenticationFailureHandler;
   import org.springframework.security.web.authentication.AuthenticationSuccessHandler;
   
   import javax.servlet.ServletException;
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   import java.io.IOException;
   import java.util.HashMap;
   import java.util.Map;
   
   public class MyAuthenticationFailureHandler implements AuthenticationFailureHandler {
   
     ObjectMapper objectMapper = new ObjectMapper();
   
     @Override
     public void onAuthenticationFailure(HttpServletRequest httpServletRequest,
         HttpServletResponse httpServletResponse, AuthenticationException e)
         throws IOException, ServletException {
       //返回json字符串给前端
       Map result = new HashMap();
       result.put("success", false);
   
       String json = objectMapper.writeValueAsString(result);
       httpServletResponse.setContentType("text/json;charset=utf-8");
       httpServletResponse.getWriter().write(json);
     }
   }
   
   ```

8. 配置spring-security.xml文件

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xmlns:security="http://www.springframework.org/schema/security"
     xsi:schemaLocation="http://www.springframework.org/schema/beans
               http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
               http://www.springframework.org/schema/security
               http://www.springframework.org/schema/security/spring-security-5.3.xsd">
   
     <!-- <security:http>: spring过滤器链配置：
                  1）需要拦截什么资源
                  2）什么资源什么角色权限
                  3）定义认证方式：HttpBasic，FormLogin（*）
                  4）定义登录页面，定义登录请求地址，定义错误处理方式
              -->
   
     <security:http>
       <!--
           pattern: 需要拦截资源
           access: 拦截方式
                   isFullyAuthenticated(): 该资源需要认证才可以访问
                   isAnonymous():只有匿名用户才可以访问（如果登录用户就无法访问）
                   permitAll():允许所有人（匿名和登录用户）方法
       -->
       <security:intercept-url pattern="/product/index" access="permitAll()"/>
       <security:intercept-url pattern="/userLogin" access="permitAll()"/>
   
       <security:intercept-url pattern="/product/add" access="hasRole('ROLE_USER')"/>
       <security:intercept-url pattern="/product/update" access="hasRole('ROLE_USER')"/>
       <security:intercept-url pattern="/product/list" access="hasRole('ROLE_ADMIN')"/>
       <security:intercept-url pattern="/product/delete" access="hasRole('ROLE_ADMIN')"/>
   
   
       <security:intercept-url pattern="/**" access="isFullyAuthenticated()"/>
   
   
       <!-- security:http-basic: 使用HttpBasic方式进行登录（认证） -->
       <!--        <security:http-basic/>-->
       <security:form-login login-page="/userLogin"
         login-processing-url="/securityLogin"
         default-target-url="/product/index"
         authentication-success-handler-ref="myAuthenticationSuccessHandler"
         authentication-failure-handler-ref="myAuthenticationFailureHandler"/>
   
       <!-- 自定义权限不足处理 -->
       <security:access-denied-handler error-page="/error"/>
   
       <!-- 关闭Spring Security CSRF机制 -->
       <security:csrf disabled="true"/>
     </security:http>
   
     <!--
        security:authentication-manager： 认证管理器
             1）认证信息提供方式（账户名，密码，当前用户权限）
     -->
     <security:authentication-manager>
       <security:authentication-provider user-service-ref="myUserDetailService"/>
       <!--        <security:authentication-provider>-->
       <!--            <security:user-service>-->
       <!--                <security:user name="eric" password="{noop}123456" authorities="ROLE_USER"/>-->
       <!--                <security:user name="jack" password="{noop}123456" authorities="ROLE_ADMIN"/>-->
       <!--            </security:user-service>-->
       <!--        </security:authentication-provider>-->
     </security:authentication-manager>
   
     <bean id="myUserDetailService" class="club.banyuan.security.MyUserDetailService"/>
   
     <bean id="myAuthenticationSuccessHandler"
       class="club.banyuan.security.MyAuthenticationSuccessHandler"/>
   
     <bean id="myAuthenticationFailureHandler"
       class="club.banyuan.security.MyAuthenticationFailureHandler"/>
   
   
   </beans>
   
   ```

### SpringSecurity+SSM


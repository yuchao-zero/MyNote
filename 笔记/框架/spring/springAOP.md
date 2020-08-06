## SpringAOP

### 概念：

1. Aspect Oriented Programming  面向切面编程。通过预编译和运行期动态代理实现编程功能的统一维护的一种技术。
2. 将程序重复的代码抽取出来，在需要执行的时候，使用动态代理的技术， 在不修改源码的基础上，对我们的已有方法进行增强。

### 作用：

在程序运行期间，不修改源码对已有方法进行增强。

### 优势：

1. 减少重复代码
2. 提高开发效率
3. 维护方便

### 动态代理：

字节码随用随创建，随用随加载。

### 动态代理的两种方式：

1. 基于接口的动态代理
   - 提供者：JDK官方的proxy类
   - 要求：被代理类至少实现一个接口
2. 基于子类的动态代理
   - 提供者：第三方的 CGLib，如果报 asmxxxx 异常，需要导入 asm.jar。
   - 要求：被代理类不能使用final修饰

### AOP相关术语：

1. Joinpoint(连接点)：所谓连接点是指那些被拦截到的点。（业务层的方法）连接业务和增强方法的点。

   在spring中，这些点指的是方法，因为 spring 只支持方法类型的连接点。

2. Pointcut(切入点)：所谓切入点是指我们要对哪些 Joinpoint 进行拦截的定义

   <font color=red>所有的切入点都是连接点，但是连接点不一定是切入点，被增强的连接点被称为切入点</font>

3. Advice(通知/增强)：所谓通知是指拦截到 Joinpoint 之后所要做的事情就是通知。

   通知的类型：前置通知，后置通知，异常通知，最终通知，环绕通知。

4. Introduction(引介)：引介是一种特殊的通知在不修改类代码的前提下，Introduction

   可以在运行期为类动态地添加一些方法或 Field。

5. Target(目标对象)：代理的目标对象。

6. Weaving(织入)：是指把增强应用到目标对象来创建新的代理对象的过程。

   spring 采用动态代理织入，而 AspectJ 采用编译期织入和类装载期织入。

7. Proxy(代理)：一个类被 AOP 织入增强后，就产生一个结果代理类。

8. Aspect(切面)：是切入点和通知(引介)的结合。

### 基于xml的AOP配置

1. 环境搭建，配置pom.xml文件

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
     <modelVersion>4.0.0</modelVersion>
   
     <groupId>club.banyuan</groupId>
     <artifactId>aop3</artifactId>
     <version>1.0-SNAPSHOT</version>
   
     <dependencies>
       <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
         <version>4.12</version>
         <scope>test</scope>
       </dependency>
       <!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-core</artifactId>
         <version>5.2.7.RELEASE</version>
       </dependency>
       <!-- https://mvnrepository.com/artifact/org.springframework/spring-beans -->
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-beans</artifactId>
         <version>5.2.7.RELEASE</version>
       </dependency>
       <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-context</artifactId>
         <version>5.2.7.RELEASE</version>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-aop</artifactId>
         <version>5.2.7.RELEASE</version>
       </dependency>
       <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
       <!-- 解析切入点表达式-->
       <dependency>
         <groupId>org.aspectj</groupId>
         <artifactId>aspectjweaver</artifactId>
         <version>1.9.5</version>
       </dependency>
     </dependencies>
   </project>
   ```

2. 创建 spring 的配置文件并导入约束

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
     xmlns:aop="http://www.springframework.org/schema/aop"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/aop
         http://www.springframework.org/schema/aop/spring-aop.xsd">
   </beans>
   ```

3. ```
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
     xmlns:aop="http://www.springframework.org/schema/aop"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/aop
         http://www.springframework.org/schema/aop/spring-aop.xsd">
     <!--  配置service对象，即需要增强方法所在的对象-->
     <bean id="accountService" class="club.banyuan.service.impl.AccountServiceImpl"></bean>
   
     <!--  aop配置步骤：
           1. 将通知类交给spring管理
           2. 使用aop:config标签表明开始aop的配置
           3. 使用aop:aspect标签表明配置切面：
               id属性：是给切面提供一个唯一标识
               ref属性：是指定通知类bean的id
           4. 在aop:aspect标签内部使用对应的标签来配置通知的类型：
               aop:before表示前置通知
                   method属性：用于指定log类中哪个方法是前置通知
                   pointcut属性：用于指定切入点表达式，指的是对业务层的那些方法增强
                   标准的表达式写法：execution(public void club.banyuan.service.impl.AccountServiceImpl.saveAccount())
                   全通配写法：* *..*.*(..)
           -->
     <!--  配置功能增强所在方法的对象（通知类）-->
     <bean id="log" class="club.banyuan.utils.Log"></bean>
   
     <!--  配置aop-->
     <aop:config>
       <!--配置切入点表达式，id属性用于指定表达式的唯一标识，expression属性用于指定表达式内容-->
       <aop:pointcut id="pt"
         expression="execution(public void club.banyuan.service.impl.AccountServiceImpl.saveAccount())"/>
       <!--    配置切面-->
       <aop:aspect id="logAdvice" ref="log">
         <!--    配置前置通知-->
         <aop:before method="log" pointcut-ref="pt"></aop:before>
       </aop:aspect>
     </aop:config>
   
     <!--  前置通知before ：joinpoint之前调用&ndash;&gt;-->
     <!--                        最终通知after：joinpoint执行中止（无论是否有异常出现）后执行-->
     <!--                        后置通知after-returnning：joinpoint正常执行完，有返回值的时候-->
     <!--                        异常通知after-throwing：当joinpoint执行时有异常抛出，执行-->
     <!--                            环绕通知 around:在joinpoint执行之前可以做功能增强，-->
     <!--                                            joinPoint执行，是手动调用方法增强-->
     <!--                                            当joinpoint执行完成后，可以继续做功能增强-->
     <!--      后置通知和异常通知只会有一个执行-->
   </beans>
   ```

4. ![image-20200803205554614](/Users/edz/Library/Application Support/typora-user-images/image-20200803205554614.png)

### 基于注解的AOP配置

1. 创建maven工程，配置pom.xml文件

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
     <modelVersion>4.0.0</modelVersion>
   
     <groupId>club.banyuan</groupId>
     <artifactId>aop3</artifactId>
     <version>1.0-SNAPSHOT</version>
   
     <dependencies>
       <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
         <version>4.12</version>
         <scope>test</scope>
       </dependency>
       <!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-core</artifactId>
         <version>5.2.7.RELEASE</version>
       </dependency>
       <!-- https://mvnrepository.com/artifact/org.springframework/spring-beans -->
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-beans</artifactId>
         <version>5.2.7.RELEASE</version>
       </dependency>
       <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-context</artifactId>
         <version>5.2.7.RELEASE</version>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-aop</artifactId>
         <version>5.2.7.RELEASE</version>
       </dependency>
       <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
       <dependency>
         <groupId>org.aspectj</groupId>
         <artifactId>aspectjweaver</artifactId>
         <version>1.9.5</version>
       </dependency>
     </dependencies>
   </project>
   ```

2. 在resources目录下创建配置文件

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
     xmlns:context="http://www.springframework.org/schema/context"
     xmlns:aop="http://www.springframework.org/schema/aop"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd
         http://www.springframework.org/schema/context
         http://www.springframework.org/schema/context/spring-context.xsd
           http://www.springframework.org/schema/aop
         http://www.springframework.org/schema/aop/spring-aop.xsd">
   
     <context:component-scan base-package="club.banyuan"></context:component-scan>
     <!--    开启Spring对AOP注解的支持-->
     <aop:aspectj-autoproxy/>
   
   </beans>
   ```

3. 修改通知类，添加注解

   ```
   package club.banyuan.utils;
   
   import org.aspectj.lang.ProceedingJoinPoint;
   import org.aspectj.lang.annotation.*;
   import org.springframework.stereotype.Component;
   
   @Component
   @Aspect//表示当前类是一个切面类
   public class Tool {
   
     @Pointcut(value = "execution(* club.banyuan.service.impl.*.*(..))")
     public void _pointcut() {
     }
   
     @Before("_pointcut()")
     public void printBefore() {
       System.out.println("Tool --- printBefore");
     }
   
     @After("_pointcut()")
     public void printAfter() {
       System.out.println("Tool --- 资源释放");
     }
   
     @AfterReturning("_pointcut()")
     public void printAfterReturnning() {
       System.out.println("Tool --- printAfterReturnning");
     }
   
     @AfterThrowing("_pointcut()")
     public void printAfterThrowing() {
       System.out.println("Tool --- 事务回滚");
     }
   
     @Around("_pointcut()")
     public Object printAround(ProceedingJoinPoint joinPoint) throws Throwable {
       System.out.println("Tool ---  开启事务");
   //        调用目标方法
       // joinPoint连接点 --- 目标方法
       Object obj = joinPoint.proceed();
       System.out.println("Tool --- 提交事务");
       return obj;
     }
   }
   
   ```

4. 
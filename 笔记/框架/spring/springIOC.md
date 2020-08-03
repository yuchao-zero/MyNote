## springIOC

### 概念：

1. 即控制反转（inversion of control），把创建对象的权利交给框架，是框架的重要特征，并非面向对象编程的专用语
2. 包括依赖注入（Dependency Injection）和依赖查找（Dependency Lookup）

明确IOC的作用：

削减计算机程序的耦合，消除代码中的依赖关系

### 使用 spring 的 IOC 解决程序耦合实例：

1. 创建maven工程，准备spring的开发包，配置pom.xml文件：

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
     <modelVersion>4.0.0</modelVersion>
   
     <groupId>club.banyuan</groupId>
     <artifactId>springIOCConstructorDI</artifactId>
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
   
     </dependencies>
   
   
   </project>
   ```

2. 创建业务层接口和实现类

   ```
   /**
   * 账户的业务层接口
   */
   public interface IAccountService {
   /**
   * 保存账户(此处只是模拟，并不是真的要保存)
   */
   void saveAccount();
   }
   
   <!-- 实现类-->
   /**
    * *账户的业务层实现类
    */
   
   public class AccountServiceImpl implements IAccountService {
   
     private IAccountDao accountDao = new AccountDaoImpl();//此处的依赖关系有待解决
   
     @Override
     public void saveAccount() {
       accountDao.saveAccount();
     }
   }
   ```

   

3. 创建持久层接口和实现类

   ```
   /**
    * 账户的持久层接口
    */
   public interface IAccountDao {
   
     /**
      * 保存账户
      */
     void saveAccount();
   }
   
   <!-- 实现类-->
   /**
    * 账户的持久层实现类
    */
   public class AccountDaoImpl implements IAccountDao {
   
     @Override
     public void saveAccount() {
       System.out.println("保存了账户");
     }
   }
   ```

4. 在类的根路径(resources)下创建一个任意名称的 xml 文件(不能是中文)

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   </beans>
   ```

5. 在配置文件中配置 service 和 dao

   ```
   <!-- bean 标签:用于配置让spring 创建对象，
     并且存入 ioc 容器之中 id 属性:对象的唯一标识。
     class 属性:指定要创建对象的全限定类名 -->
     <!-- 配置 service -->
     <bean id="accountService" class="club.banyuan.service.impl.AccountServiceImpl"></bean>
     <!-- 配置 dao -->
     <bean id="accountDao" class="club.banyuan.dao.impl.AccountDaoImpl"></bean>
   ```

6. 测试配置是否成功

   ```
   /**
    * 模拟一个表现层
    */
   public class Client {
   
     /**
      * 使用 main 方法获取容器测试执行
      */
     public static void main(String[] args) {
   //1.使用 ApplicationContext 接口，就是在获取 spring 容器
       ApplicationContext ac = new ClassPathXmlApplicationContext(
           "applicationContext.xml"); //2.根据 bean 的 id 获取对象
       IAccountService aService = (IAccountService) ac.getBean("accountService");
       System.out.println(aService);
       IAccountDao aDao = (IAccountDao) ac.getBean("accountDao");
       System.out.println(aDao);
     }
   }
   ```

### Spring 基于 XML 的 IOC 细节:

1. spring中工厂的类结构图:

   ![image-20200729202911300](/Users/edz/Library/Application Support/typora-user-images/image-20200729202911300.png)

2. **BeanFactory** 和 **ApplicationContext** 的区别:创建对象的时间点不一样

   - BeanFactory 才是 Spring 容器中的顶层接口。
      ApplicationContext 是它的子接口。
   - ApplicationContext:只要一读取配置文件，默认情况下就会创建对象。
   -  BeanFactory:什么使用什么时候创建对象。

3. **ApplicationContext** 接口的实现类:

   - ClassPathXmlApplicationContext: 它是从类的根路径下加载配置文件推荐使用这种
   - FileSystemXmlApplicationContext: 它是从磁盘路径上加载配置文件，配置文件可以在磁 盘的任意位置。
   - AnnotationConfigApplicationContext：当我们使用注解配置容器对象时，需要使用此类来创建 spring 容器。它用来读取注解。

### IOC 中 bean 标签和管理对象细节：

1. ​	**bean** 标签：

   - 作用:
      用于配置对象让 spring 来创建的。 默认情况下它调用的是类中的无参构造函数。<font color=red>如果没有无参构造函数则不能创建成功</font>。

   - 属性:
      id:给对象在容器中提供一个唯一标识。用于获取对象。 

     class:指定类的全限定类名。用于反射创建对象。默认情况下调用无参构造函数。 

     scope:指定对象的作用范围。

      	*singleton：默认值，单例的。
      	*prototype：多例的。
     	 *request：WEB 项目中,Spring 创建一个 Bean 的对象,将对象存入到 request 域中。
      	*session：WEB 项目中,Spring 创建一个 Bean 的对象,将对象存入到 session 域中。
      	*global session：WEB 项目中,应用在 Portlet 环境。如果没有 Portlet 环境那么globalSession 相当于 session。

     init-method：指定类中的初始化方法名称。 

     destroy-method：指定类中销毁方法名称。

2. **bean** 的作用范围和生命周期：

   - 单例对象:scope="singleton" 一个应用只有一个对象的实例。它的作用范围就是整个引用。 

     - 生命周期:

       对象出生:当应用加载，创建容器时，对象就被创建了。 

       对象活着:只要容器在，对象一直活着。 

       对象死亡:当应用卸载，销毁容器时，对象就被销毁了。

   - 多例对象:scope="prototype"每次访问对象时，都会重新创建对象实例。 

     - 生命周期:

       对象出生:当使用对象时，创建新的对象实例。

        对象活着:只要对象在使用中，就一直活着。

       对象死亡:当对象长时间不用时，被 java 的垃圾回收器回收了。

3. 实例化 **Bean** 的三种方式：

   - 第一种方式:使用默认无参构造函数

     ```
     <!--在默认情况下:
     它会根据默认无参构造函数来创建类对象。如果 bean 中没有默认无参构造函数，将会
     创建失败。
     -->
     <bean id="accountService" class="club.banyuan.service.impl.AccountServiceImpl"/>
     ```

   - 第二种方式:spring 管理静态工厂-使用静态工厂的方法创建对象

     ```
     /**
     * 模拟一个静态工厂，创建业务层实现类 
     */ 
     public class StaticFactory {
     	public static IAccountService createAccountService(){
     		return new AccountServiceImpl(); 
     		}
     }
     <!-- 此种方式是:
     使用 StaticFactory 类中的静态方法 createAccountService 创建对象，并存入 spring 容 器
     id 属性:指定 bean 的 id，用于从容器中获取 class 属性:指定静态工厂的全限定类名 factory-method 属性:指定生产对象的静态方法
     -->
     <bean id="accountService" class="club.banyuan.factory.StaticFactory"
     factory-method="createAccountService"></bean>
     ```

   - spring 管理实例工厂-使用实例工厂的方法创建对象

     ```
     /**
     * 模拟一个实例工厂，创建业务层实现类
     * 此工厂创建对象，必须现有工厂实例对象，再调用方法
     */
     public class InstanceFactory {
     	public IAccountService createAccountService(){ 
     		return new AccountServiceImpl();
     	} 
     }
     <!-- 此种方式是: 先把工厂的创建交给 spring 来管理。
     然后在使用工厂的 bean 来调用里面的方法
     factory-bean 属性:用于指定实例工厂 bean 的 id。 factory-method 属性:用于指定实例工厂中创建对象的方法。
     -->
     <bean id="instancFactory" class="club.banyuan.factory.InstanceFactory"></bean>
     <bean id="accountService" factory-bean="instancFactory" factory-method="createAccountService"></bean>
     ```

### spring的依赖注入：坐等框架把持久层对象传入业务层，而不用我们自己去获取

1. 构造方法注入：

   顾名思义，就是使用类中的构造函数，给成员变量赋值。注意，赋值的操作不是我们自己做 的，而是通过配置的方式，让 spring 框架来为我们注入。

   ```
   package club.banyuan.service.impl;
   
   import club.banyuan.dao.IAccountDao;
   import club.banyuan.dao.impl.AccountDaoImpl;
   import club.banyuan.service.IAccountService;
   import java.util.Date;
   
   /**
    * *账户的业务层实现类
    */
   
   public class AccountServiceImpl implements IAccountService {
   
     private String name;
     private Integer age;
     private Date birthday;
     //private IAccountDao accountDao = new AccountDaoImpl();//此处的依赖关系有待解决
   
     @Override
     public void saveAccount() {
       //  accountDao.saveAccount();
       System.out.println(name + "," + age + "," + birthday);
     }
   
     public AccountServiceImpl(String name, Integer age, Date birthday) {
       this.name = name;
       this.age = age;
       this.birthday = birthday;
     }
   }
   ```

   ```
   配置文件中配置注入
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   
     <!-- bean 标签:用于配置让spring 创建对象，
     并且存入 ioc 容器之中 id 属性:对象的唯一标识。
     class 属性:指定要创建对象的全限定类名 -->
     <!-- 配置 service -->
     <bean id="accountService" class="club.banyuan.service.impl.AccountServiceImpl">
       <constructor-arg name="name" value="zhangsan"></constructor-arg>
       <constructor-arg name="age" value="18"></constructor-arg>
       <constructor-arg name="birthday" ref="now"></constructor-arg>
     </bean>
     <bean id="now" class="java.util.Date"></bean>
     <!-- 配置 dao -->
     <bean id="accountDao" class="club.banyuan.dao.impl.AccountDaoImpl"></bean>
   </beans>
   <!-- 使用构造函数的方式，给 service 中的属性传值 要求: 类中需要提供一个对应参数列表的构造函数。 
   涉及的标签:constructor-arg
   属性:
   index:指定参数在构造函数参数列表的索引位置 
   type:指定参数在构造函数中的数据类型 
   name:指定参数在构造函数中的名称 用这个找给谁赋值
   =======上面三个都是找给谁赋值，下面两个指的是赋什么值的============== 
   value:它能赋的值是基本数据类型和 String 类型
   ref:它能赋的值是其他 bean 类型，也就是说，必须得是在配置文件中配置过的 bean -->
   ```

2. set方法注入：

   是在类中提供需要注入成员的 set 方法

   ```
   package club.banyuan.service.impl;
   
   import club.banyuan.dao.IAccountDao;
   import club.banyuan.dao.impl.AccountDaoImpl;
   import club.banyuan.service.IAccountService;
   import java.util.Date;
   
   /**
    * *账户的业务层实现类
    */
   
   public class AccountServiceImpl implements IAccountService {
   
     private String name;
     private Integer age;
     private Date birthday;
     //private IAccountDao accountDao = new AccountDaoImpl();//此处的依赖关系有待解决
   
   
     public void setName(String name) {
       this.name = name;
     }
   
     public void setAge(Integer age) {
       this.age = age;
     }
   
     public void setBirthday(Date birthday) {
       this.birthday = birthday;
     }
   
     @Override
     public void saveAccount() {
       //  accountDao.saveAccount();
       System.out.println(name + "," + age + "," + birthday);
     }
   
   //  public AccountServiceImpl(String name, Integer age, Date birthday) {
   //    this.name = name;
   //    this.age = age;
   //    this.birthday = birthday;
   }
   //  }
   
   ```

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   
     <!-- bean 标签:用于配置让spring 创建对象，
     并且存入 ioc 容器之中 id 属性:对象的唯一标识。
     class 属性:指定要创建对象的全限定类名 -->
     <!-- 配置 service -->
     <bean id="accountService" class="club.banyuan.service.impl.AccountServiceImpl">
       <!--    <constructor-arg name="name" value="zhangsan"></constructor-arg>-->
       <!--    <constructor-arg name="age" value="18"></constructor-arg>-->
       <!--    <constructor-arg name="birthday" ref="now"></constructor-arg>-->
       <property name="name" value="lisi"></property>
       <property name="age" value="20"></property>
       <property name="birthday" ref="now"></property>
     </bean>
     <bean id="now" class="java.util.Date"></bean>
     <!-- 配置 dao -->
     <bean id="accountDao" class="club.banyuan.dao.impl.AccountDaoImpl"></bean>
   </beans>
   
   <!-- 通过配置文件给 bean 中的属性传值:使用 set 方法的方式 
   涉及的标签:property
   属性:
   name:找的是类中 set 方法后面的部分 
   ref:给属性赋值是其他 bean 类型的 
   value:给属性赋值是基本数据类型和 string 类型的
   实际开发中，此种方式用的较多。
   -->
   ```

3. 集合注入

   ```
   package club.banyuan.service.impl;
   
   import club.banyuan.service.IAccountService;
   import java.util.Arrays;
   import java.util.Date;
   import java.util.List;
   import java.util.Map;
   import java.util.Properties;
   import java.util.Set;
   
   /**
    * *账户的业务层实现类
    */
   
   public class AccountServiceImpl2 implements IAccountService {
   
     private String[] myStrs;
     private List<String> myList;
     private Set<String> mySet;
     private Map<String, String> myMap;
     private Properties myProps;
   
     public void setMyStrs(String[] myStrs) {
       this.myStrs = myStrs;
     }
   
     public void setMyList(List<String> myList) {
       this.myList = myList;
     }
   
     public void setMySet(Set<String> mySet) {
       this.mySet = mySet;
     }
   
     public void setMyMap(Map<String, String> myMap) {
       this.myMap = myMap;
     }
   
     public void setMyProps(Properties myProps) {
       this.myProps = myProps;
     }
   
     @Override
     public void saveAccount() {
       System.out.println(Arrays.toString(myStrs));
       System.out.println(myList);
       System.out.println(mySet);
       System.out.println(myMap);
       System.out.println(myProps);
     }
   }
   
   ```

   ```
   <!-- 注入集合数据
   List 结构的: array,list,set
   Map 结构的 map,entry,props,prop -->
     <!-- 在注入集合数据时，只要结构相同，标签可以互换 -->
     <bean id="accountService2"
       class="club.banyuan.service.impl.AccountServiceImpl2">
       <!-- 给数组注入数据 -->
       <property name="myStrs">
         <set>
           <value>AAA</value>
           <value>BBB</value>
           <value>CCC</value>
         </set>
       </property>
       <!-- 注入 list 集合数据 -->
       <property name="myList">
         <array>
           <value>AAA</value>
           <value>BBB</value>
           <value>CCC</value>
         </array>
       </property>
       <!-- 注入 set 集合数据 -->
       <property name="mySet">
         <list>
           <value>AAA</value>
           <value>BBB</value>
           <value>CCC</value>
         </list>
       </property>
       <!-- 注入 Map 数据 -->
       <property name="myMap">
         <props>
           <prop key="testA">aaa</prop>
           <prop key="testB">bbb</prop>
         </props>
       </property>
       <!-- 注入 properties 数据 -->
       <property name="myProps">
         <map>
           <entry key="testA" value="aaa"></entry>
           <entry key="testB">
             <value>bbb</value>
           </entry>
         </map>
       </property>
     </bean>
   ```

   ### 基于注解的IOC配置

   1. 准备spring的开发包，配置pom.xml文件

      ```
      <?xml version="1.0" encoding="UTF-8"?>
      <project xmlns="http://maven.apache.org/POM/4.0.0"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>
      
        <groupId>club.banyuan</groupId>
        <artifactId>mvn-spring2</artifactId>
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
      
        </dependencies>
      
      </project>
      ```

   2. 在类的根路径(resources)下创建application. xml 文件

      ```
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:context="http://www.springframework.org/schema/context"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd">
      
        <!-- <context:property-placeholder location="const.properties"/> -->
        <!-- 告知 spring 创建容器时要扫描的包 -->
        <context:component-scan base-package="club.banyuan"></context:component-scan>
      
      </beans>
      ```

   3. 常用注解

      - 用于创建对象的：相当于配置文件中的 <bean id="" class="">
        - @component：把资源让 spring 来管理。相当于在 xml 中配置一个 bean。
          - value属性：指定 bean 的 id。如果不指定 value 属性，默认 bean 的 id 是当前类的类名。
          - @component的衍生注解：
            - @Controller：用于表现层的注解
            - @Service：用于业务层的注解
            - @Repository：用于持久层的注解
            - <font color=red >细节:如果注解中有且只有一个属性要赋值时，且名称是 value，value 在赋值是可以不写</font>。
      - 用于注入数据的：相当于配置文件中set方法注入的 <property name="" ref=""> <property name="" value="">
        - @Autowired：
          - 作用：自动按照类型注入。当使用注解注入属性时，set 方法可以省略。它只能注入其他 bean 类型。当有多个 类型匹配时，使用要注入的对象变量名称作为 bean 的 id，在 spring 容器查找，找到了也可以注入成功。找不到 就报错。
        - @Qualifier：
          - 作用：在自动按照类型注入的基础之上，再按照 Bean 的 id 注入。<font color=red>它在给字段注入时不能独立使用，必须和 @Autowire 一起使用</font>。但是给方法参数注入时，可以独立使用。
          - value属性：指定 bean 的 id。
        - @Resource：
          - 作用：直接按照 Bean 的 id 注入。它也只能注入其他 bean 类型。单独使用
          - name属性：指定 bean 的 id。
      - 用于改变作用范围的：相当于配置文件中的 <bean id="" class="" scope="">
        - @Scope：
          - 指定 bean 的作用范围。
          - value属性：指定范围的值。
          - 取值:singleton  prototype  request  session  globalsession

   4. 新注解：用于离开applicationContext.xml配置文件

      - 待改造问题

        ```
        <!-- 告知 spring 框架在，读取配置文件，创建容器时，扫描注解，依据注解创建对象，并 存入容器中 -->
        <context:component-scan base-package="com.itheima"></context:component-scan>
        ```

      - @Configuration

        - 作用：用于指定当前类是一个 spring 配置类，当创建容器时会从该类上加载注解。获取容器时

          需要使用 AnnotationConfigApplicationContext(有@Configuration 注解的类全限定名.class)。

      - @ComponentScan

        - 作用：用于指定spring在初始化容器时要扫描的包，相当于配置文件中的<context:componnet-scan base-package="club.banyuan"/>

      - ```
        package club.banyuan.config;
        
        import org.springframework.context.annotation.ComponentScan;
        import org.springframework.context.annotation.Configuration;
        import org.springframework.context.annotation.PropertySource;
        
        @Configuration
        @ComponentScan("club.banyuan")
        @PropertySource("classpath:const.properties")
        public class SpringConfiguration {
        
        }
        
        ```

        ```
        注意: 这里已经把配置文件用类来代替了
        ```

      - @Bean

        - 作用：该注解只能写在方法上，表明使用此方法创建一个对象，并且放入 spring 容器

        - 属性：name:给当前@Bean 注解方法创建的对象指定一个名称(即 bean 的 id)

        - ```
          package club.banyuan.beans;
          
          import club.banyuan.entity.User;
          import org.springframework.context.annotation.Bean;
          import org.springframework.stereotype.Component;
          
          @Component
          public class EntityFactory {
              @Bean(value="user")
              public User getUser(){
                  return new User();
              }
          }
          
          ```

          ```
          User user = (User) ctx.getBean("user");
          ```

          

        - @PropertySource

          - 作用：用于加载properties文件中的配置

          - 属性: value[]:用于指定 properties 文件位置。如果是在类路径下，需要写上 classpath:

            ```
            package club.banyuan.config;
            
            import org.springframework.context.annotation.ComponentScan;
            import org.springframework.context.annotation.Configuration;
            import org.springframework.context.annotation.PropertySource;
            
            @Configuration
            @ComponentScan("club.banyuan")
            @PropertySource("classpath:const.properties")
            public class SpringConfiguration {
            
            }
            
            ```

            ![image-20200731155153213](/Users/edz/Library/Application Support/typora-user-images/image-20200731155153213.png)

          - 通过注解获取容器

            ```
            ApplicationContext ctx =
                            new AnnotationConfigApplicationContext(club.banyuan.config.SpringConfiguration.class);
            ```

            





​			


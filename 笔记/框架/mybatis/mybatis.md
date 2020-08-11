## mybatis

### 概述：

1. 一款对SQL进行轻量级封装的高效率ORM框架(ORM:对象关系映射)
2. mybatis 是一个优秀的基于 java 的持久层框架，它内部封装了 jdbc，使开发者只需要关 注 sql 语句本身， 而不需要花费精力去处理加载驱动、创建连接、创建 statement 等繁 杂的过程。
3. mybatis 通过 ***xml*** 或***注解***的方式将要执行的各种 statement 配置起来，并通过 java 对象 和 statement 中 sql 的动态参数进行映射生成最终执行的 sql 语句，最后由 mybatis 框 架执行 sql 并将结果映射为 java 对象并 返回。
4. 采用 ***ORM 思想***（对象关系映射）解决了实体和数据库映射的问题，对 jdbc 进行了封装，屏蔽了 jdbc api 底层访问细节，使我们不用与 jdbc api （不用写持久层接口的实现类）打交道，就可以完成对数据库的持久化操作。

### mybatis入门：

#### 配置文件方式：

1. 搭建mybatis开发环境

   - 创建maven工程

2. 在pom.xml主配置文件中添加mybatis坐标

   ```
   <!--  项目对象模型-->
     <dependencies>
   <!--    mybatis的jar包-->
       <dependency>
         <groupId>org.mybatis</groupId>
         <artifactId>mybatis</artifactId>
         <version>3.4.5</version>
       </dependency>
   
   <!--    mysql的jar包-->
       <dependency>
         <groupId>mysql</groupId>
         <artifactId>mysql-connector-java</artifactId>
         <version>8.0.19</version>
       </dependency>
   
   <!--    以上是使用mybatis必须导入的坐标-->
   
   <!--    日志部分-->
       <dependency>
         <groupId>log4j</groupId>
         <artifactId>log4j</artifactId>
         <version>1.2.12</version>
       </dependency>
   
   <!--    单元测试-->
       <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
         <version>4.12 </version>
         <scope>test</scope>
       </dependency>
     </dependencies>
   ```

3. 创建实体类（实体类属性和数据库表单字段一一对应）

   ```
   路径为main/java/club/banyuan/entity
   ```

4. 创建dao层的接口（接口中的方法对应数据库的增删查改）

   ```
   路径为main/java/club/banyuan/dao
   ```

<!--以上为mybatis准备阶段-->

5. 配置mybatis的主配置文件

   ```
   <!--    创建路径为main/resources-->
   <!--    配置文件通常取名SqlMapConfig.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
     "http://mybatis.org/dtd/mybatis-3-config.dtd">
   
   <!--mybatis的主配置文件-->
   <configuration>
     <settings>
       <setting name="logImpl" value="STDOUT_LOGGING"/>
     </settings>
   
     <typeAliases>
       <package name="club.banyuan.entity"/>
     </typeAliases>
   <!--  配置环境-->
     <environments default="mysql">
   <!--    配置mysql环境-->
       <environment id="mysql">
   <!--      配置事务的类型-->
         <transactionManager type="JDBC"></transactionManager>
   <!--      配置数据源（连接池）-->
         <dataSource type="POOLED">
   <!--        配置连接数据库的四个基本信息-->
           <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
           <property name="url" value="jdbc:mysql://localhost:3306/db2"/>
           <property name="username" value="root"/>
           <property name="password" value="ych417130"/>
         </dataSource>
       </environment>
     </environments>
   <!--  指定配置映射文件的位置，映射配置指的是每个dao独立的配置文件-->
     <mappers>
   <!--    路径是重点 是"/" 不是"."-->
      // <mapper resource="club/banyuan/dao/IStudentsDao.xml"/>
     </mappers>
   </configuration>
   ```

6. 配置（dao层接口）映射文件

   ```
   <!--    创建路径为main/resources/club/banyuan/dao-->	
   <!--    映射配置文件通常取名：dao层接口名.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
     "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <!--IStudentsDao的映射配置文件-->
   <mapper namespace="club.banyuan.dao.IStudentsDao">
   <!-- 此标签内放置SQL语句-->
   <!--  配置查询所有-->
     <select id="findAll" resultType="club.banyuan.entity.Students">
       select * from student
     </select>
   
   </mapper>
   ```

7. 配置日志文件

   ```
   <!-- 文件名为log4j.properties-->
   ###显示SQL语句部分
   log4j.logger.com.ibatis=DEBUG
   log4j.logger.com.ibatis.common.jdbc.SimpleDataSource=DEBUG
   log4j.logger.com.ibatis.common.jdbc.ScriptRunner=DEBUG
   log4j.logger.com.ibatis.sqlmap.engine.impl.SqlMapClientDelegate=DEBUG
   log4j.logger.Java.sql.Connection=DEBUG
   log4j.logger.java.sql.Statement=DEBUG
   log4j.logger.java.sql.PreparedStatement=DEBUG
   ```

#### 注解方式：

1. 注解方式和配置文件方式唯一区别在于无需写映射配置文件

2. 主配置文件中的mapper标签改成

   ```
   <mapper class="club.banyuan.dao.IStudentsDao"/>
   ```

3. 接口内方法上写上注释

   ```
   例：
   public interface IStudentsDao {
   
     /**
      * 查询所有
      * @return
      */
     @Select("select * from student")
     public List<Students> findAll();
   }
   ```

<!-- 以上为mybatis的开发环境搭建流程-->

### mybatis环境搭建的注意事项：

1. 持久层操作接口名称和映射配置文件也成为mapper

```
例：IStudentsDao和IStudentsMapper一样
```

2. **创建package时club.banyuan.dao是三级目录，而创建Directory时club.banyuan.dao是一级目录，创建Directory三级目录时使用club/banyuan/dao的方式创建**

   <!-- 以下三点实现后，无需写dao接口的实现类（mybatis的核心）-->

3. mybatis的（dao层接口）映射配置文件必须和接口的包结构相同

4. 映射配置文件的mapper标签namespace属性取值必须是dao接口的全限定名

5. 映射配置文件的操作配置，id属性的取值必须是dao接口的方法名

### mybatis测试案例

1. 路径为：test/java/club/banyuan/test

2. ```
   package club.banyuan.test;
   
   import club.banyuan.dao.IStudentsDao;
   import club.banyuan.entity.Students;
   import java.io.IOException;
   import java.io.InputStream;
   import java.util.List;
   import org.apache.ibatis.io.Resources;
   import org.apache.ibatis.session.SqlSession;
   import org.apache.ibatis.session.SqlSessionFactory;
   import org.apache.ibatis.session.SqlSessionFactoryBuilder;
   import org.junit.After;
   import org.junit.Before;
   import org.junit.Test;
   
   public class TestStudents {
   
     private InputStream ins;
     private SqlSessionFactory sessionFactory;
     private SqlSession session;
     private IStudentsDao iStudentsDao;
   
     @Before
     public void init() throws IOException {
   //    读取配置文件
   //		读取配置文件时不使用绝对路径（d:/xxx/xxx.xml）和相对路径（src/main/resources/xxx.xml）
   //		电脑不一定有绝对路径的盘，项目部署后相对路径会丢失
   //    1.使用类加载器的方式，它只会读取类路径的配置文件
   //    2.使用ServletContext对象的getPath()
       ins = Resources.getResourceAsStream("SqlMapConfig.xml");
   //    创建SqlSessionFactory工厂
   //  创建工厂，mybatis使用了构建者模式：把对象的创建细节隐藏，使使用者直接调用方法拿到对象，屏蔽其他繁琐操作
       SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
       sessionFactory = builder.build(ins);
   //    使用工厂创建SQLSession对象，使用了工厂模式：解耦，降低类之间的依赖关系
       session = sessionFactory.openSession();
   //    使用SQLSession创建dao接口的代理对象，使用了代理模式：不修改源码的基础上对已有方法增强
       iStudentsDao = session.getMapper(IStudentsDao.class);
     }
   
     @Test
     public void findAll(){
       List<Students> students = iStudentsDao.findAll();
       for (Students student : students) {
         System.out.println(student);
       }
   
     }
   
     @After
     public void destroy() throws IOException {
       session.commit();
       session.close();
       ins.close();
     }
   
   }
   
   ```


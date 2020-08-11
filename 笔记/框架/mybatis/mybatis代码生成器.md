### 创建步骤：

1. 创建maven工程

2. 配置pom.xml文件

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
     <modelVersion>4.0.0</modelVersion>
   
     <groupId>club.banyuan</groupId>
     <artifactId>mybatis-generator</artifactId>
     <version>1.0-SNAPSHOT</version>
     <packaging>war</packaging>
     <parent>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-parent</artifactId>
       <version>2.3.1.RELEASE</version>
       <relativePath/> <!-- lookup parent from repository -->
     </parent>
     <properties>
       <java.version>1.8</java.version>
     </properties>
     <dependencies>
       <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-test</artifactId>
         <scope>test</scope>
         <exclusions>
           <exclusion>
             <groupId>org.junit.vintage</groupId>
             <artifactId>junit-vintage-engine</artifactId>
           </exclusion>
         </exclusions>
       </dependency>
       <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
       <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-tomcat</artifactId>
         <scope>provided</scope>
       </dependency>
       <dependency>
         <groupId>mysql</groupId>
         <artifactId>mysql-connector-java</artifactId>
         <version>8.0.20</version>
       </dependency>
       <!--mybatis-->
       <dependency>
         <groupId>org.mybatis.spring.boot</groupId>
         <artifactId>mybatis-spring-boot-starter</artifactId>
         <version>2.1.3</version>
       </dependency>
       <!--mapper-->
       <dependency>
         <groupId>tk.mybatis</groupId>
         <artifactId>mapper-spring-boot-starter</artifactId>
         <version>1.2.4</version>
       </dependency>
       <dependency>
         <groupId>org.mybatis.spring.boot</groupId>
         <artifactId>mybatis-spring-boot-starter-test</artifactId>
         <version>2.1.3</version>
         <scope>test</scope>
       </dependency>
       <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
         <version>4.13</version>
         <scope>test</scope>
       </dependency>
       <!-- mybatis 逆向生成工具  -->
       <dependency>
         <groupId>org.mybatis.generator</groupId>
         <artifactId>mybatis-generator-core</artifactId>
         <version>1.3.2</version>
         <scope>compile</scope>
         <optional>true</optional>
       </dependency>
     </dependencies>
     <build>
       <plugins>
         <plugin>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-maven-plugin</artifactId>
         </plugin>
       </plugins>
     </build>
   </project>
   
   
   ```

3. 在项目根路径下创建generator.xml配置文件，引入相应配置

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE generatorConfiguration
     PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
     "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
   
   <generatorConfiguration>
     <context id="MysqlContext" targetRuntime="MyBatis3Simple" defaultModelType="flat">
       <property name="beginningDelimiter" value="`"/>
       <property name="endingDelimiter" value="`"/>
   
       <!-- 通用mapper所在目录 -->
       <plugin type="tk.mybatis.mapper.generator.MapperPlugin">
         <property name="mappers" value="club.banyuan.my.mapper.MyMapper"/>
       </plugin>
   
       <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
         connectionURL="jdbc:mysql://localhost:3306/shoppingStreet?useSSL=false&amp;serverTimezone=UTC"
         userId="root"
         password="ych417130">
         <!--      解决生成实体类错误或者生成其他数据库表单实体类的标签-->
         <property name="nullCatalogMeansCurrent" value="true"/>
       </jdbcConnection>
   
       <!-- 对应生成的pojo所在包 -->
       <!-- 会自动生成entity包-->
       <javaModelGenerator targetPackage="club.banyuan.entity" targetProject="src/main/java"/>
   
       <!-- 对应生成的mapper所在目录 -->
       <!-- 同理-->
       <sqlMapGenerator targetPackage="mapper" targetProject="src/main/resources"/>
   
       <!-- 配置mapper对应的java映射 -->
        <!-- 同理-->
       <javaClientGenerator targetPackage="club.banyuan.mapper" targetProject="src/main/java"
         type="XMLMAPPER"/>
   
       <!-- 数据库表 -->
       <!-- 代码生成器会根据这个配置生成实体类，持久层接口，mapper配置文件-->
       <table tableName="News"></table>
       <table tableName="order"></table>
       <table tableName="user"></table>
       <table tableName="product"></table>
     </context>
   </generatorConfiguration>
   ```

4. 创建类路径，编写springboot启动类

   ```
   package club.banyuan;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import tk.mybatis.spring.annotation.MapperScan;
   
   @SpringBootApplication
   @MapperScan(basePackages = "club.banyuan.mapper")
   public class SpringBootApp {
   
     public static void main(String[] args) {
       SpringApplication.run(SpringBootApp.class);
     }
   }
   
   ```

5. 创建utils包，编写自动生成器java代码

   ```
   package club.banyuan.utils;
   
   import java.io.File;
   import java.util.ArrayList;
   import java.util.List;
   import org.mybatis.generator.api.MyBatisGenerator;
   import org.mybatis.generator.config.Configuration;
   import org.mybatis.generator.config.xml.ConfigurationParser;
   import org.mybatis.generator.internal.DefaultShellCallback;
   
   
   public class GeneratorDisplay {
   
     public void generator() throws Exception {
   
       List<String> warnings = new ArrayList<String>();
       boolean overwrite = true;
       //指定 逆向工程配置文件
       File configFile = new File("generator.xml");
       ConfigurationParser cp = new ConfigurationParser(warnings);
       Configuration config = cp.parseConfiguration(configFile);
       DefaultShellCallback callback = new DefaultShellCallback(overwrite);
       MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config,
           callback, warnings);
       myBatisGenerator.generate(null);
   
     }
   
     public static void main(String[] args) throws Exception {
       try {
         GeneratorDisplay generatorSqlmap = new GeneratorDisplay();
         generatorSqlmap.generator();
       } catch (Exception e) {
         e.printStackTrace();
       }
     }
   }
   
   ```

6. 编写application.yml配置文件

   ```
   #springboot 集成 Mybatis 环境
   #pojo 别名扫描包
   mybatis:
     type-aliases-package: club.banyuan.entity
     mapper-locations: classpath:club/banyuan/mapper/*Mapper.xml
   #  添加数据量的链接信息
   spring:
     datasource:
       driverClassName: com.mysql.cj.jdbc.Driver
       url: jdbc:mysql://localhost:3306/shoppingStreet?&useSSL=false&serverTimezone=UTC
       username: root
       password: ych417130
   ```

7. 编写通用mapper代码，路径为：main/java/club/banyuan/my/mapper

   ```
   /*
    * The MIT License (MIT)
    *
    * Copyright (c) 2014-2016 abel533@gmail.com
    *
    * Permission is hereby granted, free of charge, to any person obtaining a copy
    * of this software and associated documentation files (the "Software"), to deal
    * in the Software without restriction, including without limitation the rights
    * to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
    * copies of the Software, and to permit persons to whom the Software is
    * furnished to do so, subject to the following conditions:
    *
    * The above copyright notice and this permission notice shall be included in
    * all copies or substantial portions of the Software.
    *
    * THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
    * IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
    * FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
    * AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
    * LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
    * OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
    * THE SOFTWARE.
    */
   
   package club.banyuan.my.mapper;
   
   import tk.mybatis.mapper.common.Mapper;
   import tk.mybatis.mapper.common.MySqlMapper;
   
   /**
    * 继承自己的MyMapper
    */
   public interface MyMapper<T> extends Mapper<T>, MySqlMapper<T> {
   
   }
   
   ```

8. 启动自动生成器，java代码会读取generator.xml配置文件，自动创建路径，实体类，mapper接口，mapper映射配置文件

   ![image-20200811153618202](/Users/edz/Library/Application Support/typora-user-images/image-20200811153618202.png)

9. 编写测试类进行测试

   ```
   package club.banyuan;
   
   import club.banyuan.entity.News;
   import club.banyuan.entity.Product;
   import club.banyuan.entity.User;
   import club.banyuan.mapper.NewsMapper;
   import club.banyuan.mapper.ProductMapper;
   import club.banyuan.mapper.UserMapper;
   import org.junit.Test;
   import org.junit.runner.RunWith;
   import org.mybatis.spring.boot.test.autoconfigure.MybatisTest;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.test.autoconfigure.jdbc.AutoConfigureTestDatabase;
   import org.springframework.test.annotation.Rollback;
   import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
   
   import java.util.List;
   import tk.mybatis.mapper.entity.Example;
   
   @RunWith(SpringJUnit4ClassRunner.class)
   
   @MybatisTest
   
   //这个是启用自己配置的数据元，不加载采用虚拟数据源
   @AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)
   
   //这个是默认是回滚，不会commit入数据库，改成false 则commit
   @Rollback(false)
   public class TestGenerateBatis {
   
     @Autowired
     private NewsMapper newsMapper;
     @Autowired
     private UserMapper userMapper;
     @Autowired
     private ProductMapper productMapper;
   
     @Test
     public void testGetAll() {
       List<News> newsList = newsMapper.selectAll();
       for (News news : newsList) {
         System.out.println(news);
       }
     }
   
     @Test
     public void testGetUserAll() {
       List<User> users = userMapper.selectAll();
       for (User user : users) {
         System.out.println(user);
       }
     }
   
     @Test
     public void testGetProductAll() {
       List<Product> products = productMapper.selectAll();
       for (Product product : products) {
         System.out.println(product);
       }
     }
   
     @Test
     public void testMethod() {
       News news = new News();
       news.setContent("duang");
       news.setCreatetime("2020-7-24");
       news.setTitle("霸王洗发水");
       int insert = newsMapper.insert(news);
       System.out.println(insert);
     }
   
     /**
      * 模糊查询
      */
     @Test
     public void testSelectExample(){
       Example example = new Example(News.class);
       Example.Criteria criteria = example.createCriteria();
   
       criteria.andLike("title","%aa%");
   
       List<News> newsList = newsMapper.selectByExample(example);
       for (News news : newsList) {
         System.out.println(news);
       }
     }
   
   }
   
   ```

   
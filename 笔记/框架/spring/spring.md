### spring学习流程

1. Spring框架的概述以及spring中基于xml的ioc配置
2. Spring中基于注解的IOC和IOC的案例
3. spring中的aop和基于xml和注解的aop配置
4. spring中的JdbcTemlate以及spring事务控制

### spring的概述

1. spring是什么

   - Spring 是分层的 Java SE/EE 应用 full-stack 轻量级开源框架
   - 提供了展现层<font color=red> Spring MVC和持久层 Spring JDBC以及业务层事务管理</font>等众多的企业级应用技术
   - 能整合开源世界众多著名的第三方框架和类库

2. spring的两大核心

   - IoC(Inverse Of Control: 反转控制)和 AOP(Aspect Oriented Programming:面向切面编程)

3. spring的发展历程和优势

4. spring的体系结构

   ![image-20200726225040193](/Users/edz/Library/Application Support/typora-user-images/image-20200726225040193.png)

### 程序的耦合和解耦：

1. 在软件工程中，耦合指的就是就是对象之间的依赖性。对象之间的耦合越高，维护成本越高。
2. 划分模块的一个准则就是高内聚低耦合。

总结: 耦合是影响软件复杂程度和设计质量的一个重要因素，在设计上应采用以下原则:

如果模块间必须存在耦合，就尽量使用数据耦合，少用控制耦合，限制公共耦合的范围，尽量避免使用内容耦合。

### 解决程序耦合的思路：

工厂模式解耦：

*在实际开发中我们可以把三层的对象都使用配置文件配置起来，当启动服务器应用加载的时 候，<font color=red>让一个类中的方法通过读取配置文件，把这些对象创建出来并存起来。</font>在接下来的使用的时候，直接拿过来用就好了。那么，这个读取配置文件，创建和获取三层对象的类就是 工厂。*

### 控制反转-Inversion Of Control：

- 由于我们是很多对象，<font color=red>肯定要找个集合来存</font>。这时候有 Map 和 List 供选择。到底选 Map 还是 List 就看我们有没有查找需求。有查找需求，选 Map。

- 在应用加载时，创建一个 Map，用于存放三层对象。把这个 map 称之为<font color=red>容器</font>。
- 工厂就是负责从容器中获取指定对象的类。
- 原来：在获取对象时，都是采用 new 的方式。是主动的。
- 现在：获取对象时，跟工厂要，有工厂为我们查找或者创建对象。是被动的。
- <font color=red>这种被动接收的方式获取对象的思想就是控制反转，它是 spring 框架的核心之一</font>。


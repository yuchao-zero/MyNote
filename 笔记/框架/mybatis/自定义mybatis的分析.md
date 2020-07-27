## mybatis的有关信息分析

连接数据库的信息，创建connection对象

```
<dataSource type="POOLED">
<!--        配置连接数据库的四个基本信息-->
        <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/db2"/>
        <property name="username" value="root"/>
        <property name="password" value="ych417130"/>
      </dataSource>
```

映射配置信息

```
<!--  指定配置映射文件的位置，映射配置指的是每个dao独立的配置文件-->
  <mappers>
<!--    路径是重点 是"/" 不是"."-->
   // <mapper resource="club/banyuan/dao/IStudentsDao.xml"/>
  </mappers>
```

1. 通过此信息获取preparestatement，执行SQL语句
2. 此配置中含有封装的实体类全限定类名

```
<mapper namespace="club.banyuan.dao.IStudentsDao">
<!-- 此标签内放置SQL语句-->
<!--  配置查询所有-->
  <select id="findAll" resultType="club.banyuan.entity.Students">
    select * from student
  </select>
```


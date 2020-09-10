#### 数据库设计三大范式：

1. 第一范式：确保每列的原子性，即每列都是不可分割的最小数据单元
2. 第二范式：每个表只描述一件事情
3. 第三范式：满足第二范式，且除了主键以外的其他列都不传递依赖于主键



#### SQL：Structured Query Language 结构化查询语言



#### SQL语句分类：

1. Data Definition Language (DDL 数据定义语言) 如：建库，建表
2. Data Manipulation Language(DML 数据操纵语言)，如：对表中的记录操作增删改
3. Data Query Language(DQL 数据查询语言)，如：对表中的查询操作
4. Data Control Language(DCL 数据控制语言)，如：对用户权限的设置









### 注意事项：

1. 外键约束：

   被约束的是从表，即在从表中添加外键约束，约束的表为主表

   从表要持有一个外键字段，外键对应主表的主键

   外键约束语法：constraint 外键约束别名 foreign key (外键字段名) references 主表(主键字段名)
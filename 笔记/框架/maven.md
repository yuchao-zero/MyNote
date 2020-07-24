# maven

### 简介：

maven是一个项目管理工具，它包含了一个项目对象模型（POM），一组标准集合，一个项目生命周期，一个依赖管理系统，用来运行定义在生命周期阶段和插件目标逻辑

### maven解决的问题：

1. 批量导入jar包，避免手动添加jar包引起的jar包版本冲突
2. 编译Java文件
3. 单元测试
4. 方便打jar包和war包

### maven的依赖管理概念：

传统web项目中jar包放在开发项目中，占据大量空间。基于maven的开发项目，jar包置于本地仓库中，通过在maven的pom.xml文件中添加jar包坐标来在本地找到并利用jar包

### maven标准目录结构：

1. 核心代码部分

   **src/main/java**

2. 配置文件部分

   **src/main/resources**

3. 测试代码部分

   **src/test/java**

4. 测试配置文件

   **src/test/resources**

### maven概念模型：

1. 项目对象模型（pom.xml）：
   - 项目自身信息
   - 项目运行所依赖的jar包信息
   - 项目运行环境信息，如tomcat jdk等
2. 依赖管理模型（pom.xml配置文件中的dependency标签），即jar包：
   - groupid：公司组织名称
   - artifact：项目名
   - version：依赖工具的版本


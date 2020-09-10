1. 下载好maven后，解压到/Users/yuchao/Library/下

2. 终端输入：vi ~/.bash_profile

3. 配置

   ```
   MAVEN_HOME=/Users/yuchao/Library/apache-maven-3.6.3
   PATH=$MAVEN_HOME/bin:$PATH
   export MAVEN_HOME
   export PATH
   export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk-11.0.2.jdk/Contents/Home
   ```

4. 终端键入source ~/.bash_profile





#### idea配置：

1. maven home directory:/Users/yuchao/Library/apache-maven-3.6.3
2. User setting files:/Users/yuchao/Library/apache-maven-3.6.3/conf/settings.xml
3. local repository:默认



#### idea下maven项目报错：Cannot resolve org.springframework:spring-jdbc:5.2.7.RELEASE

解决方案：更改mavenidea配置，设置成以上内容
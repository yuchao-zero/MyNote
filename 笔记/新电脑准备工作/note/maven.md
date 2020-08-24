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
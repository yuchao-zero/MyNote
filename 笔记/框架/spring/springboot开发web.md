## springboot开发web

### Thymeleaf模板：

1. Thymeleaf 是用来开发 Web 和独立环境项目的现代服务器端 Java 模板引擎。

2. Thymeleaf 的主要作用是把 model 中的数据渲染到 html 中。

3. Thymeleaf 中所有的表达式都需要写在指令中，指令是 HTML5 中的自定义属性，在 Thymeleaf 中所有指令都是以 th:开头。因为表达式${user.name}是写在自定义属性中，因此在静态环境 下，表达式的内容会被当做是普通字符串，浏览器会自动忽略这些指令，这样就不会报错了

4. th:text 的含义就是替换所在标签 中的文本内容，于是 user.name 的值就替代了 span 中默认的请登录

   ```
   <!DOCTYPE html>
   <html lang="en" xmlns:th="http://www.thymeleaf.org">
   <head>
     <meta charset="UTF-8">
     <title>Title</title>
   </head>
   <body>
   <h1 th:text="${message}">show1</h1>
   
   <h1>
     欢迎您:<span th:text="${user.name}">请登录</span>
   </h1>
   </body>
   </html>
   ```

5. 如果不支持这种`th:`的命名空间写法，那么可以把`th:text`换成 `data-th-text`，Thymeleaf 也 可以兼容。

6. <font color=red>需要注意:${}内部的是通过 OGNL 表达式引擎解析的，外部的才是通过 Thymeleaf 的引擎 解析，因此运算符尽量放在${}外进行。</font>

### **ognl** 表达式的语法糖：

我们使用的是经典的对象.属性名方式。但有些情况下，我们的属性名可能本身也是变量，怎么办?
 ognl 提供了类似 js 的语法方式:
 例如:${user.name} 可以写作${user['name']}


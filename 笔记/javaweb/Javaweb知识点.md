1. servlet的生命周期：（单实例多线程的方式执行用户请求）

   1. 客户端发送http请求
   2. 容器解析请求（tomcat）
   3. 创建servlet实例
   4. 调用init方法进行初始化
   5. 调用service方法（doGet,doPost）
   6. 输出响应信息
   7. destroy销毁servlet对象（只有当当前的 Servlet 从 tomcat 移除的时候）
   8. 补充：5和6每次请求到达时，都会执行，其他的步骤只会执行一次，无论有几次请求

2. 转发和重定向的区别

   1. 转发是在服务器端进行，将请求在服务器资源之间传递，客户端浏览器地址栏不会显示转向后的地址
   2. 重定向是在客户端发生作用，通过发送一个新的请求实现页面跳转，地址栏可以显示转向后的地址

3. jsp页面的生命周期

   1. 第一次访问jsp页面的时候，tomcat将文件解析成servlet Java文件，既.java源代码文件
   2. tomcat将源代码文件解析成.class字节码文件，并执行
   3. 第二次访问这个jsp页面时，直接调用servlet的service方法处理请求

4. jsp的九大内置对象

   1. out：页面输出
   2. request：浏览器请求
   3. response：服务器响应
   4. session：存储用户信息
   5. config：服务器配置
   6. application：所有用户共享信息
   7. page：当前页转换后的servlet实例
   8. pageContext：jsp的页面容器
   9. exception：jsp页面发生异常

5. 四大对象作用域：

   1. page只在一个jsp页面起作用
   2. request对应用户一次请求，每次请求都是一个新的request对象
   3. session对应一个用户会话，既一个浏览器窗口，多次请求和响应
   4. application 作用域，对应整个应用上下文，当 tomcat 启动完毕后，application 就已经实例化成功了，一直到 tomcat 被关闭，application 对象才会被销毁掉。所以 application 对象的生命周期是贯穿整个 web 应用的

6. session会话机制

   1. 在服务器端内存开辟一个存储空间，把需要在多个请求和响应之间传递的值放入到这个空间中，以后无论是 servlet 还是 jsp 都可以在这个空间中取值
   2. 解决了数据无法在多次请求和响应之间传递的问题
   3. ![image-20200824094547487](/Users/yuchao/Library/Application Support/typora-user-images/image-20200824094547487.png)
   4. 每个 session 对象都与一个浏览器窗口对应
   5. session的默认非活动时间是30分钟

7. cookie

   1. 服务器端把 cookie 保存到客户端

   2. 在服务器端把客户端传来的 cookie 给读取到

   3. 默认情况下，cookie 是保存在客户端内存中，只要浏览器窗口关闭，cookie 对象也就没了

   4. 为了 cookie 能够持久化保存在客户端，能够在多个浏览器窗口间共享。我们需要设置 cookie 的生命周期，所以到服务器端加上一句话，在 Response 之前假设 cookie 的生命周期我设置为 24 小时那么 setMaxAge 的参数值就可以设为 24*3600 

   5. ```java
      HttpSession session = request.getSession(); 
      session.setAttribute("user", user); 
      Cookie cookie = new Cookie("loginName",loginName); 
      cookie.setMaxAge(24*3600); 
      response.addCookie(cookie);
      ```

8. session和cookie的区别

   1. 数据存放位置不同：session数据存放在服务器中，cookie数据存放在客户端浏览器中
   2. cookie安全性不高，外人可能通过分析存放在本地的cookie进行cookie欺骗，为了安全性应该用session
   3. 性能试用程度不同，session会在服务器中保存一定的时间，当访问量过大时，会造成服务器压力，影响性能，为了减轻服务器压力，应该是要cookie
   4. 数据存储大小不同：单个cookie保存的数据不得超过4k，很多浏览器站点会限制最多保存20个cookie。session则不同，是保存在服务器，和浏览器无关
   5. 会话机制不同：session会话机制是服务端会话机制，它使用类似哈希表结构存储信息；cookie是服务器存储在本地计算机上的小块文本，并随每个请求发送到同一服务器。 Web服务器使用HTTP标头将cookie发送到客户端。在客户端终端，浏览器解析cookie并将其保存为本地文件，该文件自动将来自同一服务器的任何请求绑定到这些cookie。


### 响应数据和结果视图：

1. 返回值分类：

   - 字符串

     jsp页面：

     ```
     <a href="test/testReturnString">testReturnString</a><br/>
     ```
     
     控制器Java代码
     
     ```
     /**
        * 测试返回值String
        *
        * @return
        */
       @RequestMapping("/testReturnString")
       public String testReturnString() {
         System.out.println("测试返回值为字符串类型的控制器");
         return "success";
       }
     ```
     
   - void
   
     jsp页面：
   
     ```
     <a href="test/testReturnVoid">testReturnVoid</a><br/>
     ```
   
     控制器Java代码
   
     ```
     /**
        * 测试返回值void
        */
       @RequestMapping("/testReturnVoid")
       public void testReturnVoid(HttpServletRequest request,
           HttpServletResponse response) throws Exception {
     //    1.请求转发
     //    request.getRequestDispatcher("/WEB-INF/pages/success.jsp")
     //        .forward(request, response);
     //    System.out.println("请求转发");
     
     //    2.请求重定向
     //    response.sendRedirect("testReturnString");
         response.sendRedirect(request.getContextPath() + "/index.jsp");
         System.out.println("请求重定向");
     
     //    3.直接响应内容
     //    response.setCharacterEncoding("utf-8");
     //    response.setContentType("text/html;charset=utf-8");
     //    response.getWriter().print("直接响应内容");
       }
     ```
   
   - ModelAndView
   
     jsp页面：
   
     ```
     <a href="test/testReturnModelAndView">testReturnModelAndView</a>
     ```
   
     控制器Java代码
   
     ```
     @RequestMapping("/testReturnModelAndView")
       public ModelAndView testReturnModelAndView(){
         ModelAndView modelAndView = new ModelAndView();
         modelAndView.addObject("username","张三");
         modelAndView.setViewName("success");
         return modelAndView;
       }
     ```
   
2. 转发和重定向

   - forward转发

     controller 方法在提供了 String 类型的返回值之后，默认就是请求转发。

     控制器java代码

     ```
     @RequestMapping("/testForward")
       public String testForward(){
         System.out.println("forward转发");
         return "forward:/WEB-INF/pages/success.jsp";
       }
     ```

   - redirct重定向

     contrller 方法提供了一个 String 类型返回值之后，它需要在返回值里使用:redirect

     ```
     @RequestMapping("/testRedirect")
       public String testRedirect(HttpServletRequest request){
         System.out.println("redirect重定向");
     //    return "redirect:testReturnString";
     //    return "redirect:" + request.getContextPath() + "/index.jsp";
         return "redirect:/index.jsp";
       }
     ```

     它相当于“response.sendRedirect(url)”。<font color=red>需要注意的是，如果是重定向到 jsp 页面， 则 jsp 页面不能写在 WEB-INF 目录中，否则无法找到</font>。

3. ResponseBody响应json数据

   - 作用：该注解用于将 Controller 的方法返回的对象，通过 HttpMessageConverter 接口转换为指定格式的 数据如:json,xml 等，通过 Response 响应给客户端

   - 需求：使用@ResponseBody 注解实现将 controller 方法返回对象转换为 json 响应给客户端。

   - 使用步骤：

     1. 导入jackson jar包

        ```
        <dependency>
              <groupId>com.fasterxml.jackson.core</groupId>
              <artifactId>jackson-annotations</artifactId>
              <version>2.9.0</version>
            </dependency>
            <dependency>
              <groupId>com.fasterxml.jackson.core</groupId>
              <artifactId>jackson-databind</artifactId>
              <version>2.9.5</version>
            </dependency>
            <dependency>
              <groupId>com.fasterxml.jackson.core</groupId>
              <artifactId>jackson-core</artifactId>
              <version>2.9.5</version>
            </dependency>
        ```

     2. jsp页面代码：

        ```
        <title>Title</title>
            <script type="text/javascript" src="${pageContext.request.contextPath}/js/jquery-1.8.2.min.js"></script>
            <script type="text/javascript">
              $(function () {
                $("#testJson").click(function () {
                  $.ajax({
                    type: "post",
                    url: "${pageContext.request.contextPath}/json/testResponseJson",
                    contentType: "application/json;charset=utf-8",
                    data: '{"id":1,"name":"test","money":999.0}',
                    dataType: "json",
                    success: function (data) {
                      alert(data);
                    }
                  });
                });
              }) </script>
              
              <!-- 测试异步请求 -->
        <input type="button" value="测试ajax请求json和响应json" id="testJson"/>
        ```

     3. 控制器Java代码

        ```
        @Controller
        @RequestMapping("/json")
        public class JsonController {
        
          @RequestMapping("/testResponseJson")
          //@ResponseBody一定要加上，否则retrun的是字符串，不是json对象或者控制器注解写成
          //@RestController
          @ResponseBody
          public Account testResponseJson(@RequestBody Account account){
            System.out.println("异步请求" + account);
            return account;
          }
        }
        ```

### springmvc实现文件上传：

1. 导入jar包

   commons-io 不属于文件上传组件的开发 jar 文件，但 Commons-fileupload 组件从 1.1 版本开始，它工作时需要 commons-io 包 的支持。

   ```
   <dependency>
         <groupId>commons-io</groupId>
         <artifactId>commons-io</artifactId>
         <version>2.6</version>
       </dependency>
       <dependency>
         <groupId>commons-fileupload</groupId>
         <artifactId>commons-fileupload</artifactId>
         <version>1.4</version>
       </dependency>
     </dependencies>
   ```

2. Springmvc.xml文件中配置文件上传解析器

   ```
   <!-- 配置文件上传解析器 -->
     <!-- id 的值是固定的-->
     <bean id="multipartResolver"
       class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
       <!-- 设置上传文件的最大尺寸为 5MB -->
       <property name="maxUploadSize">
         <value>5242880</value>
       </property>
     </bean>
   ```

3. jsp页面：

   <font color=red>注：form 表单的 enctype 取值必须是:multipart/form-data (默认值 是:application/x-www-form-urlencoded) enctype:是表单请求正文的类型；mothod属性必须是post提供一个文件选择域<input type="file" /></font>

   ```
   <form action="test/fileUpload" enctype="multipart/form-data" method="post">
       <input type="file" name="uploadFile"><br/>
       <input type="submit" value="上传">
   </form>
   ```

4. 控制器Java代码

   ```
   package club.banyuan.controller;
   
   import java.io.File;
   import java.text.SimpleDateFormat;
   import java.util.Date;
   import java.util.UUID;
   import javax.servlet.ServletContext;
   import javax.servlet.http.HttpServletRequest;
   import org.springframework.stereotype.Controller;
   import org.springframework.util.StringUtils;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.multipart.MultipartFile;
   
   /**
    * @author :yu
    * @description : TODO
    * @date :2020/8/7 15:55
    */
   @Controller
   @RequestMapping("/test")
   public class UploadController {
   
     @RequestMapping("/fileUpload")
     public String testResponseJson(String picname, MultipartFile uploadFile,
         HttpServletRequest request) throws Exception {
       //定义文件名
       String fileName = "";
   
       //1.获取原始文件名
       String uploadFileName = uploadFile.getOriginalFilename();
   
       //2.截取文件扩展名
       String extendName = uploadFileName
           .substring(uploadFileName.lastIndexOf(".") + 1, uploadFileName.length());
   
       //3.把文件加上随机数，防止文件重复
       String uuid = UUID.randomUUID().toString().replace("-", "").toUpperCase();
   
       //4.判断是否输入了文件名
       if (!StringUtils.isEmpty(picname)) {
         fileName = uuid + "_" + picname + "." + extendName;
       } else {
         fileName = uuid + "_" + uploadFileName;
       }
       System.out.println(fileName);
   
       //2.获取文件路径
       ServletContext context = request.getServletContext();
       String basePath = context.getRealPath("/uploads");
   
       //3.解决同一文件夹中文件过多问题
       String datePath = new SimpleDateFormat("yyyy-MM-dd").format(new Date());
   
       //4.判断路径是否存在
       File file = new File(basePath + "/" + datePath);
       if (!file.exists()) {
         file.mkdirs();
       }
   
       //5.使用 MulitpartFile 接口中方法，把上传的文件写到指定位置
       uploadFile.transferTo(new File(file, fileName));
       return "success";
   
     }
   }
   
   ```

### springmvc中的异常处理

1. 异常处理的思路：

   系统中异常包括两类:预期异常和运行时异常 RuntimeException，前者通过捕获异常从 而获取异常信息， 后者主要通过规范代码开发、测试通过手段减少运行时异常的发生。 系 统的 dao、service、controller 出现都通过 throws Exception 向上抛出，最后由 springmvc 前端 控制器交由异常处理器进行异常处理

   ![image-20200807172436010](/Users/edz/Library/Application Support/typora-user-images/image-20200807172436010.png)

2. 实现步骤：

   1. 编写异常类

      ```
      package club.banyuan.exception;
      
      /**
       * @author :yu
       * @description : 异常类
       * @date :2020/8/7 17:26
       */
      public class CustomException extends Exception{
      //  存储提示信息
        private String message;
      
        public CustomException(String message) {
          this.message = message;
        }
      
        @Override
        public String getMessage() {
          return message;
        }
      
        public void setMessage(String message) {
          this.message = message;
        }
      }
      
      ```

   2. jsp错误页面

      ```
      <%--
        Created by IntelliJ IDEA.
        User: edz
        Date: 2020/8/7
        Time: 17:29
        To change this template use File | Settings | File Templates.
      --%>
      <%@ page isELIgnored="false" contentType="text/html;charset=UTF-8" language="java" %>
      <html>
      <head>
          <title>Title</title>
      </head>
      <body>
      ${message}
      </body>
      </html>
      
      ```

   3. 自定义异常处理器

      ```
      package club.banyuan.exception;
      
      import javax.servlet.http.HttpServletRequest;
      import javax.servlet.http.HttpServletResponse;
      import org.springframework.web.servlet.HandlerExceptionResolver;
      import org.springframework.web.servlet.ModelAndView;
      
      /**
       * @author :yu
       * @description : 异常处理器
       * @date :2020/8/8 10:16
       */
      public class CustomExceptionResolver implements HandlerExceptionResolver {
      
        @Override
        public ModelAndView resolveException(HttpServletRequest request,
            HttpServletResponse response, Object o, Exception e) {
          e.printStackTrace();
          CustomException customException = null;
      
          //如果抛出的是系统自定义异常则直接转换
          if (e instanceof CustomException) {
            customException = (CustomException) e;
          } else {
            //如果抛出的不是系统自定义异常则重新构造一个系统错误异常。
            customException = new CustomException("系统错误，请与系统管理员联系!");
          }
          ModelAndView modelAndView = new ModelAndView();
          modelAndView.addObject("message", customException.getMessage());
          modelAndView.setViewName("error");
          return modelAndView;
        }
      }
      
      ```

   4. Springmvc.xml文件中配置异常处理器

      ```
       <!-- 配置自定义异常处理器 -->
        <bean id="handlerExceptionResolver"
          class="club.banyuan.exception.CustomExceptionResolver"/>
      ```

   5. 异常实例

      ```
      @RequestMapping("/testRedirect")
        public String testRedirect(HttpServletRequest request) throws CustomException {
          System.out.println("redirect重定向");
      
          try{
            int a = 10 / 0;
          }catch (Exception e){
            e.printStackTrace();
            throw new CustomException("错误");
          }
      //    return "redirect:testReturnString";
      //    return "redirect:" + request.getContextPath() + "/index.jsp";
          return "redirect:/index.jsp";
        }
      ```

      ![image-20200808110312096](/Users/edz/Library/Application Support/typora-user-images/image-20200808110312096.png)

### springmvc中的拦截器

1. 拦截器和过滤器的区别：

   1. 过滤器是 servlet 规范中的一部分，任何 java web 工程都可以使用。

      拦截器是 SpringMVC 框架自己的，只有使用了 SpringMVC 框架的工程才能用。

   2. 过滤器在 url-pattern 中配置了/*之后，可以对所有要访问的资源拦截。

      拦截器它是只会拦截访问的控制器方法，如果访问的是 jsp，html,css,image 或者 js 是不会进行拦截的。

2. 自定义拦截器的步骤

   1. 编写控制器java代码

      ```
      @Controller
      @RequestMapping("/login")
      public class UserLoginController {
      
        @RequestMapping("/loginRequest")
        public String loginRequest() {
          System.out.println("用户请求成功页面");
          return "success";
        }
      
        //  登录不应该被拦截
        @RequestMapping("/loginSubmit")
        public String loginSubmit(HttpSession session, String username, String password) {
          session.setAttribute("username", username);
          session.setAttribute("password", password);
          System.out.println("控制器：" + username);
          System.out.println("控制器：" + password);
          return "redirect:/index.jsp";
        }
      
      }
      ```

   2. 编写拦截器代码

      ```
      package club.banyuan.interceptor;
      
      import javax.servlet.http.HttpServletRequest;
      import javax.servlet.http.HttpServletResponse;
      import javax.servlet.http.HttpSession;
      import org.springframework.web.servlet.HandlerInterceptor;
      import org.springframework.web.servlet.ModelAndView;
      
      /**
       * @author :yu
       * @description : TODO
       * @date :2020/8/8 15:37
       */
      public class UserLoginInterceptor implements HandlerInterceptor {
      
        @Override
        public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
            throws Exception {
          System.out.println("当前路径为：" + request.getRequestURI());
      //    如果用户已登录就放行
          HttpSession session = request.getSession();
          String username = (String) session.getAttribute("username");
          String password = (String) session.getAttribute("password");
          System.out.println("拦截器：" + username);
          System.out.println("拦截器：" + password);
          if (username != null && !username.equals("") && password != null && !password.equals("")) {
            return true;
          }
      //    跳转到登录页面
          request.getRequestDispatcher("/login.jsp").forward(request, response);
          return false;
        }
      
        @Override
        public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
            ModelAndView modelAndView) throws Exception {
      
        }
      }
      
      ```

   3. 配置拦截器

      ```
      <!--  配置拦截器-->
        <mvc:interceptors>
          <mvc:interceptor>
      <!--      要拦截的具体的方法-->
      <!--      /** 表示所有控制器的方法全部拦截
                /login/* 表示只拦截在请求目录在login下的方法
                -->
            <mvc:mapping path="/login/**"/>
            <mvc:exclude-mapping path="/login/loginSubmit"/>
            <bean id="userLoginInterceptor" class="club.banyuan.interceptor.UserLoginInterceptor"/>
          </mvc:interceptor>
        </mvc:interceptors>
      ```

3. 多个拦截器的执行顺序

   ![image-20200809212220850](/Users/edz/Library/Application Support/typora-user-images/image-20200809212220850.png)



  
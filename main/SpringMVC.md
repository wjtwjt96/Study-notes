### SpringMVC

#### 1. 什么是SpringMVC？

SpringMVC是一款基于mvc设计模式的请求响应模型的轻量级web框架。（目前最流行的web框架之一，支持RESTful风格接口编程）。

#### 2. SpringMVC的工作流程

![img](C:\Users\99726\Desktop\study-notes\pic\spring\springMvc工作流程图)

#### 3. Springmvc的优点:

1. 可以支持各种视图技术，而不仅仅局限于JSP；
2. 与Spring框架集成（如IoC容器、AOP等）；
3. 清晰的角色分配：前端控制器(dispatcherServlet) , 请求到处理器映射（handlerMapping), 处理器适配器（HandlerAdapter), 视图解析器（ViewResolver）。
4. 支持各种请求资源的映射策略。
5. 支持RestFul风格

#### 4. SpringMVC与Struts2？

1. 相同点：
   - 两者底层都是基于ServletAPI实现的
   - 都是表现层框架，都是基于mvc设计模式的
   - 它们的处理请求机制都是都是一个核心控制器
2. 不同点：
   - SpringMVC是单例设计模式，基于方法来处理请求；Struts2是多例设计模式，基于类来处理请求；
   - SpringMVC的入口是Servlet，Struts2的入口是过滤器Filter；
   - Struts2的OGNL表达式开发效率更高
   - SpringMVC处理Ajax请求更简易

#### 5. SpringMVC的使用

- 第一步：导入jar包，搭建环境
- 第二步：配置SpringMVC的核心控制器
- 第三步：配置SpringMVC的主配置文件
  - 扫描controller
  - 配置视图解析器
  - 开启处理器映射器、处理器适配器
- 第四步：编写controller

#### 6. SpringMVC的参数绑定

1. **普通参数绑定**

   直接定义在Controller方法形参上，要求提交的参数名称与方法形参名称一致才能绑定。（名称不一致，需用到@RequestParam来映射配置）

2. **实体类参数绑定**

   首先存在具体的实体类，前端表单提交的参数名称与实体类保持一致才能正确绑定，然后在Controller方法形参上绑定实体类

3. **包装类参数绑定**

   存在具体的包装类，前端表单的参数名称因如此：vo.user.name。一一对应才能正确映射，然后在Controller方法形参上绑定包装类

4. **复杂类型参数绑定**（通过包装类型的集合属性接收）

   - List集合    list[0].属性名称
   - Map集合   map['属性名称']

5. **原生ServletAPI参数绑定**

   直接在在Controller方法形参上绑定对象

6. **自定义类型转换器配置**

   当参数无法正确的绑定时，可以使用自定义类型转换器。然后将数据转换后再做绑定。

   - 第一步：编写自定义类型转换器类，实现Converter接口。

   - 第二步：将自定义转换器配置到Spring容器中，如图：

     ![SpringMVC自定义类型转换器配置](F:\Typora\picture\SpringMVC自定义类型转换器配置.png)

#### 7. SpringMVC常用注解

- @RequestMapping   映射请求地址

- @RequestParam     映射前端字段到Controller方法形参上

- @RequestBody       映射请求体内容到Controller方法形参上

- @PathVariable        将请求URL上的参数占位符数据映射到，方法的形参上

- @RequestHeader   将请求头映射到Controller方法形参上

- @CookieValue        将Cookie的值映射到Controller方法形参上

- @ModelAttribute   

  - 设置在方法上，此方法会在请求的方法之前执行
  - 设置在参数上接收取值（map集合）参数

- @SessionAttributes

  - 作用在类上用于方法间数据共享；

  - 实际是往Session域中存值；

- 在方法的形参使用Model对象，可以往其中存值，可以到前端获取。实际Model对象代表的Request域对象。

#### 8. SpringMVC响应方式

- ###### 第一种：返回值类型为String
  
  - 返回值类型为String字符串，字符串代表返回的试图名称（会走视图解析器）；
  - 直接使用Model对象，进行数据的封装与返回。
  - **重定向与转发：**
    1. 转发：在返回的字符串中使用**forward**的关键字，实现转发。如：“forward：/index.jsp”
    2. 重定向：在返回的字符串中使用**redirect**的关键字，实现重定向。如：“redirect：/index.jsp"（走视图解析器）
  
- ###### 第二种：返回值类型为void
  
  - 返回值为void的类型，通过Model对象进行数据封装，使用原生的ServletAPI进行转发和重定向；
  - 使用request.getDispatcher().forwrad()实现转发
  - 使用request.sendRedirect()实现重定向
  
- ###### 第三种：返回值类型为ModelAndView对象
  
  - ModelAndView对象是SpringMVC提供的，用来封装请求返回的数据和视图View；
  - ModelAndView底层还是使用的Model对象封装数据到Request域中；
  - 通过addObject()封装数据
  - 通过setViewName()来设置返回的视图
  
- ###### 第四种：异步请求的返回
  
  - 第一步：导入Jackson的Jar包，用于将数据解析为Json
  - 第二步：通过@RequestBody接受异步请求发送的Json字符串，并封装为具体参数或实体类对象；
  - 第三步：访问持久层，获取数据
  - 第四步：通过@ResponseBody将数据转换为Json字符串，返回到前端。

#### 9. SpringMVC文件上传

- ###### 传统的Servlet技术上传

  1. Servlet3.0版本之后

     1. 通过request请求获取文件部分

        Part part = request.getPart("filename")；

     2. 获取文件入流

        InputStream is = part.getInputStream();

     3. 获取文件输出流，写出

        FileOutputStream fos = new FileOutputStream(new File(file + File.separator + filename));

  2. Servlet3.0版本之前

     - 1、文件上传要求

       - form表单method请求方式设置为post（post请求方式不支持）
       - 声明form表单属性：enctype="multipart/form-data"，设置其数据格式二进制
       - 使用Commons-fileupload组件实现文件上传，需要导入该组件相应的支撑jar包：
           Commons-fileupload和commons-io。commons-io不属于文件上传组件的开发jar文件，
           但Commons-fileupload 组件从1.1 版本开始，它工作时需要commons-io包的支持。

     - 2、步骤和方式

       1. 获取文件名，判断文件是否存在；

       2. 判断文件夹路径是否存在；否，新建；

       3. 创建文件操作工厂对象

          FileItemFactory factory = new DiskFileItemFactory();

       4.  创建文件上传核心工具类

          ServletFileUpload upload = new ServletFileUpload(factory);

       5.  设置单个文件允许的最大的大小:30M
          upload.setFileSizeMax(30*1024*1024);

       6. 判断： 当前表单是否为文件上传表单,返回true 表示是

       7.  把请求数据转换为一个个FileItem对象，再用集合封装
          List<FileItem> list = upload.parseRequest(request);

       8. 遍历list,获取单个文件对象

       9. 拼接文件名，生成唯一UUID标识

       10. 创建要上传的文件对象

           File file = new File(basePath,name);

       11. 上传,写入

           item.write(file);

       12. 删除组件运行时产生的临时文件

           item.delete(); 

- ###### SpringMVC文件上传

  - 分析：SpringMVC提供了文件解析器，当发出文件上传请求时，前端控制器会找到文件解析器，将其解析让后封装到MultipartFile对象中，作为Controller的形参。controller通过对象进行操作；

  - 操作步骤：
    1. 在springmvc配置文件中配置文件解析器
    
       ```java
       <!-- 配置文件解析器对象，要求id名称必须是multipartResolver -->   
       <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
       	<property name="maxUploadSize" value="10485760"/>    
       </bean>
       ```
    
       
    
    2. 编写文件上传Controller方法，设置形参为 **MultipartFile  upload**
    
       注意形参的参数名必须为upload与form表单的name值一致
    
    3. 调用upload的transferTo(new File())，完成上传

- ###### SpringMVC跨服务器上传

  - 操作步骤：

    1. 导入jersey  --jar包依赖
    2. 创建客户端对象
    3. 和图片服务器连接
    4. 上传

    ![SpringMVC跨服务器上传](F:\Typora\picture\SpringMVC跨服务器上传.png)

#### 10. SpringMVC异常处理机制

- 全局异常处理机制

  SpringMVC提供了默认的全局异常处理机制，程序发生异常会一层层网上抛出；直到抛到前端控制器时，它会调用配置的异常处理器来处理异常，从而跳转到友好显示页面。

  ![springmvc异常处理](..\pic\spring\SpringMvc全局异常处理机制)

- 全局异常处理机制使用步骤：
  
  方式一：
  
  1. 编写自定义异常类
  2. 编写异常处理器，实现HandlerExceptionResolver 
  3. 在主配置文件中配置异常处理器
  
  方式二：
  
  **@ControllerAdvice+ @ ExceptionHandler** 

#### 11. SpringMVC的拦截器机制

SpringMVC的拦截器类似于Servlet的过滤器Filter，用于对拦截处理器请求并进行前处理和后处理。

![img](F:\Typora\picture\拦截器与过滤器区别.png)

1. SpringMVC的拦截器执行流程

   正常情况：

   ```java
   //ABCD四个拦截器执行如下：
   A.pre -> B.pre -> C.pre -> D.pre-> handle -> D.post -> C.post -> B.post -> A.post-> D.after -> C.after -> B.after -> A.after
   ```

   异常情况：

   ```java
   //ABCD四个拦截器执行如下： C拦截器的preHandle返回为false。 后面的所有拦截器的前处理不执行，handle不执行，后处理不执行，执行after
   A.pre -> B.pre -> C.pre ->  B.after -> A.after
   ```

   

2. 拦截器的使用

   - 第一步：编写拦截器类，实现HandlerInterceptor接口，重写三个方法 

   - 配置拦截器

     ![1569336454280](F:\Typora\picture\SpringMVC拦截器配置.png)

#### 12. SpringMVC解决乱码

1. ##### 解决post请求乱码问题： 配置字符过滤器，由spring提供；

   ```java
   //在web.xml中配置一个CharacterEncodingFilter过滤器，设置成utf-8；
   <filter>
       <filter-name>CharacterEncodingFilter</filter-name>
       <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
       <init-param>
           <param-name>encoding</param-name>
           <param-value>utf-8</param-value>
       </init-param>
   </filter>
   <filter-mapping>
       <filter-name>CharacterEncodingFilter</filter-name>
       <url-pattern>/*</url-pattern>
   </filter-mapping>
   ```

   

2. ##### 解决Get请求乱码：有两种方法，方式如下：

   1. 修改tomcat配置文件添加编码与工程编码一致，如下：

      ```java
      <ConnectorURIEncoding="utf-8" connectionTimeout="20000" port="8080" protocol="HTTP/1.1" redirectPort="8443"/>
      ```

   2. 通过servletApi 对参数进行重新编码：

      ```java
      String userName = new String(request.getParamter("userName").getBytes("ISO8859-1"),"utf-8")
      // ISO8859-1是tomcat默认编码，需要将tomcat编码后的内容按utf-8编码。
      ```

      

​    
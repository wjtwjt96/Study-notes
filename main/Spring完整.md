### Spring

#### 1. 什么是Spring?

Spring是一款分层的 full-stack 轻量级框架。核心为：控制反转IOC 和 面向切面编程AOP。

#### 2. Spring框架的优势

1. 方便解耦，简化开发
2. AOP编程支持
3. 声明式事务支持
4. 方便程序的测试
5. 方便集合各种优秀的框架
6. 降低Java EE API的使用难度
7. Spring源码是学习经典范例

#### 3. 程序的解耦和耦合思想

1. 耦合：耦合是指程序间的依赖关系

   包含：

   - 方法间依赖
   - 类之间的依赖

2. 解耦：降低程序间的依赖关系

3. 实际开发：

   - 我们应该编译期不依赖，运行时依赖

4. 解耦合思路

   - 通过反射创建对象，不通过new关键字
   - 通过加载配置文件来获取全限定类名，不写死；

5. 工厂模式解耦合

   **通过定义工厂类，通过读取配置文件，来获取全限定类名，进而获取到具体的实现类，实现解耦。（单例模式更优）**

#### 5. ★★Spring的IOC

1. ##### IOC的概念

   作用：降低程序间的依赖关系

   **IOC是Spring提供的解耦合的方式**，称为控制反转。就是将对象的创建的过程交给Spring容器完成。

2. ##### IOC的使用

   1. 导入jar包
   2. 创建Spring核心配置文件
   3. 将需要Spring生成对象的类，添加到配置文件中
   4. 使用配置对象加载配置文件，获取Spring核心对象，ApplicationContext对象。
   5. 通过ApplicationContext的getBean()对获取对应的对象

3. ##### 获取Application的三种创建方式

   1. **通过加载类路径下的配置文件（常用）**

      ApplicationContext ac = new **ClassPathXmlApplicationContext**("applicationContext.xml");

      ```java
   	//1.加载配置文件
   	ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
   	```
   	
   2. ###### 通过加载绝对路径下的配置文件

      ApplicationContext ac = new **FileSystemXmlApplicationContext**("配置文件的绝对路径");

   	```java
   	//1.加载配置文件
   	ApplicationContext ac = new FileSystemXmlApplicationContext("C:\\Users\\99726\\Desktop\\applicationContext.xml");
   	```
   	
   3. ###### 通过注解配置的方式

      ApplicationContext ac3 = new AnnotationConfigApplicationContext();

      加载全注解开发的配置类

4. ##### Spring的核心容器对象

   - BeanFactory接口（使用场景：多例设计模式下）

     通过BeanFactory获取Spring的核心容器时，采用的是**延迟加载策略**，当使用具体的对象时才会创建对象。

   - Application接口  （使用场景：单例设计模式下）

     通过Application获取Spring核心容器时，采用的是**立即加载策略**，配置文件一加载，就创建对象放入到Spring容器中。

5. ##### Spring的Bean的创建方式

   bean对象的三种创建方式

      1. ###### 默认使用空参构造获取类对象

            ```java
            <!-- 通过无参构造函数创建对象 -->
            <bean id="userDao" class="com.wjt.dao.UserDao"></bean>
            ```

   2. ###### 通过普通工厂获取类对象

      ```java
      <!-- 通过普通工厂创建对象，使用工厂类的某个方法创建对象 -->
      <bean id="beanFactory" class="com.wjt.factory.BeanFactory2"></bean>
      <bean id="userDao"  factory-bean="beanFactory" factory-method="getBean"></bean>
      ```

   3. ###### 通过静态工厂获取类对象

         ```java
         <!-- 通过静态工厂创建对象，使用工厂类的某个方法创建对象 -->
         <bean id="userDao" class="com.wjt.factory.BeanFactory3" factory-method="getBean"></bean>
         ```

         注意：

         - bean：是指组件类（不仅仅指实体类）
         - javaBean：是指基于java语言的组件类

6. ##### Spring的Bean的作用范围

   bean的作用范围通过**scope属性**来设置。

   ```java
   <bean id=""  class=""  scope=""/>
   ```

   bean的作用范围有五种：

   1. singleton：单例的（常用的）
   2. prototype：多例的
   3. request：适用于web应用的请求过程，每次请求创建一个bean
   4. session：适用于web应用的会话过程，一次会话创建一个bean
   5. global-session：全局session；当服务器是集群时，全局session表示集群内所有计算机共享session对象；当服务器不是集群时，global-session等价于session；

7. ##### Spring的Bean的生命周期

   ![](..\pic\Spring-Bean的生命周期.png)

   ###### 1.详细描述：

   1. 首先容器启动后，会对scope为singleton且非懒加载的bean进行实例化，
   2. 按照Bean定义信息配置信息，注入所有的属性，
   3. 如果Bean实现了BeanNameAware接口，会回调该接口的setBeanName()方法，传入该Bean的id，此时该Bean就获得了自己在配置文件中的id，
   4. 如果Bean实现了BeanFactoryAware接口,会回调该接口的setBeanFactory()方法，传入该Bean的BeanFactory，这样该Bean就获得了自己所在的BeanFactory，
   5. 如果Bean实现了ApplicationContextAware接口,会回调该接口的setApplicationContext()方法，传入该Bean的ApplicationContext，这样该Bean就获得了自己所在的ApplicationContext，
   6. 如果有Bean实现了BeanPostProcessor接口，则会回调该接口的postProcessBeforeInitialzation()方法，
   7. 如果Bean实现了InitializingBean接口，则会回调该接口的afterPropertiesSet()方法
   8. 如果Bean配置了init-method方法，则会执行init-method配置的方法，
   9. 如果有Bean实现了BeanPostProcessor接口，则会回调该接口的postProcessAfterInitialization()方法，
   10. 经过流程9之后，就可以正式使用该Bean了,对于scope为singleton的Bean,Spring的ioc容器中会缓存一份该bean的实例，而对于scope为prototype的Bean,每次被调用都会new一个新的对象，期生命周期就交给调用方管理了，不再是Spring容器进行管理了
   11. 容器关闭后，如果Bean实现了DisposableBean接口，则会回调该接口的destroy()方法，
   12. 如果Bean配置了destroy-method方法，则会执行destroy-method配置的方法，至此，整个Bean的生命周期结束

   ###### 2. 大体概述：

   1. singleton：单例模式的生命周期

      - 初始化：当spring核心容器创建时，bean对象初始化。

      - 存在：当spring核心容器存在时，bean对象一直存在

      - 销毁：当spring核心容器销毁时，bean对象销毁（必须手动调用核心容器的销毁时，才能展示出）

      **总结：** 单例模式的生命周期是伴随着Spring核心容器的创建、存在和销毁。

   2. prototype：多例模式的生命周期

      - 初始化：当bean对象被调用时，由Spring核心容器初始化

      - 存在：只要bean对象在被使用的情况下，一直存在。

      - 销毁：当bean对象不被使用（没有引用指向该对象时），有java的垃圾回收机制回收。

      **总结：**多例模式下的生命周期，不由Spring核心容器来把控，是根据对象的引用情况来被java垃圾回收机制销毁。

8. ##### Spring的自动装配

   在spring中，对象无需自己查找或创建与其关联的其他对象，由容器负责把需要相互协作的对象引用赋予各个对象，使用autowire来配置自动装载模式。

   在Spring框架xml配置中共有5种自动装配：

   - no：默认的方式是不进行自动装配的，通过手工设置ref属性来进行装配bean。

   - byName：通过bean的名称进行自动装配，如果一个bean的 property 与另一bean 的name 相同，就进行自动装配。 

   - byType：通过参数的数据类型进行自动装配。

   - constructor：利用构造函数进行装配，并且构造函数的参数通过byType进行装配。

   - autodetect：自动探测，如果有构造方法，通过 construct的方式自动装配，否则使用 byType的方式自动装配。

9. ##### Spring核心容器

   ###### BeanFactory和ApplicationContext有什么区别？

   ​    BeanFactory和ApplicationContext是Spring的两大核心接口，都可以当做Spring的容器。其中ApplicationContext是BeanFactory的子接口。

   1. **BeanFactory：**是Spring里面最底层的接口，包含了各种Bean的定义，读取bean配置文档，管理bean的加载、实例化，控制bean的生命周期，维护bean之间的依赖关系。

   2. **ApplicationContext接口，**作为BeanFactory的派生，除了提供BeanFactory所具有的功能外，还提供了更完整的框架功能：

      - 继承MessageSource，因此支持国际化。

      - 统一的资源文件访问方式。
      -  提供在监听器中注册bean的事件。
      - 同时加载多个配置文件。
      - 载入多个（有继承关系）上下文 ，使得每一个上下文都专注于一个特定的层次，比如应用的web层。

10. ##### Spring 框架中都用到了哪些设计模式？

    1. 工厂模式：BeanFactory就是简单工厂模式的体现，用来创建对象的实例；
    2. 单例模式：Bean默认为单例模式。
    3. 代理模式：Spring的AOP功能用到了JDK的动态代理和CGLIB字节码生成技术；
    4. 模板方法：用来解决代码重复的问题。比如. RestTemplate, JmsTemplate, JpaTemplate。
    5. 观察者模式：定义对象键一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都会得到通知被制动更新，如Spring中listener的实现--ApplicationListener。

#### 6. ★★Spring的依赖注入（DI）

1. ##### 什么是依赖注入？

   我们通过IOC来降低程序间的依赖关系，以后程序间的依赖交由Spring容器来管理。我们只需要在配置文件中写出程序间的依赖关系，spring就可以帮我们自动维护。这就成为Spring的依赖注入。

2. ##### 依赖注入的三种数据类型：

   1. 基本数据类型和String

          <property name="password" value="123456"></property>

   2. 其他bean类型

          <property name="user" ref="user"></property>

   3. 复杂的数据类型/集合数据类型：分为两类

      1. List类集合

         ```java
         //数组类型
         <property name="myArr">
             <array>
             	<value>AAA</value>
             </array>
         </property>
         
         //list类型
         <property name="myList">
             <list>
                 <value></value>
                 <ref bean=""/>
             </list>
         </property>
         
         //set类型
         <property name="myList">
             <set>
                 <value></value>
                 <ref bean=""/>
             </set>
         </property>
         <property name="myMap">
         	<entry>
         	</entry>
         </property>
         <property name="myProps">
             <props>
            	 <prop key="">123</prop>
             </props>
         </property>   	
         ```
      
      2. Map类集合
      
         ```java
         <property name="myMap">
         	<entry>
           	</entry>
           </property>
           <property name="myProps">
               <props>
              	 <prop key="">123</prop>
               </props>
           </property> 
         ```
         
         
      

3. ##### 依赖注入的三种方式：

   1. ###### 通过构造方法进行注入

          <!-- 通过有参构造函数创建对象
              type:通过参数的数据类型，来获取制定的参数
              index:通过参数的索引来获取参数
              name:根据参数的名称来获取参数
              value:注入的数据
              ref:选用spring容器中出现的bean
          -->
          <bean id="userDao" class="com.wjt.dao.UserDao">
              <constructor-arg name="user" value="吴江涛"></constructor-arg>
              <constructor-arg name="age" value="18"></constructor-arg>
          </bean> 

   2. ###### 通过set方法注入

          <!-- 通过set方法创建对象
              name:根据参数的名称来获取参数
              value:注入的数据
              ref:选用spring容器中出现的bean
          -->
          <bean id="userDao" class="com.wjt.dao.UserDao">
              <property name="password" value="123456"></property>
              <property name="user" ref="user"></property>
          </bean> 

   3. ###### 通过注解注入
   
      - @Autowird  默认按照byType注入，byType失败后按照ByName注入。
      - @Resource 默认按照ByName注入

#### 7. 基于注解开发

**注意：优先导入aop的包，以及context的约束文件，然后再配置自动扫描注解** 

1. ##### 常用注解（四类）

   1. ###### 用于创建Bean对象的注解

      - @Component

        作用：用于创建Bean对象，并加入Spring核心容器中

        属性：value：指定bean对象的id

   2. ###### 通过@Component衍生出与三层架构有关的创建Bean对象的注解

      - @Controller

        作用：定义在控制层，标识控制层类，加入Spring容器

        属性：value

      - @Service

        作用：定义在业务层，标识控制层类，加入Spring容器

        属性：value

      - @Respository

        作用：定义在持久层，标识控制层类，加入Spring容器

        属性：value

   3. ###### 用于注入数据的注解

      **注意：**使用注解注入是就不需要使用set方法了

      1. 注入**bean对象**的注解

         1. @Autowired

            - **作用：**注入Bean对象到成员变量，或方法变量上。是基于**类型注入**，当同一类型具有多个bean时，再按照**名称注入**；
            
         2. @Qualifier
         
            - **作用：**在Autowired的基础上，按照**名称注入**。不能单独使用
            - **属性：** value：bean的id
            
         3. @Resource
               - **作用：**按照名称注入；
               - **属性：**name：bean的id
         
      2. 注入**基本类型和String**数据
      
         **备注：**集合类型数据，只能通过xml方式注入
      
         1. @Value
      
         - **作用：**注入数据；
         - **属性：**name：bean的id；可以使用spEl表达式
      
   4. ###### 用于限定Bean范围的
   
      1. @Scope
         - **作用：**限定bean的返回
         - **属性：**value：作用范围
   
   5. ###### 用于Bean的生命周期：
   
      - @PreDestory
   - @PostConstruct
   
2. ##### 基于全注解开发

   **基于全注解开发是将xml文件的的内容转换为配置类，然后通过注解的方式实现。**

   涉及到注解：

   1. @Configuration

      **作用：** 配置在类上，标记该类为配置类

      **属性：** value

   2. @ComponentScan

      **作用：**扫描制定包下的所有注解

      **属性：**

      - value：扫描包的路径
      - basePackages：扫描包的路径

   3. @Bean

      **作用：** 将方法的返回值对象加入到Spring容器中

      **属性：**name：bean的id；默认不写时，默认值是当前方法的名称

   4. @Import

      作用：导入其他配置类

      属性：value：用于指定其他配置类的字节码文件

   5. @PeopertySource

      **作用：**扫描properties配置文件

      **属性：** value：classpath:文件名称

3. ##### Spring整合Junit测试

   解释：如果使用普通的Junit测试，我们测试Spring框架是否正确时，需要手动创建Spring的核心容器。为了能直接在测试时就能由Junit创建Spring核心容器，因此需要使用Spring整合Junit测试。

   ###### Spring整合Junit测试的步骤

   1. 导入Spring和Junit的整合jar包

   2. 使用注解将普通的测试方法改为能自动创建容器的测试方法

      @Runwith

   3. 告知junit是基于xml开发的，还是注解开发的。

   4. 通过注解配置指定xml的地址或者是配置类的字节码文件

4. ##### 动态代理

      1. 特点：字节码随用随创建，随用随加载

      2. 作用：不修改源码的基础上对方法进行增强

      3. 分类：

         - 基于接口的动态代理
         - 基于子类的动态代理

      4. 基于接口的动态代理：

         1. 涉及的类：Proxy

         2. 提供者：JDK官方

         3. 如何创建代理对象：

            使用Proxy.newProxyInstance()方法

         4. 创建代理对象的要求：

            被代理类必须实现最少一个接口，如果没有则不能使用。

         5. newProxyInstance()方法的参数解析：

            - ClassLoader：类加载器（固定）

              用于加载代理对象字节码的，和被代理对象用同一个类加载器

            - Class[]：字节码数组（固定）

              用于存被代理对象实现的接口的字节码文件，代理对象和被代理对象实现相同接口

            - InvocationHandler：用于增强的代码

              用于写如何实现代理。谁用谁写。一般使用匿名内部类的形式实现

            ```java
            public AccountService getAccountService() {
                AccountService proxyAccountService = (AccountService) Proxy.newProxyInstance(accountService.getClass().getClassLoader(),
                            accountService.getClass().getInterfaces(), new InvocationHandler() {
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                            Object rValue = null;
                            try {
                                    //开启事务
                                    tx.beginTransation();
                                    rValue = method.invoke(accountService, args);
                                    tx.commit();
                                    return rValue;
                            } catch (RuntimeException e) {
                                tx.rollback();
                                e.printStackTrace();
                            } finally {
                                tx.realease();
                            }
                        	return rValue;
                        }
                    });
                return proxyAccountService;
            }
            ```

      5. 基于子类的动态代理：

         1. 涉及的类：Enhancer

         2. 提供者：第三方cglib库

         3. 如何创建代理对象：

            使用Enhancer类中的create()方法

         4. 创建代理对象的要求：

            被代理类不能数最终类

         5. create()方法的参数解析：

            - ClassLoader：类加载器（固定）

              用于加载代理对象字节码的，和被代理对象用同一个类加载器

            - Callback：用于增强的代码(MethodInterceptor)

              用于写如何实现代理。谁用谁写。一般使用匿名内部类的形式实现

####  8. ★★★AOP（面向切面变编程）

1. **概述：**通过预编译方式和运行期动态代理的方式来实现程序功能的统一维护。简单来说就是将重复的代码，在不改变原有的代码的基础上，提取出来使用动态代理技术实现对已有的方法增强。

2. **作用：**在运行期间，对已有的方法进行动态增强。

   **优点：**减少代码重复性；提高开发效率；维护方便；

3. **AOP实现方式：**动态代理技术

4. ###### **AOP的相关术语：**

   - 连接点（JoinPoint）：业务逻辑中可以被增强的方法
   - 切入点（Pointcut）：业务逻辑中需要被增强的方法
   - 通知（Advice）：就是具体的增强方法，分为前置、后置、异常、最终、环绕通知
   - 目标对象（Target）：被代理的对象
   - 织入（Weaving）：就是将通知加到切入点的过程
   - 代理对象（Proxy）：代理对象
   - 切面（Aspect）：就是切入点和通知的结合（实际上就是被代理对象经过增强后生成的代理对象）

5. ###### **AOP的xml实现**步骤

   1. 导包、以及解析切入点表达式的AspectW依赖
   2. 加入aop的约束
   3. 创建通知类
   4. 创建目标类、明确切入点
   5. 配置切面

6. ###### **AOP的通知详解**

   - 前置通知：在切入点方法调用执行之前调用

   - 后置通知：在切入点方法正常调用执行之后调用

   - 异常通知：在切入点方法调用执行抛出异常之后调用

   - 最终通知：在切入点方法调用执行之后调用（无论是否抛出异常）

   - 环绕通知：环绕通知是通过编码的形式，手动控制通知的位置。使用环绕通知需要手都调用切入点方法。

     **注意：**

     1. Spring框架为我们提供了一个接口：ProceedingJoinPoint.，该接口有一个方法proceed()，此方法就相当于明确调用切入点方法们使用

     2. spring中的环绕通知它是 **spring框架为我们提供的一种可以在代码中手动控制增强方法何时执行的方式**

        ```JAVA
        @Around("pt1()")
         public Object around(ProceedingJoinPoint joinPoint){
            Object obj = null;
         try {
                Object[] args = joinPoint.getArgs();
         this.beginTransation();
                obj= joinPoint.proceed(args);
         this.commit();
                return obj;
            } catch (Throwable throwable) {
         this.rollback();
                throwable.printStackTrace();
            } finally {
                this.realease();
            }
            return obj;
        }
        ```

7. **切入点表达式写法：**

   关键字：execution（表达式）

   1. 标准写法：

      权限修饰符   返回值类型   包名.包名.包名.类名.方法名（参数列表）

   2. 全通配写法：*  \*..\*.*(..)*

   3. *常用写法：* com.wjt.service.impl.AccountServiceImpl.transfer(..)

       ```java
      <aop:config>
          <!--配置切点-->
          <aop:pointcut id="pt1" expression="execution(* com.wjt.service.impl.AccountServiceImpl.transfer(..))"/>
      	<!--配置切面-->
      	<aop:aspect id="logAdvice" ref="tx">
              <aop:before method="beginTransation" pointcut-ref="pt1"/>
              <aop:after-returning method="commit" pointcut-ref="pt1"/>
              <aop:after-throwing method="rollback" pointcut-ref="pt1"/>
              <aop:after-returning method="realease" pointcut-ref="pt1"/>
          </aop:aspect>
      </aop:config>
       ```

8. ###### AOP注解开发

   注意：要优先开启Aop注解的使用

   常用注解

   @Before前置

   @AfterRunning后置

   @AfterThrowing异常

   @After最终

   @Around环绕

   @Pointcut定义切入点

   @Aspect定义切面类

#### 9. Spring事务

   1. ##### JDBCTempleate

      1. spring-jdbc下有自带的数据源
      2. jdbctemplate的常用操作。

   2. ##### Spring事务控制

      1. ###### 事务管理器

         DataSourceTransactionManager

      2. ###### 事务的隔离级别： **isolation**

         - DEFAULT   采用数据库默认的隔离级别

           **（mysql 默认是：REPEATABLE_READ；Oracle默认是：READ_COMMITTED；）**

         - READ_UNCOMMITTED : 读未提交

         - READ_COMMITTED ：读已提交

         - REPEATABLE_READ：可重复读

         - SERIALIZABLE：串行化

      3. ###### 事务的传播行为 ： **propagation**

         - REQUIRED  ： 当前需要事务，没有事务的话新建一个事务；
         - SUPPORTS ：支持当前事务，有事务以事务运行，没有则以非事务方式运行
         - MANDATORY：支持当前事务，如果当前没有事务，就抛出异常。
         - REQUIRES_NEW：新建事务，如果当前存在事务，把当前事务挂起。
         - NOT_SUPPORTED：以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。
         - NEVER：以非事务方式运行，有事务则抛出异常！
         - NESTED：如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则进行与PROPAGATION_REQUIRED类似的操作。

      4. ###### 事务定义信息

      5. ###### 事务的状态

3. ##### 事务控制——xml配置模式
   1. 导入spring-tx和Spring-aop的依赖

   2. 导入tx和aop的约束

   3. 配置事务管理器

   4. 配置事务通知

   5. 配置切面和切点

       ```java
       <!--配置事务管理器-->
       <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
           <property name="dataSource" ref="dataSource"></property>
       </bean>
       
       <!--配置事务通知-->
       <tx:advice id="txAdvice" transaction-manager="transactionManager">
           <tx:attributes>
               <tx:method name="transfer" isolation="DEFAULT" propagation="REQUIRED" read-only="false"/>
           </tx:attributes>
       </tx:advice>
       
       <!--配置切面-->
       <aop:config>
           <!--配置切点-->
           <aop:pointcut id="pt1" expression="execution(* com.wjt.service.impl.*.*(..))"></aop:pointcut>
           <!--配置事务切面-->
           <aop:advisor advice-ref="txAdvice" pointcut-ref="pt1"></aop:advisor>
       </aop:config>
       
       <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
           <property name="dataSource" ref="dataSource"/>
       </bean>
       ```

   

4. ##### 注解式事务配置

   ###### 1. 配置事务管理器

   ```java
   <!--配置事务管理器-->
   <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
       <property name="dataSource" ref="dataSource"></property>
   </bean>
   ```
   ###### 2. 开始注解式事务

   ```java
   <tx:annotation-driven transaction-manager="transactionManager"/>
   ```

   ###### 3. 在需要使用注解事务的地方加上

   ```java
   @Transactional(propagation = Propagation.REQUIRED,isolation = Isolation.DEFAULT,readOnly = false)	
   ```

   
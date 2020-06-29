#### MyBatis

1. ##### 什么是框架？

   软件开发中的一套解决方案，不同框架解决不同的问题！

   **好处：**封装了很多繁琐的细节操作，使开发者可以使用极简的方式实现功能，提高开发效率。

2. ##### 三层架构（MVC）

   - 持久层：操作数据库数据，与数据库交互

   - 视图层：渲染、展示数据

   - 控制层：处理和控制业务需求

3. ##### 持久层技术解决方案

   1. JDBC技术（sun公司提供的数据库访问规范）：

   2. Spring的JdbcTemlate（工具类）:

      Spring对Jdbc进行的简单封装

   3. Apache的DButils（工具类）:

      对Jdbc进行的简单封装

4. ##### Mybatis的概述

   Mybatis是一个持久化层框架，基于Java编写。准确的说是一个半自动持久化框架，对Jdbc进行了封装，我们不需要关注驱动注册，创建连接等，只需要关注sql的编写。使用了ORM思想。

   **ORM思想：对象关系映射。**

   - 将数据库表和实体类及实体类的属性进行关联；

   - 通过操作实体类来操作数据库表；

5. ##### Mybatis的主配置文件

```java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--Mybatis主配置文件-->
<configuration>
    <!--加载配置文件-->
    <!--
        方式一：直接配置数据库连接信息
        <properties>
            <property name="driver" value="com.mysql.jdbc.Driver"/>
            <property name="url" value="jdbc:mysql://localhost:3306/wjt"/>
            <property name="username" value="root"/>
            <property name="password" value=""/>
        </properties>
    -->
    <!--方式二：通过引入外部配置文件，加载数据库配置信息-->
    <properties resource="jdbc.properties"/>
    
    <!--配置别名-->
    <typeAliases>
        <!--配置别名的好处：是为了简化我们写返回值类型和参数类型时的写法-->
        <!--
            方式一：
            配置每一个实体类的别名（所有实体类都得一一设置）
            type：需要设置别名的实体类全限定类名
            alias：别名
        -->
        <!--<typeAlias type="com.wjt.domain.User" alias="user"/>-->
        <!--
            方式二：
            通过配置包路径的形式，将该包下所有的类都设置相应的别名，别名为类名
        -->
        <package name="com.wjt.domain"/>
    </typeAliases>

    <!--配置数据库访问的环境信息-->
    <environments default="mysql">
        <environment id="mysql">
            <!--配置事务类型-->
            <transactionManager type="JDBC"/>
            <!--配置数据库连接（或称为配置数据源）-->
            <dataSource type="POOLED">
				<!--
				方式一：直接配写死在配置文件中
                    <property name="driver" value="com.mysql.jdbc.Driver"/>
                    <property name="url" value="jdbc:mysql://localhost:3306/wjt"/>
                    <property name="username" value="root"/>
                    <property name="password" value=""/>
                -->
				<!--
				方式二：将数据库连接信息通过properties标签引入，然后通过OGNL表达式获取
                    <property name="driver" value="${driver}"/>
                    <property name="url" value="${url}"/>
                    <property name="username" value="${username}"/>
                    <property name="password" value="${password}"/>
                -->
				<!--方式三：根据外部应用的配置文件获取数据库信息，然后通过OGNL表达式获取-->
                   <property name="driver" value="${jdbc.driver}"/>
                   <property name="url" value="${jdbc.url}"/>
                   <property name="username" value="${jdbc.username}"/>
                   <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    <!--配置每个dao所对应的映射文件-->
    <mappers>
        <!--XML方式，resource="Mapper的具体位置"-->
        <!--<mapper resource="com\wjt\dao\UserMapper.xml"/>-->

        <!--注解方式 class="全限定类名"-->
        <!--<mapper class="com.wjt.dao.UserDao"/>-->

        <!--扫描包，代表扫描指定包下所有的注解或加载指定包下的所有类的映射文件（常用）-->
        <package name="com.wjt.dao"/>
    </mappers>
</configuration>
```

6. ##### Mybatis连接池

   1. ###### 连接池：

      ​	我们在实际开发中都会使用连接池。
      因为它可以减少我们获取连接所消耗的时间。

   2. ###### mybatis中的连接池

      1. mybatis连接池提供了3种方式的配置：
      		配置的位置：
      			主配置文件SqlMapConfig.xml中的dataSource标签，type属性就是表示采用何种连接池方式。
      	
      2. type属性的取值：

          - POOLED	 

            采用传统的javax.sql.DataSource规范中的连接池，mybatis中有针对规范的实现

          - UNPOOLED 

            采用传统的获取连接的方式，虽然也实现Javax.sql.DataSource接口，但是并没有使用池的思想。

          - JNDI	 

            采用服务器提供的JNDI技术实现，来获取DataSource对象，不同的服务器所能拿到DataSource是不一样。(tomcat服务器提供数据源)

            注意：如果不是web或者maven的war工程，是不能使用的。
            		   我们课程中使用的是tomcat服务器，采用连接池就是dbcp连接

   3. ###### mybatis中的事务

       1. 什么是事务?

          事务就是指一系列的业务操作，要么全部执行，要么全部不执行；

       2. 事务的四大特性ACID

          **原子性  一致性   隔离性  持久性**

       3. 不考虑隔离性会产生的3个问题

          **脏读   不可重复读  幻读**

       4. 解决办法：四种隔离级别

          - 读未提交
          - 读已提交
          - 可重读
          - 串行化

      > **注意：****Mybatis** 是通过**SqlSession**对象的 commit方法 和 rollback方法 实现事务的提交和回滚；

7. ##### Mybatis动态sql

      1. ###### 使用**\<if test="判断条件">**标签来按判断是否凭借条件

         例如：

         ```java
   select * from user  where  1 = 1
         <if test="username != null">
   	and username = #{username}
         </if>
   ```
      
   
      
2. ###### 使用**\<where>**标签来让mybatis动态拼接是否加 and 
      
   例如：
      
   ```java
      select * from user
   <where>
          <if test="username != null">
           and username  =  #{username}
          </if>
       <if test="age != null">
              and age  =  #{age}
       </if>
      </where>
   ```
      
   
      
3. ###### 使用\<foreach collection="" open="" close="" item="" separator="" index="">标签来拼接同一字段多参数查询；如：in (p1,p2,……)
      
   ```java
         <foreach  collection="集合" open="开始的sql片段" close="结束的sql片段" item="遍历出的参数" separator="分割符" index="索引">
   </foreach>
         
         例如：
         <select id="findByConditions" resultType="user" parameterType="list">
         	select * from user
             <where>
             <foreach collection="list" open="and id in (" close=")" item="id" separator="," index="">
            		 #{id}
             </foreach>
             </where>
         </select>
         ```
      
      4. ###### 使用\<sql id="名称">来提取公共sql片段，然后使用\<include refid="名称"/>来引入到相应的位置
      
8. ##### 多表操作

      1. 一对多

            在多的一方的实体中，添加一的一方的对象应用。然后用resultMap映射结果集。

      2. 多对多

            可在任意一方的实体中设置另一方实体的集合，然后用resultMap映射结果集。\<collection>

9. ##### mybatis延迟加载

      1. 什么是延迟加载？

            在真正使用数据时才发起查询，不用时不查询。也称为按需加载，懒加载

      2. 什么是立即加载？

            不管用不用，只要一调方法，马上发起查询。

      3. 多表关系加载时机

            一对多、多对多，通常使用延迟加载

            多对一、一对一，通常使用立即加载 

      4. 设置全局延迟加载

            | 设置名                | 描述                                                         | 有效值        | 默认值                  |
            | --------------------- | ------------------------------------------------------------ | ------------- | ----------------------- |
            | cacheEnabled          | 全局地开启或关闭配置文件中的所有映射器已经配置的任何缓存。   | true \| false | true                    |
            | lazyLoadingEnabled    | 延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。特定关联关系中可通过设置 `fetchType` 。属性来覆盖该项的开关状态。 | true \| false | false                   |
            | aggressiveLazyLoading | 当开启时，任何方法的调用都会加载该对象的所有属性。否则，每个属性会按需加载（参考 `lazyLoadTriggerMethods`)。 | true \| false | false （在 3.4.1 及之前 |

      5. 设置延迟加载，需将两个表的关联查询改成，单表查询，当使用时再去动态查询另一个表的方法。

10. ##### Mybatis缓存

        1. ###### 什么是缓存？

              存在内存的临时数据。

        2. ###### 为什么使用缓存？

              减少和数据的交互次数，提高执行效率。	

        3. ###### 什么数据使用缓存？什么数据不能使用缓存？

              - 适用于缓存

                经常查询，且数据不经常改变的。

                数据的正确与否对最终的结果不影响的。

              - 不适用于缓存

                经常查询，且数据经常改变的。

                数据的正确与否对最终的结果影响很大的。

                如：商品库存、银行汇率等。

        4. ###### Mybatis的一级缓存和二级缓存

              1. **一级缓存（mybatis默认开启）：**

                    - 它指的是Mybatis中SqlSession对象的缓存。
                    - 当我们执行查询时，会将数据保存到一级缓存中（SqlSession对象提供的一块区域），下次查询时，优先去缓存中查看是否存在。存在，就直接将缓存中的数据返回；不存在，就访问数据库执行查询。
                    - 该区域结构是一个Map。
                    - 当SqlSession对象消失时，缓存消失。
                      1. sqlSession.close();
                      2. sqlSession.clearCache();
                    - 一级缓存，在Sqlsession对象调用添加、删除、更新、commit() 、close() 等方法是就会清空一级缓存。

              2. **二级缓存（手动开启）：**

                    - 它是指的是由Mybatis中的SqlSessionFactory对象的缓存，由一个SqlSessionFactory对象创建，被所有SqlSession对象共享。

                    - 二级缓存，缓存的是数据，不是对象。

                    - 二级缓存使用的步骤：

                      1. 在mybatis主配置文件中开启缓存

                         ```java
         <settings  name="cacheEnabled"  value="true"/>
                         ```

                      2. 在mybatis实体映射文件中配置缓存

                         ```java
         <cache/>
                         ```
                      
                      3. 让当前的操作支持二级缓存
                      
                         ```java
                         <select useCache="true"> 
                         ```
                         
                         

11. ##### mybatis注解开发

1. 常用注解：
   - @Select()
   - @Update()
   - @Insert()
   - @Delete()
   - @Results(id,@Result（id，column，property））  //配置字段映射
   - @cacheNamespace(blocking = true)      //开启二级缓存注解
     - 注意：先在主配置文件中开启二级缓存
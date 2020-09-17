###  Mybatis

---

#### 1. 初识

是一款优秀的持久层框架；可以定制化SQL、存储过程以及高级映射；几乎避免了所有JDBC代码和手动设置参数获取结果集；使用简单的xml和注解来配置和映射原生类型、接口和POJO为数据库中的记录

优点：1、传统的JDBC代码太复杂；2、简化，帮助程序员将数据存入数据库中 

- 简单易学
- 灵活
- sql与代码分离，提高了可维护性
- 提供映射标签，支持对象数据库的orm字段关系映射
- 提供对象关系映射标签，支持对象关系组件维护
- 提供xml标签，支持编写动态sql

#### 2. 程序流程

1. 编写工具类，解析mybatis-config.xml，得到SqlSessionFactory对象，通过工厂得到sqlSession实例（执行sql命令的方法）

   - SqlSessionFactoryBuilder，创建多个SqlSessionFactory实例对象，一旦创建SqlSessionFactory就不再使用，最佳作用域是方法作用域

   - SqlSessionFactory这个实例对象一旦被创建就在应用运行期间一直存在，最佳作用域为应用作用域，单例模式或者静态单例模式

   - SqlSession：实例不是线程安全的，每个线程都有自己的`SqlSession`实例，最佳作用域是方法作用域或者请求作用域，每次发起一个请求就创建一个实例对象，一定要关闭

2. 在resources目录下编写配置文件/mybatis-config.xml文件

3. 创建实体类（和数据库中的表对应）

4. 创建持久层接口

5. 接口实现类由转化为mapper.xml文件（注意：xml文件都写在resource目录下，如果放在java包下，记得要在maven中进行配置，导出资源）

```xml
<build>
    <resources>
        <resource>
            <directory>src/main/resources</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>

        </resource>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>

        </resource>
    </resources>
</build>

```



#### <font color = "green" >3. CRUD</font>

##### 3.1 namespace

配置映射文件的时候，mapper标签中的属性，namespace的包名要和接口的包名一致

-  id:就是对应命名空间的接口中的方法名
- resultType: Sql语句执行的返回值
- paramterType:参数类型

<font color = "red">注意：增删改要记得提交事务(使用相对应的标签)</font>

##### 3.2 错误分析

1. 标签匹配正确

2. resource绑定mapper，需要使用路径

3. 空指针异常，没有注册到资源

4. maven没有导出资源问题

##### 3.3使用 map

​	倘若实体类中存在的参数过于多，我们可以可以考虑使用map,ji仅仅关心键的对应，而不用和实体类中的属性相对应

##### 3.4模糊查询

 1. 代码执行的时候，传递通配符`%%`

    ```.sql
    List<User> list = mapper.getUserLike("%小%");
    ```

 2. 在sql拼接中使用通配符

    ```.mysql
    select * from mybatis.user where name like "%"#{name}"%"
    ```

    

#### 4. 配置解析

##### 4.1 核心配置文件

- mybatis-config.xml

- 标签

  ``` .xml
  configuration(配置)
  properties(属性)
  settings(设置)
  typeAliases(别名)
  typeHandlers(类型处理器)
  objectFactory(对象工厂)
  plugins(插件)
  enviroments(环境配置)
  enviroment(环境变量)
  transactiomanger(事务管理器)：两种事务管理器，JDBC|MANAGED
  dataSource(数据源):UNPOOLED|POOLED|JND
  databaseIdProvider(数据库厂商标识)
  mappers(映射器)
  ```

##### 4.2 环境变量（enviroments)

​	Mybatis可以适应多种环境，但是尽管可以配置多种环境，但是每个SqlSessionFactory实例只能选择一种环境

可以配置多套运行环境，默认的事务管理器是JDBC，默认的是连接池

##### 4.3 属性（properties）

可以通过properties属性来实现引用配置文件

这些属性都是可以外部配置且动态替换的，可以在典型的Java属性文件中配置，也可以通过properties属性的子元素进行传递【db.properties】

- 编写一个配置文件【db.properties】

  ```properties
  driver = com.mysql.jdbc.Driver
  url = jdbc:mysql://127.0.0.1:3306/mybatis?useSSL=false
  username = root
  password = 123456
  ```

  

- 直接在核心配置文件中映入

  ```xml
  <properties resource="db.properties">
          <property name="name" value="root"/>
          <property name="pwd" value="123456"/>
  </properties>
  ```

  

- 在核心配置文件中引入（<font color="red">在xml中所有的标签都可以规定其顺序</font>）

##### 4.4别名（typeAliases）

利用全限定包名过于冗余的时候，可以使用取别名的方式，简化代码，使其清楚。只和配置相关

方式一：利用typeAlias标签

```xml
<typeAliases>
  <typeAlias alias="Author" type="domain.blog.Author"/>
</typeAliases>
```

方式二：利用package标签

```xml
<typeAliases>
  <package name="domain.blog"/>
</typeAliases>
```

注意：在没有注解的情况下，会使用首字母小写的非限定类名来作为它的别名；如果存在注解就使用注解

在实体类比较少的情况下，使用第一种，实体类比较多的情况下，使用第二种

存在一些常见java内建类型别名，具体查看官网



##### 4.5映射器(mappers)

SQL的映射语句，告诉Mybatis去哪里寻找映射，存在两种方式

- 使用resource属性，使用类路径资源引用

  ```xml
  <mappers>
  	<mapper resource="nancy/dao/UserMapper.xml"/>
  </mappers>
  ```
  
- 使用class属性，使用映射器接口的实现类的全限定包名引用

  ```xml
  <mappers>
      <mapper class="nancy.dao.UserMapper"/>
  </mappers> 
  ```

- 使用name 属性，将包下的接口实现类(xml文件)，全部设置为映射器

  ```xml
  <mappers>
      <package name="nancy.dao"/>
  </mappers>
  ```

  注意:

  	1. 接口和它的mapper配置文件必须同名
  
   	2. 接口和它的mapper配置文件必须在同一个包下

作用域和生命周期

SqlSessionFactoryBuilder:一旦创建就不再使用，局部变量

SqlSessionFactory:可以联想到数据库连接池，一旦创建就在程序应用期间一直存在，最佳作用域是应用作用域，使用单例模式或者静态单例模式

SqlSession：连接数据库的一个请求，不是线程安全的，最佳作用域是请求或者方法作用域，用完之后立即关闭，防止占用资源



#### 5. 属性名(实体类)和字段名(数据库表)不一致问题

结果：查询出来的类中的属性名与字段名不一致的查询结果为null

解决方法：

- 在sql查询语句中给字段起别名as

-  利用resultMap结果集映射v

  ``` xml
      <resultMap id="UserType" type="User">
          <!--        column中的字段名数据库，-->
          <result column="id" property="id"/>
          <result column="name" property="name"/>
          <result column="pwa" property="password"/>
      </resultMap>
  
  
      <select id="getUserList" resultMap="UserType">
          select * from mybatis.user
      </select>
  ```

  resultMap设计思想，对于简单的语句根本不需要显示的去配置结果映射，而对于复杂一点的我们只需要简单的描述他们的关系

#### 5.日志

##### 5.1 日志工厂

![image-20200913103057055](C:\Users\nancy\AppData\Roaming\Typora\typora-user-images\image-20200913103057055.png)

1. SLF4J

2. LOG4J 

3. LOG4J2 

4. JDK_LOGGING 

5. COMMONS_LOGGING:标准日志输出(每一步都记录的清楚）

   在核心配置文件中配置我们的Setting

   ```xml
   <settings>
       <setting name="logImpl" value="STDOUT_LOGGING"/>
   </settings>
   ```

   ​				![image-20200913104104682](C:\Users\nancy\AppData\Roaming\Typora\typora-user-images\image-20200913104104682.png)

6. STDOUT_LOGGING 

7. NO_LOGGING

##### 5.2LOG4J

是Apache的一个开源项目，通过使用Log4j，我们可以控制日志信息的输送目的地是控制台、文件、GUI组件；也可以控制每一条日志的格式；通过每一条日志信息的级别，可以细致的控制日志的生成过程；通过配置文件很好的进行设置，不需要修改源代码

- 1、导入依赖包

  ```xml
  <dependency>
      <groupId>log4j</groupId>
      <artifactId>log4j</artifactId>
      <version>1.2.17</version>
  </dependency>
  ```

- 2、配置properties文件

  ```properties
  log4j.rootLogger=DEBUG,console,file
  
  #控制台输出的相关设置
  log4j.appender.console = org.apache.log4j.ConsoleAppender
  log4j.appender.console.Target = System.out
  log4j.appender.console.Threshold=DEBUG
  log4j.appender.console.layout = org.apache.log4j.PatternLayout
  log4j.appender.console.layout.ConversionPattern=[%c]-%m%n
  
  #文件输出的相关设置
  log4j.appender.file = org.apache.log4j.RollingFileAppender
  log4j.appender.file.File=./log/nancy.log
  log4j.appender.file.MaxFileSize=10mb
  log4j.appender.file.Threshold=DEBUG
  log4j.appender.file.layout=org.apache.log4j.PatternLayout
  log4j.appender.file.layout.ConversionPattern=[%p][%d{yy-MM-dd}][%c]%m%n
  
  #日志输出级别
  log4j.logger.org.mybatis=DEBUG
  log4j.logger.java.sql=DEBUG
  log4j.logger.java.sql.Statement=DEBUG
  log4j.logger.java.sql.ResultSet=DEBUG
  log4j.logger.java.sql.PreparedStatement=DEBUG
  ```

- 3、配置log4j为日志的实现

  ```xml
  <settings>
      <setting name="logImpl" value="LOG4J"/>
  </settings>
  ```

  简单实用

   1. 在需要使用log4j的类中，导包

   2. 日志对象的生成，参数当前类的class对象

      ``` java
      static Logger logger = Logger.getLogger(UserDaoTest.class);
      ```

   3. 日志级别

      ```java
      logger.info("info:进入了testlog4j");
      logger.debug("debug:进入了log4j");
      logger.error("errot:进入了testlog4j");
      ```

#### 6.分页

减少数据的处理量

核心是sql

##### 6.1 利用limit实现分页

1. 接口

   ```java
    //分页实现
   List<User> getUserLimit(Map<String,Integer> map);
   ```

2. Mapper.xml

   ```xml
   <!--使用的是resultMap,属性名和字段名不匹配的情况-->
   <select id="getUserLimit" parameterType="map" resultMap="UserType">
       select * from mybatis.user limit #{startindex},#{pageSize}
   </select>
   ```

3. 测试类

   ```java
   public void getUserLimit(){
       SqlSession sqlSession = MybatisUtils.getsqlSession();
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
   
       HashMap<String,Integer> map = new HashMap<String, Integer>();
       map.put("startindex",0);
       map.put("pageSize",2);
       List<User> list = mapper.getUserLimit(map);
       for(User user : list){
           System.out.println(user);
       }
       sqlSession.close();
   
   }
   ```

##### 6.2 RowBounds分页

##### 6.3分页插件 Mybatis PageHelper

#### 7.注解

本质就是反射机制实现，底层就是动态代理 

1. 注解在接口上实现

   ```java
   @Select("select * from user")
   List<User> getUsers();
   ```

2. 需要在核心配置文件进行配置mapper

   ```xml
   <mapper class="nancy.dao.UserMapper"/>
   ```

 细节：可以在工具类创建sqlSession的时候自动提交事务

@Param()注解：

- 基本类型和String类型需要加上
- 引用类型不需要加
- 倘若只存在一个参数，也可以忽略
- 在SQl中引用的就是其里面设置的属性名

#{}，${}区别：

前者用来传入参数，sql在解析的时候会加上“ ”，当成字符串来解析，在很大程度上可以防止sql注入

后者传入的数据直接显示在生成的sql中，无法防止sql注入，一般用来传入数据库对象

**注意**：Mybaties排序时使用order by动态参数时，需要使用${}



#### 8.多对一处理（test2)

- 嵌套查询

  ```xml
  <select id="getStudents" resultMap="StudentTeacher">
      select * from student
  </select>
  
  <resultMap id="StudentTeacher" type="student">
      <result property="id" column="id"/>
      <result property="name" column="name"/>
      <!--复杂的属性需要特殊处理
          对象使用association
          集合使用collection
      -->
      <association property="teacher" column="tid" javaType="teacher" select="getTeacher"/>
  </resultMap>
  
  <select id="getTeacher" resultType="teacher">
      select * from teacher where id = #{id}
  </select>
  ```

- 按照结果嵌套处理

  ```xml
  <select id="getStudents" resultMap="StudentTeacher2">
      select s.id sid,s.name sname,t.name tname,t.id tid from student s,teacher t where t.id = s.tid
  </select>
  <resultMap id="StudentTeacher2" type="student">
      <result property="id" column="sid"/>
      <result property="name" column="sname"/>
      <association property="teacher" javaType="teacher">
          <result property="name" column="tname"/>
          <result property="id" column="tid"/>
      </association>
  </resultMap>
  ```

#### 9.一对多处理(test3)

```xml
<!--嵌套查询-->
<select id="getTeacher" resultMap="teacherMap">
    select s.id sid,s.name sname,t.id tid,t.name tname
    from teacher t,student s
    where t.id = s.tid and t.id = #{tid}
</select>

<resultMap id="teacherMap" type="teacher">
    <result property="id" column="tid"/>
    <result property="name" column="tname"/>
    <!--javaType制定属性的类型，集合中的泛型我们使用ofType获取-->
    <collection property="students" ofType="student">
        <result property="id" column="sid"/>
        <result property="name" column="sname"/>
        <result property="tid" column="tid"/>
    </collection>
</resultMap>

  <!--子查询-->
    <select id="getTeacher2" resultMap="getTeacher2">
        select * from mybatis.teacher where id = #{tid}
    </select>
    <resultMap id="getTeacher2" type="teacher">
        <collection property="students" javaType="ArrayList" ofType = "student" select="teacherById" column="id"/>
    </resultMap>
    <select id="teacherById" resultType="student">
        select * from mybatis.student where tid = #{tid}
    </select>
```



总结：关联就是使用对象使用association，多对一使用collection

javaType & ofType 

- 前者用来指定实体类中属性的类型
- 后者用来指定映射到list或者集合中的pojo类型，也就是泛型中的约束条件



慢SQL

MySQL引擎，InnoDB底层原理、索引、索引优化



#### 10.动态SQL（test4 ）

其实就是根据不同的条件生成不同的SQL语句

​	

##### 10.1 if

```xml
<select id="queryBlogIF" parameterType="map" resultType="blog">
    select * from mybatis.blog where 1=1
    <if test="title != null">
        and title = #{title}
    </if>
    <if test="author != null">
        and author = #{anthor}
    </if>
</select>
```

##### 10.2 choose（when,otherwise)

相当于Java中的switch

```xml
<select id="queryBlogChoose" parameterType="map" resultType="blog">
    select * from mybatis.blog
    <where>
        <choose>
            <when test="title != null">
                title = #{title}
            </when>
            <when test="author != null">
                and author = #{author}
            </when>
            <otherwise>
                and views = #{views}
            </otherwise>
        </choose>

    </where>
</select>
```

##### 10.3 trim(where,set)

where元素只会在至少有一个子元素的条件下才会插入where语句，如果语句开头是and或者or，where元素也会将他们去除

```xml
<select id="queryBlogIF" parameterType="map" resultType="blog">
    select * from mybatis.blog 
    <where>
         <if test="title != null">
            and title = #{title}
        </if>
        <if test="author != null">
            and author = #{anthor}
        </if>
    </where>
</select>
```

set元素会动态前置set关键字，同时也会删掉没有的逗号

```xml
<update id="updateBlog" parameterType="blog">
        update mybatis.blog
        <set>
            <if test="title != null">
                title = #{title},
            </if>
            <if test="author != null">
                author = #{author}
            </if>
        </set>
        where id = #{id}
</update>
```

动态sql本质还是sql，不过是在sql层面进行了一些逻辑处理if when、set、choose、where

##### 10.4 SQL片段

当多个配置文件中存在多个相同的代码的时候，我们可以使用sql标签，极大增强代码的利用率

```xml
<!--sql片段-->
<sql id="if-title-author">
    <if test="title != null">
        and title = #{title}
    </if>
    <if test="author != null">
        and author = #{anthor}
    </if>
</sql>

<!--利用include标签将其引入进来-->
<select id="queryBlogIF" parameterType="map" resultType="blog">
    select * from mybatis.blog where 1=1
    <include refid="if-title-author" ></include>
</select>
```



##### 10.4 Foreach

是对集合进行遍历，通常是在in语句中使用

![image-20200915145934861](C:\Users\nancy\AppData\Roaming\Typora\typora-user-images\image-20200915145934861.png)

```xml
<!-- select * from mybatis.blog where 1=1 and (id = 1 or id=2 or id = 3)-->
<select id="queryBlogForEach" parameterType="map" resultType="blog">
    select * from mybatis.blog
    <where>
        <foreach collection="ids" item="id" open=" (" close=")" separator="or">
            id = #{id}
        </foreach>
    </where>

</select>
```

#### 11.缓存（test5）

##### 11.1 概述

``` md
查询需要连接数据库消耗资源
一次查询的结果，给他暂存在一个可以直接取到的地方，当再次查询的时候就直接取数据就不用再次查询了
```

1. 缓存
   - 存在内存中的临时数据
   - 将用户经常使用到的数据放入缓存中当用户查询数据的时候就不用去磁盘上查询，直接在缓存中查询，从而提升查询效率

2. 使用缓存
   - 减少和数据库交互的次数，减少系统的开销，提高系统效率
   - 经常查询并且不经常改变的数据

##### 11.2mybatis缓存

其可以定制和配置缓存，缓存可以极大的提高效率

默认两种缓存：一级缓存和二级缓存

1. 默认情况下，一级缓存是开启的（本地缓存，sqlsession级别的缓存）
2. 二级缓存需要手动配置和开启，基于nameapace级别的缓存

###### 11.2.1一级缓存

1. 开启日志

2. 在同一个sqlsession查询两次相同的数据

3. 查看结果

   ![image-20200915163208555](C:\Users\nancy\AppData\Roaming\Typora\typora-user-images\image-20200915163208555.png)

缓存失效的情况：

- 增删改操作会胡改变原来的数据，所以一定会刷新缓存
- 查询不同的数据
- 查询不同Mapper.xml
- 手动清除

*一级缓存默认是开启的，只在一次sqlsession中有效*

###### 11.2.2 二级缓存

二级缓存也叫做全局缓存，一级缓存作用域太低了；他是基于namespace命名空间，一个名称空间对应一个缓存

工作机制：

1. 一个会话查询一条语句，这个数据就会在当前会话的一句缓存中
2. 如果当前会话关闭了，这个对话锁对应的一级缓存也就没有了，一级缓存中的数据被保存在二级缓存中
3. 新的会话查询，就可以从二级缓存中获取内容
4. 不同的mapper查出的数据会放在自己对应的缓存中

步骤：

- 开启全局缓存

  ```xml 
  <setting name="cacheEnabled" value="true"/>
  ```

- 在需要使用的地方开启缓存

  ```xml
  <cached/>
  ```

  当然也可以自定义一些参数，这个更高级的配置创建了一个 FIFO 缓存，每隔 60 秒刷新，最多可以存储结果对象或列表的 512 个引用，而且返回的对象被认为是只读的，因此对它们进行修改可能会在不同线程中的调用者产生冲突

  ```xml
  eviction="FIFO"
  flushInterval="60000"
  size="512"
  readOnly="true"
  ```

- 测试：我们需要将实体类实现序列化

总结：

1. 只要开启了二级缓存，在同一个mapper下的就有效果

2. 所有的数据都会先放在一级缓存中，
3. 只有当会话提交的时候才会放入二级缓存中

##### 11.3 缓存原理

![image-20200915185754488](Mybatis.assets/image-20200915185754488.png)

##### ehcache缓存

是一个纯Java的进程内缓存的框架

导包
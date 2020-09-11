### Mybatis

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

#### 4.2 环境变量（enviroments)

​	Mybatis可以适应多种环境，但是尽管可以配置多种环境，但是每个SqlSessionFactory实例只能选择一种环境

可以配置多套运行环境，默认的事务管理器是JDBC，默认的是连接池

#### 4.3 属性（properties）

可以通过properties属性来实现引用配置文件

这些属性都是可以外部配置且动态替换的，可以在典型的Java属性文件中配置，也可以通过properties属性的子元素进行传递【db.properties】

- 编写一个配置文件【db.properties】

  ```db.properties
  driver = com.mysql.jdbc.Driver
  url = jdbc:mysql://127.0.0.1:3306/mybatis?useSSL=false
  username = root
  password = 123456
  ```

  

- 直接在核心配置文件中映入

  ```.xml
  <properties resource="db.properties">
          <property name="name" value="root"/>
          <property name="pwd" value="123456"/>
  </properties>
  ```

  

- 在核心配置文件中引入（<font color="red">在xml中所有的标签都可以规定其顺序</font>）














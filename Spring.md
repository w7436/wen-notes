


## Spring

Spring理念：使得现有的技术更加的实用，整合现有的框架技术

优点：

- Spring是一个免费的开源的框架，容器
- 轻量级框架，非入侵式的
- 控制反转IOC，面向切面AOP

总而言之，就是一个轻量级的控制反转IOC和面向切面编程AOP的容器

![1594608371197](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1594608371197.png)

#### 控制反转（IOC）

就是一种设计思想，DI是IOC的一种实现方式，在没有IOC的程序中，对象的创建与对象之间的依赖关系完全编码在程序中，对象的创建完全由程序自己控制，控制反转后，对象的创建转移给第三方，由第三方管理对象

耦合：程序间的依赖关系（类之间的依赖，方法之间的依赖）

解耦：降低程序间的依赖关系

实际的开发中应该做到编译时不依赖，运行时才依赖，这种才可以解决程序独立性很差的问题

解耦的思路：（JDBC注册时forName）

1、使用反射来创建对象，而避免使用new关键字

2、通过读取配置文件来获取要创建的对象的全类限定名

**概念**：将创建对象的权利交给框架，包括**依赖注入（DI）**和**依赖查找**

**作用**：降低程序间的耦合（依赖关系）

- DI：依赖关系的维护，可以注入的数据有三类：基本类型和String；其他的bean类型；复杂类型/集合类型；

  ​		注入的方式有三种：使用构造函数提供；使用set方法提供；使用注解提供



IOC创建对象的方式

- 使用无参构造创建对象，默认

  ```xml
   <bean id="hello" class="nancy.pojo.Hello" >
          <!--属性注入-->
          <property name="str" value="Spring"/>
   </bean>
  ```

  

- 使用有参构造创建对象

  - 下标赋值

    ```xml
     <bean id="hello2" class="nancy.pojo.Hello">
            <!--使用下标进行赋值-->
            <constructor-arg index="0" value="nancy"/>
     </bean>
    ```

    

  - 利用类型进行创建（不推荐使用）----存在相同的类型

    ```xml
    <bean id = "hello3" class="nancy.pojo.Hello">
            <!--利用类型进行注  注意:基本数据类型使用int 引用类型使用全包名 -->
            <constructor-arg type="java.lang.String" value="nancy"/>
    </bean>
    ```

    

  - 利用参数名进行赋值

    ```xml
     <!-- 使用参数名进行注入-->
    <bean id = "hello4" class="nancy.pojo.Hello">
        <constructor-arg name="s" value="nancy"/>
    </bean>
    ```



总结：在配置文件加载的时候，容器中管理的对象就已经创建了

#### AOP（面向切面编程）

是指通过**预编译的方式**和**运行期动态代理**实现程序功能的统一维护的一种技术，利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务各逻辑各部分之间的耦合度降低，提高程序的可重用性，提高了开发的效率

简单点说就是把我们程序中重复的代码拿出来，在需要执行的时候，使用动态代理的技术，在不修改源码的方式下，对我们已有的方法进行增强



## Spring MVC 

MVC三层架构：模型（Model)、视图（View)、控制器(Controller),是将业务、数据、显示分离的方法来组织代码。

- 模型（Model)：数据和行为。（dao，service）提供了模型数据查询和模型数据更新的等功能，包括数据和业务
- 视图（View)：进行吗模型展示，就是我们见到的用户界面
- 控制器(Controller)：接收用户的请求，委托给模型进行处理，处理完成之后，将返回的数据模型交给视图。

**职责**：

- 模型（Model）：业务逻辑、保存数据的状态（javaBean）
- 视图（View）：显示页面（JSP）
- 控制器（Controller)：拿到表单的数据、调用业务逻辑、转向指定的页面（Servlet）

Spring MVC 是Spring FrameWork的一部分，是基于Java实现MVC轻量级的Web框架，它通过一套注解，让一个简单的Java类称为成为处理请求的控制器，不需要实现任何接口，支持RestFul编码风格的请求

特点：轻量级，简单易学；高效，基于请求响应的MVC框架；与Spring兼容性好；简洁灵活；功能强大；约定优于配置

![1596600165960](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1596600165960.png)



用户发起请求的时候，会经过前端控制器DispatcherServlet，DispatcherServlet会根据这个请求找到映射器，然后将映射器返回回来，根据这个映射器再去适配这个映射器（cotroller），由具体的controller去执行，会返回model和view，在根据modol和view去配置具体的视图解析器视图解析器返回给前端

**SpringMVC和Struct2的区别**

共同点：都是表现层框架，都是基于MVC模型编写的

​				底层都是利用ServletAPI

​				处理请求机制都是一个核心控制器

区别：SpringMVC的入口是Servlet，Struct2的是Filter

​            SpringMVC是基于方法设计的，Struct2是基于类设计的，Struct2的框架是多例的，每次发送一个请求，都会创造一个Struct2的框架，然后执行方法，处理请求；而MVC是单例的，只会创建一个实例，当接收一个请求的时候，调用方法进行处理  











## Spring Boot

可以说Spring boot框架是spring家族中一个全新的框架，简化了spring应用程序的创建和开发的过程，可以简化我们以前使用Spring MVC+Spring+MyBatis框架开发使用的搭建整合三大框架，我们需要配置web.xml,配置Spring,需要大量的进行配置，Spring Boot框架抛弃了繁琐的xml配置过程，采用默认的配置简化了我们的开发过程

特性：

1、可以快速创建基于Spring的应用程序

2、可以直接使用java main方法启动内嵌的Tomcat服务器运行Spring Boot程序，而不需要使用war

3、根据maven的依赖配置，Spring Boot自动配置Spring和Spring MVC

4、可以完全使用注解





=======




## Spring

Spring理念：使得现有的技术更加的实用，整合现有的框架技术

优点：

- Spring是一个免费的开源的框架，容器
- 轻量级框架，非入侵式的
- 控制反转IOC，面向切面AOP

总而言之，就是一个轻量级的控制反转IOC和面向切面编程AOP的容器

![1594608371197](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1594608371197.png)

#### 控制反转（IOC）

就是一种设计思想，DI是IOC的一种实现方式，在没有IOC的程序中，对象的创建与对象之间的依赖关系完全编码在程序中，对象的创建完全由程序自己控制，控制反转后，对象的创建转移给第三方，由第三方管理对象

耦合：程序间的依赖关系（类之间的依赖，方法之间的依赖）

解耦：降低程序间的依赖关系

实际的开发中应该做到编译时不依赖，运行时才依赖，这种才可以解决程序独立性很差的问题

解耦的思路：（JDBC注册时forName）

1、使用反射来创建对象，而避免使用new关键字

2、通过读取配置文件来获取要创建的对象的全类限定名

**概念**：将创建对象的权利交给框架，包括**依赖注入（DI）**和**依赖查找**

**作用**：降低程序间的耦合（依赖关系）

- DI：依赖关系的维护，可以注入的数据有三类：基本类型和String；其他的bean类型；复杂类型/集合类型；

  ​		注入的方式有三种：使用构造函数提供；使用set方法提供；使用注解提供



IOC创建对象的方式

- 使用无参构造创建对象，默认

  ```xml
   <bean id="hello" class="nancy.pojo.Hello" >
          <!--属性注入-->
          <property name="str" value="Spring"/>
   </bean>
  ```

  

- 使用有参构造创建对象

  - 下标赋值

    ```xml
     <bean id="hello2" class="nancy.pojo.Hello">
            <!--使用下标进行赋值-->
            <constructor-arg index="0" value="nancy"/>
     </bean>
    ```

    

  - 利用类型进行创建（不推荐使用）----存在相同的类型

    ```xml
    <bean id = "hello3" class="nancy.pojo.Hello">
            <!--利用类型进行注  注意:基本数据类型使用int 引用类型使用全包名 -->
            <constructor-arg type="java.lang.String" value="nancy"/>
    </bean>
    ```

    

  - 利用参数名进行赋值

    ```xml
     <!-- 使用参数名进行注入-->
    <bean id = "hello4" class="nancy.pojo.Hello">
        <constructor-arg name="s" value="nancy"/>
    </bean>
    ```



总结：在配置文件加载的时候，容器中管理的对象就已经创建了

#### AOP（面向切面编程）

是指通过**预编译的方式**和**运行期动态代理**实现程序功能的统一维护的一种技术，利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务各逻辑各部分之间的耦合度降低，提高程序的可重用性，提高了开发的效率

简单点说就是把我们程序中重复的代码拿出来，在需要执行的时候，使用动态代理的技术，在不修改源码的方式下，对我们已有的方法进行增强



## Spring MVC 

MVC三层架构：模型（Model)、视图（View)、控制器(Controller),是将业务、数据、显示分离的方法来组织代码。

- 模型（Model)：数据和行为。（dao，service）提供了模型数据查询和模型数据更新的等功能，包括数据和业务
- 视图（View)：进行吗模型展示，就是我们见到的用户界面
- 控制器(Controller)：接收用户的请求，委托给模型进行处理，处理完成之后，将返回的数据模型交给视图。

**职责**：

- 模型（Model）：业务逻辑、保存数据的状态（javaBean）
- 视图（View）：显示页面（JSP）
- 控制器（Controller)：拿到表单的数据、调用业务逻辑、转向指定的页面（Servlet）

Spring MVC 是Spring FrameWork的一部分，是基于Java实现MVC轻量级的Web框架，它通过一套注解，让一个简单的Java类称为成为处理请求的控制器，不需要实现任何接口，支持RestFul编码风格的请求

特点：轻量级，简单易学；高效，基于请求响应的MVC框架；与Spring兼容性好；简洁灵活；功能强大；约定优于配置

![1596600165960](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1596600165960.png)



用户发起请求的时候，会经过前端控制器DispatcherServlet，DispatcherServlet会根据这个请求找到映射器，然后将映射器返回回来，根据这个映射器再去适配这个映射器（cotroller），由具体的controller去执行，会返回model和view，在根据modol和view去配置具体的视图解析器视图解析器返回给前端

**SpringMVC和Struct2的区别**

共同点：都是表现层框架，都是基于MVC模型编写的

​				底层都是利用ServletAPI

​				处理请求机制都是一个核心控制器

区别：SpringMVC的入口是Servlet，Struct2的是Filter

​            SpringMVC是基于方法设计的，Struct2是基于类设计的，Struct2的框架是多例的，每次发送一个请求，都会创造一个Struct2的框架，然后执行方法，处理请求；而MVC是单例的，只会创建一个实例，当接收一个请求的时候，调用方法进行处理  











## Spring Boot

可以说Spring boot框架是spring家族中一个全新的框架，简化了spring应用程序的创建和开发的过程，可以简化我们以前使用Spring MVC+Spring+MyBatis框架开发使用的搭建整合三大框架，我们需要配置web.xml,配置Spring,需要大量的进行配置，Spring Boot框架抛弃了繁琐的xml配置过程，采用默认的配置简化了我们的开发过程

特性：

1、可以快速创建基于Spring的应用程序

2、可以直接使用java main方法启动内嵌的Tomcat服务器运行Spring Boot程序，而不需要使用war

3、根据maven的依赖配置，Spring Boot自动配置Spring和Spring MVC

4、可以完全使用注解





>>>>>>> 3cfa628290ebd6ef1c2e348bb66e5178b0ffd18b

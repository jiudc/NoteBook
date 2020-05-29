# Spring

​	Spring是一个开源的轻量级Java SE（Java 标准版本）/Java EE（Java 企业版本）开发应用框架，其目的是用于简化企业级应用程序开发。特点如下：

1. Spring能帮我们根据配置文件创建及组装对象之间的依赖关系。
2. Spring 面向切面编程能帮助我们无耦合的实现日志记录，性能统计，安全控制。
3. Spring能非常简单的帮我们管理数据库事务。
4. Spring还提供了与第三方数据访问框架（如Hibernate、JPA）无缝集成，而且自己也提供了一套JDBC访问模板，来方便数据库访问。
5. Spring还提供与第三方Web（如Struts、JSF）框架无缝集成，而且自己也提供了一套Spring MVC框架，来方便web层搭建。
6. Spring能方便的与Java EE（如Java Mail、任务调度）整合，与更多技术整合（比如缓存框架）。

## IOC-Inversion Of Control

​	传统Java SE程序设计，我们直接在对象内部通过new进行创建对象，是程序主动去创建依赖对象；而IoC是有专门一个容器来创建这些对象，即由Ioc容器来控制对象的创建；谁控制谁？当然是IoC 容器控制了对象；控制什么？那就是主要控制了外部资源获取（不只是对象包括比如文件等）。

​	IoC容器就是具有依赖注入功能的容器，IoC容器负责实例化、定位、配置应用程序中的对象及建立这些对象间的依赖。应用程序无需直接在代码中new相关的对象，应用程序由IoC容器进行组装。在Spring中BeanFactory是IoC容器的实际代表者。

## DI-Dependency Injection

- 谁依赖于谁：当然是应用程序依赖于IoC容器；
- 为什么需要依赖：应用程序需要IoC容器来提供对象需要的外部资源；
- 谁注入谁：很明显是IoC容器注入应用程序某个对象，应用程序依赖的对象；
- 注入了什么：就是注入某个对象所需要的外部资源（包括对象、资源、常量数据）。

## Bean

​	由IoC容器管理的那些组成你应用程序的对象我们就叫它Bean， Bean就是由Spring容器初始化、装配及管理的对象，除此之外，bean就与应用程序中的其他对象没有什么区别了。

​	配置文件的根元素是beans，每个组件使用bean元素来定义。Bean的两个元素是必须的：id-组件的默认名称，class-类的全名，name-是class属性的一个别名

## 注入

### 手工装配

list、map、property、util:list、p:、c:

### 自动装配

1. byType：xml中autowire=“byName”
2. byNames：xml中autowire=“byTpe”

1. SET方法注入：Property
2. 构造方法注入：Constructor-org
3. 接口注入

![CDATA[]]-字面包含特殊字符可以使用

## Spring 核心思想

​	降低Java开发的复杂性

1. 基于POJO(Plain Old Java Object)的轻量级和最小侵入性编程；
2. 通过依赖注入和面向接口实现松耦合；
3. 基于切面和惯例进行申明式编程；
4. 通过切面和模板较少样板式代码

### 应用上下文

1. AnnotationConfigApplicationContext：从一个或多个基于Java配置类中加载Spring应用上下文
2. AnnotaConfigWebApplicationContex：从一个或多个基于Java配置类中加载Spring Web应用上下文
3. ClassPathXmlApplictionContext：从类路径下的一个或多个XML配置文件中加载上下文定义，把应用上下文的定义文件作为类资源
4. FileSystemXmlapplicationcntext：从文件系统下的一个或多个XML配置文件中加载上下文定义
5.  xmlWebApllicationContext：从Web应用下的一个或多个XML配置文件中加载文件定义

### 生命周期

![image-20200305084321404](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200305084321404.png)

### 框架

![image-20200305084350091](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200305084350091.png)

## 装配Bean

​	Spring提供三种装配机制。

1. 在XML中进行显示装配；
2. 在JAVA中进行显示装配；
3. 隐式的bean发现机制和自动装配

### 自动化装配

1. 组件扫描
2. 自动装配

- @Component();
- @Component(“bean_name”);
- @Named();
- @ComponentScan();
- @ComponentScan(“Libname”);
- @ComponentScan(basePackages = “Libname”);
- @ComponentScan(basePackages = {“Nm1”,”Nm2”});
- @ComponentScan(basePackageClases ={CDplayer.class,DVDPlayer.class});
- @Autowired;
  - 可用于构造器
  - 属性的Setter方法
  - 类的任何方法，Spring都会尝试满足方法参数上所申明的依赖，若没有匹配的bean，那么在应用上下文创建的时候回抛出异常。
    - @Autowired(required=false);若Spring没有找到合适的bean不会报异常，使得该bean处于未装配状态
- @Bean:
  - 告诉Spring这个方法将会返回一个对象，该对象要注册为Spring应用上下文的bean

## XML装配

### 构造器注入

- <constructor-arg>

  - ```xml
    <bean id="cdplayer1" class="com.ldc.chapter02.CDPlayer">
        <constructor-arg ref="castle"></constructor-arg>
    </bean>
    ```

- c命名空间

  - ```xml
    xmlns:c="http://www.springframework.org/schema/c"
    <bean id="cdplayer2" class="com.ldc.chapter02.CDPlayer"
          c:cd-ref="castle"/>
    ```

  - c-命名空间前缀，构造器参数名，注入bean引用

  - c:_0-ref：参数的索引

  - c:_-ref：若只有一个参数

### 字面量注入

- ```xml
  <bean id="muse" class="com.ldc.chapter02.BlankDisc">
      <constructor-arg value="Muse"></constructor-arg>
      <constructor-arg value="Jolion"></constructor-arg>
  </bean>
  <bean id="play" class="com.ldc.chapter02.BlankDisc"
        c:artist="Jolion"
        c:_0="Play"/>
  ```

### 装配集合

- ```xml
  <constructor-arg>
      <list>
          <value>1</value>
          <value>2</value>
      </list>
  </constructor-arg>
  ```

### 设置属性

- <property>：通过set方法注入属性中
- p-命名空间：xmlns:p="http://www.springframework.org/schema/p"

### util-命名空间

​	用于装配集合等

- util:constant:引用某个雷星星的public static域，并将其暴露为bean
- util:list
- util:map
- util:propeties
- util:property-path:引用一个bean的属性（或内嵌属性），并将其暴露为bean
- util:set

### 混合配置

#### JavaConfig中引入XML配置

- ```java
  @Configuration
  @Import(*.class)
  @ImportResource("classpath:*.xml")
  ```

#### XML配置中引用JavaConfig

- ```xml
  <bean class="*.*"/>
  <import resource=".xml"/>
  ```

## 高级装配


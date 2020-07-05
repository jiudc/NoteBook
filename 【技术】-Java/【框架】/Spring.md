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

1. byType：xml中autowire=“byType”
2. byNames：xml中autowire=“byName”

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

![img](https://raw.githubusercontent.com/jiudc/pictures/master/java0-1558500658.jpg)

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

- @Component();    //类名的第一个字母变成小写
- @Component(“bean_name”);
- @Named();
- @ComponentScan();
- @ComponentScan(“Libname”);
- @ComponentScan(basePackages = “Libname”);
- @ComponentScan(basePackages = {“Nm1”,”Nm2”});
- @ComponentScan(basePackageClases ={CDplayer.class,DVDPlayer.class});    //类所在的包作为组件扫描的基础包
- @ContextConfiguration(classes=CDPlayer.class);    //从类中加载配置，该类可使用@ComponentScan注解
- @Autowired;
  - 可用于构造器
  - 属性的Setter方法
  - 类的任何方法，Spring都会尝试满足方法参数上所申明的依赖，若没有匹配的bean，那么在应用上下文创建的时候会抛出异常。
    - @Autowired(required=false);若Spring没有找到合适的bean不会报异常，使得该bean处于未装配状态
- @Bean:
  - 告诉Spring这个方法将会返回一个对象，该对象要注册为Spring应用上下文的bean

### Java显式装配

+ @Bean：

  + 告诉Spring这个方法将会返回一个对象，该对象要注册为Spring应用上下文的bean

  + 默认，bean的ID与带有@Bean注解的方法名一样

  + @Bean(name="beanName")：为bean指定名称

  + ```java
    @Bean
    public CompactDisc sgtPeppers(){
            return new SgtPeppers();
    }
    ```

  + 可以通过构造器或者Setter方法注入bean对象

  + ```java
    @Bean
    public CDPlayer cdPlayer(CompactDisc cd){
            return new CDPlayer(cd);
    }
    ```

  + 使用这种方式引入bean不要求注入的对象和本对象在同一个配置类中，被注入的对象可以通过组件扫描或者XML配置

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

### 按照环境装配

#### Java

```java
@Configuration
public class DataSourceConfig {

    @Bean(destroyMethod = "shutdown")
    @Profile("dev")
    public DataSource dataSource() {
        return new EmbeddedDatabaseBuilder()
                .setType(EmbeddedDatabaseType.H2)
                .addScript("classpath:schema.sql")
                .build();
    }

    @Bean
    @Profile("prd")
    public DataSource jndiDataSouce() {
        JndiObjectFactoryBean jndiObjectFactoryBean =
                new JndiObjectFactoryBean();
        jndiObjectFactoryBean.setJndiName("jdbc/myDS");
        jndiObjectFactoryBean.setResourceRef(true);
        jndiObjectFactoryBean.setProxyInterface(javax.sql.DataSource.class);
        return (DataSource) jndiObjectFactoryBean.getObject();
    }
}
```

#### xml

```xml
<beans profile="dev">
    <jdbc:embedded-database id="dataSource">
        <jdbc:script location="classpath:schema.sql"/>
        <jdbc:script location="classpath:test-data.sql"/>
    </jdbc:embedded-database>
</beans>

<beans profile="prd">
    <jee:jndi-lookup id="dataSource" jndi-name="dataSource"
                     resource-ref="true"
                     proxy-interface="javax.sql.DataSource"/>
</beans>
```

#### 如何激活

​	spring.profiles.active和spring.profiles.default

- 作为DispatcherServlet的初始化参数
- 作为Web应用的上下文参数
- 作为JNDI条目
- 作为环境变量
- 作为JVM的系统属性
- 在集成测试类上，使用@ActiveProfiles注解设置

### 条件化Bean

@conditional(ObjectExistCondition.class)

```java
public class ObjectExistCondition implements Condition {
    @Override
    public boolean matches(ConditionContext conditionContext, AnnotatedTypeMetadata annotatedTypeMetadata) {
        Environment env = conditionContext.getEnvironment();
        return env.containsProperty("magic");

    }
}
```

​	ConditionContext可做到如下几点：

![image-20200529182818748](https://raw.githubusercontent.com/jiudc/pictures/master/image-20200529182818748.png)

- getRegistry()检查bean定义
- getBeanFactory()检查bean是否存在，探测bean的属性
- getEnvironment()检查环境变量是否存在以及值
- getResouceLoader返回ResouceLoader所加载的资源
- getClassLoader返回ClassLoader加载并检查类是否存在

  AnnotatedTypeMetadata用于检查@Bean注解的方法上还有什么其他的注解

### 处理装配歧义性

#### 标示首选

- @Primary
- primary="true"

#### 限定自动装配的Bean

- @Qualifier()与Autowired和Inject协同使用，限定符与注入的bean名称紧耦合，对类名的改动会导致限定符时效

- 自定义限定符

  - ```java
    @Component
    @Qualifier("cold")
    ```

  - ```java
    @Autowired/@Bean
    @Qualifer("cold")
    ```

- 自定义注解：可以添加多个限定符

  - ```java
    @Target({ElementType.CONSTRUCTOR, ElementType.FIELD,
            ElementType.METHOD, ElementType.TYPE})
    @Retention(RetentionPolicy.RUNTIME)
    @Qualifier
    public @interface Cold {
    }
    ```

  - ```java
    @Component
    @Cold
    @Creamy
    ```

### Bean的作用域

- 单例（Singleton）
- 原型（Prototype）：每次注入或通过Spring上下文获取均会创建新的
- 会话（Session）：会话级别
- 请求（Request）：请求级别

  设置作用域

- 组件扫描发现

  ```java
  @Component
  @Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE)
  public class NotePad {
  }
  ```

- Java配置

  ```java
  @Bean
  @Scope(ConfigurableBeanFactory.SCOPE_SINGLETON)
  public NotePad notePad(){
      return new NotePad();
  }
  ```

- xml配置

  ```xml
  <bean id="notepad" class="com.ldc.chapter03.NotePad"
                scope="prototype"/>
  ```

#### 会话和请求作用域

```java
@Component
@Scope(value = WebApplicationContext.SCOPE_REQUEST,
        proxyMode = ScopedProxyMode.INTERFACES)
public class ShoppingCart {
}
```

proxyMode是用于指示代理要实现接口。若修饰的bean类型是接口，则需要修改为

```java
proxyMode = ScopedProxyMode.TARGET_CLASS
```

#### XML中申明代理作用域

```xml
<bean id="notepad" class="com.ldc.chapter03.NotePad"
    scope="session">
    <aop:scoped-proxy proxy-target-class="true"/>
</bean>
```

### 运行时植入

  避免使用硬编码，Spring提供两种在运行时求值的方式

- 属性占位符
- Spring表达式语言（SpEL）

#### 注入外部的值

```java
@Configuration
@PropertySource("classpath:/com/ldc/chapter03/cd.properties")
public class EnvConfig {
    @Autowired
    Environment env;
    @Bean
    public BlankDisc blankDisc(){
        return new BlankDisc(env.getProperty("cd.artist"),env.getProperty("cd.title"),
                Collections.singletonList(env.getProperty("cd.tracks")));
    }
}
```

​	通过PropertySource可以将文件里面的类容加载到env中，通过getProperty()

- String getProperty(String key,【String defaultValue】)
- T getProperty(String key, Class<T> type, 【T defaultValue】)
- getRequiredProperty()
- containsProperty()
- getPropertyAsClass()：将属性解析为类
- 检查哪些profile处于激活状态
  - getActiveProfiles()
  - getDefaultProfiles()
  - acceptsProfile(String... profiles)：如果environment支持给定profile，返回true

#### 占位符

- 组件扫描或者自动装配

  ```java
  public BlankDisc(@Value("${disc.title}") String title, @Value("${disc.artist}")String artist, List<String> tracks) {
      this.title = title;
      this.artist = artist;
      this.tracks = tracks;
  }
  ```

- xml

  ```xml
      &lt;bean id=&quot;play&quot; class=&quot;com.ldc.chapter02.BlankDisc&quot;
            c:artist=&quot;Jolion&quot;
            c:_0=&quot;${cd.title}&quot;
            c:_2-ref=&quot;trackList&quot;/&gt;  
  ```

  要使用占用符，需要配置PropertyPlaceholderConfigure或PropertySourcePlaceholerConfigure。推荐后者。使用XML则<context:property-placeholder>

### 使用Spring表达式语言进行装配

​	Spring Expression Language，使用#{}

- 表示字面值：
  - #{3.14}
  - #{9.87E4}科学计数
  - #{'Hello'} String
  - #{false} 布尔类型
- 使用bean的ID来引用bean
  - #{sgtPepper}
- 调用方法和访问对象的属性
  - #{sgtPepper.artist} 属性
  - #{sgtPepper.selectArtist().toUpperCase()} 调用方法，若出现null，可通过类型安全的运算符#{sgtPepper.selectArtist()?.toUpperCase()}
- 在表达式中使用类型
  - T()结果会是一个Class对象，如T(java.lang.Math).PI
- 对值进行算数、关系和逻辑运算
  - 算数运算，+、-、*、/、%、^
  - 比较运算，<、>、<=、>=、lt、gt、eq、le、ge
  - 逻辑运算，and、or、not、|
  - 条件运算，？:(ternary)
  - 正则表达式，matches
  - 举例：#{2 * T(java.lang.Math).PI * circle.radius)
- 正式表达式匹配
  - #{admin.emal mathes '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\\\.com'}
- 集合操作
  - #{jukebox.song[4].title}
  - #{jukebox.song[T(java.lang.Math).random()*junkebox.songs.size()].title}
    - .?[]:表达式的计算结果为true，name条目会放到新的集合中
    - .^[]:集合中查询第一个匹配项
    - .$[]:集合中最后一个匹配项
    - .![]：投影运算符，它会从集合的每个成员中选择特定的属性放到另一个集合中

## 切面

### 术语

#### 通知（Advice）

- 前置通知(Before)
- 后置通知(After)
- 返回通知(AfterReturning)
- 异常通知(AfterThrowing)
- 环绕通知(Around)

#### 连接点

​	应用执行过程中能够插入切面的一个点

#### 切点

​	匹配通知所要织入的一个或多个连接点

#### 切面

​	通知和切点的结合。通知和切点共同定义了切面的全部内容。

#### 引入

   向现有的类添加新方法或属性

#### 织入

​	把切面应用到目标对象并创建新的代理对象的过程。

- 编译器：在目标类编译时被织入
- 类加载期：在目标类加载到JVM被织入。需要特殊的类加载器（ClassLoader）
- 运行期：在应用运行时刻织入

### Spring对AOP的支持

- 基于代理的经典Spring AOP
  - Spring通知是java编写的
  - Spring在运行时通知对象
  - Spring只支持方法级别的连接点，不支持字段和构造器
- 纯POJO切面
- @AspectJ注解驱动的切面
- 注入式AspectJ切面

### 通过切点来选择连接点

- arg():限定连接点匹配参数为指定类型的执行方法

- @args():限制连接点匹配参数由指定注解标注的执行方法

- execution():用于匹配是连接点的执行方法

- this():限制连接点匹配AOP代理的bean引用为指定类型的类

- target:限制连接点匹配目标对象为指定类型的类

- @target():限制连接点匹配特定的执行对象，这些对象对应的类具有指定类型的注解

- within():限定连接点匹配指定的类型

- @within():限定连接点匹配指定注解所标注的类型

- @annotation:限定匹配带有指定注解的连接点

- @bean()

  举例：execution(* connert.Performance.perform(...)) && within(concert.*)

  execution(* connert.Performance.perform(...)) and !bean('woodstock')

### 使用注解创建切面

​	AspectJ 5引入。

#### 定义切面

```java
@Aspect
public class Audience {

    @Pointcut("execution(* com.ldc.chapter04.Performance.perform(..))")
    public void performance() {
    }

    @Before("performance()")
    public void silenceCellPhones() {
        System.out.println("Silencing cell phones");
    }

    @Before("performance()")
    public void takeSeats() {
        System.out.println("Taking seats");
    }

    @AfterReturning("performance()")
    public void applause() {
        System.out.println("CLAP CLAP CLAP");
    }

    @AfterThrowing("performance()")
    private void demandRefund() {
        System.out.println("Demanding  a refund");
    }

}
```

#### 启动自动代理

- java：@EnableAspectJAutoProxy

  ```java
  @Configuration
  @EnableAspectJAutoProxy
  @ComponentScan
  public class ConcertConfig {
      @Bean
      public Audience audience() {
          return new Audience();
      }
  
      @Bean
      public Performance performance() {
          return new JolinPerformance();
      }
  }
  ```

- xml：<aop:aspectj-autoproxy/>

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xmlns:aop="http://www.springframework.org/schema/aop"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">
      <context:component-scan base-package="com.ldc.chapter04"/>
      <aop:aspectj-autoproxy/>
      <bean class="com.ldc.chapter04.Audience"/>
      <bean class="com.ldc.chapter04.JolinPerformance"/>
  </beans>
  ```

- 环绕通知：ProceedingJoinPoint ，jp.proceed();

  ```java
  @Aspect
  public class Audience {
  
      @Pointcut("execution(* com.ldc.chapter04.Performance.perform(..))")
      public void performance() {
      }
  
      @Around("performance()")
      public void watchPerformance(ProceedingJoinPoint jp){
          try{
              System.out.println("Silencing cell phones");
              System.out.println("Taking seats by jp");
              jp.proceed();
              System.out.println("CLAP CLAP CLAP");
          }catch (Throwable e){
              System.out.println("Demanding  a refund");
          }
      }
  }
  ```

- 处理通知中的参数
  execution(* connert.Performance.perform(int)) && args(num)

- 引入新功能

  ```java
  @Aspect
  public class ExtendAspectConfig {
      @DeclareParents(value = "com.ldc.chapter04.Performance+",
              defaultImpl = DefaultEncoreable.class)
      public static Encoreable encoreable;
  }
  ```

#### 在XML中申明切面

| AOP配置元素                           | 用途                                     |
| ------------------------------------- | ---------------------------------------- |
| \<aop:advisor\>                       | 通知器                                   |
| \<aop:after> before                   | 后置/前置                                |
| \<aop:after-returning> after-throwing | 返回/异常                                |
| \<aop:around>                         | 环绕                                     |
| \<aop:aspect> aspectj-autoproxy       | 定义切面/启动@AspectJ注解驱动的切面      |
| \<aop:config>                         | 顶层的AOP配置元素                        |
| \<aop:declare-parents>                | 以透明的方式为被通知的对象引入额外的接口 |
| \<aop:pointcut>                       | 切点                                     |

切面引入新的功能：

- default-impl：直接标志委托
- delegate-ref：间接委托，区别在于它是bean

### 注入AspectJ

 AspectJ需要使用factory-method=“aspectOf”初始化
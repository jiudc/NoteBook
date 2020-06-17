# MyBatis

## 运行流程结构图

![image-20200604214705473](https://github.com/jiudc/pictures/blob/master/image-20200604214705473.png)

## 搭建环境

${}符号表示拼接SQL串，将接收到的内容不加任何修饰地拼接在SQL中，在${}中只能使用value代表其中的参数。可能引起SQL注入。

## 配置环境

### configuration

顶级标签

### properties

  可以供整个配置文件中的其他配置使用。添加默认值。

```xml
<properties resource="db.properties">
    <property name="org.apache.ibatis.parsing.PropertyParser.enable-default-value" value="true"/>
</properties>
<property name="username" value="${db.username:root}"/>
```

### setting

| 设置参数                         | 描述                                                         | 有效值                                                       | 默认值                                                |
| -------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ----------------------------------------------------- |
| cacheEnabled                     | 全局地开启或关闭配置文件中的所有映射器已经配置的任何缓存。   | true \| false                                                | true                                                  |
| lazyLoadingEnabled               | 延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。特定关联关系中可通过设置fetchType属性来覆盖该项的开关状态。 | true \| false                                                | false                                                 |
| aggressiveLazyLoading            | 当开启时，任何方法调用都会加载该对象的所有属性。否则，每个属性会按需加载（参考lazyLoadTriggerMethods） | true \| false                                                | false(true in <= 3.4.1)                               |
| multipleResultSetsEnabled        | 是否允许单一语句返回多结果集（需要兼容驱动）                 | true \| false                                                | true                                                  |
| useColumnLabel                   | 使用列标签代替列名。不同的驱动在这方面会有不同的表现，具体可参考相关驱动文档或通过测试这两种不同的模式来观察所用驱动的结果。 | true \| false                                                | true                                                  |
| useGeneratedKeys                 | 允许JDBC支持自动生成主键，需要驱动兼容。如果设置为true则这个设置强制使用自动生成主键，尽管一些驱动不能兼容但仍可正常工作（比如Derby） | true \| false                                                | false                                                 |
| autoMappingBehavior              | 指定MyBatis应如何自动映射列到字段或属性。NONE表示取消自动映射；PARTIAL只会自动映射没有定义嵌套结果集映射的结果集。FULL会自动映射任意复杂的结果集（无论是否嵌套） | NONE,PARTIAL,FULL                                            | PARTIAL                                               |
| autoMappingUnknownColumnBehavior | 指定发现自动映射目标未知列（或者未知属性类型）的行为。·NONE：不做任何反应·WARNING：输出提醒日志(‘org.apache.ibatis.session.AutoMappingUnknownColumnBehavior’的日志等级必须设置为WARN)·FAILING：映射失败（抛出SqlSessionException） | NONE,WARNING,FAILING                                         | NONE                                                  |
| defaultExecutorType              | 配置默认的执行器。SIMPLE就是普通的执行器；REUSE执行器会重用预处理语句（prepared statements）；BATCH执行器将重用语句并执行批量更新。 | SIMPLE,REUSE,BATCH                                           | SIMPLE                                                |
| defaultStatementTimeout          | 设置超时时间，它决定驱动等待数据库响应的秒数                 | 任意正整数                                                   | Not Set(null)                                         |
| defaultFetchSize                 | 为驱动的结果集获取数量（fetchSize）设置一个提示值。此参数只可以在查询设置中被覆盖。 | 任意正整数                                                   | Not Set(null)                                         |
| safeRowBoundsEnabled             | 允许在嵌套语句中使用分页（RowBounds）。如果允许使用则设置为false。 | true \| false                                                | False                                                 |
| safeResultHandlerEnabled         | 允许在嵌套语句中使用分页（ResultHandler）。如果允许使用则设置为false。 | true \| false                                                | True                                                  |
| mapUnderscoreToCamelCase         | 是否开启自动驼峰命名规则（camel case）映射，既从经典数据库列名A_COLUMN到经典java属性名aColumn的类似映射。 | true \| false                                                | False                                                 |
| localCacheScope                  | MyBatis利用本地缓存机制（Local Cache）防止循环引用（circular references）和加速重复嵌套查询。默认值为SESSION，这种情况下会缓存一个会话中执行的所有查询。若设置值为STATEMENT，本地会话仅用在语句执行上，对相同SqlSession的不同调用将不会共享数据。 | SESSION \| STATEMENT                                         | SESSION                                               |
| jdbcTypeForNull                  | 当没有为参数提供特定的JDBC类型时，为空值指定JDBC类型。某些驱动需要指定列的JDBC类型，多数情况直接用一般类型即可，比如NULL，VARCHAR或OTHER。 | jdbc Type 常量，大多都为：NULL，VARCHAR and OTHER            | OTHER                                                 |
| lazyLoadTriggerMethods           | 指定哪个对象的方法触发一次延迟加载。                         | 用逗号分隔的方法列表                                         | equals,clone,hashCode,toString                        |
| defaultScriptingLanguage         | 指定动态SQL生成的默认语言。                                  | 一个类型别名或完全限定类名                                   | org.apache.ibatis.scripting.xmltags.XMLLanguageDriver |
| defaultEnumTypeHandler           | 指定Enum使用的默认TypeHandler.（从3.4.5开始）                | 一个类型别名或完全限定类名                                   | org.apache.ibatis.type.EnumTypeHandler                |
| callSettersOnNulls               | 指定当结果集中值为null的时候是否调用映射对象的setter（map对象时为put）方法，这对于有Map.keySet()依赖或null值初始化的时候是有用的。注意基本类型（int，boolean等）是不能设置成null的。 | true \| false                                                | false                                                 |
| returnInstanceForEmptyRow        | 当返回行的所有列都是空时，MyBatis默认返回null。当开启这个设置时，MyBatis会返回一个空实例。请注意，它也适用于嵌套的结果集（i.e.collection and association）。（从3.4.2开始） | true \| false                                                | false                                                 |
| logPrefix                        | 指定MyBatis增加到日志名称的前缀。                            | 任何字符串                                                   | Not set                                               |
| logImpl                          | 指定MyBatis所用日志的具体实现，未指定时将自动查找。          | SLF4J \| LOG4J \| LOG4J2 \|JDK_LOGGING \|COMMONS_LOGGING \| STDOUT_LOGGING \| N0_LOGGING | Not set                                               |
| proxyFactory                     | 指定MyBatis创建具有延迟加载能力的对象所用到的代理工具。      | CGLIB \| JAVASSIST                                           | JAVASSIST(MyBatis 3.3 or above)                       |
| vfsImpl                          | 指定VFS的实现                                                | 自定义VFS的实现的类全限定名，以逗号分隔                      | Not set                                               |
| useActualParamName               | 允许使用方法签名中的名称作为语句参数名称。为了使用该特性，工程必须采用java 8编译，并加上-parameters选项（从3.4.1开始） | true \| false                                                | true                                                  |
| configurationFactory             | 指定一个提供Configuration实例的类。这个被返回的Configuration实例用来加载被反序列化对象的懒加载属性值。这个类必须包含一个签名方法static Configuration getConfiguration()。（从3.2.3版本开始） | 类型别名或者全类名                                           | Not set                                               |

### typeAliases

- 对一个类定义别名
- 对一个包定义别名，对应包装类的类名编程小写
- 使用@Alias("user")注解定义别名
- 常见类型别名：_byte对应byte，intege对应java.lang.Integer

```xml
<typeAliases>
    <typeAlias alias="User" type="com.ldc.mvc.entity.User"/>
</typeAliases>
```

### typeHandlers


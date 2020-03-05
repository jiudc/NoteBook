# EasyMock

### 主体步骤

1. 使用EasyMock生成Mock对象

	1. 使用EasyMock动态构建：

		```java
		ResulteSet mockResultSet = EasyMock.create(ResultSet.class);
		```

	2.  使用IMocksControl构建：

		```java
		IMocksControl control = EasyMock.createControl();
		java.sql.Connection mockConnection = control.createMock (Connetcion.class);
		java.sql.Resultset mockResultset = control.createMock(ResultSet.class);
		```

2. 设定Mock对象的预期行为和输出（Record状态，录制预期行为和输出

	1. Invoke mockObject.method()

	2.  通过expectLastCall获取上一次方法调用对应的IExpectationSetters实例：

		```java
		mockObject.method(parameters…);
		 expectLastCall().andReturn(“My return value”);
		```

3. 通过IExpectationSettes实例设定Mock对象的预期输出
	1. 产生返回值
		1. 一次设定：expectLastCall().andReturn(“My return values”);
		2. 默认设定：expectLastCall().andStubReturn(mockObject);
	2.  异常
		1. 一次设定：IExpectationSetters<T> andThrows(Throwable throwable)
		2. 默认设定：void andStubThrow(Throwable throwable)
	3.  设定次数
		1. expectLastCall().andReturn(“My return values”).time(3);
		2. expectLastCall().andReturn(“My return values”).time(3,5);
		3. expectLastCall().andReturn(“My return values”).atLeastOnce();
		4. expectLastCall().andReturn(“My return values”).anyTimes();
4.  将Mock对象切换到Replay状态
	1. Replay(mockObject)
	2.  Control.replay()
5. 调用Mock对象进行单元测试
6. 对Mock对象的行为进行验证
	1. Verify(mockObject)
	2. Control.replay()

###  Mock对象的重

- reset
- IMocksControl reset

###  参数匹配器

1. anyObject()
2. aryEq()
3. isNull()
4. notNull()
5. sam()
6. lt(X value)\leq\geq\gt
7. startWith)\contains()\endWith()
8. matches()

​	自定义参数匹配器需要实现org.easymock.IArgumentMatcher借口，matches实现匹配逻辑，appendTo(StringBuffer buffer)添加匹配失败的提示的信息；

​	需要使用静态方法包装，通过reportMatcher方法报告给EasyMock。

```JAVA
Public Static String sqlEquals(String in){
 reportMatcher(new SQLEquals(in));
 return in;
}

mockStatment.executeQuery(sqlEquals(SELECT * FROM table))；
```

###  特殊Mock对象类型

- Stcik Mock：时序敏感
- Nick Mock：使用Mock对象对非预期方法调用默认为抛出AssertionError，使用此种返回0，null，或者false
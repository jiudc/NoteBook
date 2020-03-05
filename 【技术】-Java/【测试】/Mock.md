# Mock

​	Mock使用虚拟对象来模拟测试的方法。

- Mock对象：模拟对象
- Stub桩：替代具体功能的程序段

​	添加Mock依赖：

```xml
<dependency> 

<groupId>org.mockito</groupId> 

<artifactId>mockito-all</artifactId> 

<version>2.0.2-beta</version>

 </dependency> 
```

### 创建Mock

- 通过方法创建

```java
 class CreateMock { 
  @Before 
  public void setup() { mockUserDao = mock(UserDao.class); 
  userService = new UserServiceImpl(); 
  userService.setUserDao(mockUserDao); }
 } 
```

- 通过注解创建

```java
 class CreateMock { 
  @Mock UserDao mockUserDao; 
  @InjectMocks private UserServiceImpl userService; 
  @Before public void setUp() { //初始化对象的注
  MockitoAnnotations.initMocks(this); } 
 } 
```

### 设置对象期望和返回值

​	以下写法结果相同

1. When(mock.someMethod()).thenReturn(values1).thenReturn(values2);
2. When(mock.someMethod()).thenReturn(values1,vaue2);
3. When(mock.someMethod()).thenReturn(values1);
4. When(mock.someMethod()).thenReturn(values2);
5. doReturn(value1).doReturn(value2).when(mock).someMethod();

- 若返回void，设置doNothing：doNothing().when(mock).someMethod;
- 对方法设定返回异常：When (mock.someMethod()).thenThrow(new RuntimeException());
	doThrow(new RuntimeException).when(mock).someMethod();
- 参数匹配器：When(list.get(anInt())).thenReturn(“hello”);

###  结果验证

- 验证调用次数
	- Verify(mock1,timeout(100).time(2)).get(anyInt());
	- Nerver()，没有被调用，相当于times(0);
	- atLeast(N) 至少被调用N次
	- atLeastOnce() 相当于atLeast(1)
	- atMost(N) 最多被调用N次
- 超时验证：Timeout
- 方法调用顺序：Inorder
- 验证否存在被调用
	- verifyNoMoreInteractions 
	- verifyZeroInteractions
- 参数捕获器：ArgumentCaptor 可在验证时对方法的参数进行捕获，最后验证捕获的参数值。如果方法有多个参数都要捕获验证，那就需要创建多个ArgumentCaptor对象处理。

### Spy对象

​	Mockito提供给我们一种真实对象的操作方——Spy
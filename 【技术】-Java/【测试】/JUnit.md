# JUnit

### 基本用法

- 创建TestJunit.java测试类
-  测试类中添加testMethodName()的方法
- 方法中添加Annotation @Test
- 测试中使用Junit的assertEquals API检查
- 创建TestRunner类
- 使用JunitCore类则runClasses方法运行上述测试类的案例：
	 Result result = JUnitCore.runClasses(TestJunit.class);
- 获取在Result Object中运行的测试案例的结果
- 获取Result Object的getFailures()方法中的失败结果：
	 for(Failure failure : result.getFailures()){
	  System.out.println(failure.toString());
	 }
- 获取Result Object的wasSuccessful()方法中成功结果:
	 System.out.println(result.wasSuccessful());

### API

​	Junit中junit.framework包含了它的核心类

- Asert
	- Void assertEquals(Boolean expected, Boolean actual)
	- Void assertFalse(Boolean condition)
	- Void assertNotNull(Object object)
	- Void assertNull(Object object)
	- Void assertTrue(Boolean condition)
	- Void fail()
	- Void assertSame(expectedObj,resultObj)
	- Void assertArrayEquals(expectedArray, resultArray)
- TestCase
	- Int countTestCase()
	- TestResult createResult()
	- String getName()
	- TestResult run()
	- Void run(TestResult result)
	- Void setName(String name)
	- Void setup()
	- Void tearDown()
	- String toString()
- TestResult
- TestSuite

### 注释

- @Test:说明依附在Junit的public void方法作为一个测试案例
- @Before：该方法在test方法前运行，比如创建对象
- @After：在test后运行，比如释放在Before中分配的资源
- @BeforeClass：在public void方法中加该注释表明该方法在类中所有方法前运行
- @AfterClass:是方法在所有测试结束后执行，清理活动
- @Ignore：忽略不需要执行的测试

执行顺序：BeforeClass->Before->test 1->After->Before->test 2->After->AfterClass

### 套件测试

​	捆绑几个单元测试案例一起执行。@RunWith\@Suite

1. 创建一个java类
2. 在类上附上@RunWith(Suite.class)](mailto:在类上附上@RunWith(Suite.class))注释
3. 使用@Suite。SuiteClasser注释给Junit测试类加上引用

```java
@RunWith(Suite.class)
@Suite.SuiteClasses({
 TestJunit1.class,
 TestJunit2.class
})
Public class JunitTestSuite{
}
```

### 时间测试

@Test(timeout=1000)

### 异常测试

@Test(expected = ArithmeticException.class)

### 参数化测试

- 用@RunWith(Parameterized.class)](mailto:用@RunWith(Parameterized.class))来注释test类
- 创建一个由@Parameters注释的公共的静态方法，它返回一个对象的集合作为测试数据集合
- 创建一个公共的构造函数，它接受和一行测试数据等同的东西
- 为每一列测试数据创建一个实例变量
- 用实例对象为测试数据的来源来创建你的测试用例
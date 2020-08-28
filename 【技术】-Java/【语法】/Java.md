# Java

## 注释

```java
/**
*Description
*/文档注释，会被提取到API文档中
```

### javadoc标记

- @author
- @version
- @deprecated:不推荐使用的方法
- @param
- @return
- @see:参见
- @exception
- @throws 

​	Javadoc默认不会提取@author、@version

## 数据类型

### 直接量

- Int:二进制0B、0b开头；八进制：0开头；十六进制0x、0X
- Long：3L，0x12L
- Float：5.34F，3.14E5f
- Double:5.34,3.14E5
- Boolean：true，false
- Char：’a’,’\n’,’\u0061’ 
- String：“”
- Null
- 正无穷：Double，Float类的POSITIVE_INFINITY
- 负无穷：Double，Float类的NEGATIVE_INFINITY
- 非数：NaN，不与任何数相等，甚至NaN
- 65->’A’,97->’a’,48->’0’
- 127,32767,2147483647
- 数值可以使用下划线double pi = 3.14_159_23_3

### 算术运算符

+\-\*\/\++\--

### 赋值运算符

=

### 位运算符

&、|、~、^、<<、>>、>>>

### 比较运算符

\>，<,>=，<=,==,!=

### 逻辑运算符

&&\||\!\^\&\|

&与|，不短路的与和或

### 三目运算符

?:

## 流程控制

- switch：判断条件成立，则顺序执行，不再判断，因此需要加break
- break可以跳出外围循环，需要加上tag，如outloop:，tag只有放在循环语句之前才有用

## 数组

​	不要同时指定长度和分配初始值。数组需要初始化后才能使用，初始化即分配内存空间，并为每个数组元素指定初始值。

-  静态初始化，程序员指定元素，系统决定系统长度
- 动态初始化，程序员指定长度，系统分配初始值

​	两种不要混合使用。

1. 整数型：int,long,short,byte，初始值为0
2. 浮点型：float,double，初始值为0.0
3. 字符型：char，初始值为\u0000
4. 布尔型：boolean，初始值false
5. 引用类型：接口、数组、类，初始值null

### 静态初始化

```java
Int [] arr = new int[]{1,2,3};
Int [] arr ={1,2,3};//简化版
```

### 动态初始化

```java
Int[] arr = new int[5];
```

### Foreach

```java
for（type var: array）{
}
```

### 初始化

​	初始化多维数组的时候需要指定最左边维的大小

### Array

​	二分查找需要数组按照升序排列

- Int binarySearch（type[] a,type key）
- Int binarySearch(type[] a,int fromIndex,int toIndex,type key)
- Type[] copyOf(type[] original,int length)
- Type[] copyOfRange(type[] original,int from,int to)
- Boolean equals(type[] a,type[] a2)
- Void fill(type[] a,type val)
- Void fill(type[]a, int fromIndex,int toINdex,type val)
- Void sort(type[] a)
- Void sort(type[] a,int fromIndex,int toIndex)
- String toString(type[] a):顺序连接，多个数组元素使用英文逗号，和空格隔开

JAVA 8增强了Arrays类的功能

- Void parallerPrefix(xxx[] array,XxxBinaryOperater op):计算结果作为新元素，op计算公式包括left、right两个形参，当计算第一数组元素时，left的值默认为1
- Void parallelPrefix(xxx[] array,int fromIndex,int toIndex,XxxBinaryOperator op)
- Void setAll(xxx[] array,IntToXxxfunction generator)
- Void parallerseAll(xxx[] array,IntToXxxfunction generator)
- Void parallerSort(xxx[] a)
- Void parallerlSort(xxx[] a)
- Spliterator.OfXxx spliterator(xxx[] array):将数组的所有的元素都转换成对应的Spliterator对象
- Spliterator.OfXxx split erator(xxx[] array,int startInclusive,int endExclusive)
- XxxStream stream(xxx[] array)
- XxxStream stream(xxx[] array,int startInclusive,int endExclusive)

​	把char型数字转换成int型数字，因为他们的ASCII码值恰好相差48:

```java
Int num = numstr.charAt(i) - 48
```

## 方法

1. 方法不能独立定义，方法只能在类体内定义
2. 从逻辑意义上来看，方法要么属于类，要么属于内的对象
3. 永远不能独立执行方法，执行方法必须使用类或者对象，this

​	Java中方法都是按值传递。定义形参可变的方法，public static void test(int v1,string … v2)

## 对象和内存控制

​	This总是指向调用该对象的对象。有两种情况：

- 构造器中引 用该构造器正在初始化的对象
- 在方法中引用调用该方法的对象

​	新建一个对象，person p1 = new person()。首先会在堆中生成person类的空间，初始化static变量，后面才是分配对象

内存管理分为：

1. 内存分配
2. 内存回收

局部变量：储存在栈中

- 形参
- 方法内的局部变量
- 代码块内的局部变量

成员变量：

- 静态变量或类变量：使用static修饰：
- 非静态变量或实例变量

非法前向引用：

1. 非法

```java
Static int num1 = num2 + 2；
Static int num2 = 20;
```

```java
Int num1 = num2 + 2；
Int num2 = 20;
```

2. 合法

```java
Int num1 = num2 + 2;
Static int num2 = 20 ;
```

### 实例

1. 定义实例变量时指定初始值；
2. 非静态初始块中对实例变量指定初始值；
3. 构造器

1、2比3早，1,2的顺序按照他们在源程序中顺序

Javap –c x.class反编译

### 类变量

1. 定义实例变量时指定初始值；
2. 静态初始块中对实例变量指定初始值；

### 父类构造器

​	当调用某个类的构造器创建Java对象时，系统会优先调用父类的非静态初始块，再调用父类的构造器。

​	This代表正在初始化的JAVA对象。当变量的编译和运行的类型不同时，通过该变量访问它引用的对象的实例对象时，该实例对象的值由申明该变量的类型决定；通过该变量调用它引用的对象的实例方法时，该方法的行为由它实际所引用的对象来决定。

### 父子实例的内存控制

​	针对成员方法，编译器会**将父类的方法直接转移到子类**中，因此若子类重写，则会完全覆盖父类的函数。而针对成员变量，则不会出现该现象。两个变量依然存在。

​	Java程序允许某个方法通过return this返回该方法的JAVA对象，但不允许直接return super，甚至不允许直接将super当成一个引用变量使用。

### Final修饰符

- Final可修饰变量，被赋初始值之后，不能对它重新赋值
- Final可修饰方法，被final修饰的方法不能被重写
- Final可修饰类，不能派生类

​	Final修饰的变量必须现实指定初始值，而只能在如下三个位置指定：

1. 定义final实例对象的时候指定初始值
2. 在非静态初始化块中
3.  构造器中

​	Final修饰的类变量，同样必须显示指定

1. 定义final类变量时
2. 静态初始化块中
3. Final可用于宏替换

### 字符串缓冲池

​	Java会缓冲曾经用过的字符串。

## 代码块

执行顺序

1. 静态代码块
2. 构造块
3. 构造方法

### This

1. 调用本类的构造方法
2. this表示当前对象
3. 表示类中的属性

## 封装Encapsulation

- Private(当前类)->default(当前包)->protected(子类)->public(公共)
- mport package.sub.*;*代表类不能代表包
- Java默认为所有的源文件导入java.lang包下的所有类，因此String，System无需导入
- 使用import可以省略写包名。而使用import static（静态导入）可以连类名都可以省略

## 多态Polymorphism

​	Instanceof运算符的前一个操作数通常是一个引用类型变量，后面一个操作数通常是一个类。用于判断前面的对象是否是后面的类，后者其子类、实现类的实例。Instanceof运算符前面的操作数要么与后面的类相同，要么与后面的类具有父子继承关系，否则会引起编译错误。

## Java增强包

1. Int型自动装箱，128以下会相等，超过则不等。Inetger有一个cache数组，-128~127，若超过则会重新新建一个Integer实例。
2. \==号对于引用对象，只有它们指向同一个对象时，\==判断才返回true。\==不可用于比较类型上没有父子关系的两个对象。
3. Hello直接量和new String(“hello”)，JVM会使用常量池管理这些字符串，当调用new String（“hello”）,JVM会先使用常量池来保存“hello”直接量，再调用String类的构造器来创建一个新的String对象，会被保存在堆内存中，会产生两个字符串对象。
4. JVM常量池保证相同的字符串直接量只有一个，不会产生多个副本。

## Equals

​	正确重写equals()方法应该满足一下条件：

1. 自反性
2. 对称性
3. 传递性
4. 一致性
5. 对任何不是null的x，x.equals(null)一定返回false

## 单例类（Singleton）

​	如果一个类始终只能创建一个实例，这个类成为单例类。将构造器用private修饰。一旦把构造器隐藏起来，就需要提供一个public方法作为该类的访问点，用于创建该类的对象，且该方法必须使用static修饰。此外，还需要缓存已创建的对象，因此需要为该类使用一个成员变量保存，需用static修饰。

## 不可变类

​	Java提供的8个包装类和java.lang.String类都是不可变类。创建实例后，其实例变量不可变。

1. 使用private和final修饰符来修饰该类的成员变量
2. 提供带参数构造器，用于根据传入的参数来初始化成员变量
3. 仅为该类的成员变量提供getter方法，不要为该类的成员变量提供setter方法
4. 如果有必要，重写Object类的hashcode()和equals()方法

​	设计的时候需要考虑到如果是引用型成员变量，需要采取一些措施实现不可变类。如改写设置引用变量的方法，也改变此变量的getter方法。

### 缓存实例的不可变类

​	使用数组实现缓存

## 抽象类

​	抽象类和方法必须使用abstract修饰符来定义，有抽象方法的类只能被定义成抽象类，抽象类里没有抽象方法。

1. 抽象类和方法必须使用abstract修饰，抽象方法不能有方法体
2. 抽象类不能被实例化，无法使用new来调用构造函数
3. 抽象类可以包括成员变量、方法（普通和抽象）、构造器、初始化块、内部类（接口、枚举）

​	Final和abstract不能同时使用。Static和abstract不能同时修饰某个方法。

## 内部类

​	内部类作用：

- 内部类提供了更好的封装，可以把内部类隐藏在外部类之内，不允许同一个包中的其他类访问。
- 内部类可以访问外部类的私有数据。
- 匿名内部类适合用于创建那些仅需要一次使用的类
- 内部类比外部类可以多使用三个修饰符：private、protected、static
- 非静态内部类不能拥有静态成员

### 静态内部类

​	归属于类

### 局部内部类

1把一个内部类放在方法中定义

### Java8改进的匿名内部类

​	匿名内部类继承一个父类或实现一个接口（interfaceimp）。匿名内部类定义时会直接创建一个实例后该类定义消失。

- 匿名内部类不能定义成抽象类
- 匿名内部类中不能定义构造函数
- 如果局部变量被匿名内部类访问，则自动变为final

### Java8新增的Lambda表达式

​	Lambda表达式支持将代码块作为方法参数，Lambda表达式允许使用更简洁的代码创建只有一个抽象方法的接口。

(形参)->{代码块}

​	若代码块只有一行，可以省略花括号；若此时要求返回，可以省略rerun。还可以使用方法引用和构造器引用。Lambda表达式的类型，也称为目标类型，必须是函数式接口。函数式接口代表只能包含一个抽象方法的接口。

​	JAVA8提供了@FunctionalInterface，告诉编译器检查该接口必须是函数式接口。因为Lambda表达式接口被当成对象，可以用来赋值。Runnable r = Lambda；Runnable是一个函数式接口。两者参数要一致。

1. 将Lambda表达式赋值给函数式接口类型的变量
2. 将Lambda表达式作为函数式接口类型的参数传给某个方法
3. 使用函数式接口对Lambda进行强制类型转换（Runnable）（Lambda）

可以使用方法引用替换Lambda表达式，如：

-  Converter c1 = s->Integer.valueOf(s);
- Converter c1 = Integer::vauleOf;

引用特定对象

- Converter c2 = s->“fkit.irg”.valueOf(s);
- Converter c2 = “fkit.irg”.valueOf;

引用某类对象的实例方法

- Mytest mt = （a,b,c）-> a.substring(b,c)
- Mytest mt = String::substring;

引用构造器

- YourTest yt = （String a）->new JFrame(a);
- YourTest yt = JFrame::new;

## Enum枚举类

- 枚举类可以实现一个或多个接，默认继承java.lang.Enum类，而不是继承Object。
- 使用enum定义、非抽象的枚举类默认为final，不能派生
- 枚举类的构造器只能使用private访问控制符
- 枚举类型的所有实例必须在枚举类的第一行显示列出，否则枚举类永远不能产生实例，列出这些实例，系统会自动添加public static final

枚举类默认提供了values()方法，可以遍历所有的枚举值。Java.lang.Enum提供了几个方法

- Int compareTo(E o)
- String name(),返回名称
- Int ordinal()，返回枚举值在枚举类中的索引值
- String toString(),返回枚举常量的名称
- Public static<T extends Enum<T>> T valueOf(Class<T> enumType,String name)

枚举.values()，获取枚举类的数据。Enum实现了Comparable,Serializable接口

## 对象与垃圾回收

​	垃圾回收之前都会调用finalize(),可使对象重新复活

对象在内存中的状态：

1. 可达状态
2. 可恢复状态
3. 不可达状态

强制垃圾回收

1. System.gc()
2. Runtime.getRuntime().gc()

运行时使用-verbose:gc，可以看到每次垃圾回收时提示消息。

finalize()定义在Object类的实例方法

1. 永远不要主动调用某个对象的finalize()
2. finalize()何时被调用具有不确定
3. 当JVM执行可恢复对象的finalize()方法时，变成可达
4. 当JVM执行finalize()出现异常，垃圾回收机制不会报告异常

## 对象的软、弱和虚引用

​	Java.lang.ref包下提供三个类，SoftReference、PhantomReference和WeakReference，分别表示软引用，虚引用和弱引用。

## JAR包

三种发布方式

1. 使用平台相关编译器将整个应用编译成平台相关的可执行文件。丧失跨平台性
2. 为应用编辑一个批处理文件。
3. 将一个程序制作成可执行JAR包，会映射成javaw.exe打开，使用jar cvfe test.jar test.Test test

## 常用包

1. Java.lang:这个包下包含了Java语言的核心类，如String、Math、System和Thread类等，该包自动导入
2. Java.util：这个包包含有Java的大量工具类，例如Arrays和List、Set
3. Java.net:包含Java网络编程相关
4. Java.io:包含了一些Java输入/输出的编程相关的类/接口
5. Java.text：包含一些Java格式化相关的类
6. Java.sql:包含Java进行JDBC数据库编程相关的
7. Java.awt：包含了抽象窗口工具集的相关类/接口，用于构建GUI
8. Java.swing:包下包含了Swing图形用户界面编程，可用于构建平台无关的GUI
9. java.lang.reflect：反射机制的包

## 常用类

###  String

 两种实例化：

```java
String str = “hello”;\\保存在字符串池中
String str = new String(“hello”); \\可使用intern()方法入池
```

常用方法

- Sting(char[] value)\(char[] value,int offset,int count)\(byte[] value)
- public char[] toCharArray()
- public char charAt(int index)
- public byte[] getByte()
- length\indexOf\trim\substring\split\toUpperCase()\toLowerCase()
- boolean startWith\endWith\equals\equalsIgnoreCase()
- replaceAll(String regex, String replacement)

### System

- Getnev()\getPropeties()\getProperty
- public static void exit(int status)
- publi static long currentTimeMillis()       
- public static void arraycopy(Object src,int srcPos,Object dest,int destPos,int length)
- public static Properties getProperties()
- public static String getProperty(String key)
- System.getProperties().list(System.out)
- protected void finalize() throws Throwable

### Runtime

​	Runtime表示运行操作类，是一个封装了JVM进程的类。构造方法私有化，属于单例设计。

- public long freeMemory()\maxMemory()
- public void gc()
- public Process exec(String command) throws IOException

```java
Runtime rt = Runtime.getRuntime();
Rt.exec(“notepad.exe”)
```

​	可使用Process提供的destory关闭该任务

```java
Runtime runtime = Runtime.*getRuntime*();
System.***out\***.println(runtime.freeMemory());
Process process = **null**;
try {
 	process = runtime.exec("notepad.exe");
   } catch (Exception e) {
 // **TODO**: handle exception
 }
try {
  Thread.*sleep*(1000);
    } **catch** (Exception e) {
    // **TODO**: handle exception
    }
    process.destroy();
```

### Object

- Equals()
- finalized()
- getClass()
- hashCode()
- toString()
- super.clone()

### String\StringBuffer\StringBuilder

后面两者可变，StringBuffer是线程安全的，而StringBuilder没有实现，因而性能略高

- append()
- insert()
- reverse()
- setCharAt()
- setLength()

### Random/ThreadLocalRandom

- Random  rand = new Random(System.currentTimeMills());
- nextBoolean\Float\Int\Long\Double\Int(int n)

### BigDecimal

​	构建时不要使用double来构建，会造成精度丢失

- BigDecimal f1 = new Bigdecimal(“0.05”);
- BigDecimal f2 = new BigDecimal.valuesOf(0.01);

### Date

- date(),creat date object use current time
- date(long date)
- after()\before()\compareTo()
- ouput with format:Data date = new Date();
	         String day = String.format(“%te”,date);
	- %te,一个月的某一天
	- %tb,指定语言的月份简称
	- %tB,全称
	- %tA,星期几全称
	- %ta，星期几简称
	- %tc，全部日期和时间信息
	- %tY，4位年份
	- %tj，一年的第几天
	- %tm：月份
	- %td：一个月的第几天
	- %ty：二位年份
	- %tH：hous in 24
	- %tM：minute
	- %tS：second
	- %tF：2018-05-06
	- %tD：05/06/18
	- %tr:11:01:01 pm
	- %tT:23:01:01
	- %tR:23:01
- public void setTime(long time)
- public long getTime()

### Calendar

- public static final intYEAR\MONTH\DAY_OF_MONTH\MINUTE\SECOND\MILLSECOND
- public static Calendar getInstance()
- public Boolean after(Object when)
- public Boolean before(Object when)
- public int get(int field)

### DateFormat

​	DateFormat是一个抽象类

- public static final DateFromat getDateInstance();//日期
- public static final DateFromat getDateInstance(int style, Locale aLocale);//根据Locale得到
- public static final DateFromat getDateTimeInstance();//日期时间
- public static final DateFromat getDateTimeInstance(int dateStyle,int timeStyle,Locale aLocale)

eg:

```java
df=DateFormat.getDateTimeInstanece(DateFormat.YEAR_FIELD,DateFormat.ERA_FIELD,
new Local(“zh”,”CN”))
```

### SimpleDateFormat

- pubilc SimpleDateFormat(String pattern)
- public Date parse(String source) throws PareseException
- public final String format(Date date)

eg:(String->Date)

```java
SimpleDateFormat sd = new SimpleDateFormat(“yyyy-MM-dd HH:mm:sss.SSS”);
Date d = sd.parse(“2018-5-10 16:45:17.123”);
```

### Math

- sin(double a)\cos\tan\asin\acos\atan
- toRadians(double angdeg):change to radian
- toDegrees(double angrad)
- exp\log\log10\sqrt\cbrt\pow
- ceil(>=current smallest interger)\floor\rint(nearst,default even)\round
- max\min\abs
- random:0.00~1.00(Math.random())
- Random:Random r = new Random(seedValue);
	     nextInt()\netInt(int n)(0~n)\nextLong\nextBoolean\nextFloat\nextDouble\nextGaussian

### BegInteger

- BigInteger(String val)
- public BigInteger add(BigInteger val)\subtract\multiply\divide\max\min
- public BigInteger[] divideAndRemainder(BigInteger bal)

### BigDecimal

- BigDecimal(double/int/String val)
- add\subtract\multiply\divide

RoundingMode中的舍入方式：

1. ROUND_UP：远离零方向舍入，向绝对值最大的方向，只要舍弃位非零即进位
2. ROUND_DOWN：向零方向靠拢，所有的位都舍弃
3. ROUND_CEILING：向正无穷方向舍入
4. ROUND_FLOOR：向负无穷方向舍入
5. HALF_UP：四舍五入（5进）
6. HALF_DOWN：四舍五入（5舍）
7. HALF_EVEN：银行家算法

### NumberFormat

- public static Locale[] getAvailableLocale ();//返回所有语言环境的数组
- public static final NumberFormat getInstance()；//当前默认语言环境的数字格式
- public static NumberFormat getInstance(Locale inLocale);
- public static final NumberFormat getCurrencyInstance()；//当前默认语言环境的货币格式
- public static NumberFormat getCurrencyInstance(Locale inLocale);

### 对象克隆技术

protected Object clone() throws CloneNotSupportedException

​	必须要实现Cloneable接口，需要复写此方法，只能在方法中调用父类的clone()方法。

### Arrays类

- public static Boolean equals(int[] a , int[] a2)
- fill(int[] a,int val)\sort(int[] a)\binarySearch(int[] a, int key)\toString(int[] a)

### Comparable接口

​	需要实现Comparable接口，复写compareTo方法，才能使用java.util.Arrays.sort(对象数组)进行排序，若没有实现则会报错，ClassCaseException，因为在排序时，所有对象都向Comparable进行转换。

### Comparator

​	如果一个类已经开发完成，但建立初期没有实现comparable接口，此时无法排序，个Compator,在java.util包下。此时需要指定好一个比较器的比较规则类才可以完成数组排序。

eg：

```java
public class StudentComparator implements Comparator<Student>{
    public int compare(Student s1,Student s2){}
}
```

排序：java.util.Arrays.sort(stu,new StudentComparator())

中文排序：Comparator c = Collator.getInstance(Locale.CHINA);

## 获取系统时间

1. Date
2. Calendar
3. Java8专门新增一个java.time包

```java
Date date = new Date();
		Timestamp CUR_time = new Timestamp(date.getTime());
```

## 定时调度

### Timer类

​	Timer类是一种线程设施，可实现在某一段时间或某一段时间后安排某一个任务执行一次或者定期重复执行。需与TimeTask配合。

- public Timer()-启动
- public void cancel()-终止，放弃已安排的任务，对正在执行的任务没有影响
- public int purge-将所有已移除的任务移除，释放内存空间
- public void schedule(TimeTask task,Date time):安排一个任务在指定时间执行，若超过则立即执行
- public void schedule(TimeTask task,Date firstTime,long period):repeat
- public void schedule(TimeTask task,long delay)
- public void schedule(TimeTask task,long delay,long period)
- public void scheduleAtFixedRate(TimeTask task,Date firstTime,long period)
- public void scheduleAtFixedRate(TimeTask task,long delay,long period)

### TimerTask类

- public void cancel()
- public void run()-引入接口Runnable，需要复写
- public long scheduled ExectionTime()-返回最近一次要执行任务的时间，若正在执行，返回安排时间，一般在run中调用，判断当前是否有足够时间执行完成该任务

```java
public void MyTask extends TimerTask{
    public void run(){ }
}
Timer t = new Timer();
MyTask mytask = new MyTask();
t.schedule(mytask,1000,2000);
```

## 正则表达式

1. Boolean matches(String regex)
2. String replaceAll(String regex, String replacement)
3. String replaceFirst()
4. String[] split(String regex)

​	正则表达式所支持的合法字符，x 字符x，可代表任何合法字符

- $匹配一行的结尾
- ^匹配一行的开头

### Pattern类和Matcher类

两个类都在java.util.regex中定义。

常用正则规范：

- \\ \t \n [abc] [\^abc] [a-zA-Z0-9] \d（数字） \D（非数字） 
- \w(字母、数字、下划线) \W \s(所有空白字符，换行、空格) \S
- $行的结尾 ^行的开头 .匹配除换行符之外的任意字符

数量表示：

- X-必须出现一次 X?(0或1次) X*(0或1或多次) X+(1或多次)
- X{n}出现n次， X{n，}必须出现n次以上 X{n,m}出现n~m次

逻辑运算符

- XY，X规范后跟着Y规范
- X|Y，X或Y规范
- (X)作为一个捕获组规范

Pattern常用方法，要取得Pattern实例必须调用compile()方法

- public static Pattern cmpile(String regex);
- public Matcher matcher(CharSequence input);
- public String[] split(CharSequence input);

Matcher类的常用方法

- public Boolean matches()
- public String replaceAll(String replacement)

```java
 	String string = "1990-10-28";
    Pattern pattern = Pattern.*compile*("-");
    Matcher matcher = pattern.matcher(string);
    System.***out\***.println(matcher.matches());
    String[] strings = pattern.split(string); 
    String strings2= matcher.replaceAll("%");
    for(String x:strings) {
      System.***out\***.println(x);
    }
    System.***out\***.println(strings2);
```

### String对正则表达式的支持

- public Boolean matches(String regex)
- public String replaceAll(String regex, String replacement)
- public String[] split(String regex)

## 集合

​	当使用Iterator迭代访问Collection集合元素时，元素不能改变，只有通过Interator的remove方法删除上一次next方法返回的集合元素才可以。

​	Interator迭代器采用的快速失败机制，一旦迭代过程中检测到该集合已经被修改，程序立即引发ConcurrentModificationException，而不是现实修改后的结果。

可使用如下删除：

```java
Iterator<type> it = list.iterator();
While(it.hasNext){
  Type x = it.next();
  If(){it.remove();}
}
```

Collection

- Set
	- HashSet
	- TreeSet
- List
	- LinkedList
	- ArrayList
- Queue
- Map
	- HashMap
	- TreeMap

### Collection

- Add(E o)/addAll(Collection <? Extends E>c)/clear()
- contains(Object o)/containAll(Collection<?> c)/equals()/hashCode()/isEmpty()
- Iterator<E> iterator()/remove(Object o)/removeAll(Collection<?> c)
- retainAll(Collection<?> c)/size()/toArray()/public <T> T[] toArray(T[] a)

### List

- indexOf()-取得重复数据的第一个
- lastIndexOf()-取得重复对象最后一次出现的索引位置
- ArrayList：实现可变长数组，可包含null，可快速随机访问，insert or delete教慢，需要后面所有数据移位
- Iterator\ListIterator
- List接口中的subList()方法为返回一个列表的子视图，所有的改变作用于原字符串:List.subList(20,30).clear()

### Set

不包含重复对象

### Map

- put\containKey\containValue\get\keySet\values

###  Java8新增的Predicate操作集合

Java8为Collection集合新增removeIf(Predicate filter)方法，Predicate也是函数式接口，可以使用Lambda作为参数。

### Collections

addAll()/reverse()/replaceAll()/sort()/swap()/EMPTY_LIST/EMPTY_SET/EMPTY_MAP

### Stack

Empty()/peek()/pop()/push()/search()

### Properties

属性类Properties为Hashtable的子类

GetProperty()/setProperty()/list()/load()/loadFromXML/store()/storeToXML()

###  运算

- 并集：list.addAll(list2)
- 交集：list.retainAll(list2)
- 差集：list.remove(list2)
- 无重复的并集：list2.removeAll(list1)  list1.addAll(list2)
- Collections.shuffle()，打乱列表

## StringBuilder

- StringBuilder相较于“+”，效率高，不会重复创建
- StringBuilder（速度快）与StringBuffer(线程安全)
- Append()\insert()\delete()\reverse()

## 泛型

​	允许在定义类、接口、方法时使用类型形参。不管泛型的类型形参传入哪一种类型实参，对于Java来说，依然被当成一个类处理，因而静态方法、静态初始化块或静态变量的申明和初始化都不允许使用类型形参。

- 受限泛型通配符：list<?extends Shape>
- 下限通配符<? Super Type>

1. 对于泛型如果实例化时不指定，则擦除泛型，使用Object表示，出现警告
2. 使用通配符？可使用任意的泛型对象，但如果使用？接受泛型对象，则不能设置被泛型指定的内容
3. 子类的泛型是无法使用父类的泛型，如Info<String>不能使用Info<Object>接收
4. 泛型可以使程序的操作更加安全，可以避免发生类转换异常

### 泛型方法与通配符区别

​	类型通配符既可以在方法签名中定义形参的类型，也可以用于定义变量的类型；但泛型的类型形参必须在对应方法中显式申明。

### 泛型接口

​	两种实现方式：

1. 在子类的定义上声明泛型类型

	```java
	 interface Info<T>;
	 class InfoImp<T> implements Info<T>;
	 Info<String> I = new InfoImp<String>(“LDC”);
	```

2. 直接在接口中指定泛型

	```java
	 interface Info<T>;
	 class InfoImp implements Info<String>;
	 Info<String> I = new InfoImp(“LDC”);
	```

### 泛型数组

​	使用泛型方法时，传递或者返回一个泛型数组：public static <T> T[] fun(T parm[])

## 国际化程序

​	国际化需要依托，java.util.Locale,表示一个国家语言类

- java.util.ResourceBundle：用于访问资源文件
- java.text.MessageFormat:格式化资源文件的占位字符串

### Locale

- public Locale(String language)
- public Locale(String language, String country)
- zh-CN\en-US\fr-FR

### ResourceBundle

ResourceBundle类主要是用来读取属性文件，读取文件时指定名称，无需后缀.properties

- public static final ResourceBundle getBundle(String baseName)
- public static final ResourceBundle getBundle(String baseName, Locale locale)
- public final String getString(String key)

```java
Message.properties: info=hello
ResouceBundle rb = ResouceBundle.getBundle(“Message”);
rb.getString(“info”);
```

### 国际化程序实现

```java
Message_zh_CN.properties:不支持中文，需要转化为Unicode，使用“native2ascii.exe”转换
Message_en_US.properties
Message_fr_FR.properties
Local zhLoc = new Local(“zh”, “CN”);
Local enLoc = new Local(“en”, “US”);
Local frLoc = new Local(“fr”, “FR”);
ResouceBundle zhrb = ResouceBundle.getBundle(“Message”,zhLoc);
ResouceBundle enrb = ResouceBundle.getBundle(“Message”,enLoc);
ResouceBundle frrb = ResouceBundle.getBundle(“Message”,frLoc);
zhrb.getString(“info”);
zhen.getString(“info”);
zhfr.getString(“info”);
```

### MessageFormat处理动态文本

​	Fromat中派生MessageFormat、DateFormat、NumberFormat。info=hello,{0}!,用占位符表示。读取后需要使用MessageFormat处理，主要使用一下方法：

```java
public static String format(String pattern, Object … argument)
MessageFormat.format(str,{0},{1},{2}…)
stren = zhen.getString(“info”);
MessageFormat.format(stren,”LDC”)
```

### 使用类文件代替资源文件

1. 必须继承java.util.ListResourceBundle类
2. 需要复写gtContent()方法
3. 属性是个二维数组，key-value
4. 若同时出现Message_zh_CN.class > Message_zh_CN.properties > Message.properties

## 异常处理

- Java异常包括两种，checked异常和runtime异常
- 把所有非正常的情况分为两种：异常Exception和错误Error。
- RuntimeException可以不处理，其他Exception需要处理。

### 常见的异常类之间的继承关系

- IndexOutOfBoundsException
- NumberFormatException
- ArithmeticException

### 访问异常信息

- getMerssage():返回该异常的详细描述字符串
- printStackTrace():将该异常的跟踪栈信息输出到标准错误输出
- printStackTrace(PrintStream s):输出到指定流
- getStackTrace()：返回该异常的跟踪栈信息

### finally

​	异常处理代码中使用return不会退出finally语句，而System.exit(1)语句退出虚拟机则会。不要在finally块中使用如return或throw等导致方法终止的语句。

### Java7的自动关闭资源的try语句

​	Try关键字后面紧跟圆括号，声明、初始化一个或多个资源，此处的资源是指那些在程序结束时显式关闭的资源（如数据库连接、网络连接等）。Try语句会在该语句结束时自动关闭这些资源。

​	为保证try可以正常关闭资源，这些资源必须实现AutoCloseable或Closeable接口，实现close()方法。

## 断言

assert boolean 表达式；如果要使断言语句生效，需要使用-enableassertions参数，简写为-ea

java –ea Test

## AWT(Abstract Window Tookit)

### AWT容器

​	Container是Component的子类。AWT主要提供两种主要的容器类型。

- Window：可独立存在的顶级窗口
- Panel：可作为容器容纳其他组件，不能独立存在，必须被添加到其他容器中

## Annotation

### @Override

强制编译器校验重写

### @Deprecated

表明该方法、类过程，其他程序访问时编译器会警告

### @SuppressWarnings

抑制编译器警告

```java
@SuppressWarnings({“unchecked”, “deprecation”})
@SuppressWarnings(value={“unchecked”, “deprecation”})
```

- Deprecation、unchecked
- Fallthrough:switch操作case后未加break
- Path：设置错误的文件路径、源文件路径
- Serial：可序列化类上缺少serialVersionUID
- Finaly：任何finally子句不能正常完成时候的警告
- All

### @SafeVarags

堆污染警告

### @FunctionalInterface

规定该接口是函数式接口

### 自定义

使用@interface相当于继承了Annotation接口

```java
Public @interface MyDefaultAnnotationMoreParm{
     Public String key() default “”;    
	 Public String value();
}
@MyDefaultAnnotationMoreParm(key = “”, value = “”)
Public @interface MyDefaultAnnotationArrayParm{
    Public String[] value();
}
```

### Retention和RetentionPolicy

Retention定义中存在一个RetentionPolicy变量用于指定Annotation范围。

- SOURCE：保留在源文件中*.java，override、supresswarinings*
- CLASS：保留在源文件和变异之后的类文件*.class
- RUNTIME：还会加载到JVM中，deprecated

### 通过反射取得Annotation

```java
Class<?> c = null;
C = Class.forName(“”);
Method toM = c.getMethod(“methodname”);
If(toM.isAnnotationPresent(className.class)){
    className mda = null;
    mda = toM.getAnnotion(className.class);
    String key = mda.key();
}
```

### @Target

使用@Target限定Annotation的使用位置。

ElementType[]枚举类型，如ANNOTATION_TYPE，用在注释的申明中。

### @Documented

自定义的Annotation可通过@Documented注释，生成javadoc的时候可以通过@Documented将一些文档的说明信息写入。

```java
@Documented
Public @interface AnnotationName{}
```

生成文档：javadoc -d doc SimpleBeanDocumented.java

### @Inherited

标注父类的注释是否可以被子类所注释。需要被继承则添加。

## 多线程

- New状态表示刚刚创建的线程，这种线程还没有开始执行 
- RUNNABLE:当线程创建好之后，调用线程的start方法就会进入就绪状态。 
- BLOCKED:当线程运行过程如果遇到了Syschronized就会进入阻塞状态。 
- TIMED_WATING:表示等待状态，有时间限制的（sleep） 
- WAITING:表示进入一个无时间限制的等待状态（wait） 
- TERMINATED：表示结束状态

创建线程：

- 实现Runnable接口
- 继承Thread类

推荐使用第一种，原因：

1. Thread仅支持单继承
2. 创建大量的Thread类，开销较大
3. 5.0中新增的很多简化线程的类是使用Runnable接口的

- Start-就绪-阻塞-运行-终止

不能直接使用run()启动进程，需要使用Thread类的start()启动，实际是调用

private native void start0();

操作系统的函数，若多次调用，会报异常：IllegalThreadStateException

一个JAVA程序启动至少启动两个线程：1）main；2）垃圾收集线程

- suspend()-挂起
- resume()-恢复挂起
- stop()-停止线程

以上三种均不推荐，可能造成死锁，可使用加入标志，stop中修改该标志

### Thread

Thread也实现了Runnable接口，位于java.lang。可通过继承Thread来启动新线程。

Thread.State

(new thread)-New-(start())->Runnable-(run()finished or exception)->TERMINATED

Runnable: 

- BLOCKED-得到和等待锁
- TIMED_WATTING-休眠状态-sleep()
- WATTING-等待状态-wait()

1. Thread(Runnable target)
2. Thread(Runnable target, String name)
3. public static Thread currentThread()
4. getName()\getPriority()\isInterrupted()\isAlive()
5. run()\start()\setName()\setPriority()\toString()\sleep()
6. public final void join() throws Interrupted Exception;等待线程死亡，线程强制执行
7. public final sychronized void join(long millis) throws Interrupted Exception;等待millis，线程死亡
8. public static void yield();将正在执行的线程暂停，允许其他线程执行
9. public final void setDaemon(Boolean on)：将一个线程设置成后台运行
10. threada.join()-强制执行

### 1.11.2 Runnable

该接口中只有run()方法。

- 新建类，实现Runnable接口，复写run()
- 实例化类对象
- 使用Thread类的构造方法创建Thread对象
- 调用Thread类的start()方法来运行新线程

### Attribute

1. Priority: gePriority()\setPriority()
2. nameAttribute:getName()\setName(),default:Thread-number
3. Id:gitId(),Class thread donnot support modify method of id
4. daemon thread
5. state:getState()-Thread.currentThread().getState()

### Control

1. Sleep:public static void sleep(long millis, int nanos)throws InterruptedException
2. Join: public final void join(long millis, int nanos)throws InterruptedException,with no para;with long millis
3. Stop:Though thread support stop() method ,but it’s not safe.Recommand use Boolean flag to control

### Sychronized

1. Sychronized method:expand much resource,not recommended\
2. Sychronized block:
3. Volatile:notify JVM the para may be modify,need be checked
4. ReentrantLock(可重入锁):Java.util.concurrent-ReentrantLock()\lock()\unlock()


```java
  lock.lock()
  try{}
  finally{lock.unlock()}
```

5. AtomicPara:java.util.concurrent.atomic,AtomicInteger(int initialValue)\getAndIncrement()

### Callable

Java.util.concurrent包含高并发

@FunctionalInterface

```java
public interface Callable<V> {
   V call() throws Exception;
}
```

### Priority

- public static final int MIN_PRIORITY- 1
- public static final int NORMAL_PRIORITY- 5
- public static final int MAX_PRIORITY- 10

### Sychronized

- 同步代码块

	```java
	synchronized(同步对象){
	    需要同步的代码；
	}
	```

- 同步方法
	- 死锁
	- wait()
	- notify()-唤醒第一个等待的线程
	- notifyAll()-唤醒所有等待的线程

## 类加载机制与反射

![img](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5894b04c0c3a4481936fb18c372b9cd1~tplv-k3u1fbpfcp-zoom-1.image)

正常方式：引入需要的“包.类”名称->通过new实例化->取得实例化】

反射方式：实例化对象->getClass()方法->得到完整的包.类名称

Class的常用方法：forName()/getConstructors()/getDeclaredFields()(单独定义的所有属性)/getFields()(继承而来的全部属性)/getMethods()/getMethod()/getInterfaces()/getName/getPackage()/getSuperclass()/newInstance()/getComponentType()/isArray()

### Class实例化

1. Class.forName(“java.util.lang”);
2. New X().getClass();//需要明确的类
3. X.class();

每个类的Class对象只有一个。

### 构造类的实例化对象

1. Class对象调用newInstance()方法

   ```java
   Class.forName("...").newInstance()
   ```

2. Constructor构造器调用newInstance()方法

   ```java
   Constructor constructor Class.forName("...").getConstrucor(para.class);
   constructor.setAccessible(true);
   constructor.newInstance(para);
   ```

### 获取类中的变量（Field）

- Field[] getFields()：获取类中所有被`public`修饰的所有变量
- Field getField(String name)：根据**变量名**获取类中的一个变量，该**变量必须被public修饰**
- Field[] getDeclaredFields()：获取类中所有的变量，但**无法获取继承下来的变量**
- Field getDeclaredField(String name)：根据姓名获取类中的某个变量，**无法获取继承下来的变量**

### 获取类中的方法（Method）

- Method[] getMethods()：获取类中被`public`修饰的所有方法
- Method getMethod(String name, Class...<?> paramTypes)：根据**名字和参数类型**获取对应方法，该方法必须被`public`修饰
- Method[] getDeclaredMethods()：获取`所有`方法，但**无法获取继承下来的方法**
- Method getDeclaredMethod(String name, Class...<?> paramTypes)：根据**名字和参数类型**获取对应方法，**无法获取继承下来的方法**

### 获取类的构造器（Constructor）

- Constuctor[] getConstructors()：获取类中所有被`public`修饰的构造器
- Constructor getConstructor(Class...<?> paramTypes)：根据`参数类型`获取类中某个构造器，该构造器必须被`public`修饰
- Constructor[] getDeclaredConstructors()：获取类中所有构造器
- Constructor getDeclaredConstructor(class...<?> paramTypes)：根据`参数类型`获取对应的构造器

每种功能以Declared细分为2类：

Declared可获取内部包含的所有变量、方法和构造器，但是无法获取**继承下来的信息**

无Decleared可获取该类的public修饰的变量、方法和构造器，可获取继承下来的信息

父类的属性使用protected修饰，利用反射是无法获取到的

### 获取注释

**反射中，Field，Constructor 和 Method 类对象都可以调用下面这些方法获取标注在它们之上的注解。**

- Annotation[] getAnnotations()：获取该对象上的**所有注解**
- Annotation getAnnotation(Class annotaionClass)：传入`注解类型`，获取该对象上的特定一个注解
- Annotation[] getDeclaredAnnotations()：获取该对象上的显式标注的所有注解，无法获取`继承`下来的注解
- Annotation getDeclaredAnnotation(Class annotationClass)：根据`注解类型`，获取该对象上的特定一个注解，无法获取`继承`下来的注解

只有注解的`@Retension`标注为`RUNTIME`时，才能够通过反射获取到该注解，@Retension 有`3`种保存策略：

- `SOURCE`：只在**源文件(.java)**中保存，即该注解只会保留在源文件中，**编译时编译器会忽略该注解**，例如 @Override 注解
- `CLASS`：保存在**字节码文件(.class)**中，注解会随着编译跟随字节码文件中，但是**运行时**不会对该注解进行解析
- `RUNTIME`：一直保存到**运行时**，**用得最多的一种保存策略**，在运行时可以获取到该注解的所有信息

### 调用类中的方法

- C = Class.forName(“java.util.***”);//实例化class对象
- Obj = c.newInstance();//实例化操作对象，对象中需要有无参构造函数
- Met = obj.getClasss().getMethod(methodName,methodParameter1Type,…);//获取参数方法
- Met.invoke(obj, methodParameter1,…);//调用方法，如果**调用静态方法则参数1传入参数null**

### 操作属性

1. 可以通过调用类中的getter、setter方法

2. 直接通过Field类中的提供的set、get方法，由于类中属性大多数为private，需要使用setAccessible(true)方法将需要操作的方法设置为可以外部访问

	```java
	C = Class.forName(“org.***”);
	Org = c.getInstance();
	nameField = c.getDeclaredField(“name”);
	nameFiled.setAccessible(true);
	nameField.set(org, “nameValue”);
	```

### 使用反射操作应用类

### 类的生命周期

- 装载：通过类加载器把.class二进制文件装入JVM的方法区，并在堆区创建描述该类的java.lang.Class对象
- 链接：把二进制数据组装成可以运行的状态
	- 校验：确认该文件是否适合当前的JVM版本
	- 准备：为静态成员分配内存空间，设置默认值
	- 解析：转换常量池的代码为直接引用的过程，直到所有的符号引用都可被运行程序使用
- 初始化
- 对象实例化
- 垃圾收集
- 对象终结
- 卸载

### 动态代理

​	需要java.lang.reflect.InvocationHandle接口和java.lang.reflect.Proxy类的支持。

```java
Public interface InvocationHandle{
    Public Object invoke(Object proxy, Method method, Object[] args) throws Throwable
}
```

​    Proxy类专门完成代理的操作类。

```java
Public static Object newProxyInstance(ClassLoader loader, Class<?>[] interfaces,
InvocationHandle h) throws IllegalArgumentException
```

​	ClassLoader

- BootStrap ClassLoader：C++编写
- Extension ClassLoader：用来进行扩张类的加载，对应jre\lib\ext目录中的类
- APPClassLoader：加载classpath制定的类

```java
class MyInvocationHandler implements InvocationHandler{
    private Object target;
    public Object bind(Object target){
        this.target = target;
        return Proxy.newProxyInstance(target.getClass().getClassLoader(), target.getClass().getInterfaces(), this);
    }
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        Object returnResult = method.invoke(this.target, args);
        return returnResult;
    }
}
```

### 反射的优势和曲线

#### 优点

- 增加程序的灵活性

#### 缺点

- 破坏类的封装性
- 性能损耗

## JVM

.java->javac->.class->JVM(指令集、寄存器、类文件格式、栈、垃圾回收堆、存储区)

## 文件

- File.separator(分隔符，根据操作系统决定是使用/或者\)
- File.pathSeparator(表示路径的分隔符，如  ；)
- New file(path);
- OutputStream
	- FileOutputStream
	- ByteArrayOutputStream
	- PipedOutputStream
	- FilterOutputStream
		- PrintStream
	- ObjectOutputStream
		- DateOutputStream
		- DeflaterOutputStream
			- ZipOutputStream

- InputStream
	- FileInputStream
	- ByteArrayOutputStream
	- PipedInputStream
	- FilterInputStream
		- DataInputStream
	- SequenceInputStream
		- InflaterInputStream
			- ZipInputStream
	- ObjectInputStream
	- PushbackInputStrea
- Writer
	- OutputStreamWriter
		- FileWriter
	- PrintWriter
- Reader
	- InputStreamReader
		- FileReader
	- BufferedReader
	- FilterReader
		- PushbackReader

### 取得文件信息

- 判断是否为文件：public Boolean isFile();
- 判断是否为目录：public boolean isDirectory()
- 列出该集目录下所有文件：public [File](https://docs.oracle.com/javase/8/docs/api/java/io/File.html)[] listFiles()    

### 常用方法

- getName()
- canRead()
- canWrite()
- exist()
- length()
- getAbsolutePath()
- getParent()
- isHidden()
- Long lastModified()
- listRoots()
- mkdir()
- renameTo()

### InputStream

```java
InputStream io = System.in;
byte[] b = new byte[1024];
System.out.println("Please input!");
io.read(b);
String input = **new** String(b);
System.out.println("Input is:"+input.trim());
```

### OutputStream

```java
OutputStream io = System.*out*;
byte[] bs ="Hello World!\n".getBytes();
io.write(bs);
```

### 文件输入流

- FileInputStream(File file);
- FileInputStream(String path);

### 文件输出流

FileOutputStream

```java
 File file = new File("D:\\test.txt");
     if(!file.exists()) {
        file.createNewFile();
      }
      FileOutputStream io = new FileOutputStream(file);
      byte[] bs = new String("Heollo everbody!!!!").getBytes();
      io.write(bs);
```

### 缓存输入流

```java
bufferedInputStream(inputStream in);
bufferedinputStream(inputStream in, int size);
```

### 缓存输出流

BufferedoutputStream

### 数据输入流

DateInputStream(inputStream in)

- readBoolean()
- readByte()
- readChar()
- readInt()
- readFloat()
- readUTF()

### 数据输出流

DateOutputStream

### 字符输入流

Reader-InputStreamReader

- close()

+ mark()
+ read()
+ reset()

FIleReader，Window换行符为\r\n，linux为\n

### 字符输出流

Writer-PrintWriter

- close()
- flush()
- write()

FileWriter

### 管道流

PipedInputStream/PipedOutputStream

若要进行管道输出，则必须把输出流连接到输入流上，如：

PipedOutputStream类中Public void connect(PipedInputStream snk) throws IOExcetpiton

### Scanner

1. Scanner(File source)
2. Scanner(String source)
3. Scanner(InputStream source)

- findInLine()
- nextInt()
- nextShort()
- nextFloat()

```java
	String input = new String("2 018-hleoolo-98912-gsgsgs-1234-agag8788");
    Scanner scanner= new Scanner(input);
    scanner.findInLine("\\d");
    MatchResult result = scanner.match();
    for(int i =0;i<result.groupCount();i++) {
      System.println(result.group(i));
    }
    scanner.close();
```

### System中常量

- Public static final PrintStream out\err;
- Public static final InputStream in;

重定向方法

- Public static void setOut(PrintStream out);
- Public static void setErr(PrintStream err);
- Public static void setIn（InputStream in）；

## 编码

### ISO8859-1

单字节编码，0~255

### GBK/GBK2312

中文的国际编码，双字节编码，GBK可以表示简体中文和繁体中文，二GB2312只能表示简单中文。

### Unicode

16进制表示编码

### UTF

UTF兼容IOS8859-1，为不定长编码，每个字符长度为1~6个字节不等，一般在中文网页使用。

### 本机编码

System.getProperty(“file.encoding”);

指定编码

Byte b[] = “中国，你好”.getBytes(“IOS8859-1”);

## 序列化

### Serializable

- 需要实现Serializable接口（java.io.Serializable-标识接口）
- 实现序列化接口的对象可以经过而二进制数据流进行传输，需要依靠对象输出流和输入流。（ObjectOutputStream、ObjectInputStream）。只有属性被序列化。
- 序列化和反序列化若JDK版本不一致，则会造成异常。当实现java.io.Serializable没有显式定义serialVersionUID，则会自动生成该long变量。

1. 序列化：ObjectOutputStream，writeObject
2. 反序列化：ObjectInputStream，readObject

### Externalizable

指定序列化的内容，实现该接口。需要复写以下方法。

- WriteExternal(ObjectOutput out)
- readExteranl(ObjectOutput in)

### Transient

使用transient申明可以不序列化

## 网络编程

TCP(Transmission Control Protocol)

UDP(User Datagram Protocol)

### InetAddress

java.net.InetAddress

- getByName()-返回InetAddress
- getHostAddress()
- getHostName()
- get()
- getLocalHost()-返回InetAddress
- toString()

### ServerSocket

1. ServerSocket();
2. ServerSocket(int port);
3. ServerSocket(int port, int backlog);

构造会抛出IOException

- accept(),返回Socket
- isBound()
- getInetAddress()-返回InetAddress
- isClosed()
- bind()
- getLocalPort()

### Socket

1. Socket(String host,int port);
2. Socket(InetAddress address,int port);

- getInetAddress()-return InetAddress
- getPort()
- getLocalAddress()
- close()
- getInputStream()
- getOutputStream()

###  UDP

### URL

Uniform Resource Locator，统一资源定位符

- Public URL(String protocol, String host, String file) throws MalformedURLException
- Public URLConnection openConnection() throws IOException
- Public final InputStream openStream() throws IOException

###  Encode\Decode

Public static String encode(String s, String enc) throws UnsupportedEcodingException

##  设计模式

###  工厂模式

### 代理模式

### 适配器模式

### 观察者设计模式

​	在java.util包中提供了Observable类和Observer接口，可完成观察者模式。被观察者需要继承Observable类。

- public void addObserver(Observer o)
- public void deleteObserver(Observer o)
- public void setChanged()
- public void notifyObserver(Object arg)       

每一个观察者需要实现Observer接口，Observer接口定义如下：

```java
public interface Observer{
   void update(Observer o, object arg);
}
```

## Logger

### 创建Logger对象

- Static Logger getLogger(Sting name)：为指定子系统查找或创建一个logger。

- Static Logger getLogger(Sting name，String resourceBundleName)：为制定子系统查找或者创建一个logger。

### Logger级别

| SEVERE  | 严重                 |
| ------- | -------------------- |
| WARNING | 警告                 |
| INFO    | 信息                 |
| CONFIG  | 配置                 |
| FINE    | 良好                 |
| FINER   | 较好                 |
| FINEST  | 最好                 |
| ALL     | 开启所有级别日志记录 |
| OFF     | 关闭所有级别日志记录 |

Logger的默认级别为info。登记在jre的lib的logging.properties文件中

```java
java.util.logging.ConsoleHandler.level = INFO
java.util.logging.ConsoleHandler.formatter = java.util.logging.SimpleFormatter
```

### Handle

​	Handle对象从Logger中获取日志信息，并将这些信息导出。

- java.util.logging.Handler 
- java.util.logging.MemoryHandler 
- java.util.logging.StreamHandler 
- java.util.logging.ConsoleHandler 
- java.util.logging.FileHandler 
- java.util.logging.SocketHandler

​    可通过setLevel(Level.OFF)来禁用Handle，并可通过执行适当级别的setLevel来重新启用。Handle类通常使用LogManager属性来设置Handle的Filter、Formatter和Level的默认值。

### Formatter

- java.util.logging.Formatter
- java.util.logging.SimpleFormatter
- java.util.logging.XMLFormatter

​    每个日志记录Handler都有关联的Formatter。Formatter接收LogRecord，并将它转换为一个字符串。
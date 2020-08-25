# Scala

## Scala与Java关系

- 扩展Java
- SDK依赖于JDK之上运行
- Scala推崇函数式编程，偏函数、函数柯里化、高阶函数，函数是Scala的一等公民
- 从底层看，所有的Scala代码实际上是Java类\接口的包装
- Scala和Java8编译器都出自马丁.奥德斯基
- Scala将OOP思想发挥了极致。基本数据类型也属于对象
- Java由两个命令:javac和java，对应Scala为scalac和scala

## Scala语言特点

- 静态类型编程语言，先编译后在JVM上运行
- 多范式编程，支持面向对象和函数式编程

## 基本语法

- 区分大小写
- 类名：对于所有的类名的第一个字母要大写
- 方法名称：所有的方法名称的第一个字母要小写
- 程序文件名：程序文件的名称应该与对象名称完全匹配（新版本不需要）
- def main(args:Array[String])，主函数运行在伴生对象Object中

## 包 

### 定义包

package

### 引用

Import语句可以出现在任何地方，而不是只能在文件顶部。如果想要引入包中的几个成员，可以使用selector（选取器）

- import java.awt.Color
- import java.awt._
- import java.awt.(Color, Font)
- import java.util.{HashMap=>JavaHashMap}//重命名成员
- import java.util.{HashMap=>_,_}//引入util包的所有成员，但是HashMap被隐藏了

注意：默认情况下，Scala总会引入java.lang._、scala._、Predef._。

## Scala数据类型

- Byte：-128到127
- Short：-32768到32767
- Int
- Long
- Float
- Double
- Char
- String
- Boolean
- Unit
- Null：scala.Null
- Nothing：任何其他类型的子类型
- Any：所有其他类的超类
- AnyRef：所有引用类的基类

### 符号字面量

‘x’被映射成预定义的Symbol的实例，scala.Symbol(“x”)

### 字符字面量

用单引号定义‘a’、‘\u0041’、‘\n’

### 字符串字面量

用双引号定义，多行字符串用三个双引号表示

## Scala变量

### 变量申明

- 变量：var
- 常量：val

### 变量类型申明

val\var VariableName : DataType [= Initial Value]

在scala中声明变量一定需要赋值，但是可使用_表示这个属性默认值

## 访问修饰符

- Private：比java严格，仅在成员定义的类或对象内部可见
- Protected：比java严格，只允许保护成员在定义了该成员的类的子类中被访
-  作用域保护：private[x]、protected[x]，这个成员处理对[…]中的类或[…]的包中类及他们的伴生对象可见外，对其他所有类都是private。

## 运算符

取消了++和--运算符，移除了?:

- 算数运算符：+、-、*、/、%
- 关系运算符：==、!=、>、<、>=、<=
- 逻辑运算符：&&、||、！
- 位运算：&、|、^、~、<<、>>、>>>

## 操作流程

取消了switch分支

```scala
for(i<-arr)println(i)
for(i<-1 to 10)println(i)
for(i<-1 until 10)println(i)
```

根据数组下标迭代元素,indices本质上是一个对0 until length的一个包装

```scala
for(index<-arr.indices)println(s"第${index}个位置;${arr(index)}")
```

没有continue，取而代之是循环守卫概念

```scala
//输出0-10以内的偶数。
for(i <- 0 to 10 if i % 2 ==0) { println(i)}
```

```scala
for{i <- arr.indices
    j <- 0 until arr.length-i-1
    if arr(j) > arr(j+1)
   }{
    var temp = arr(j)
    arr(j) = arr(j+1)
    arr(j+1) = temp
}
```

使用Range类进行跨步长迭代，构造区间为[start,end)

```scala
Range(start:Int,end:Int,step:Int)
//即通过起始下标，终止下标，步长来生成一个等差为step的数列。注意，生成的序列中不包含end，但是包含start。
```

利用yield收集数据

```scala
//将每一个元素i装载到list内。
val ints: immutable.IndexedSeq[Int] = for(i <- Range(1,10,3)) yield i
```

取消了break，可使用主动抛异常的方式实现中断

```scala
import scala.util.control.Break._
//breakable是一个控制抽象。内部执行一个()=>Unit的函数。
breakable(() => {
  var i = 0
  while (i < 10) {
    if (i == 5) break()
    i += 1
  }
}
)
```



## 方法和函数

Scala方法(def)是类的一部分，而函数(val)是一个对象可以赋值给一个变量，其实是继承了Trait类的对象。

```scala
//定义函数的完整写法。
def function(p: ParamType) : ReturnType = {...}

//定义函数时，允许省略返回值类型。
//这不代表不返回值，而是取决于此函数最后一个可以返回值的语句。
def function(p : ParamType) = {...}

//注意，这是空括号函数。
def function() = {...}

//注意，这是无参数函数。
def funtion = {...}

//这个函数允许有多个参数列表。（这种声明与函数柯里化相关。）
def function()()() ={...}
```

- 方法定义：def functionName ([参数列表]):[return type]={
	   function body
	   return [expr]
	 }
- 函数传名调用：使用=>符号来设置传名调用
- 指定函数参数名：指定函数参数名，不需要按照顺序向函数传递参数
- 可变参数：def printStrings(args:String*)
- 递归函数：函数可以调用自身
- 默认参数值：可以为函数参数指定默认值，可以不传递参数，若传递则会取代默认值
- 高阶函数：操作其他函数的函数，def apply(f:Int=>String,v:int)=f(v)
- 函数嵌套：函数类定义函数，局部函数
- 匿名函数：箭头左边是参数列表，右边是函数体。Val inc=(x:Int)=>x+1
- 偏应用函数：不需要提供函数需要的所有参数，只提供部分，或不提供所需参数，使用_替换确实的参数列表，val logWithDateBound=log(date,_:String)
- 函数柯里化：是指将原来接受两个参数的函数编程新的接受一个参数的函数的过程。新的函数返回一个以原有第二个参数为参数的函数。
- scala允许定义函数的时候省去具体的返回值类型，scala会使用类型推断来判断函数的返回值。
     - 若scala知道此函数具备返回值，但是无法推断详细类型，则默认返回值为Any
     - 若scala认为此函数不具备返回值，则默认返回值为Unit
     - scala参数列表写法：参数名在前，：隔开，参数类型在后
     - scala不需要显示地使用return关键字表示返回值

### 可变参数

Java中可变参数实际上是一个数组。

可用数组为不定参数赋值，但是反过来，不能用多个参数对一个数组参数赋值。为避免赋值混乱，**将不定参数放在最后赋值**。

```scala
public static void number(int a,int... Ins){}//Java版本
def number(int:Int,ints:Int*):Unit={}//Scala版本
```

### 惰性函数

```scala
lazy val string :String = getString
```

初始化工作被推迟到只有在被调用的那一刻才会进行，lazy不能用在var类型的变量上

### 递归函数

1. 临界条件
2. 递归方程

递归大忌就是重复计算。

## 闭包

闭包是一个函数，返回值依赖于申明在函数外部的一个或多个变量。如：

var factor=3

var multiplier=(i:Int)=>i*factor

## 字符串

Scala本身没有字符串，实际上是Java String。

- char charAt(int index) 

## 数组

- 申明：var z:Array[String]=new Array[String](3)
- 多维数组：var myMatrix=ofDim[Int](3,3)
- 数组方法：import Array._

| **def apply( x: T, xs: T\* ): Array[T]**  创建指定对象 T 的数组, T 的值可以是 Unit, Double, Float,  Long, Int, Char, Short, Byte, Boolean。 |
| ------------------------------------------------------------ |
| **def  concat[T]( xss: Array[T]\* ): Array[T]**  合并数组    |
| **def  copy( src: AnyRef, srcPos: Int, dest: AnyRef, destPos: Int, length: Int ):  Unit**  复制一个数组到另一个数组上。相等于 Java's  System.arraycopy(src, srcPos, dest, destPos, length)。 |
| **def  empty[T]: Array[T]**  返回长度为 0 的数组             |
| **def  iterate[T]( start: T, len: Int )( f: (T) => T ): Array[T]**  返回指定长度数组，每个数组元素为指定函数的返回值。  以上实例数组初始值为 0，长度为 3，计算函数为**a=>a+1**：  scala>  Array.iterate(0,3)(a=>a+1)  res1:  Array[Int] = Array(0, 1, 2) |
| **def  fill[T]( n: Int )(elem: => T): Array[T]**  返回数组，长度为第一个参数指定，同时每个元素使用第二个参数进行填充。 |
| **def  fill[T]( n1: Int, n2: Int )( elem: => T ): Array[Array[T]]**  返回二数组，长度为第一个参数指定，同时每个元素使用第二个参数进行填充。 |
| **def  ofDim[T]( n1: Int ): Array[T]**  创建指定长度的数组   |
| **def  ofDim[T]( n1: Int, n2: Int ): Array[Array[T]]**  创建二维数组 |
| **def  ofDim[T]( n1: Int, n2: Int, n3: Int ): Array[Array[Array[T]]]**  创建三维数组 |
| **def  range( start: Int, end: Int, step: Int ): Array[Int]**  创建指定区间内的数组，step 为每个元素间的步长 |
| **def  range( start: Int, end: Int ): Array[Int]**  创建指定区间内的数组 |
| **def  tabulate[T]( n: Int )(f: (Int)=> T): Array[T]**  返回指定长度数组，每个数组元素为指定函数的返回值，默认从 0 开始。  以上实例返回 3 个元素：  scala>  Array.tabulate(3)(a => a + 5)  res0:  Array[Int] = Array(5, 6, 7) |
| **def  tabulate[T]( n1: Int, n2: Int )( f: (Int, Int ) => T): Array[Array[T]]**  返回指定长度的二维数组，每个数组元素为指定函数的返回值，默认从 0 开始。 |

## _含义

1. 通配符:import scala.math._
2. _*作为一个整体，告诉编译器你希望将某个参数当做参数序列处理
3. 指代一个集合中的两个元素
4. 在元组中，访问组员
5. 使用模式匹配可以用来获取元组的组员
6. 下划线代表某一个类型的默认值
7. 在方法名称m后面紧跟一个空格 和下划线 告诉编译器将方法转换成函数,而不是调用这个方法,也可以显示的告诉编译器需要改将这个方法转换成函数

## 伴生对象

### 起因

​	Scala的设计者将程序的静态内容和非静态内容区分开来，他任务静态内容不应该属于OOP对象的范畴。因此设计者将同一个类的非静态部分申明为class:伴生类，将它的静态部分申明为object：伴生对象。

## 字符串输出

- java风格：println(string1 + "," + string)
- c风格：printf("%s,%s",string1,string)
- Linux风格：printf(s"$string,$string1")

## 类

在Scala中，类成员的访问权限被简化：

- 不带任何修饰符来表示公有
- 带上private或者protected表示私有变量只允许子类访问

### 构造器

- Java中一个类可以定义多个不同的构造方法。如果没有显示声明构造器，则Java默认提供无参构造器。如果显示申明了构造器，则Java不再提供无参构造器。Java的构造器第一行省略了super()
- Scala的构造器主要包括柱构造器和辅助构造器。编译器通过不同参数在区分选择何种构造器

#### 主构造器

在class上进行声明，类名的后面直接跟进一个参数列表。class内部可以看做是一个柱构造函数的函数体，当主程序使用主构造器创建一个类实例时，会像执行函数一样顺序执行class内部的每一条语句，而函数体内的申明语句则成为该类的属性成员和内部方法。

```scala
class Teacher(inAge:Int=25,inName:String="Tom"){
	var age:Int=inAge
	var name:String=inName
}
```

更精简的写法

```scala
class Teacher(var inAge:Int=25,var inName:String="Tom")
```

省略了将外部参数赋值给内部成员的步骤（这种省略方式仅限于主构造器当中使用）

可将构造器声明为私有：

```scala
class Teacher private (var inAge:Int=25,var inName:String="Tom")
```

#### 辅助构造器

用this关键字为类内部的方法命名，以表示该方法是一个辅助构造器。**辅助构造器内的第一行必须显式或隐式地调用主构造。**

原因：只有主构造器通过extends关键字建立了父类的联系。而子类的辅助构造器无法直接调用父类的任何构造器。

## 异常

在Scala中，只需要写一个catch：他会自动对捕捉到的异常进行模式匹配

```scala
try{
    var a:Int=10/0
    println(a)
}catch{
    case ex:ArithmeticException=>ex.printStackTrace()
    case ex:Exception=>ex.printStackTrace()
}finally{
    println("try-catch执行完毕")
}
```

Scala在def函数上加上一个@throws注解的方式来表示可能抛出的异常

```scala
@throws(classOf[NumberFormatException])
def mis(stringInt:String):Int={
    stringInt.toInt
}
```



## @BeanProperty注解

提供某个属性的set/get方法，若要使用此注解，则内部成员需要时可变var的，并且不能使用private关键字来修饰

## .scala文件的一些细节

- 一个.scala源文件内部可以存在多个公共的类
- 编译期间，scalac会将一个.scala文件中不同的类编译成分别的.class类文件
- 如果声明为class则只有一个；若为object则有两个
- 


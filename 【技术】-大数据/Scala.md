# Scala

## 基本语法

- 区分大小写
- 类名：对于所有的类名的第一个字母要大写
- 方法名称：所有的方法名称的第一个字母要小写
- 程序文件名：程序文件的名称应该与对象名称完全匹配（新版本不需要）
- def main(args:Array[String])

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

## 访问修饰符

- Private：比java严格，仅在成员定义的类或对象内部可见
- Protected：比java严格，只允许保护成员在定义了该成员的类的子类中被访
-  作用域保护：private[x]、protected[x]，这个成员处理对[…]中的类或[…]的包中类及他们的伴生对象可见外，对其他所有类都是private。

## 运算符

- 算数运算符：+、-、*、/、%
- 关系运算符：==、!=、>、<、>=、<=
- 逻辑运算符：&&、||、！
- 位运算：&、|、^、~、<<、>>、>>>

## 方法和函数

Scala方法(def)是类的一部分，而函数(val)是一个对象可以赋值给一个变量，其实是继承了Trait类的对象。

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
# Scala核心编程

## Scala概述

REPL：Read-Evaluation-Print-Loop

### 执行编译流程

1. .scala通过scalac编译>.class字节码文件通过scala运行>结果
2. .scala通过scala>结果

### 注意事项

1. 以.scala结尾
2. 执行入口是main()
3. 严格区分大小写
4. 方法由一条条语句构成，每个语句不需要分号
5. 若一行中有多条语句，则需要分号

### 常用转义字符

- \t
- \n
- \\\
- \r

### 规范的代码风格

- 合理使用tab
- 一行最长不超过80个字符

## 变量

### 变量的介绍

scala要求变量申明初始化

### 使用注意事项

1. 声明变量时，类型可以省略（编译器自动推导）
	val age = 10
	age = 20 //修改值
2. 在类型确定后，就不能修改，Scala是强数据类型的语言
	age = “Tom”//错误的
3. 在声明/定义一个变量时，使用var（可变）或val（不可变）
4. val修饰的变量在编译后，等同于加上final
5. 通过反编译看下底层代码  

## Scala键盘输入

```scala
import scala.io.StdIn
var name = StdIn.readline()
```

## 流程控制

### 分支控制

```scala
if (条件表达式) {
	执行代码块
}
```

- scala中if else是有返回值的                                                                                                                                                                                          

### for循环

- for(i<- 1 to 3){}：前后闭合
- for(i<-1 until 3){}：前闭后开
- for(i<- 1 to 3 if !=2){}：循环守卫，保护式
- for(i<-1 to 3;j=4-i){}：循环引入变量
- for(i<-1 to 3,i<-1 to 3)：嵌套循环
- val res = for(i<- 1 to 10)yield i：循环返回值。Vector
- 可以用{}替换()，for推导式有个不成文的规定，包含单一表达式用圆括号，多个用{}，{}来换行写表达式时，分号就不用写了
- 注意事项
	- for循环是表达式，有返回值
	- for循环控制步长：for(i<-Range(1,10,2)) 

### While

- whle没有返回值

## 构造器

​	构造器用来完成对象的初始化，而不是创建。包括==主构造器==和==辅助构造器==。辅助构造器可以有多个，编译器通过参数的个数来区分。

```scala
class Person(inAge:INt,inName:String){//主构造器
 	var age：Int=inAge
    var name：String=inName
}
```


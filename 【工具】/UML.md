# 类描述

类字体为斜体：**抽象类**

类名下边的区域：**属性**

属性下边：**方法**

可见性：

	- +：public
	- -：private
	- #：protected

# 接口

接口名需要添加<<Interface>>标志

可见性必须用**+**

# 类图关系

![img](https://user-gold-cdn.xitu.io/2019/7/19/16c098ab66f35678?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

## 继承

​	空三角形+实线

## 实现

​	空三角形+虚线

## 关联

​	一个类知道另一个类的属性和方法。关联可以是双向或者单向。

​	实线箭头

## 依赖

​	一个类依赖于另一个类的定义。依赖关系在Java语言中体现为局部变量、方法的形参或者对静态方法的调用。

​	虚线箭头

## 组合

​	一种强的拥有关系，体现了严格的部分和整体的关系，部分和整体的生命周期一样。

​	实心菱形实线。

## 聚合

 一种弱的拥有关系，体现是A对象可以包含对象B，但是B对象不是A对象的一部分。

​	空心菱形实线。


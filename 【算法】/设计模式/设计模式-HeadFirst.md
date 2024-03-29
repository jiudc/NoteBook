# 设计原则

- 多用组合，少用继承
- 找到程序中会变化的方面，然后将其和固定不变的方面相分离
- 针对接口编程，不针对实现编程
- 类应该对扩展开放，对修改关闭

## 策略模式

定义了算法族，分别封装起来，让他们之间可以互相替换，此模式让算法的变换独立于使用算法的客户

![image-20210718110443799](E:\git\NoteBook\【工具】\【图片】\image-20210718110443799.png)

## 观察者模式

定义了对象之间的一对多以来，当一个对象状态改变时，它的所有依赖着都会收到通知并自动更新。

![image-20210718121304863](E:\git\NoteBook\【工具】\【图片】\image-20210718121304863.png)

使用Java内置的类

![image-20210718192946619](E:\git\NoteBook\【工具】\【图片】\image-20210718192946619.png)

## 装饰者模式

动态地将责任附加到对象上，若要扩张功能，装饰着提供比继承更有弹性的替代方案。
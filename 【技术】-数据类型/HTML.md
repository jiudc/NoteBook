# HTML

## 定义

​	HTML，超文本标记语言，Hyper Text Makeup Language。

## 语法

- 标题：<h1>-<h6>
- 段落：<p>
- 链接：\<a>，anchor，\<a href="http://www.w3cschool.cn">这是一个链接</a>，Href
- 自关闭元素：<img>，不需要配对结尾，用src属性指向地址

## 常用模板引擎

### Thymeleaf

1. <html xmlns:th="http://www.thymeleaf.org">:引入域空间

2. 简单表达式

	1. ${...}  变量表达式
	2. *{...}  选择变量表达式
	3. #{...}  消息表达式
	4. @{...}  链接url表达式

3. 字面量

	1. 文本：'one text','another one!',... 
	2.  数值：0,34,3.0,12.3,...
	3. 布尔类型：true false 
	4. 空：null 
	5. 文本字符：one,sometext,main 

4. 文本操作

	1. 字符串连接：+，|The name is ${name}

5. 算术运算

	1. 二元运算符：+, - , * , / , %  
	2. 一元运算符：-(负号)

6. 布尔操作

	1. 二元操作符：and,or  
	2. 一元操作符：!,not 非

7. 关系操作符

	1. gt , lt , ge , le：> , < , >= , <= 
	2. eq, ne：== , != 

8. 条件判断

	1. if-then：(if) ? (then)    
	2. if-then-else：(if) ? (then) : (else)    

9. 常用标签

	| 关键字      | 功能介绍                                     | 案例                                                         |
	| ----------- | -------------------------------------------- | ------------------------------------------------------------ |
	| th:id       | 替换id                                       | <input th:id="'xxx' + ${collect.id}"/>                       |
	| th:text     | 文本替换                                     | <p th:text="${collect.description}">description</p>          |
	| th:utext    | 支持html的文本替换                           | <p th:utext="${htmlcontent}">conten</p>                      |
	| th:object   | 替换对象                                     | <div th:object="${session.user}">                            |
	| th:value    | 属性赋值                                     | <input th:value="${user.name}" />                            |
	| th:with     | 变量赋值运算                                 | <div th:with="isEven=${prodStat.count}%2==0"></div>          |
	| th:style    | 设置样式                                     | th:style="'display:' + @{(${sitrue} ? 'none' : 'inline-block')} + ''" |
	| th:onclick  | 点击事件                                     | th:onclick="'getCollect()'"                                  |
	| th:each     | 属性赋值                                     | tr th:each="user,userStat:${users}"                          |
	| th:if       | 判断条件                                     | <a th:if="${userId == collect.userId}" >                     |
	| th:unless   | 和th:if判断相反                              | <a th:href="@{/login}" th:unless=${session.user != null}>Login</a> |
	| th:href     | 链接地址                                     | <a th:href="@{/login}" th:unless=${session.user != null}>Login</a> /> |
	| th:switch   | 多路选择 配合th:case 使用                    | <p th:case="'admin'">User is an administrator</p>            |
	| th:fragment | 布局标签，定义一个代码片段，方便其它地方引用 | <div th:fragment="alert">                                    |
	| th:include  | 布局标签，替换内容到引入的文件               | <head th:include="layout :: htmlhead" th:with="title='xx'"></head> /> |
	| th:replace  | 布局标签，替换整个标签到引入的文件           | <div th:replace="fragments/header :: title"></div>           |
	| th:selected | selected选择框 选中                          | th:selected="(${xxx.id} == ${configObj.dd})"                 |
	| th:src      | 图片类地址引入                               | \<img class="img-responsive" alt="App Logo" th:src="@{/img/logo.png}" /> |
	| th:inline   | 定义js脚本可以使用变量                       | <script type="text/javascript" th:inline="javascript">       |
	| th:action   | 表单提交的地址                               | <form action="subscribe.html" th:action="@{/subscribe}">     |
	| th:remove   | 删除某个属性                                 | <tr th:remove="all">  　　　　　　　　　　　　　　　　　　<br />1 all:删除包含标签和所有的孩子。 　　　　　　　　　　　　　　　　　　　　2.body:不包含标记删除,但删除其所有的孩子。 　　　　　　　　　　　　　　　　3.tag:包含标记的删除,但不删除它的孩子。 　　　　　　　　　　　　　　　　　　4.all-but-first:删除所有包含标签的孩子,除了第一个。th:selected |
	| th:attr     | 设置标签属性，多个属性可以用逗号分隔         | 比如 th:attr="src=@{/image/aa.jpg},title=#{logo}"，此标签不太优雅，一般用的比较少 |


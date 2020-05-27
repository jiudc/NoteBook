# Idea

## 编辑

| 快捷键              | 英文说明                                        | 中文说明                                                     |
| ------------------- | ----------------------------------------------- | ------------------------------------------------------------ |
| Ctrl+Shift+Space    | Smart code completion                           | 列出的可选项中显示输入最相关的信息                           |
| Ctrl+Shift+Enter    | Complete statment                               | 代码补全后，自动在代码末尾添加分号结束符                     |
| Ctrl+P              | Parameter info                                  | 某个方法后，显示参数                                         |
| Ctrl+Q              | Quick doucumentation                            | 显示某个类或者方法的API说明文档                              |
| Alt+Insert          | Generate Code                                   | 自动生成某个类的Getter等                                     |
| Ctrl+O              | Override method                                 | 展示该类中所有覆盖或者实现的方法列表                         |
| Ctrl+Alt+T          | Surroud With                                    | 自动生成具有环绕性质的代码                                   |
| Ctrl+/              | Comment with line                               | 注释                                                         |
| Ctrl+Shift+/        | Comment with block                              | 对代码块，添加或删除注释                                     |
| Ctrl+W              | Select successively increasing code blocks      | 选中当前光标所在的代码块，多次触发                           |
| Ctrl+Shift+W        | Decrease                                        | 上行的方向操作                                               |
| Ctl+Shift+]/[       | Select till code block end/start                | 从当前光标开始，一直选择到当前光标所在代码段起始或者结束位置 |
| Alt+Q               | Context info                                    | 展示包含当前光标所在代码的父节点信息                         |
| Alt+Enter           | Show intention actions and quick-fixs           | 展示当前光标所在代码，可以变化的扩展操作                     |
| Ctl+Alt+L           | Reformat code                                   | 格式化代码                                                   |
| Ctrl+Alt+O          | Optimize imports                                | 去除没有实际用到的包                                         |
| Ctl+ALt+I           | Auto-indent line                                | 按照缩进的设定，自动缩进所选择的代码段                       |
| Ctl+Shift+V         | Paste from recent buffers                       | 从之前的剪切或拷贝的代码历史记录中，选择内容                 |
| Ctl+D               | Duplicate current line or select block          | 复制当前选中的代码行                                         |
| Ctl+Y               | Delete line at caret                            | 删除当前光标所在的代码行                                     |
| Ctl+Shift+J         | Smart line join                                 | 把下一行的代码续接到当前的代码行                             |
| Ctl+Enter           | Smart line split                                | 当前代码行与下一行代码之间插入一个空行，光标不变             |
| Shift+Enter         | Start new line                                  | 当前代码行与下一行代码之间插入一个空行，光标在下一行         |
| Ctrl+Shift+U        | Toggle case for word at caret or selected block | 所选择的内容进行大小写转换                                   |
| Ctrl+Delete         | Delete to word end                              | 删除从当前光标所在位置，直到这个单词的结尾的内容             |
| Ctl+NumPad(+/-)     | Expand/collapse code block                      | 展示或收缩代码                                               |
| Ctl+Shift+NumPad(+) | Expand all                                      | 展示所有代码段                                               |
| Ctl+SHift+NumPad(-) | Collapse all                                    | 收缩所有代码段                                               |
| Ctl+F4              | Close active editor                             | 关闭当前标签页                                               |
| Shift+F6            |                                                 | 修改名字                                                     |

## 查找或替换

| 快捷键       | 英文说明        | 中文说明                               |
| ------------ | --------------- | -------------------------------------- |
| Ctl+F        | Find            | 在当前标签页中进行查找，支持正则表达式 |
| F3           | Find next       | 如果找到了多个查找结果，跳至下一个结果 |
| Shift+F3     | Find previous   | F3的方向操作                           |
| Ctrl+R       | Replace         | 替换                                   |
| Ctrl+Shift+F | Find in path    | 通过路径查找                           |
| Ctrl+Shift+R | Replace in path | 通过路径替换                           |

## 查看使用情况

| 快捷键         | 英文说明            | 中文说明                                       |
| -------------- | ------------------- | ---------------------------------------------- |
| Alt+F7         | Find usages         | 在当前项目中的使用情况，会打开一个使用情况面板 |
| Ctl+F7         | Find usages in file | 在当前文件中的使用情况，找到的内容会低亮显示   |
| Ctl+Shift+F7   |                     | 在当前文件中的使用情况，找的内容会高亮显示     |
| **Ctl+Alt+F7** | show usage          | 打开使用情况列表                               |

## 编译与运行

| 快捷键        | 英文说明                                    | 中文说明                                                   |
| ------------- | ------------------------------------------- | ---------------------------------------------------------- |
| Ctl+F9        | Make Project(Compile modifed and dependecy) | 编译项目（如果之前编译过，只会编译那些修改的类或依赖的包） |
| Ctl+Shift+F9  | Compile selected file，package or module    | 编译选中的范围                                             |
| Atl+Shift+F10 | Select configuration and run                | 会打开一个已经配置的运行列表，选择一个运行                 |
| Alt+Shift+F9  | Select configuration and debug              |                                                            |
| **Shift+F10** | Run                                         | 立即运行当前配置的运行实例                                 |
| Ctl+Shift+F10 | Run context configuration from editor       | 按照编辑器绑定的文件类型，运行相关的程序                   |

## 调试

| 快捷键        | 英文说明            | 中文说明                                                   |
| ------------- | ------------------- | ---------------------------------------------------------- |
| F8            | Step over           | 跳到当前代码下一行                                         |
| F7            | Step into           | 跳入调用的方法内部代码                                     |
| Shift+F7      | Smart Step into     | 会打开一个面板，选择具体要跳入的类                         |
| Shift+F8      | Strp out            | 跳出当前的类，到上一级                                     |
| Alt+F9        | Run to cursor       | 让代码运行到当前光标所在                                   |
| Alt+F8        | Evaluate expression | 打开一个表达式面板，然后进行进一步的计算                   |
| F9            | Resume program      | 结束当前断点的本轮调试，如果有下一个断点会跳到下一个断点中 |
| Ctrl+F8       | Toggle breakpoint   | 在当前光标处，添加或者删除断点                             |
| Ctrl+Shift+F8 | View breakpoints    | 打开当前断点的面板，可以进行条件过滤                       |

## 导航

| 快捷键                     | 英文说明                                  | 中文说明                                                     |
| -------------------------- | ----------------------------------------- | ------------------------------------------------------------ |
| Ctl+N                      | Go to Class                               | 打开类查询框                                                 |
| Ctl+Shift+N                | Go to file                                | 打开文件查询框                                               |
| Ctl+Alt+Shift+N            | Go to symbol                              | 打开文本查询框框                                             |
| Alt+右箭头/左箭头          | Go to next/previous editor tab            | 跳到下一个/上一个编辑标签                                    |
| F12                        | Go back to previous tool window           | 如果当前在编辑窗口，触发后，会跳到之前操作过的工具栏上。     |
| ESC                        | Go to editor (from tool window)           | 从工具栏上，再跳回原来的编辑窗口，一般与 F12 配合使用。      |
| Shift + ESC                | Hide active or last active window         | 隐藏最后一个处于活跃状态的工具窗口。                         |
| Ctrl + Shift + F4          | Close active run/messages/find/… tab      | 同时关闭处于活动状态的某些工具栏窗口。                       |
| Ctrl + G                   | Go to line                                | 跳转至某一行代码                                             |
| Ctrl + E                   | Recent files popup                        | 打开曾经操作过的文                                           |
| Ctrl + Alt + 右箭头/左箭头 | Navigate back/forward                     | 在曾经浏览过的代码行中来回跳                                 |
| Ctrl + Shift + Backspace   | Navigate to last edit location            | 跳转到最近的编辑位置（如果曾经编辑过代码）                   |
| Alt + F1                   | Select current file or symbol in any view | 打开一个类型列表，选择后会导航到当前文件或者内容的具体与类型相关的面板中 |
| Ctrl + B 或 Ctrl +鼠标左键 | Go to declaration                         | 如果是类，那么会跳转到当前光标所在的类定义或者接口；如果是变量，会打开一个变量被引用的列表。 |
| Ctrl + Alt + B             | Go to implementation(s)                   | 跳转到实现类，而不是接口                                     |
| Ctrl + Shift + I           | Open quick definition lookup              | 打开一个面板，里面包含类代码                                 |
| Ctrl + Shift + B           | Go to type declaration                    | 打开变量的类型所对应的类代码，只对变量有用                   |
| Ctrl + U                   | Go to super-method/super-class            | 打开方法的超类方法或者类的超类，只对有超类的方法或者类有效   |
| Alt + 上/下箭头            | Go to previous/next method                | 在某个类中，跳到上一个/下一个方法的签名上                    |
| Ctrl + ]/[                 | Move to code block end/start              | 移动光标到类定义的终止右大括号或者起始左大括号               |
| Ctrl + F12                 | File structure popup                      | 打开类的结构列表。（常用）                                   |
| Ctrl + H                   | Type hierarchy                            | 打开类的继承关系列表。（常用）                               |
| Ctrl + Shift + H           | Method hierarchy                          | 打开某个类方法的继承关系列表                                 |
| Ctrl + Alt + H             | Call hierarchy                            | 打开所有类的方法列表，这些方法都调用了当前光标所处的某个类方法。（常用） |
| F2/Shift + F2              | Next/previous highlighted error           | 在编译错误的代码行中来回跳                                   |
| F4                         | Edit source                               | 打开当前光标所在处的方法或类源码                             |
| Alt + Home                 | Show navigation bar                       | 激活包路径的导航栏                                           |
| F11                        | Toggle bookmark                           | 把光标所处的代码行添加为书签或者从书签中删除。（常用）       |
| Ctrl + F11                 | Toggle bookmark with mnemonic             | 把光标所处的代码行添加为带快捷键的书签或者从快捷键书签中删除 |
| Ctrl + [0-9]               | Go to numbered bookmark                   | 跳转到之前定义的快捷键书签                                   |
| Shift + F11                | Show bookmarks                            | 打开书签列表。（常用）                                       |

## 自定义注释

![image-20200527163401715](https://raw.githubusercontent.com/jiudc/pictures/master/image-20200527163401715.png)

 


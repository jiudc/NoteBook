# Shell

## 变量

### 系统变量

- HOME
- PATH

### 申明变量

- 变量=
- 撤销：unset 变量
- 申明静态变量：readonly 变量，不能unset
- 提升为全局变量，可供其他shell使用：export 变量名

### 特殊变量

- $0：代表该脚本名称
- $1~$9，${10}
- $#：获取参数个数
- $*：代表命令行中所有的参数，会把所有参数看错一个整体
- $@：代表命令行中的所有参数，把每个参数区分对待
- $?：上一个命令的返回值

## 运算符

### 基本语法

- ${运算式}或$(())
- expr +,-,\\*,/,% ==expr运算符间要有空格==
	- expr 3 + 2
	- expr `expr 2 + 3` \* 4
	- $((2+3)*4)

## 条件判断

### 基本语法

- [ condition ] ==condition前后要有空格==

1. 两个整数之间
	1. = 字符串比较
	2. -lt
	3. -le
	4. -eq
	5. -gt
	6. -ge
	7. -ne
2. 按照文件权限判断
	1. -r
	2. -w
	3. -x
3. 按照文件类型进行判断
	1. -f 文件存在并且是一个常规的文件
	2. -d 文件存在并且是一个目录
	3. -e 文件存在

eg：

- [ 23 -ge 22 ]
- [ -w hello.sh ]

4. 多条件判断
	1. [ condition ] && [ condition ]
	2. [ condition ] || [condition]

## 流程控制

### if判断

```shell
#!/bin/bash
if [ 条件判断式 ];then
	程序
fi
if [ 条件判断式 ]
	then
		程序
fi
```

### case 语句

```shell
case $变量名 in
"值1")
	程序
    ;;;
“值2")
	程序
	;;;
*)
	;;;
esac
```

### for 循环

``` shell
#!/bin/bash
s=0
for ((i=1;i<100;i++))
do
	s=$[$s+$i]
done
echo $s
```

```shell
#!/bin/bash
for i in $*
do
	echo "$i"
done
```

“$*”一个整体，“$@‘’、$@、$*一样

### while

```shell
#!/bin/bash
i=1
s=0
while [ $i -le 100 ]
do
	s=$[$s + $i]
	i=$[$i + 1]
done
```

## read读取控制台输入

- read
	- -p:指定读取时的提示符
	- -t:指定读取时的等待时间
		- read -t 7 -p “Enter your name in 7 seconds” NAME

## 函数

### 系统函数

- basename [string/pathname] [suffix]
	- 将pathname或string中的suffix去除，获取文件名称
- dirname 文件绝对路径
	- 返回路径

### 自定义函数

```shell
[ function ] funname [())]
{
	Action;
	[return init;]
}
```

- 函数必须先声明，后使用
- 返回值，只能通过$?(0-255)

## Shell工具

### cut

- cut [选项参数] filename：从每一行中剪切字符

	- -f：列号，提取第激烈

	- -d：分隔符，按照分隔符分隔列

		```shell
		cut -d " " -f 1,2 cut.txt
		cat cut.txt|grep guan|cut -d " " -f 1
		echo $PATH|cut -d : -f 3-
		ifconfig eth0|grep "inet addr"|cut -d : -f 2|cut -d " " -f 1
		```

### sed

sed是一种流处理器，一次处理一行内容。处理时，把当前处理的行存储在临时缓冲区，模式空间，接着用sed命令处理缓冲区内容，处理完成后，把缓冲区内容送往屏幕。

- sed [选项参数] ’command‘ filename

	- -e：直接在指令列模式上进行sed的动作编辑

	- 命令功能

		- a：新增，在下一行出现

		- d：删除

		- s：查找并替换

			```shell
			sed "2a xxx" sed.txt
			sed "/wo/d" sed.txt
			sed "s/wo/ni/g" sed.txt
			sed -e "2d" -e "s/wo/ni/g" sed.txt
			```

	### awk

	​	把文件逐行读入，一空格为默认分隔符将每行切开，切开的部分再进行分析处理。

	- awk [选项参数] ’pattern1{action1} pattern2{action2}…’ filename

	- -F:指定输入文件拆分隔符

	- -v：赋值一个用户定义变量

		```shell
		awk -F : '/^root/ {print $7}' passwd
		awk -F : '/^root/ {print $1","$7}' passwd
		awk -F : 'BEGIN{print "user,shell"} {print $1","$7 END{print "end"}' passwd
		awk -F : -v i=1 'print $3+i' passwd
		```

	- 内置变量
		- FILENAME：文件名
		- NR：已读的记录数
		- NF：浏览记录的域的个数（切割后，列的个数）

### sort

- sort

	- -n：按照数值大小

	- -r：倒序

	- -t：设置排序时所用的风格字符

	- -k：指定需要排序的列

		```shell
		sort -t : -nrk 2
		```

		


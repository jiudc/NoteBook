# Shell脚本调试技术

## trap

用于捕获指定的信号并执行预定义的命令

trap ‘command’ signal

signal是要捕获的信号，command是捕获到信号之后，所要执行的命令。可使用kill -l命令看到系统中全部可用的信号名，捕获信号后所执行的命令可以是任何一条或多条合法的shell语句，也可以是一个函数名

shell脚本在执行时，会产生三个伪信号，因为这个三个信号是shell产生的，而其他的信号是操作系统产生的

| 信号名 | 何时产生                                       |
| ------ | ---------------------------------------------- |
| EXIT   | 从一个函数中退出或整个脚本执行完毕             |
| ERR    | 当一条命令返回非零状态时（代表命令执行不成功） |
| DEBUG  | 脚本中每一条命令执行前                         |

$LINENO为shell内置变量，代表shell脚本的当前行号

DEBUG可用来对相关变量进行全程跟踪。

## tee

在shell脚本中管道以及输入输出重定向使用非常多，因为使用管道，中间结果不会打印在屏幕上，给调试带来的困难，可借助tee。

ipadd=`/sbin/ifconfig |grep 'inet addr:' |grep -v '127.0.0.1'|tee temp.txt|cut -d : -f3| awk '{print $1}'

## 使用调试钩子

在C语言中，常用DEBUG宏来控制是否输出调试信息，shell中可使用这样的机制。

```shell
if [[ "$DEBUG" == "true" ]];then
  echo "debugging"
fi
```

在脚本调试阶段，先执行export DEBUG=true命令来打开钩子

如果在每一处需要输出调试信息的地方用if语句来判断DEBUG变量的值，还是比较繁琐，通过定义一个DEBUG函数可更加简洁

```
DEBUG(){
  if [[ "$DEBUG" == "true" ]];then 
  	 $@
  fi
}
```

## 使用shell的执行选项

-n只读取shell脚本，但不实际执行
-x进入跟踪方式，显示执行的每一条命令
-c ’strings‘从strings中读取命令

-x模式：前面有+号的是shell脚本实际执行的命令，前面有++号的行是执行trap机制中指定的命令，其他的行则是输出信息。

```shell
set -x #开启-x
set +x #关闭-x
DEBUG set -x
DEBUG set +x
```

## 对“-x”选型的增强

shell内置的环境变量

$LINENO：代表shell脚本的当前行号，类似于C语言内置宏\_\_LINE\_\_
$FUNCNAME：函数的名字，是个数组变量，包含整个调用链上所有函数的名字，${FUNCNAME[0]}表示shell脚本当前正在执行的函数的名字，而${FUNCNAME[1]}则表示调用函数${FUNCNAME[0]}的函数的名字
$PS4:主提示符变量$PS1和第二级提示符变量$PS2比较常见，而$PS4为第四级提示符变量。$PS4的值将被显示在“-x”选项输出的每一条命令的前面。在Bash Shell，缺省的$PS4是“+”。可以使用内置变量重定义来增强"-x"

```shell
export PS4='{$LINENO:${FUNCNAME[0]}}'
```

## BASHDB


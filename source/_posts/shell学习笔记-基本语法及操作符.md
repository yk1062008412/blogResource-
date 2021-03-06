---
title: shell学习笔记-基本语法及操作符
date: 2017-11-01 15:39:32
categories: "Linux"
tags: 
    - Linux
    - Shell
---

本章讲述了shell的基本语法及操作符。

# 入门

## -e 执行控制字符

echo -e "\a"

|控制字符|作用|
|----|----|
|\a|警告音|
|\b|退格键，即向左删除一格|
|\n|换行符|
|\r|回车键|
|\t|制表符，即Tab键|
|\v|垂直制表符|
|\0nnn|八进制输出|
|\xhh|十六进制输出|

## 字体颜色

echo -e "\e[1;31m 文字 \e[0m"

\e:调用颜色
[1:开启颜色选项
\e[0：关闭颜色选项

|代码|颜色|
|----|----|
|30m|黑色|
|31m|红色|
|32m|绿色|
|33m|黄色|
|34m|蓝色|
|35m|洋红色|
|36m|青色|
|37m|白色|

## 脚本执行

- 赋予执行权限，直接运行
```
chmod 755 hello.sh
./hello.sh
```
- 通过bash调用执行脚本
```
bash hello.sh
```
# bash的基本功能

## 查看与设定别名

查看别名：alias

设定别名：

alias 别名='原命令'

## 别名永久生效

```
vi ~/.bashrc
# 写入环境变量配置文件
```
删除别名
```
unalias 别名
```

## 常用快捷键

|快捷键|功能|
|---|---|
|ctrl+c|强制终止当前命令|
|ctrl+l|清屏|
|ctrl+a|光标移动到命令行首|
|ctrl+e|光标移动到命令行尾|
|ctrl+u|从光标所在位置删除到行首|
|ctrl+z|把命令放入后台|
|ctrl+r|在历史命令中搜索|

# 输出重定向

```
ifconfig > test.log
```

将ifconfig应该显示到显示器上的内容，写入test.log中。如果test.log有内容，则覆盖内容。

```
ifconfig >> test.log
```

将ifconfig应该显示到显示器上的内容，写入test.log中。如果test.log有内容，则追加内容。

```
aaaaaa 2> test.log
```

将命令的错误输出，输出到指定的文件，写入test.log中。如果test.log有内容，则覆盖内容。

```
aaaaaa 2>> test.log
```

将命令的错误输出，输出到指定的文件，写入test.log中。如果test.log有内容，则追加内容。

|格式|含义|
|---|---|
|命令 > 文件 2>&1|以覆盖的方式，把正确输出和错误输出都保存到同一个文件当中。|
|命令 >> 文件 2>&1|以追加的方式，把正确输出和错误输出都保存到同一个文件当中。|
|命令 &> 文件|以覆盖的方式，把正确输出和错误输出都保存到同一个文件当中。|
|命令 &>> 文件|以追加的方式，把正确输出和错误输出都保存到同一个文件当中。|
|命令 >> 文件a 2>>文件b|把正确的输出追加到文件a中，把错误的输出追加到文件b中|

```
命令 &>/dev/null
```

将命令的运行结果丢弃，弃之不用。

# 输入重定向

```
wc < test.log
或者
wc test.log
```

统计test.log文件下的行数，单词数，字符数

**wc 的选项：**
- -c：统计字节数
- -w：统计单词数
- -l：统计行数

# 管道符

## 多命令顺序执行

|多命令执行符|格式|作用|
|---|---|---|
|;|命令1;命令2|多个命令顺序执行，命令之间没有任何逻辑联系|
|&&|命令1&&命令2|逻辑与；当命令1正确执行，则命令2才会执行；当命令1执行不正确，则命令2不会执行|
|&#124;&#124;|命令1&#124;&#124;命令2|逻辑或；当命令1执行不正确，则命令2才会执行；当命令1正确执行，则命令2不会执行|

```
命令 && echo yes || echo no
ls && echo yes || echo no
```
即：如果命令正确执行，输出yes，如果没有正确执行，输出no。

## 管道符

```
语法：命令1 | 命令2
```
命令1的正确输出作为命令2的操作对象。

常用管道符： grep、more、wc

# 通配符

|通配符|作用|
|---|---|
|?|匹配一个任意字符。|
|*|匹配0个或任意多个任意字符，即匹配任何内容。|
|[]|匹配中括号中任意一个字符。例如:[abc]|
|[-]|匹配中括号中任意一个字符，-代表范围。例如：[a-z]|
|[^]|逻辑非，表示匹配不是中括号内的一个字符。例如：[^0-9]代表匹配非数字字符|

## bash中其他特殊符号

|符号|作用|
|---|---|
|''|单引号。单引号中的所有特殊符号，如“$”,“`”都没有特殊含义。|
|""|双引号。双引号中的特殊符号没有特殊含义，但“$”，“`”，“\”例外，分别表示“调用变量的值”，“引用命令”，“转义符”|
|``|反引号。反引号括起来的内容是系统命令，在bash中会先执行它，和$()作用一样，不过推荐使用$(),因为反引号非常容易看错|
|$()|和反引号作用相同，用来引用系统命令。|
|#|在shell脚本中，#开头的行表示注释|
|$|用于调用变量的值，如需要调用变量name的值时，需要用$name的方式得到变量的值|
| \ |转义符，跟在\之后的特殊符号将失去特殊含义，变为普通字符。如\$将输出“$”符号，而不当做是变量引用。|

# Bash变量

## 什么是变量

**在Bash中，变量的默认类型都是字符型。**

查看所有变量命令：set.

查看环境变量命令：env.

set -u:如果设定此选项，调用未声明变量时会报错(默认无任何提示)。

删除变量：unset。

例：unset name；删除name变量

**删除变量name，直接写变量名name就可以了，不需要写变量值$name;**

## 变量的分类

### 用户自定义变量

变量名=变量值

**等号左右均没有空格！！！**

如果变量值有空格，必须用双引号或单引号包含起来。

```
例：
    x=5
    name="zhang san"
```

变量叠加

```
x=123       # x=123
x="$x"456   # x=123456
x=${x}789   # x=123456789
```

### 环境变量

环境变量是全局变量，在所有shell中都有效；
用户自定义变量是局部变量，只在当前shell中有效。

用户定义环境变量
```
export 变量名=变量值
或
变量名=变量值
export 变量名
```
常用环境变量
```
HOSTNAME:主机名
SHELL:当前的shell
TERM:终端环境
HISTSIZE:历史命令条数
SSH_CLIENT:当前操作环境是用ssh连接的，这里记录客户端ip
SSH_TTY:ssh连接的终端是pts/1
USER:当前登录的用户
```

### 语系变量

语言支持

安装第三方插件 zhcon 可以使Linux支持中文。

### 位置参数变量

|位置参数变量|作用|
|---|---|
|$n|n为数字，$0代表命令本身，$1-$9代表第1到第9个参数，10以上的参数需要用大括号包含，如${10}.|
|`$*`|这个变量代表命令行中所有的参数，`$*`把所有的参数看成一个整体.|
|$@|这个变量也代表命令行中所有的参数，不过$@把每个参数区分对待.|
|$#|这个变量代表命令行中所有参数的个数.|

### 预定义变量

|预定义变量|作用|
|---|---|
|$?|最后一次执行的命令的返回状态。如果这个变量的值为0，证明上一个命令正确执行；如果这个变量的值为非0(具体是哪个数，由命令自己来决定),则证明上一个命令执行不正确|
|$$|当前进程的进程号(PID)|
|$!|后台运行的最后一个进程的进程号(PID)|

**接收键盘输入**

接收键盘输入命令：read

```
read [选项] [变量名]
- 选项：
    * -p "提示信息"：在等待read输入时，输出提示信息
    * -t 秒数：read命令默认会一直等待用户输入，使用此选项可以设置等待时间
    * -n 字符数：read命令只接受指定的字符数，就会执行
    * -s 隐藏：read命令隐藏输入的内容，适用于密码等机密信息的输入
```

# shell变量

缺点：

- 弱类型
- 默认字符串型

## declare声明变量类型

```
语法：declare [+/-][选项] 变量名
选项：
    -：给变量设定类型属性
    +：取消变量的类型属性
    -a:将变量声明为数组型
    -i:将变量声明为整数型(integer)
    -x:将变量声明为环境变量
    -r:将变量声明为只读变量
    -p:显示指定变量的被声明的类型
```
**export test=1 相当于 declare -x test=1,都是将变量声明为环境变量**

声明数组变量

```
// 声明数组
a[0]=js
a[1]=jq
declare -a a[2]=css
// 查看数组
echo ${a}       // js
echo ${a[2]}    // css
echo ${a[*]}    // js jq css
```

## 数值运算方法

方法1：declare 运算符

```
a=1
b=2
declare -i c=$a+$b
echo $c
```

方法2：expr 和 let 数值运算工具

```
a=1
b=2
c=$(expr $a + $b)
# c的值是a和b的和。注意！！！'+'号左右两侧必须有空格
echo $c
```

方法3："$((运算式))" 或者 "$[运算式]"

**推荐使用$(())**

```
a=1
b=2
c=$(($a+$b))
echo $c
```

运算符优先级：

优先级越大的先执行

|优先级|运算符|说明|
|---|---|---|
|13|-,+|此处代表正负数，不是加减|
|12|!,~|逻辑非，按位取反或补码|
|11|*,/,%|乘，除，取模|
|10|+,-|加，减|
|9|<<,>>|按位左移，按位右移|
|8|<=,>=,<,>|小于等于，大于等于，小于，大于|
|7|==,!=|等于，不等于|
|6|&|按位与|
|5|^|按位异或|
|4|&#124;|按位或|
|3|&&|逻辑与|
|2|&#124;&#124;|逻辑或|
|1|=,+=,-=,*=,/=,%=,&=,^=,&#124;=,<<=,>>=|赋值，运算且赋值|

## 变量测试

- 变量测试仅仅适用于shell编程
- 变量测试在脚本优化的时候有用


本章完！



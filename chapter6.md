## shell script
### 什么是shell脚本
---
#### shell脚本
* shell的作用是解释执行用户的命令，输入一条就执行一条 交互式
* 批处理 事先写脚本，有很多命令，一次执行完
* shell脚本以行为单位执行，通常以sh作扩展名，包含注释、命令、shell变量和流程控制语句等
```
注释 用于对脚本进行解释说明，在注释行前要加\#
命令 在shell脚本中可以出现任何在交互方式下使用的命令
shell变量 shell支持具有字符串值的变量
流程控制 主要为一些用于流程控制的内部命令
```
> shell脚本用途
* 自动化管理的重要依据
* 追踪与管理系统重要工作
* 连续指令单一化
* 简单的数据处理
* 跨平台支持且易学习
* 但执行效率低
> shell脚本说明
* 从上至下，从左到右
* \# 注释行
* 一行写不开，用\延伸至下一行
* 读到回车尝试运行
* 纯文本
* 命令与选项间的多个空白会被忽略
* 空白行会被忽略，tab视作空格
> 编写shell脚本的好习惯
* 脚本的说明
* 注释
* 缩进
* 最好使用vim
#### shell脚本举例
> 交互式脚本
* read命令可以从键盘读入，通过read命令可以实现从键盘输入值的交互式脚本。
```
read -p "Where are you from:" country
read -p "which game do you like" game
echo " just a test : $country $game"
```
* 创建文件名随日期变化的文件
```
#filename changed through date
read -p "input your filename:" fileuser
date1=$(date +%Y%m%d)
file1=${fileuser}_${date1}
date2=$(date --date='2 days ago' +%Y%m%d)
file2=${fileuser}_${date2}
touch "${file1}"
touch $file2

[umiz@centos7-1 shelltest]$ bash test2.sh
input your filename:abc  
[umiz@centos7-1 shelltest]$ ls
abc_20201204  abc_20201206  test1.sh  test2.sh
```
* 数值运算
```
read -p "input a:" a
read -p "input b:" b
declare -i c=$a+$b
#c=$a+$b
echo "The sum of $a and $b is:"$c
mul=$(($a*$b))
echo "The mul of $a * $b is:" $mul
bash shell预设仅支持整数，需要用declare –i 将变量声明为整数才能运算。此外，还可使用$((计算表达式))来进行数值运算。
```
### shell script的执行方式
---
#### shell脚本的执行方式
> 直接指令下达
* 要求test.sh文件必须可读可执行
* 执行方式：
```
	绝对路径：使用/home/liu/test.sh 下达指令
	相对路径：当前目录/home/liu, 使用./test.sh
	将first.sh放到环境变量PATH指定的目录中, /home/liu/bin可以尝试
```
> 透过bash程序执行
* 要求test.sh可读
* 执行方式：
```
	bash test.sh
	sh test.sh
sh 参数：	
	sh  -n  只读shell脚本，检查语法，不执行 
	sh	-x 显示执行的每条指令
```
> source命令
* 要求test.sh可读
```
source test.sh
```
#### 执行方式的差别
> 直接指令下达、sh命令
* 执行时会调用新的bash（子进程）来执行脚本中的指令。执行完成后，回到父进程，子进程中的变量和动作结束后不会回传到父进程
> source命令
* 会直接在父进程执行，各项变量动作都会在原bash中生效
### shell脚本的条件判断
---
#### shell脚本的判断式
* 判断字符串是否相等、检查文件状态或数字测试
* shell脚本编程使用'test'命令和'\[]'符号实现判断
> test 选项 文件/数值/字符串
* 执行结果没有任何输出，同时可以用&?、&&、||显示结果
* 文件名类型判断
```
-e 该文件名是否存在
-f 该文件名是否存在且为file
-d 该文件名是否存在且为目录
-b 该文件名是否存在且为一个block
-c 该文件名是否存在且为一个character device设备
-S 该文件名是否存在且为一个socket文件
-p 该文件名是否存在且为一个FIFO（pipe）文件
-L 该文件名是否存在且为一个连接文件
```
* 文件权限判断
```
-r 检测文件名是否存在且具有“可读”权限
-w 检测文件名是否存在且具有“可写”权限
-x 检测文件名是否存在且具有“可执行”权限
-u 检测文件名是否存在且具有“SUID”权限
-g 检测文件名是否存在且具有“SGID”权限
-k 检测文件名是否存在且具有“Sticky bit”权限
-s 检测文件名是否存在且为“非空白文件”
```
* 文件比较
```
-nt 判断file1是否比file2新//newer than
-ot 判断file1是否比file2老//older than
-ef 判断file1和file2是否为同一个文件，用于判断hard link
```
* 数值判断
```
-eq equal
-ne not equal
-gt greater than
-lt less than
-ge greater than or equal
-le less than or equal
```
* 字符串判断
```
-z string 判断字符串是否为空，为空则返回true
-n string 判断字符串是否为空
test str1 == str2 判断是否相等，相等返回true
test str1 != str2 判断是否相等，相等返回false
```
* 多重条件判断
```
-a 两个条件同时成立返回true //test -r file -a test -x file
-o 任何一个条件成立返回true //test -r file -o test -x file
!  反向状态，条件不成立返回true //test !-x file
```
> \[]
* 用法 \[-选项 文字/字符]
* 中括号判断方式与测试标志与test相同
* 注意:
```
中括号两端要有空格，中括号内的每个选项\符号间都需要有空格。
中括号内的变量，最好用双引号括起来，避免出问题
中括号内的常量，最好用单引号或双引号括起来；
```
#### shell脚本传参
> 脚本的参数有固定名称，在脚本中使用这些名称应用变量
* $0：执行的脚本名称
* $1、$2..：脚本携带的第一个参数、第二个参数..
* $#：后面接的变量的个数
* $@：代表“$1”“$2”变量列表，每个变量是独立的，用双引号括起来
* $\*：代表“$1 $2 $3”,	默认用c对变量进行分隔。
### 分支结构
---
> 单层条件判断
```
if [ 条件判断式 ];then
    成立时，执行
条件判断式可以用test 或 [] ,当多个条件时可以用 –a –o, 也可两个条件间用&& || 分隔。
```
> 多重条件判断
```
if [ 条件判断式 ];then
    符合条件，执行
else
    条件判断不成立时，执行
fi
或者
if  [ 条件判断式1 ]; then
    符合条件判断1，执行
elif [ 条件判断式2 ]; then
    复合条件判断2，执行
else
    当判断式1和2都不成立时，执行	
fi	
```
> case...esac 判断
* 对同一变量进行多次测试，逐个和str1,str2比较，相等则执行command-list1，不相等则执行\*后面的语句
```
case $变量 in
"str1")
    commands-list1;;
"str2")
    commands-list2;;
    ...
*)
    commands-listn;;
esac
```
### 循环结构
---
#### while do done
* 判断式条件成立时执行
```
while [ 判断式 ]
do //循环开始
    执行程序
done //循环结束
```
#### until do done
* 判断式条件不成立时执行
```
until [ 条件判断 ]
do
    程序段落
done
```
#### for...do...done
> 固定循环
* 与while需判断条件不同，for循环已知循环次数
```
for var in con1 con2 con3...
do
    程序段
done
```
> 数值处理
```
for (( 初始值；限制值；执行步长 )) 
do
    程序段
done
初始值：变量开始值，如i=1
限制值：当变量在这个限制范围内，就继续循环，如i<=100
执行步长：每次循环的变化量，如i=i+1
```
#### break
* 用于从for、、while、until循环中跳出
* break \[n]
* n代表循环嵌套的层级，若指定了n，则退出n级嵌套循环。默认n=1如果没有指定n或n不大于等于1，则退出状态码为0，否则退出状态码为n。
#### continue
* 用于跳过循环体中的剩余命令，跳转到循环体的顶部，进行下一次循环
* 用于for、while、until循环
* continue \[n]
* 把n层循环剩余的代码都去掉，但是循环的次数不变。默认n=1。
### shell script中的函数
---
#### 函数
* shell脚本同样支持函数，来减少重复执行的代码段，简化程序代码
* 脚本中的函数一定要在脚本最前面定义
> 定义函数的语法
```
function_name()
{
    commands... //函数体 函数体，也叫复合命令块，是包含在{}之间的命令列表。
    //return语句可选，若无，返回最后一条命令的运行结果
    //若有，后跟n（0-255）
    [ return int; ]
}
```
* 也可以在函数名前加上关键字function
```
function function_name()
{
    commands...
}
//若有function关键字，可以省略（）。
```
* 可以在一行内定义一个函数，函数体内各命令需要用分号隔开
```
function function_name { command1;command2;commandN; } //大括号前后要有空格
或
function_name() { cmd1;cmd2;cmd3; } 
```
> 函数的调用
* 语法： 函数名 参数1 参数2 ... 参数n
```
function_name foo bar
foo是参数一，传递给函数的第一个参数
以此类推
```
> shell脚本参数和函数参数
* 传参
```
shell脚本 ./test.sh a b c
函数传参  function_name a b c
```
* 取参
```
shell脚本 $0,$1,$2...
函数取参  $0,$1,$2...

函数中取用的$0 $1…一定是调用函数时传递的参数
shell脚本中的$0 $1来自与脚本执行时传递的参数

function function_name
{
    echo "your name is ${1}"
}
function_name zhang
echo ${1}
```
#### 本地变量
* 默认条件下，在函数和shell主体中建立的变量都是全局变量，可以相互引用，当shell主体部分与函数部分拥有名字相同的变量时，可能会相互影响
* local命令只能在函数内部使用。
* local命令将变量名的可见范围限制在函数内部
* 使用局部变量，使得函数在执行完毕后，自动释放变量所占用的内存空间，从而减少系统资源的消耗，在运行大型的程序时，定义和使用局部变量尤为重要。
```
local var=value
local varName
function name
{
    #定义一个本地变量var
    local var=${1}
    command on $var
}
```
#### 函数返回值
* 内置有return return语句可选，若无，返回最后一条命令的运行结果；若有，后跟n（0-255）
* return可以放到函数体的任意位置，用于返回某些值。
* shell在执行到return之后，就停止往下执行，返回到主程序的调用行，return的返回值只能是0~255之间的一个整数，返回值将保存到变量“$?”中。

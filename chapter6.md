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

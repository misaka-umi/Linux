## bash shell
### bash shell 介绍
---
#### 什么是shell
> 用户与操作系统内核之间的接口，协调用户与系统的一致性，在用户与系统之间进行交互
* 计算机实现指定功能，需要硬件、操作系统、应用程序
* 系统核心需要接受应用程序的指令，但用户不能直接操纵，利用shell和Kernel沟通
```
由外到内
utilifies | gcc、ls、cat、who、wc、grep、diff、vi、etc..
Shell     | Bash、sh
Kernel    | TCP/IP、multitasking、device、interface、stack
hardware
```
> shell优势
* 功能完善，各发行版使用相同bash
* 远程管理，文字接口传输速度更快
* 更好的管理主机
> 系统中有多个shell
* Bourne Shell(sh)
* SUN: C Shell(csh)
* K Shell
* TCSH
* Bourne Again Shell(bash)
```
cat /etc/shells //查看当前系统支持的shell
```
#### bash shell功能
> bash是GNU计划的重要工具软件之一，是预设的标准shell，兼容sh
* **主要功能**
* 命令编修（history）
* 命令与文件补全（tab）
* 命令别名设定（alias）
* 工作控制、前景背景控制（jobs）
* 程序化脚本（shell scripts）
* 通配符（Wildcard）
> **type 选项 name**
* 查看指令类型，不加任何参数会显示出name是外部命令还是bash内建命令
```
-t 显示命令的意义
-p 若name是外部命令，显示完整文件名
-a 将所有含name的变量进行罗列，包括别名
```
> 调用命令
```
若为内置命令，则直接进行内核中的系统功能调用
若为外部命令或实用程序，先在系统中查找该命令的文件并调入内存执行，后面同上
```
### shell变量
---
#### shell变量
> 什么是变量
* 用某一个特定字符串代表比较复杂或容易变动的内容，方便使用
* y=ax+b
> shell变量的好处
* 影响bash环境的变量
```
使用shell获得bash运行程序
系统通过变量提供数据的存取，或环境的配置参数值
```
* 脚本程序中的变量
```
大型script中，有些变量经常被使用，
可以进行声明，改变一行就能应用到整个脚本
```
#### 变量的取用与设定
> 变量的取用
* echo $变量或${变量}
```
$ echo ${variable}
$ echo $variable
```
> 变量的设定
* "="
```
$ var=abc //var被设定/修改为abc
```
> 变量设定的规则
* 变量与变量内容以等号链接```dark=fkgfw```
* 等号两边不能有空格 错误示范```dark = fkgfw```
* 变量名称只能是英语字母与数字，且第一个字符不能是数字 错误示范```1god=ccp```
* 变量内容如果有空格符，可以用双引号或单引号，但是两者有区别
```
$ var ="lang is $LANG" //设定
$ echo $var //调用
lang is zh_CN. UTF-8 //双引号内$等特殊字符保持了原有特性
$ var='lang is $LANG'
$ echo $var //调用
lang is $LANG //特殊字符的特性消失
```
* 反斜杠"\"可以将特殊符号转义
* 其他命令的返回值作为变量值的情况，十一使用\`内容\`或$(内容)
```
$ uname -r
3.10.0-1127.el7.x86_64
$ version=$(uname -r)
$ echo $version
3.10.0-1127.el7.x86_64
$ version=`uname -r`
$ echo $version
3.10.0-1127.el7.x86_64
```
* 如果需要增加变量的内容，可以使用$内容或${内容}累加
```
$ echo $god
ccp is
$ god="$god"" the world"
$ echo $god
ccp is the world
```
* 如果该变量需要运行其他子程序
```
export god //使用export使god变成环境变量
```
* 通常大写字符是系统默认变量，自行配置的应尽量小写，便于判断//非强制
* 取消变量使用unset
```
[umiz@centos7-1 ~]$ unset god
[umiz@centos7-1 ~]$ echo $god

```
#### shell中的环境变量
> 由shell定义并赋初值的shell变量，能被子程序所引用
* shell用环境变量确定查找路径、注册目录、终端类型、终端名称、用户名等
* 环境变量都是全局变量，可以由用户重新设置
* 用env指令查看当前shell环境中的所有环境变量
* 用export指令将自定义变量转换成环境变量
> 常用环境变量
* PATH     决定了shell将到哪些目录寻找命令和程序
* HOME     当前用户主目录
* HISTSIZE 历史记录数
* LOGNAME  当前用户的登录名
* HOSTNAME 指主机的名称
* SHELL    当前用户Shell类型
* LANGUGE  语言相关的环境变量，多语言可以修改此环境变量
* MAIL     当前用户的邮件存放目录
> 环境变量与自定义变量的差异
* 环境变量在子进程中可见，自定义变量则不可见
> 重要系统变量
* set可查看所有命令
* 重要变量: ?、$、PS1
```
[umiz@centos7-1 ~]$ echo $? 
//上一个指令回传的值，执行成功回传0，发生错误回传非0
127
[umiz@centos7-1 ~]$ echo $$ //当前shell进程的id
16298
[umiz@centos7-1 ~]$ echo $PS1 //基本提示符，对于root用户是#，对于普通用户是$
[\u@\h \W]\$
```
> **read 选项 变量名**
* 读取来自键盘的输入
* -p：后面可以接提示字符
* -t:接等待的秒数
```
[umiz@centos7-1 ~]$ read -p 'input a number:' num
input a number:123 
[umiz@centos7-1 ~]$ echo $num
123
```
> **declare 选项 变量名**
* 定义变量的类型
```
-a: 将变量定义成数组
-i: 将变量定义成为整数数字
-x: 将变量变为环境变量
-r: 将变量设定为readonly
```
### 命令的别名与历史命令
---
#### 别名的意义
* 简化比较长的惯用命令
* 限制导致严重后果命令的执行
* 支持用户使用习惯
#### 别名的使用
> 为命令添加别名
* alias 别名='指令 选项'
```
alias ls='ls --color=auto'
```
> 查看有哪些命令别名
```
[umiz@centos7-1 ~]$ alias
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'
alias ls='ls --color=auto'
alias vi='vim'
alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'
```
> 取消命令别名
* unalias 别名
> 查看历史命令
* history 选项
```
n   数字，列出最近n条历史命令；
-c  将⽬前shell的所有历史命令清除；
-a  将⽬前新增的history命令追加到histfiles中；如果没有指定histfiles，默认写⼊~/.bash_history；
-r  将histfiles中的内容读到⽬前shell的histroy中；
-w  将⽬前history所记录的内容写⼊histfiles。
```
> 历史命令读取和记录的过程
* 登录 从~/.bash_history读取
* 数量 HISTSIZE
* 登出 更新~/.bash_history
* history -w 可强制写入
> 利用"!"重复执行历史命令
```
!number：执行历史命令中的第number调指令
[umiz@centos7-1 ~]$ !2
su root
密码：

!command：由最近指令向前搜索，找到开头为“command的指令”，并执行
[umiz@centos7-1 ~]$ !echo
echo $num


!!: 执行上一条指令
[umiz@centos7-1 ~]$ !!
history 1
  538  history 1
```
### bash shell的操作环境
---

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
#### bash shell的工作环境
> 路径与命令搜索环境
在bash shell环境下，下达指令后的搜索顺序：
* 以相对/绝对路径执行命令```bin/ls或./ls```
* 由alias找到该指令来执行 //别名
* 由bash内建指令来执行
* 透过$PATH这个变量的顺序搜索到的第一个指令来执行
> bash进站欢迎信息的设置
更改配置文件/etc/issue，显示在登陆之前：
* \d 本地端时间的⽇期；
* \l 显⽰第几个终端机接⼜；
* \m 显⽰硬件的等级 (i386/i486/i586/i686...)；
* \n 显⽰主机的网络名称；
* \o 显⽰ domain name；
* \r 操作系统的版本 (相当于 uname -r)
* \t 显⽰本地端时间的时间；
* \s 操作系统的名称；
* \v 操作系统的版本
更改配置文件/etc/motd,成功登陆后显示信息
> bash的环境配置文件
* login与non-login shell读取的配置文件不一致
```
login shell: 取得bash需要完整的登录流程//需要输入用户的账号密码
non-login shell: 不需要重复登录
```
#### login shell下
> login shell读取的配置文件
* /etc/profile 用于系统的整体设定，不建议修改
* \~/.bash_profile或\~/bash_login或\~/.profile 用于个人设定，可以修改自己的配置信息
```
登陆后先读取整体的配置文件/etc/profile，再按顺序读取每个人的配置文件
```
> 读入环境配置文件
* /etc/profile，\~/.bash_profile都是在取得login shell的时候才会读取的配置文件，所以添加后需要注销登录才能生效。
* 可以用source命令或'.'使配置文件立即生效
```
[umiz@centos7-1 ~]$ source ~/.bashrc
[umiz@centos7-1 ~]$ . ~/.bashrc
```
#### non-login shell下
> non-login shell读取的配置文件
* ~/.bashrc  //会调用/etc/bashrc
* /etc/bashrc
#### 终端环境配置
在登陆中断后，可以退格删除、ctrl+c强行终止命令运行，可以通过stty查看当前环境的按键设置
> stty -a
* 列出目前所有的按键与内容
> set 选项
* 除了显示变量外，还可以通过set设定整个指令的输入/输出环境
```
-u 当使用未设定变量，会显示错误信息
-v 在信息被输出前，会先显示信息的原始内容
-x 指令被执行前，会先显示指令内容
相应的，如果-u启用，再+u即关闭
[umiz@centos7-1 ~]$ set -u
[umiz@centos7-1 ~]$ echo $god
bash: god: 为绑定变量
[umiz@centos7-1 ~]$ god="the world"
[umiz@centos7-1 ~]$ echo $god
the world
[umiz@centos7-1 ~]$ set -v
__vte_prompt_command
[umiz@centos7-1 ~]$ echo $god
echo $god
the world
__vte_prompt_command
[umiz@centos7-1 ~]$ set -x
输出一大堆东西
[umiz@centos7-1 ~]$ echo $god
echo $god
+ echo the world
the world
等等一大堆东西
```
#### 通配符与特殊符号
> ctrl+ //貌似不分大小写
* c 终止当前命令
* d 输入结束EOF //意义不同
* m 相当于enter
* s 暂停屏幕输出
* q 恢复屏幕输出
* u 在提示字符下，将整列命令删除
* z 暂停目前的命令
> 通配符
* \* 代表“0到无穷多个”任一字符
* ? 代表“一定有一个”任意字符
* \[\] 代表一定有一个括号内的字符，如\[abc\]
* \[-\] 代表编码顺序内的所有字符，如\[0-9\]代表0到9的所有数字
* \[^\] 表示反向选择，如\[^abc\]
//中括号这几个都是只占一个字符
> 特殊符号
* # 注释
* \ 转义
* | 管道，分隔两个管道命令的界定
* ； 连续命令下达分隔符
* ~ 用户家目录
* ￥ 取用变量前导符
* & 将命令变成后台工作
* ！ 逻辑运算意义上的“非”
* {} 内容是命令区块的组合
* () 中间是子shell的起始与结束
* \` \` 中间是可以先运行的命令，也可用$()
* " " 具有变量置换的功能
* ' ' 不具有变量置换的功能
* \<,\<< 数据流重导向，输入导向
* \>,\>> 数据流重导向，输出导向，分别是新建和追加
* / 目标符号，路径分隔符号
### 输入输出重定向
---
> 输入输出重定向
* 不使用系统的标准输入端口、标准输出端口或标准错误端口，重新进行指定
* 输入重定向 重新指定设备/文件代替键盘作为新的输入
* 输出重定向 重新指定设备/文件代替显示器作为新的输出
> 输出重定向实现
* 标准输出 命令运行所返回的正确信息；代码为1，使用>或>>
* 标准错误输出 命令运行失败后返回的错误信息，代码为2，使用2>或2>>
```
命令 > 文件   命令执行的标准输出结果重定向到指定文件，若文件已包含数据，会清空再写入
命令 >> 文件  同上，但是不会清空，会写在原有内容后面
命令 2> 文件  命令执行的错误输出结果重定向到指定文件，若文件已包含数据，清空再写入
命令 2>> 文件 同上，但不会清空，会写在原有内容后面
命令 >> 文件  2>&1或者 命令 &>> 文件：将标准输出或者错误输出写入到指定文件，如
果该文件中已包含数据，新数据将写入到原有内容的后面。注意，第一种格式中，最后的
2>&1 是一体的，可以认为是固定写法。
特殊的重定向位置/dev/null 不想显示也不想存储的信息可以重定向到这里，看作回收站
```
> 输入重定向实现
* 标准输入 代码为0，使用<或<<
```
命令 < 文件：将指定文件作为命令的输入设备
命令 << 分界符：表示从标准输入设备（键盘）中读入，直到遇到分界符才停止（读入的数据不包括分界符），这里的分界符其实就是自定义的字符串
命令 < 文件 1 > 文件 2: 将文件 1 作为命令的输入设备，该命令的执行结果输出到文件 2 中。
常用cat：cat < new.txt > result.txt
cat << eof > result.txt
```
### 多命令间的逻辑关系
---
#### 命令执行的判断依据
* 某些情况下需要一次执行多条命令
* 常见：sync; sync; shutdown -h
> 命令间逻辑关系
* cmd1;cmd2; 命令间不存在相关性
* cmd1 && cmd2 cmd1执行正确则执行cmd2，否则不执行
* cmd1 || cmd2 cmd1执行错误则执行cmd2，否则不执行
* 判断命令执行正确与否，依据变量$?的值,为0表示正确，否则表示不正确。
```
[umiz@centos7-1 cmdtest]$ ls abc && touch abc/test
ls: 无法访问abc: 没有那个文件或目录
[umiz@centos7-1 cmdtest]$ ls abc || mkdir abc
ls: 无法访问abc: 没有那个文件或目录
[umiz@centos7-1 cmdtest]$ ls
abc
```
### 管道命令
---
> 什么是管道命令
* 许多linux命令具有过滤性，可以把自身的输出作为下一条命令的输入
* cmd1 | cmd2 | cmd3
> 管道命令的限制
* 仅处理标准输出，忽略标准错误输出
* 管道命令必须能够接收来自另一个指令的数据作为其标准输入继续处理才可以。
* 常用管道命令：
```
more、less、cut、sort、grep
而ls、cp、mv等不能接受前一个指令的数据，因此不是
```
#### 常用管道命令（截取）
> cut 选项 file/标准输入
* 将同一行的数据进行切分
* 选项
```
-d ：自定义分隔符，默认为制表符。
-f ：与-d一起使用，指定显示哪个区域。
-c ：以字符为单位进行分割, 5-10
[umiz@centos7-1 cmdtest]$ echo $PATH
/usr/local/bin:/usr/local/sbin:/usr/bin:/usr/sbin:/bin:/sbin:/home/umiz/.local/bin:/home/umiz/bin
[umiz@centos7-1 cmdtest]$ echo $PATH | cut -d ':' -f 2
/usr/local/sbin
[umiz@centos7-1 cmdtest]$ echo $PATH | cut -c 10
l
[umiz@centos7-1 cmdtest]$ echo $PATH | cut -c 5-15
/local/bin:
```
> grep 选项 查找模式 文件名或标准输入
* 用于查找内容包含指定的范本样式的文件，如果发现某文件的内容符合所指定的范本样式，预设 grep 指令会把含有范本样式的那一列显示出来。
```
-v：列出不匹配的行。
-c：对匹配的行计数。
-l：只显示包含匹配模式的文件名。
-h：抑制包含匹配模式的文件名的显示。
-n：每个匹配行只按照相对的行号显示。
-i：对匹配模式不区分大小写。
```
#### 常用管道命令（排序）
> sort 选项 文件/标准输入
* 对文本内容进行排序，以行为单位
```
-b 忽略每行前面开始出的空格字符。
-f 排序时，将小写字母视为大写字母。
-M 将前面3个字母依照月份的缩写进行排序。
-n 依照数值的大小排序。
-u 意味着是唯一的(unique)，输出的结果是去完重了的。
-o<输出文件> 将排序后的结果存入指定的文件。
-r 以相反的顺序来排序。
-t<分隔字符> 指定排序时所用的栏位分隔字符。
范例1：将/etc/passwd中的内容以：来分隔，并按照第3栏排序
cat /etc/passwd | sort –t ‘:’ –k 3
范例2：将/etc/passwd中的内容以：来分隔，并按照第3栏数字大小排序
cat /etc/passwd | sort –t ‘:’ –nk 3
```
> uniq 选项
* 用于检查并删除文本文件中重复出现的行列，一般与sort命令结合使用
```
-i 忽略大小写字符的不同
-c 进行计数
```
#### 常用管道命令（双向重导向）
> tee 选项 filename
* 将前一条指令的结果输出到标准输出设备，并保存为文件
* -a 以累加方式将数据加入filename
```
[umiz@centos7-1 cmdtest]$ ls
abc  test1.c
[umiz@centos7-1 cmdtest]$ ls -al | tee -a myfile | more
总用量 8
drwxrwxr-x.  3 umiz umiz   32 12月  5 21:25 .
drwx------. 22 umiz umiz 4096 12月  5 20:37 ..
drwxrwxr-x.  2 umiz umiz    6 12月  5 20:38 abc
-rw-rw-r--.  1 umiz umiz  103 12月  5 21:25 test1.c
```
#### 常用管道命令（字符串转换）
> tr 选项 字符串
* 读取命令输出，经字符串转译，结果输出到标准输出设备
* -d 删除信息中的字符串
* -s 取代重复的字符
```
[umiz@centos7-1 cmdtest]$ ls -al
总用量 12
drwxrwxr-x.  3 umiz umiz   46 12月  5 21:42 .
drwx------. 22 umiz umiz 4096 12月  5 20:37 ..
drwxrwxr-x.  2 umiz umiz    6 12月  5 20:38 abc
-rw-rw-r--.  1 umiz umiz  209 12月  5 21:42 myfile
-rw-rw-r--.  1 umiz umiz  103 12月  5 21:25 test1.c
[umiz@centos7-1 cmdtest]$ ls -al | tr -d 'umiz'
总用量 12
drwxrwxr-x.  3     46 12月  5 21:42 .
drwx------. 22   4096 12月  5 20:37 ..
drwxrwxr-x.  2      6 12月  5 20:38 abc
-rw-rw-r--.  1    209 12月  5 21:42 yfle
-rw-rw-r--.  1    103 12月  5 21:25 test1.c
[umiz@centos7-1 cmdtest]$ ls -al | tr 'umiz' 'uuu'
总用量 12
drwxrwxr-x.  3 uuuu uuuu   46 12月  5 21:42 .
drwx------. 22 uuuu uuuu 4096 12月  5 20:37 ..
drwxrwxr-x.  2 uuuu uuuu    6 12月  5 20:38 abc
-rw-rw-r--.  1 uuuu uuuu  209 12月  5 21:42 uyfule
-rw-rw-r--.  1 uuuu uuuu  103 12月  5 21:25 test1.c
```
> col 选项
* 用于过滤控制字符，通常用来把【tab】转成空白
* -x 将tab键转换成对等的空格键
```
cat –A /etc/man_db.conf # 显示所有特殊按键
cat /etc/man_db.conf | col –x | cat –A | more # 替换tab
```
#### 常用管道命令（分区命令）
> split 选项 filename PREFIX
* 切分大文件
* -b 接想要区分成的文件大小，可加单位如b,k,m
* -l 以行数进行分区
* PREFIX 前导符，作为分区文件的前导文字
```
/etc/service文件分成300k一个
cd /tmp; split –b 300k /etc/services services
合并：cat services* >> serviceback
ls -al /etc | split -l 10 - etc #每10条记录一个文件
```
> xargs 选项 cmd
* 产生某个指令的参数，将stdin的数据以空格字符或断行字符分隔成argument
```
-0:将stdin的特殊字符还原成一般字符；
-e:end of file，后面可以接字符串，xargs分析到这个字符串时，会停止工作。
-p：在执行每个指令的argument时，都会询问使用者
-n:后面接次数，每次command指令执行时，需要使用几个参数的意思
```
### 正则表达式
---
> 基本概念
* 处理字符串的方法，以行为单位对字符串进行处理，通过一些特殊符号的辅助，搜索/删除/取代某特定字符串的处理程序
* 表示方法，工具程序支持该方法即可对字符串进行处理，如vi、grep、awk、sed
> 用途
* 分析日志、简单的垃圾邮件过滤、软件（系统）配置等
> 常用字符
* ^ 以什么开头
* $ 以什么结尾
* . 匹配一个字符
* * 匹配0个或多个字符
* \[] 匹配集合中的
* \[x-y] 匹配集合范围中的
* \[^ ] 匹配不在集合中的
* \ 转义
> 常用字符
* \[:alnum:] 代表A-Z，a-z，0-9
* \[:alpha:] 代表A-Z，a-z
* \[:blank:] 代表空格键和tab键
* \[:cntrl:] 代表键盘上的控制按键，包括CR、LF、Tab、del等
* \[:digit:] 代表0-9
* \[:graph:] 除了空格键、tab键的所有按键
* \[:lower:] 代表a-z
#### 实际运用
> 搜索文件中的字符串
* 搜索指定字符串
```
grep -n 'the' man_db.conf
```
* 含有元字符的搜索
```
grep -n 't[ae]st' man_db.conf
grep -n '[^g]oo' man_db.conf
grep -n '[^a-z]' man_db.conf //a-z都不能有
grep -n '[^[:lower:]]oo' man_db.conf
grep -n '[^[:digit:]]' man_db.conf
```
* 指定开始结尾字符串的搜索
```
grep -n '^the' man_db.conf
grep -n '[a-z]&' man_db.conf
grep -n '[^a-z]' man_db.conf //a-z都不能有
```
* 含有任意、重复字符的字符串搜索
```
grep –n ‘g..d’ regular.txt
grep –n ‘o*’ regular.txt
```
* 给定字符范围的字符串搜索
```
{}能指定字符个数，但它在shell中有特殊意义，因此需要用\转义
grep –n ‘o\{2\}’ regular.txt
grep –n ‘o\{2,5\}’ regular.txt
```
#### 正则表达式与通配符的区别
* 通配符是bash接口的一个功能
* 正则表达式是一种对字符串的处理方法
* 其中包括\*以及？等都有区别
```
通配符中，*代表0~无限多个字符， 而正则中*标识重复前一个字符多次
通配符中，?标识任意一个字符， 正则中则用’.’表示
```
#### sed工具
* 利用脚本处理文本文件
* 依照脚本指令处理、编辑文本文件
* 主要用来自动编辑一个或多个文件，简化对文件的反复操作，编写转换程序等
> sed \[-hnV]\[-e<script>]\[-f<script文件>]\[文本文件]
```
-e<script>或--expression=<script> 以选项中指定的script来处理输入的文本文件。
-f<script文件>或--file=<script文件> 以选项中指定的script文件来处理输入的文本文件。
-h或--help 显示帮助。
-n或--quiet或--silent 仅显示script处理后的结果。
-V或--version 显示版本信息。
```
> 动作说明
* a 新增 a后面接字串，出现在目前的下一行
* c 取代 c后面接字串，取代n1-n2之间的行
* d 删除
* i 插入 i后面接字串，出现在目前的上一行
* p 打印 讲某个选择的数据印出，通常和sed -n一起运行
* s 取代 直接进行取代工作
> 应用举例
* 以行为单位

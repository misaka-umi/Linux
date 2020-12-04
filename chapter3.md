## vi编辑器及Linux C开发工具
### vi编辑器的使用
---
#### 介绍
* vi编辑器是Linux和Unix最基本的文件编辑器，可执行各种文本操作，并且不需要图形界面，它在系统和服务器管理中的功能是其他图形编辑器无法比拟的。
* vim是vi加强版，可用不同颜色或底行显示特殊信息，支持多种程序的语法检测
> 优势
* unix like系统都会内建	//老IE了，但是好用，微软拉了
* 很多软件的编辑接口都会system call vi编辑器    //老IE了
* vim能够进行程序编辑，可以透过颜色辨别语法正确性    //建议正儿八经编程，拿vi编程有病吧
* 程序简单，所以编辑速度快    //一加出来挨打。
#### 工作模式
分为命令模式、插入模式、底行模式
> 命令模式 
* 输入的字符会被判断成命令，且不显示
* 常用命令
![](https://github.com/misaka-umi/Linux/blob/main/vi-1.png) 
![](https://github.com/misaka-umi/Linux/blob/main/vi-2.png)
![](https://github.com/misaka-umi/Linux/blob/main/vi-3.png)
```
谁爱整理谁整理，傻逼玩意儿真他妈多
```
> 插入模式 
* 所输入的都会被判断成正在编辑的文件内容
* 在此模式下进行文本的输入操作
* 通过"esc"键返回命令模式

> 底行模式
* 在命令模式下输入冒号/问号/斜杠可进入底行模式
* 常用指令
```
:w 将编辑的数据写入硬盘//保存
:w! 强制写入，能否写入与权限有关
:q 离开vi
:q! 强制离开//不保存
:wq 保存并离开
:wq! 强制保存并离开
```
* 次级常用指令
```
ZZ			文件若无改动，则不保存离开；若有改动，则保存离开
:w			filename 储存成另一个文件//另存为
:set nu     显示行号
:set nonu   取消行号
/word 		在光标之下寻找word字符串
?word 		在光标之上寻找word字符串
:n1,n2s/word1/word2/g 在n1到n2列之间寻找word1并把它替换成word2
:1,$s/word1/word2/gc  在第一列到最后一列之间寻找word1并替换成word2，并让用户confirm
```
#### vi暂存文件
编辑文件时会在同文件下生成.filename.swap，若不正常退出/掉线，可以此文件救援
下次编辑文件时会出现如下提示：
* Oopen Read-only：通过只读方式打开，可多人同时查看
* Edit anyway：正常方式打开文件，但不会载入暂存文件，可能出现两个使用者互相改变对方的文件问题。
* Recover：加载暂存文件，将之前未存储内容找回。但要手动删除swap文件
* Delete it: 确定暂存文件误用可以直接删除。
* Quit：离开vim
* Abort:和quit类似，回到命令行
#### vi编辑器扩展功能
> 区块选择
* v      字符选择，会将光标经过处反白选择
* V      行选择，会将光标经过的行反白选择
* Ctrl+v 区块选择，以长方形方式选择文件内容
> 多文本编辑器/当vi打开多个文件时
* :n     编辑下一个
* :N     编辑上一个
* :files 列出打开的所有文件
```
还有许多命令
```
> 多窗口功能
* :sp filename  //ctrl+w+(上/下）
```
若无该文件则创建新文件
```
#### vi编辑器常用设置
* 使用vi编辑器会在家目录下生成.viminfo文件，记录访问设置、搜索记录等
* 使用set all可以查看所有环境的设置
* 配置文件/etc/vimrc
### Linux C 开发工具
---
#### 概述
* GCC全称GNU Compiler Collection，GNU编译套件。
* GCC是由GNU开发的编程语⾔编译器，包括C、Cpp、Objective-C、Fortran、Java、Ada、Golang。
#### 编译过程
> 可直接编译
``` 
gcc 现有文件 -o 目标文件
```
> 四阶段
* 预处理(ccp) gcc -E filename -o filename.i
* 编译器(gcc) gcc -S filename.i -o filename.s
* 汇编器(as)  gcc -c filename.s -o filename.o
* 链接器(ld)  gcc filename.o -o filename
> 执行
* ./filename
#### 多文件编译
> 格式一：多文件同时编译
```
gcc 1.c 2.c 3.c –o test
```
> 格式2：每个文件分别编译，并链接成可执行文件
```
gcc –c 1.c –o 1.o
gcc –c 2.c –o 2.o
gcc –c 3.c –o 3.o
gcc 1.o 2.o 3.o –o test
```
### 静态与动态链接库
---
#### Linux库的创建和使用
> 什么是库
* 写好的代码文件编译后可以直接调用，本质是可执行代码的二进制形式那么就可以写入内存
* 系统提供的库的路径
```
/usr/lib
/usr/lib64
```
* Linux库文件名的组成
```
前缀(lib) + 库名 + 后缀（.a静态库；.so动态库）
libmm.a: 库名为mm的静态库
libnn.so: 库名为nn的动态库
```
> 静态库与动态库的区别
* 静态库被调用是完全拷贝，一次链接即可，代码体积大，内存中会调用函数多个副本
* 动态库类似快捷方式，程序执行时在调用时载入，代码体积小，被调用函数在内存中只有一个副本
> 静态库的创建
* 在头文件中声明静态库中导出的函数
* 在源文件中实现






### 分布式版本控制系统git
---
#### git简介
开源的分布式版本控制系统，Linus Torvalds为帮助管理Linux内核开发所开发的版本控制软件。
与常用的版本控制工具不同，采用了分布式版本库，不需要服务器端软件支持。
#### git安装
> 安装
```
sudo yum install git //centos
sudo apt-get install git //ubuntu
```
> 配置
```
git config --global user.email 'saudhius@gamil.com'
git config --global user.name 'asioj'
```
#### git使用
```
mkdir dir1 //创建工作目录（工作区）
cd dir1
git init // 初始化，生成.git目录（用于跟踪管理版本库）

git config --global user.email 'saudhius@gamil.com'
git config --global user.name 'asioj' //配置邮箱和用户名

//工作区 →(add)→ stage →(commit)→ master

touch README.txt //新建readme
git add README.txt // 添加进stage（暂存区）
git commit -m "This is README.txt" //将暂存区所有文件提交到当前的分支
git status //查看状态

//修改

vi README.txt //修改
git status
git diff //用diff格式显示此时的文件与已提交的有什么不同
git add README.txt
git commit -m "change sth" //修改了什么

//开发时有时需退回到之前的版本

git log //查看提交记录，并复制要退回的版本号
git reset --hard HEAD^ //直接退回到上一个版本号
git reset --hard 版本号 //进入指定修改版本
//尽管git log 与git reflog的版本号显示方式不同，
但前者的commit 较长字符 与 后者的最前面的较短的字符
都是版本号

//撤销修改
vi a.txt //修改文件
git checkout -- a.txt //使该文件回到最后一次git commit或git add时的状态
touch b.txt
git add b.txt
git reset HEAD b.txt //撤销b.txt的git add，即从暂存区撤销
git rm a.txt //将提交到版本库的文件删除
(可用git checkout -- filename从版本库恢复)//没能恢复

//远程操作，上传到github

//首先应在github上创建和该项目同名的repository
git remote add origin git@github.com:misaka-umi/项目名
git push -u origin master //第一次提交

//添加配置

//远程克隆，将他人/开源的项目克隆到本地开发
git clone git@github.com:user_name/reposity_name

//分支管理,每次提交都是一条时间线，只有一个分支叫做master
git checkout -b branch_name //创建一个名为branch——name的分支
git branch                  //查看所在分支
git checkout master         //切换回master
git merge branch_name       //合并分支
git branch -d branch_name   //删除分支

//冲突管理，多分支修改相同文件，合并时会冲突
git checkout -b bra1
vi README.txt //修改
git add README.txt
git commit -m 'change'
git checkout master
vi README.txt  //修改
git merge bra1 //出现冲突
git status     //检查
//提交解决冲突后的文件，删除分支
```
> ssh
```
Secure Shell(SSH) 是由 IETF(The Internet Engineering 
TaskForce) 制定的建立在应用层基础上的安全网络协议。它
是专为远程登录会话(甚至可以用Windows远程登录Linux服务
器进行文件互传)和其他网络服务提供安全性的协议。
ssh user@host
```
### 调试器GDB的使用
---
> 介绍
* GNU开源组织发布的Linux下的程序调试工具
* 可以在程序中设置断点、查看变量值，追踪执行过程
* 利用上述功能可方便找出非语法错误
> gdb的使用
```
gcc -g test1.c -o test1 //必须有-g，才能使用gdb
./test1 //运行出错，但代码无语法错误
gdb -q //-q是关闭版权信息
//进入gdb，前缀有（gdb），不用自己输入，下文省略

list        //输出上次调用list的后10行代码
list -      //输出上次调用list的前10行代码
list n      //输出第n行附近的10行代码
list 函数名 //输出函数附近的10行代码
list 5，10  //输出第5到第10行的代码

//搜索字符串
list 1，1              //将当前行设置为第一行
serach get_sum         //搜索第一行以后包含get_sum字符串的行（不包含第一行）
forward get_sum        //同上
reverse-search get_sum //从当前行向前查找第一个匹配的

//执行
run

//设置断点
break n                //运行到指定行不再执行
break 函数名           //运行到该函数之前就不再运行，停在该函数第一行
break n/函数名 if条件
break get_sum if i==99 //执行到get_sum时，若i==9成立则停止
watch 条件表达式       //run之后才能设置，且要保证条件表达式中的变量已经使用过

//管理断点
info breakpoints //查看当前设置的断点
disable 断点编号 //使该断点失效
enable 断电编号  //使该断点有效
clear 行号       //删除此行断点
delete           //删除程序中所有断点
delete 断电编号  //删除该断点，多个断点则空格隔开

//查看和设置变量的值
程序运行到中断点的时候查看
* print
print 变量/表达式 //打印变量或表达式的值
print 变量=值     //对变量赋值
* whatis
whatis 变量/表达式 //显示变量或表达式的数据类型
* set
set variable 变量=值 //给变量赋值

//控制程序的执行
continue //使程序运行到下一个断点或完成运行
kill     //结束当前程序的调试（中断该次运行）
next/step //一次一条执行该程序代码

//帮助命令
help 类名 //直接输入help可查看类名

quit //退出
```
### 工程管理器make的使用
---
#### make概念
* 诞生于1977年，主要用于C语言项目
* 通过makefile文件描述源程序之间的相互关系并自动编译
* 若之后修改个别文件，会自动检测哪些修改过，只对这些文件编译
#### 使用
```
make a.txt //制作文件a.txt，但此时make本身并不知道
假设a.txt依赖于b.txt和c.txt，是后面两个文件连接的结合
那么make需知道如下规则：
a.txt: b.txt c.txt 
cat b.txt c.txt > a.txt //输出重定向
这样的规则应写入makefile文件，同时也可指定其他名字
make -f rules.txt 
make --file=rules.txt
//规则形式
<target>:<prerequisites> //目标：前置条件
[tab]<command>  //此处tab是按键，然后输入命令，前置条件和命令必须有一个，目标是必须有的
```
#### 文件格式
> target
* 一个target构成一个规则，通常是文件名，指出命令要构建的对象，多个文件就用空格隔开
* 除了文件名还可以是某项操作，“伪目标”
```
clean:
  rm *.o
 //删除文件
但如果有个clean文件，仍然要使命令为目标则
.PHONY: clean //声明clean是伪目标
//makefile目标往往有很多种，若make命令未指定，则会执行第一个目标
```
> prerequisites
* 前置条件是一组文件名，用空格分隔
* 只要前置文件不存在或更新过，目标就要重新构建
```
result.txt: source.txt
  cp source.txt result.txt
构建source.txt的前置条件是source.txt，若路径下有该文件则可运行，
否则就要再写一条规则来生成source.txt
source.txt:
  echo"this is the source">source.txt
source.txt无前置条件，只要source.txt不存在，
每次调用make source.txt都会生成source.txt
```
* 若连续执行两次上述的make result.txt
```
第一次会先创建source.txt再创建result.txt
第二次执行，make会发现source.txt没有更新就会不执行任何操作
```
> command
* 命令表示如何更新目标文件，由一行或多行shell命令组成
* 构建“目标”的具体指令，结果通常是生成目标文件
* 每行命令前都要有tab键，若想换其他键需要用.RECIPEPREFIX声明
```
每⾏命令都是在⼀个单独运⾏的Shell中执⾏的，这些Shell间没有继承关系。
var:
export foo=bar
echo “foo=[$$foo]”
结果：
解决⽅法：
(1) ;
(2) \ # “;”后可以换行
(3) .ONESHELL
```
> 使用实例（构建）
```
cat makefile //查看makefile内容
main: main.o module1.o module2.o
  gcc main.o module1.o module2.o -o main
main.o: main.c head1.h head2.h common_head.h
  gcc -c main.c //会生成main.o
module1.o: module1.c head1.h
  gcc -c module1.c
module2.o: module2.c head2.h
  gcc -c module2.c

make //第一次执行
gcc -c main.c
gcc -c module1.c
gcc -c module2.c
gcc main.o module1.o module2.o -o main

vi head1.h //修改头文件
make
gcc -c main.c
gcc -c module1.c
gcc main.o module1.o module2.o -o main
```

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
* y = ax+b
> shellbian

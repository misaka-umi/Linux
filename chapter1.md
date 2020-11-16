## Linux基本介绍

#### 内核版本
>major.minor.patch-bulid.desc

```
major: 主版本号，结构性变化时变更
minor: 次版本号，新增功能时改变
patch-build: 释出版本-修改版本
desc: 当前版本的特殊信息，可省略
```
#### 内核模块
>主要由五个模块构成

```
内存管理模块: 合理有效地管理内存，响应对内存分配的请求
进程管理模块: 控制系统进程对CPU的访问
进程间通信模块: 控制不同进程之间在用户空间的同步、数据共享和交换
文件系统模块: 支持对外部设备的驱动和存储
网络接口模块: 提供了对各种网络标准的实现和各种网络硬件的支持
```

#### 命令介绍
>命令字 [选项] [参数]

```
选项: 调节命令的具体功能。'-'引导短格式选项，'--'引导长格式选项（多个字符），如'--color'。
注:多个短格式选项可写在一起，如'-al'
参数: 命令操作的对象，如文件、目录名等。
```

>man

```
功能:查看手册
使用:man 命令名
e.g.man ls
(q键退出)
```

>shutdown

```
功能:关机
reboot:
-k: 不关机，发送警告
-r: 重启
-h: 关机后停机
-c: 取消关机动作
-t seconds: 设定在几秒后关机
time: 设定关机时间
message: 传送给所有使用者的警告讯息
e.g. shutdown -k now 'just notice'
e.g. shutdown -r +30 'restart after 30 mins'
e.g. shutdown -h +10 'shutdown after 10 mins'
```
>poweroff 、 reboot 、 halt （shutdown)


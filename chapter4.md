## 用户与群组管理
### 用户账户与群组概念
---
#### 理解用户账户与群组
> 用户账户
```
Linux操作系统是多用户多任务的操作系统，
用户账户是身份标识，通过用户账户可以登
录到系统中并且访问已授权的资源，系统依
据账户区分每个用户的文件进程和任务，提
供特定的工作环境
```
* 普通用户 | 只能进行普通工作，访问他们拥有的或有权限执行的文件
* 超级用户 | root用户，也叫管理员账户，任务是对普通用户和整个系统进行管理，具有绝对控制权
* 系统用户 | 与系统服务有关，不能用于登录
> 群组
* 具有相同特性的用户的逻辑集合，按组群划分并管理，提高工作效率
* 一个用户账户可以是多个组群的成员，其中某组群为该用户的主组群（私有组群），其他的组群则为附属组群（标准组群）
```
root用户UID为0，系统用户UID从1到999，
普通用户UID可以在创建时指定，若不指定
则从1000开始顺序编号，创建用户账户的
同时也会创建一个与用户同名的组群，该
组群是用户的主组群，普通组群的GID默认
从1000开始编号。
```
### 用户账户与群组文件
---
#### 用户账户文件
> /etc/passwd
* 用户名：加密口令：UIG：GID：用户的描述信息：主目录：命令解释器（登录shell）
```
root:x:0:0:root:/root:/bin/bash //root用户
bin:x:1:1:bin:/bin:/sbin/nologin //标准账户
user1:x:1001:1001::/home/ueser1:/bin/bash //root用户创建的user1
```
> /etc/shadow
* 由于所有用户都能读取/etc/passwd，因此加密之后的口令都存放在该文件夹，只对root用户可读
* 分为九个域，以":"隔开，每个用户的信息一行，九个域分别是：
1. 用户登陆名
2. 加密后的用户口令，\*为非登录用户，！！表示无密码
3. 1970年1月1日起，到用户最近一次口令被修改的天数
4. 从1970年1月1日起，到用户可以更改密码的天数，即最短口令存活期
5. 从1970年1月1日起，到用户必须更改密码的天数，即最长口令存活期
6. 口令过期前几天提醒用户更改口令
7. 口令过期后几天账户被禁用
8. 口令被禁用的具体日期（相对日期，从1970年1月1日至禁用时的天数）
9. 保留域，用于功能扩展
```
root:$6$PQxz7W3s$Ra7Akw53/n7rntDgjPNWdCG66/5RZgjhoe1zT2F00ouf2iDM.AVvRIYoez10hGG7kBHEaah.oH5U1t6OQj2Rf.:17654:0:99999:7:::
bin:*:16925:0:99999:7:::
daemon:*:16925:0:99999:7:::
bobby:!!:17656:0:99999:7:::
user1:!!:17656:0:99999:7:::
```
#### 群组文件
> /etc/group
* 存放用户的组账户信息，该文件的内容任何用户都能读取，每个组一行分为四个域
* 组群名称：组群口令：GID：组群成员列表
* 用户所在的主组群不会列出该用户，只会列将它作为附属组群的用户
```
root:x:0:
bin:x:1:
daemon:x:2:
user1:x:1001:user2,user3
user2:x:1002:
user3:x:1003:user2
```
> /etc/gshadow
* 存放组群的加密口令、组管理员等信息，仅root用户可读取
* 每个组群只占一行，以":"分隔4个域
* 组群名称：加密后的组群口令（没有就是"!"）：组群管理员：组群成员列表
```
root:::
bin:::
user1:!::user2,user3
user2:!::
user3:!::user2
```
![](https://github.com/misaka-umi/Linux/blob/main/same.png)
### 用户账户与群组管理
---
#### 用户账户管理
> useradd
* 新建用户
* useradd 选项 username
```
useradd -u 1004 -g 1004 -G 1002,1003 -p 123455 -f -1 user3
tail -1 /etc/passwd
user3:x:1004:1004::/home/user3:/bin/bash
//同时有附属组群1002，1003，密码为123456，永不过期
```
![](https://github.com/misaka-umi/Linux/blob/main/useradd.png)
> passwd
* 指令和修改用户账户的口令，root可以为自己和其他用户设置，普通用户只能为自己设置
* passwd 选项 username
```
-l 锁定（停用）用户账户
-u 口令解锁
-d 将用户口令设置为空，这与未设置口令的账户不同。
   未设置口令的账户无法登录系统，而口令为空的账户可以
-f 强迫用户下次登录时必须修改口令
-n 指定口令的最短存活期
-x 指定口令的最长存活期
-w 口令要到期前提前警告的天数
-i 口令过期后多少天停用账户
-S 显示账户口令的简短状态信息
```
> usermod
* 修改用户账户信息
* usermod 选项 用户名
```
-c 填写用户账户的备注信息
-d -m 参数-m与参数-d连用，可重新指定用户的家目录并自动把旧的数据转移过去
-e 账户的到期时间，格式为YYYY-MM-DD
-g 变更所属用户组
-G 变更扩展用户组
-L 锁定用户禁止其登录系统
-U 解锁用户，允许其登录系统
-s 变更默认终端
-u 修改用户的UID
```
> userdel
* 删除用户账户
* userdel -r username
```
-r 选项，删除用户账户的同时还会将用户主目录及包含的所有文件和目录都删掉
```
> 禁用和恢复用户账户
* passwd
```
passwd -1 user1 //锁定用户密码
passwd -u user1 //重新启用
```
* usermod
```
usermod -L user1 //锁定
usermod -U user1 //恢复
* 修改/etc/passwd文件，passwd域添加"*"
```
#### 查看账户信息
> id
* 查看当前用户的uid、gid、所属群组信息，若不指定则显示当前用户
* id username
> whoami
* 查看当前用户名
* whoami
> w
* 查看当前登录系统用户和详细信息
* w
#### 群组管理
> 添加群组
* groupadd 选项 新群组名
> 删除群组
* groupdel 选项 群组名
> 修改群组
* groupmod 选项 群组名
```
groupmod -g 1003 u1 //将u1组gid改为1003
groupmod -n newname u1 //将u1组改名为newname
```
> 为群组添加用户
* gpasswd 选项 用户 组
* 在附属组中增加删除用户使用该命令，仅有root用户和组管理员可使用
```
-a 将用户加入
-d 将用户移除
-r 取消组的密码
-A 给组指派管理员
```
#### 初始群组和有效群组
* 创建用户时，用户被分配初始群组GID，加入其他群组后拥有其他群组的权限
* 创建文件时，文件群组为有效群组
```
groups //查看有效群组
newgrp 群组 //切换有效群组
```
### 用户身份切换
> su - username
* 用户在不退出的情况下切换到其他用户
* root切换到普通用户不需密码，反之需要
* 缺陷：会暴露root密码，增加了被获取密码的概率
> sudo 选项 要使用的命令
* 给普通用户临时的root权限
```
-h 列出帮助信息
-l 列出当前用户可执行的命令
-u用户名或UID值 以指定的用户身份执行命令
-k 清空密码的有效时间，下次执行sudo时需要再次进行密码验证
-b 在后台执行指定的命令
-p 更改询问密码的提示语
```
* 使用sudo前需要在配置文件中提供集中的用户管理、权限与主机等参数
```
visudo
90 ##
91 ## Allow root to run any commands anywhere
92 root ALL=(ALL) ALL
93 test ALL=(ALL) /usr/bin/cat  //test用户仅能以root身份执行cat
94 user1 ALL=(ALL) ALL //可以用root身份执行所有命令
//谁可以使用 允许使用的主机=（以谁的身份） 可执行命令的列表
```
* 除了上述修改配置文件，还可以将用户加入到wheel群组
```
visudo 去掉%wheel的注释符号"#"
usermod -G wheel user_name
```
#### 批量创建用户
> newusers
* newusers filename
```
vi filename
user1:x:2222:2004::/var/user1:/bin/tcsh
zhang:x:2005:2005::/home/zhang:/bin/bash
user3:x:1010:1010::/home/user3:/bin/bash
user4:x:2010:2010::/home/user4:/bin/bash
user5:x:2011:2011::/home/user5:/bin/bash
即格式与/etc/passwd相同
```
#### 批量改用户密码
> chpasswd
* cat filename | chapasswd
```
vi filename
user4:123456
user5:123456

---
title: linux常用命令
date: 2022-10-30T15:04:12Z
lastmod: 2022-10-30T15:04:12Z
---

# linux常用命令

---

### 切换目录命令 cd：

- |使用||
  |命令|功能|
  | ------| --------------------|
  |cd app|切换到 app 目录|
  |cd ..|切换到上一层目录|
  |cd /|切换到系统根目录|
  |cd ~|切换到用户主目录|
  |cd -|切换到上一个所在目录|
  |Pwd|查看当前目录|

### 列出文件列表：

##### 【ls ll dir(*****)】

- ls(list)是一个非常有用的命令，用来显示当前目录下的内容。配合参数的使用，能以不同的方式显示目录内容。
  - 格式：ls[参数][路径或文件名]
  - 常用：在 linux 中以 . 开头的文件都是隐藏的文件
    - ls
    - ls -a 显示所有文件或目录（包含隐藏的文件）
    - ls -l 缩写成 ll

### 创建目录和移除目录：

##### 【mkdir rmdir】

- |mkdir(make directory)命令可用来创建子目录||
  |命令|功能|
  | -----------------------------------------| --------------------------|
  |mkdir app|在当前目录下创建 app 目录|
  |mkdir –p app2/test|级联创建 aap2 以及 test 目|
- |rmdir(remove directory)命令可用来删除“空”的子目录||
  |命令|功能|
  | ---------------------------------------------------| -------------|
  |rmdir app|删除 app 目录|

### 浏览文件

##### 【cat、more、less】

- cat:用于显示文件的内容。
  - 格式：cat[参数]<文件名>
    - cat yum.conf
- more:一般用于要显示的内容会超过一个画面长度的情况。按空格键显示下一个画面。回车显示下一行内容。按 q 键退出查看。
  - more yum.conf
    - 空格显示下一页数据 回车显示下一行的数据
- less:用法和 more 类似，不同的是 less 可以通过 PgUp、PgDn 键来控制。
  - less yum.conf
    - PgUp 和 PgDn 进行上下翻页.

##### 【tail】

- tail 命令是在实际使用过程中使用非常多的一个命令，它的功能是：用于显示文件后几行的内容。
  - 用法:
    - tail -10 /etc/passwd 查看后 10 行数据
    - tail -f catalina.log 动态查看日志
    - ctrl+c 结束查看

### 文件操作：

- 这时使用工具来连接 linux 进行操作
- 首先要获取虚拟机的 IP 地址，使用 ifconfig 命令，如果没有 IP 地址，则是没有设置开机时启动网上,设置操作如下

```bash
vim /etc/sysconfig/network-scripts/ifcfg-eth0
TYPE=Ethernet  #以太网
BOOTPROTO=DHCP 	#dhcp方式分配网址
NAME=eth0	#网卡名字
ONBOOT=yes #启动网卡：
wq！
Reboot [重启]
```

　　#####【rm】

- touch a.txt 创建一个文件
- rm 删除文件 - 用法：rm [选项]... 文件... - rm a.txt 删除 a.txt 文件,删除需要用户确认，y/n - rm -f a.txt  不询问，直接删除 - rm -r a 删除目录 - rm -rf a 不询问递归删除 - rm -rf * 不询问删除当前目录所有文件 #####【cp、mv】
- cp(copy)命令可以将文件从一处复制到另一处。一般在使用 cp 命令时将一个文件复制成另一个文件或复制到某目录时，需要指定源文件名与目标文件名或目录。

  - cp a.txt b.txt 将 a.txt 复制为 b.txt 文件
  - cp a.txt ../ 将 a.txt 文件复制到上一层目录中
- mv 移动或者重命名

  - mv a.txt ../ 将 a.txt 文件移动到上一层目录中
  - mv a.txt b.txt 将 a.txt 文件重命名为 b.txt

##### 【tar】命令:(打包或解压)

- tar 命令位于/bin 目录下，它能够将用户所指定的文件或目录打包成一个文件，但不做压缩。一般 Linux 上常用的压缩方式是选用 tar 将许多文件打包成一个文件，再以 gzip 压缩命令压缩成 xxx.tar.gz(或称为 xxx.tgz)的文件。 - 常用参数： - -c：创建一个新 tar 文件 - -v：显示运行过程的信息 - -f：指定文件名 - -z：调用 gzip 压缩命令进行压缩 - -t：查看压缩文件的内容 - -x：解开 tar 文件 - 打包： - tar –cvf xxx.tar ./_ - 打包并且压缩： - tar –zcvf xxx.tar.gz ./_
  - 解压
    - tar –xvf xxx.tar [解压当前目录]
    - tar -xvf xxx.tar.gz -C /usr/aaa 【解压到指定目录】

###### 【grep】命令:查找符合条件的字符串。

- 用法: grep [选项]... PATTERN [FILE]...
- 示例：
  - grep lang anaconda-ks.cfg 在文件中查找 lang
  - grep lang anaconda-ks.cfg –-color 高亮显示

### 其他常用命令

##### 【pwd】

　　显示当前所在目录

##### 【touch】

　　创建一个空文件

- touch a.txt

##### 【ll -h】

　　友好显示文件大小

##### 【wget】

　　下载资料

- wget [http://nginx.org/download/nginx-1.9.12.tar.gz](http://nginx.org/download/nginx-1.9.12.tar.gz)

## Vi 和 Vim 编辑器

### 重定向输出>和>>

- `> 重定向输出，覆盖原有内容`
- `>> 重定向输出，又追加功能`
- 示例：
  - cat /etc/passwd > a.txt 将输出定向到 a.txt 中
  - cat /etc/passwd >> a.txt 输出并且追加
  - ifconfig > ifconfig.txt

### 管道 |

　　管道是 Linux 命令中重要的一个概念，其作用是将一个命令的输出用作另一个命令的输入。

- 示例
  - ls --help | more 分页查询帮助信息
  - ps -ef 查看当前所有运行程序(进程)【笔记题：查看进程命令,也就是当前运行程序】
  - ps –ef | grep java 查询名称中包含 java 的进程
  - ifconfig | more
  - cat index.html | more
  - ps –ef | grep aio

### &&命令执行控制：

　　命令之间使用 && 连接，实现逻辑与的功能。

- 只有在 && 左边的命令返回真（命令返回值 $? == 0），&& 右边的命令才会被执行。
- 只要有一个命令返回假（命令返回值 $? == 1），后面的命令就不会被执行。
- 示例
  - mkdir test && cd test

### 网络通讯命令

- ifconfig 显示或设置网络设备。
- ifconfig 显示网络设备
- ifconfig eth0 up 启用 eth0 网卡
- ifconfig eth0 down 停用 eth0 网卡
- ping 探测网络是否通畅。
- ping -c 4 192.168.0.1
- netstat 查看网络端口【常用】。
- netstat -an | grep 3306 查询 3306 端口占用情况

### 系统管理命令

- date 显示或设置系统时间
- date 显示当前系统时间
- date -s “2014-01-01 10:10:10“ 设置系统时间
- df 显示磁盘信息
- df –h 友好显示大小
- free 显示内存状态
- free –m 以 mb 单位显示内存组昂头
- top 显示，管理执行中的程序
- clear 清屏幕
- ps 正在运行的某个进程的状态
- ps –ef 查看所有进程
- ps –ef | grep ssh 查找某一进程
- kill 杀掉某一进程【杀死 tomcat,后面讲】
- kill 2868 杀掉 2868 编号的进程
- kill -9 2868 强制杀死进程
- du 显示目录或文件的大小。
- du –h 显示当前目录的大小
- who 显示目前登入系统的用户信息。
- hostname 查看当前主机名
  - 修改：vi /etc/sysconfig/network
- uname 显示系统信息。
- uname -a 显示本机详细信息。

　　(依次为：内核名称(类别)，主机名，内核版本号，内核版本，内核编译日期，硬件名，处理器类型，硬件平台类型，操作系统名称)

## Linux 的用户和组

### 用户的管理

- useradd 添加一个用户
- useradd test 添加 test 用户
- useradd test2 -d /home/t2 指定用户 home 目录
- passwd 设置、修改密码
- passwd test 为 test 用户设置密码【mm20170706】

### 切换登录：

- ssh -l test -p 22 192.168.19.128
- su – 用户名 【记下】
- userdel 删除一个用户
- userdel test2 删除 test2 用户(不会删除 home 目录)
- userdel –r test2 删除用户以及 home 目录

### 组管理：

- 当在创建一个新用户 user 时，若没有指定他所属于的组，就建立一个和该用户同名的私有组
- 创建用户时也可以指定所在组
  - groupadd 创建组
  - groupadd public 创建一个名为 public 的组
  - useradd u1 –g public 创建用户指定组
  - groupdel 删除组，如果该组有用户成员，必须先删除用户才能删除组。
  - groupdel public

### id，su 命令

　　#####【id 命令】

- 功能：查看一个用户的 UID 和 GID
- 用法：id [选项]... [用户名]
  - 直接使用id
  - 直接使用id 用户名

　　#####【su 命令】

- 功能：切换用户。
- 用法：su [选项]... [-]用户 [参数]... ]
- 示例：
  - su - u1 切换到 u1 用户，并且将环境也切换到 u1 用户的环境（推荐使用）

　　#####【账户文件】

- /etc/passwd 用户文件
- /etc/shadow 密码文件
- /etc/group 组信息文件 #####【用户文件】
- `root:x:0:0:root:/root:/bin/bash`
- 账号名称： 在系统中是唯一的
- 用户密码： 此字段存放加密口令
- 用户标识码(User ID)： 系统内部用它来标示用户
- 组标识码(Group ID)： 系统内部用它来标识用户属性
- 用户相关信息： 例如用户全名等
- 用户目录： 用户登录系统后所进入的目录
- 用户环境: 用户工作的环境 #####【密码文件】
- shadow 文件中每条记录用冒号间隔的 9 个字段组成.
- 用户名：用户登录到系统时使用的名字，而且是惟一的
- 口令： 存放加密的口令
- 最后一次修改时间: 标识从某一时刻起到用户最后一次修改时间
- 最大时间间隔: 口令保持有效的最大天数，即多少天后必须修改口令
- 最小时间间隔： 再次修改口令之间的最小天数
- 警告时间：从系统开始警告到口令正式失效的天数
- 不活动时间： 口令过期少天后，该账号被禁用
- 失效时间：指示口令失效的绝对天数(从 1970 年 1 月 1 日开始计算)
- 标志：未使用

　　#####【组文件】

- `root:x:0:`
- 组名：用户所属组
- 组口令：一般不用
- GID：组 ID
- 用户列表：属于该组的所有用户

## Linux 的权限命令

### 文件权限

|属主（user）|属组（group）|其他用户|
| ------------| -------------| --------|
|r w x|r w x|r w x|
|4 2 1|4 2 1|4 2 1|

### Linux 三种文件类型：

- 普通文件： 包括文本文件、数据文件、可执行的二进制程序文件等。
- 目录文件： Linux 系统把目录看成是一种特殊的文件，利用它构成文件系统的树型结构。
- 设备文件： Linux 系统把每一个设备都看成是一个文件

### 文件类型标识

- 普通文件（-）
- 目录（d）
- 符号链接（l）
- 进入 etc 可以查看，相当于快捷方式
- 字符设备文件（c）
- 块设备文件（s）
- 套接字（s）
- 命名管道（p）

### 文件权限管理：

- chmod 变更文件或目录的权限。
- chmod 755 a.txt
- chmod u=rwx,g=rx,o=rx a.txt
- chmod 000 a.txt / chmod 777 a.txt
- chown 变更文件或目录改文件所属用户和组
- chown u1:public a.txt ：变更当前的目录或文件的所属用户和组
- chown -R u1:public dir ：变更目录中的所有的子目录及文件的所属用户和组

　　####开机关机命令

- shutdown 关机 shutdown -h 0
- reboot 重启
- init 0~6
  - 0:停机
  - 1:单用户形式，只 root 进行维护
    - test 用户不能使用 init 系统维护命令
  - 2：多用户，不能使用 net file system
  - 3：完全多用户
  - 5：图形化
  - 4：安全模式
  - 6：重启

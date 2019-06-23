---
layout: post
title: "linux常用命令"
tags: [linux]
---

* [1 linux帮助命令](#1)

* * [1.1 man命令](#1.1)

* * [1.2 help命令](#1.2)
 
* * [1.3 type命令](#1.3)

 * [2 文件和目录操作命令](#2)

* * [2.1 ls命令](#2.1)

<h1 id="1">1 linux帮助命令</h1>
**linux帮助命令**

<h2 id="1.1">1.1man命令</h2>
在线查看命令帮助。

man命令是Linux下的帮助指令，通过man指令可以查看Linux中的指令帮助、配置文件帮助和编程帮助等信息。
>(1).语法

~~~bash
man(选项)(参数)
man  [-acdfhkKtwW]  [-m  system]  [-p  string]  [-C  config_file]  [-Mpath]  [-P  pager]  [-S
section_list][section] name ...
~~~
>(2).选项

~~~wget
-a：在所有的man帮助手册中搜索；
-f：等价于whatis指令，显示给定关键字的简短描述信息；
-P：指定内容时使用分页程序；
-M：指定man手册搜索的路径。
~~~
>(3).参数

~~~wget
 @ 数字：指定从哪本man手册中搜索帮助；
 @ 关键字：指定要搜索帮助的关键字。
 ~~~
>(4).实例

~~~bash
 man ls #查看ls命令的帮助信息
 man 1 man 
 man 7 man 
~~~
>(5).代号 代表內容

~~~wget
1  使用者在 shell 中可以操作的指令或可执行档
2  系統核心可呼叫的函数与工具等
3  一些常用的函数(function)与函数库(library)，大部分是 C 的函数库(libc)
4  装置档案的说明，通常在/dev 下的档案
5  设定档或者是某些档案的格式
6  游戏(games)
7  惯例与协定等，例如 Linux 档案系统、网络协定、ASCII code 等等的說明
8  系統管理員可用的管理指令
9  跟 kernel 有关的文件
~~~
>(6).由于手册页 man page 是用 less 程序来看的(可以方便地使屏幕上翻和下翻),  所以 在 man page 里可以使用 less 的所有选项。

<h2 id="1.2">1.2hlep命令</h2>
#help内部帮助命令
内部命令的帮助文档，使用help的格式为。

~~~bash
[root@mail ~]# help cd
~~~

#"--help"选项
大多数外部命令都可以使用--help来获取帮助

~~~bash
date --help 
~~~
<h2 id="1.3">1.3tpye命令</h2>
没有存储位置的为内部命令，可以理解为内部命令嵌入在linux的shell中，所以看不到。

type来判断到底为内部命令还是内部命令

~~~bash
[root@mail ~]# type help     //查看help命令的内外类型
help is a shell builtin      //可以看到help为内部命令
[root@mail ~]# tpye passwd   //查看passwd这条命令是否在linux系统中存在
-bash: tpye: command not found //可以看到passwd的存储位置，因此存在，为外部命令
~~~
<h1 id="2">2 文件和目录操作命令</h1>
<h2 id="2.1">2.1 ls命令</h2>
ls命令——list缩写
>1.1相关参数

~~~wget
ls :列出文件或者目录
"-l"，使用长格式显示
"-a"，显示文件名以"."开头的隐藏文件
"-h"，以human易读格式显示，主要是看容量的时候使用"KB" "MB""GB"，指当前文件夹目录的大小
"-lh" ,文件或者目录大小的, 方便识别
"-lhS" ,文件从大到小排序
"-lg" ,不打印所有者信息
"-ln" ,打印UID和GID
"-l --si" si以1000为单位，而-h以1024为单位。
"-l --block-size=M": 设置文件显示单位
"-li"，显示inode号
"-r"，reverse，改变归类的顺序，例如和-t配合使用，-tr和-t显示顺序是颠倒的。
"-R"，递归列出子目录
"-lX/ -l --sort=extension"：扩展名排序
"-t"，按照修改时间顺序归类文件。
"-d"，列出目录本身的信息，而不是目录里边的内容。
~~~
>1.2字节相关单位：

~~~wget
K = Kilobyte 千字节
M = Megabyte 兆字节
G = Gigabyte 十亿字节
T = Terabyte 兆兆字节
P = Petabyte 10的15次方字节
E = Exabyte 艾字节
Z = Zettabyte 泽它字节或皆字节
Y = Yottabyte 尧字节
~~~
>1.2法： ls  (选项) (参数)

~~~wget
选项：
-a: 显示所有档案以及目录（ls内定将档案或目录名称为“./..”的视为隐藏）
-A: 显示除隐藏文件“./..”以外的所有文件列表
-b: 将文件中的不可输出的字符以反斜线加字符编码的方式输出
-c : 与”-lt“ 选项连用时，按照文件状态时间排序输出目录内容， 排序的依据是文件的索引节点中的ctime 字段。 与”-l“连用时，排序的依据是文件的状态改变时间。
-C: 多列显示输出结果（只有文件名信息）
-d : 仅显示目录名，而不显示目录下的内容列表， 显示符号链接文件本身， 而不显示其指定的目录列表。
-F:  在每个输出项后最佳文件的类型标识符， * 可执行权限的普通文件，/ 表示目录， @ 表示符号链接，|表示命令管道， = 表示sockets 套接字，  普通文件不输出标识符。
-h: 以human易读格式显示， 文件大小以kb,mb显示
-i :  显示文件索引节点号（inode）,一个索引节点代表一个文件
-l : 以长格式显示目录下的内容列表，输出信息：文件名，文件类型，权限模式，硬链接数，所有者，组，文件大小， 文件最后修改时间。
-L : 如果遇到性质为符号链接的文件或目录， 直接列出该链接所造的原始文件或目录
-m: 以逗号分隔每个文件和目录的名称
-n : 以用户标志码和群组识别码替代其名称uid /gid
-r : 以文件名反序排序并输出目录内容列表
-R：递归处理，将制定目录下的所有文件及子目录一并处理
-s : 显示文件和目录的大小， 以区块为单位
-t : 用文件和目录的更改时间排序
~~~
>1.3参数：

~~~wget
目录：制定要显示列表的文件，也可以是具体的目录
~~~
>1.4实例：

~~~bash
ls -lX / -l --sort=extension"：扩展名排序
total 60
lrwxrwxrwx.   1 root root     7 Aug 19  2017 bin -> usr/bin
dr-xr-xr-x.   4 root root  4096 Aug 19  2017 boot
drwxr-xr-x   18 root root  2980 Dec 26 17:53 dev
drwxr-xr-x.  80 root root  4096 Dec 26 17:53 etc
drwxr-xr-x.   3 root root  4096 Sep  1  2017 home
lrwxrwxrwx.   1 root root     7 Aug 19  2017 lib -> usr/lib
lrwxrwxrwx.   1 root root     9 Aug 19  2017 lib64 -> usr/lib64
drwx------.   2 root root 16384 Aug 19  2017 lost+found
drwxr-xr-x.   2 root root  4096 Jun 10  2014 media
drwxr-xr-x.   2 root root  4096 Jun 10  2014 mnt
drwxr-xr-x.   4 root root  4096 Sep  2  2017 opt
dr-xr-xr-x  358 root root     0 Dec 26 17:53 proc
dr-xr-x---.   4 root root  4096 Dec 26 01:12 root
drwxr-xr-x   25 root root   700 Dec 26 17:53 run
lrwxrwxrwx.   1 root root     8 Aug 19  2017 sbin -> usr/sbin
drwxr-xr-x.   2 root root  4096 Jun 10  2014 srv
dr-xr-xr-x   13 root root     0 Dec 26 17:53 sys
drwxrwxrwt.   9 root root  4096 Dec 26 19:36 tmp
drwxr-xr-x.  13 root root  4096 Aug 19  2017 usr
drwxr-xr-x.  21 root root  4096 Dec 25 20:04 var
ls -lhS" ,文件从大到小排序
total 40K
-rw-r--r--  1 root root  15K Nov 30 06:27 zabbix-release-4.1-1.el7.src.rpm
-rw-r--r--  1 root root  14K Oct  2 15:36 zabbix-release-4.0-1.el7.noarch.rpm
drwxr-xr-x  4 root root 4.0K Sep  2  2017 rpmbuild
-rw-------. 1 root root 1.2K Aug 19  2017 anaconda-ks.cfg
ls -l --si : si以1000为单位，而-h以1024为单位。
total 41k
-rw-------. 1 root root 1.2k Aug 19  2017 anaconda-ks.cfg
drwxr-xr-x  4 root root 4.1k Sep  2  2017 rpmbuild
-rw-r--r--  1 root root  14k Oct  2 15:36 zabbix-release-4.0-1.el7.noarch.rpm
-rw-r--r--  1 root root  15k Nov 30 06:27 zabbix-release-4.1-1.el7.src.rpm
ls -l --block-size=M: 设置文件显示单位
total 1M
-rw-------. 1 root root 1M Aug 19  2017 anaconda-ks.cfg
drwxr-xr-x  4 root root 1M Sep  2  2017 rpmbuild
-rw-r--r--  1 root root 1M Oct  2 15:36 zabbix-release-4.0-1.el7.noarch.rpm
-rw-r--r--  1 root root 1M Nov 30 06:27 zabbix-release-4.1-1.el7.src.rpm
只显示目录下目录
ls -F | grep "/$"
[root@bogon ~]# ls -F | grep "/$"
anaconda3/
bin/
~~~
<h2 id="2.2">2.2 cd命令</h2>
cd命令用于切换当前工作目录至 dirName(目录参数)
其中 dirName 表示法可为绝对路径或相对路径。若目录名称省略，则变换至使用者的 home 目录 (也就是刚 login 时所在的目录)。
另外，"~" 也表示为 home 目录 的意思，"." 则是表示目前所在的目录，".." 则表示目前目录位置的上一层目录。
>语法

~~~wget
cd [dirName]
~~~
>实例

~~~wget 
cd / #跳转到根下
cd ~ #跳转到登录用户的home目录下。
cd ../.. #跳转到当前目录的上二层目录。
cd ../ #跳转到当前目录的上一层目录。
~~~
<h2 id="2.3">2.3 pwd命令</h2>
pwd是Print Working Directory的缩写，其功能是显示当前所在工作目录的全路径。主要用在当不确定当前所在位置时，通过pwd来查看当前目录的绝对路径。
>语法

~~~wget
pwd [选项]
~~~
>参数

~~~wget
-L：--logical，显示当前的路径，有连接文件时，直接显示连接文件的路径，(不加参数时默认此方式)，参考示例1。 
-p：--physical，显示当前的路径，有连接文件时，不使用连接路径，直接显示连接文件所指向的文件，参考示例2。 当包含多层连接文件时，显示连接文件最终指向的文件，参考示例3。 
--help：显示帮助信息。 
--version：显示版本信息。
~~~
>实例

~~~bash
pwd -P  
/etc/rc.d/rc0.d 
pwd -L  
/etc/rc0.d
pwd
/opt/remi
~~~


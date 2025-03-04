### 	终端

[ root@localhost 桌面#]

用户名@主机名 工作目录 身份识别符

shutdown -h：正常关机命令

halt：关闭内存（也起关机作用）

init0（未必兼容的关机命令）



什么是Linux指令？

指Linux终端（命令行）中输入的内容就称之为指令。

一个完整指令的标准格式（Linux通用的格式）：

\#指令 [选项] {操作对象]

一个指令可以包含多个选项

操作对象也可以是多个



###Linux基本指令

#####ls指令

含义：list

用法1：#ls：列出当工作目录下的所有文件/文件夹的名称

用法2：#ls 路径：列出指定路径下的所有文件/文件夹的名称

用法3：#ls 选项 路径：在列出指定路径下的文件/文件夹的名称，并以指定的格式进行显示。

常见的选项：

-l：表示以列表的形式进行展示

-a：表示显示所有的文件/文件夹（包含了隐藏文件/文件夹）

用法4：#ls -lh 路径：列出指定路径下的所有文件/文件夹的名称，以列表的形式并且以可读性较高的形式显示。



##### pwd指令

用法：#pwd：打印当前工作目录(print working diractory)



##### CD指令

用于切换当前的工作目录（change directory）

用法：#cd/ #cd 路径



##### mkdir指令

创建目录。(make directory)

用法：#mkdir [选项] 路径 

-p：一次性创建多层不存在的目录

路径可以是多个，表示创建多个目录

路径：可以说文件夹名称也可以是包含名称的一个完整路径



##### touch指令

作用：创建文件

语法：#touch 文件路径



##### cp指令

负责文件/文件夹到指定位置。（copy）

语法：#cp 被复制的文档路径 文档被复制到的路径

添加选项 -r才能复制文件夹，不然会被忽略（-r表示递归复制）

注意：在复制过程中可以对新位置的文件进行重命名。



##### mv指令

移动/剪切文档到新的位置

语法：#mv 需要移动的文档路径

补充：在Linux中重命名的命令也是mv，语法和移动语法一样。



##### rm命令

移除/删除文档

用法：#re 选项 需要移除的文档路径

选项：-f（force强制删除，不提示是否删除）   -r  -rf



##### vim指令

vim是一款文本编辑器

语法：#vim 文件的路径，打开一个或有或无的文件



##### 输出重定向

一般命令的输出都会显示在终端中，有时需要将一些命令的执行结果保存到文件中进行后续的分析和统计，这时候需要使用到输出重定向技术。

\>：覆盖输出，会覆盖掉原来的文件内容。

\>>:追加输出，不会覆盖原始文件内容，会在原始内容末尾继续添加。

语法：#正常执行的指令 >/>> 文件的路径



##### cat指令

①cat有直接打开并读取一个文件的功能。

用法：#cat 文件路径

②可以对文件进行合并

用法：#cat 文件路径1 文件路径2 ... 文件路径n >  合并后的文件路径



##### head指令

查看文件的前n行，不指定n时默认为前10行。

用法：head -n 文件路径



##### tail指令

查看文件的后n行，不指定n时默认为后10行。

用法：tail -n 文件路径



##### less指令

查看文件，以较小的内容进行输出，按下辅助功能键查看更多。（）

用法：#less 文件路径



##### wc指令

用于统计文件内容信息（包括行数、单词数、字节数）

用法：#wc -lwc 需要统计的文件路径

-l:表示lines，行数

-w:表示words，表示单词数

-c:表示bytes，字节数



##### date指令

作用：表示操作时间日期（读取、设置），

用法1：#date     输出的形式为：2021年5月18日 星期一   15:54:28

用法2:   #date  +%F(等价于#date"%Y-%m-%d") 输出形式：2021-05-18

用法3：#date "+%F %T" 引号表示让“年月日时分秒”成为一个不可分割的整体输出的形式：2021-05-18 16:01:00

等价操作#date "+%Y-%m-%d %H:%M:%S"

用法4：#date -d "-1 day" "+%Y-%m-%d %H:%M:%S"

获取之前或者之后的某个时间

单位可选：day/month/year

%F表示完整的年月日

%T表示完整的时分秒

%Y表示四位年份

 

##### cal指令

用来操作日历

用法1：#cal  ：直接输出当前月份的日历，等价于#cal -1

用法2：#cal -3 ：表示输出上一个月+本月+下一个月的日历。

用法3：#cal -y 年份：表示输出某一年份的日历。



##### clear/ctrl+l 指令

清除终端中已经存在的命令和结果（信息）。

（可以通过滚动条继续查看以前的信息）



##### 管道（重要）

管道符：I

作用：管道一般可以用于“过滤”，“特殊”，“拓展处理”

管道不能单独使用，必须配合前面所讲的一些指令来一起使用，其作用主要是辅助作用。

e.g:需要通过管道查询出根目录下包含“y”字母的文档名称

\#ls /lgrep y

针对上面这个命令说明：

①以管道作为分界线，前面的命令有个输出，后面需要先输出，然后再过滤，最后再输出，通俗的讲就是管道前面的输出就是后面指令的输入。

②grep指令：主要用于过滤





### 高级指令

##### hostname指令

操作服务器的主机名（读取、设置）

用法：#hostname 选项    含义：输出完整的主机名

-f：输出当前主机名中的FQDN（全限定域名）



#####id指令

查看一个用户的一些基本信息（包含用户id,用户组id，附加组id），该指令如果不指定用户则默认当前用户。

用法：#id 用户名  含义：显示指定用户的基本信息



##### whoami指令

显示当前的用户名，一般用于shell脚本



#####ps -ef指令（重点）

主要是查看服务器的进程信息。

选项含义：

-e:等价于“-A”，表示列出所有的进程

-f:显示全部的列（显示全字段）

列的含义：

UID:该进程执行的用户id；

PID：进程id；

PPID：该进程的父级进程id，如果一个程序的父级进程找不到，该程序的进程称之为僵尸进程；

C：Cpu的占用率，其形式是百分数；

STIME：进行的启动时间；

TTY：终端设备，发起该进程的设备识别符号，如果显示"?"则表示该进程并不是由终端设备发起；

TIME：进程的执行时间；

CMD：该进程的名称或者对应的路径；

用法：#ps -ef|grep 进行名称 



##### top指令

查看服务器的进程占的资源。

用法：

进入命令：#top（动态显示）

推出命令：按下q键

表头含义：

PID：进程id；

USER：该进程对应的用户；

PR：优先级；

VIRT：虚拟内存；

RES：常驻内存；

SHR：共享内存；

计算几个进程实际使用的内存=常驻内存（RES）-共享内存(SHR)

S：表示进行的状态（sleeping,其中S表示睡眠，R表示运行）

%CPU：表示CPU的占用百分比；

%MEM：表示内存的占用百分比；

TIME+：执行时间；

COMMAND：进程的名称或者路径。

在运行top的时候，可以按下快捷键：

M：表示讲结果按照内存（MEM）从高到低进行降序排列；

P：表示将结果按照CPU使用率从高到低进行降序排列。

1：当服务器拥有多个cpu的时候可以使用“1”快捷键来切换是否展示显示各个cpu的详细信息。



##### du -sh指令

查看目录的真实大小

用法：#du -sh 目录路径

选项含义：

-s：summaries，只显示汇总的大小

-h:表示以高可读性的形式进行显示



##### find指令

用于查找文件（其参数由55个之多）

用法：#find 路径范围 选项 选项的值

选项：

-name:按照文档名称进行搜索（支持模糊搜索）

-type：按照文档的类型进行搜索

文档类型：“-”表示文件（用find时需要用f来替换）“d”表示文件夹



##### service指令(重点)

用于控制一些软件的服务启动/停止/重启

用法：#service 服务名 start/stop/restart

（可通过ps命令来检查服务是否启动）



##### kill/killall指令

杀死进程

用法：

\#kill 进程PID

\#killall 进程名称



##### ifconfig指令

用于操作网卡相关的指令。

用法：#ifconfig(获取网卡信息)

（inet addr就是网卡的ip地址）



##### reboot指令

重新启动计算机

语法：#reboot 重启

选项：-w 模拟重启，但是不重启（只写关机与开机信息）



##### shutdown指令

关机（慎用）

用法：#shutdown -h now ”关机提示“

如果想要取消关机计划的话，则可以ctrl+c或者#shutdown -c



##### uptime指令

输出计算机的持续在线时间（计算机从开机到现在运行的时间）



##### uname指令

获取计算机操作系统相关信息。

用法：#uname  获取操作系统的类型

选项：-a  all,表示获取全部的系统信息（类型、全部主机名、内核版本、发布时间、开源计划）



##### netstat -tnlp指令

查看网络的连接状态。

选项：-t  表示只列出tcp协议的连接

​			-n 表示将地址从字母组合转化为ip地址，将协议转化为端口号来显示；

​			-l  表示过滤出”state（状态）“列中其值为LISTEN（监听）的连接。

​			-p  表示显示发起连接的进程pid和进程名称。



##### man指令

manual,手册（包含了Linux中全部命令手册，英文）

用法：#man 命令（退出按下q键）





###其他

注意：ls列出的结果颜色，其中蓝色的名称表示文件夹，黑色表示文件，绿色的其权限为拥有所有权限。



在Linux中有一个特殊的符号”~“，表示当前用户的家目录。



目录是4k不表示内容的大小。



关于路径（重要）：

路径可以分为两种：相对路径、绝对路径。

相对路径：相对首先得有一个参照物（一般就是当前的工作路径）

绝对路径：绝对路径不需要参照物，直接从根“/”开始寻找对应路径

相对路径的写法：在相对路径中通常会用到两个符号:

./：表示当前目录下

../：表示上一级目录下

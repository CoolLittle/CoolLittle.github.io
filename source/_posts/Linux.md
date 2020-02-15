---
title: Linux学习  
tags:   
  - linux  
categories:
  - linux    
description: linux持续学习.....    
date: 2020-02-01 15:19:00
---

**一切皆文件...**

----

### Linux目录介绍

* **/bin:** 存放经常使用的命令，例如：cd、chmod、chown等等
* **/boot:** 存放Linux启动的一些核心文件，包括一些连接文件以及镜像文件。
* **/dev:** 存放Linux的外部设备，类似于Windows下的设备管理器，Linux将设备作为文件进行管理
* **/etc:** 系统配置文件这个目录用来存放所有的系统管理所需的配置文件和子目录
* **/home:** 用户主目录，每个用户都有一个属于自己的主目录
* **/lib:** 存放动态链接库，几乎所有应用程序都需要用到这些共享库。类似于Windows下的DLL文件
* **/lost+found:** 一般情况为空，当硬盘系统发生错误后会将一些遗失文件放置在该目录下。当新挂载一个磁盘就会出现该目录。
* **/media:** 会将一些媒体设备挂载到该目录下，例如U盘、光驱。
* **/mnt:** 挂载目录，可以挂载临时文件系统。
* **/opt:** 额外软件安装目录。
* **/usr/local:** 是另一个给主机额外安装软件的目录，一般是通过编译方式安装的程序存放到该目录下。
* **/proc:** 内存虚拟目录，是系统内存的映射。
* **/root:** root用户主目录
* **/sbin:** 存放系统管理员使用的系统管理程序
* **/selinux:** 这个目录是Redhat/CentOS所特有的目录，Selinux是一个安全机制，类似于windows的防火墙，但是这套机制比较复杂，这个目录就是存放selinux（安全子系统）相关的文件的。
* **/srv:**  该目录存放一些服务启动之后需要提取的数据。
* **/sys:** 这是linux2.6内核的一个很大的变化。该目录下安装了2.6内核中新出现的一个文件系统 sysfs。sysfs文件系统集成了下面3种文件系统的信息：针对进程信息的proc文件系统、针对设备的devfs文件系统以及针对伪终端的devpts文件系统。该文件系统是内核设备树的一个直观反映。当一个内核对象被创建的时候，对应的文件和目录也在内核对象子系统中被创建。
* **/tmp:** 存放系统临时文件
* **/usr:** 这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似于windows下的program files目录。
* **/usr/bin:** 系统用户使用的应用程序。
* **/usr/sbin:** 超级用户使用的比较高级的管理程序和系统守护程序。
* **/usr/src:** 内核源代码默认的放置目录。
* **/var:** 这个目录中存放着在不断扩充着的东西，通常放置经常被修改的目录放在这个目录下。包括各种日志文件。
* **/run:** 是一个临时文件系统，存储系统启动以来的信息。当系统重启时，这个目录下的文件应该被删掉或清除。如果你的系统上有 /var/run 目录，应该让它指向 run。

在 Linux 系统中，有几个目录是比较重要的，平时需要注意不要误删除或者随意更改内部文件。
**/etc：** 上边也提到了，这个是系统中的配置文件，如果你更改了该目录下的某个文件可能会导致系统不能启动。
**/bin, /sbin, /usr/bin, /usr/sbin:** 这是系统预设的执行文件的放置目录，比如` ls `就是在 **/bin/ls** 目录下的。
值得提出的是，**/bin, /usr/bin** 是给系统用户使用的指令（除`root`外的通用户），而 **/sbin, /usr/sbin** 则是给`root`使用的指令。
**/var：** 这是一个非常重要的目录，系统上跑了很多程序，那么每个程序都会有相应的日志产生，而这些日志就被记录到这个目录下，具体在 `/var/log` 目录下，另外mail的预设放置也是在这里。

### Linux系统启动过程

如下5个阶段：

1. 内核的引导
2. 运行 `init`
3. 系统初始化
4. 建立终端
5. 用户登陆系统

#### 内核引导

当计算机打开电源后，首先是BIOS开机自检，按照BIOS中设置的启动设备（通常是硬盘）来启动。

操作系统接管硬件以后，首先读入 /boot 目录下的内核文件。 

<img src="启动过程/core.png" />

#### 运行 `init`

`init` 进程是系统所有进程的起点，你可以把它比拟成系统所有进程的老祖宗，没有这个进程，系统中任何进程都不会启动。

`init` 程序首先是需要读取配置文件 `/etc/inittab`。 

<img src="启动过程/init.png" />

#### 运行级别

许多程序需要开机启动。它们在Windows叫做"服务"（service），在Linux就叫做"守护进程"（daemon）。

init进程的一大任务，就是去运行这些开机启动的程序。

但是，不同的场合需要启动不同的程序，比如用作服务器时，需要启动Apache，用作桌面就不需要。

Linux允许为不同的场合，分配不同的开机启动程序，这就叫做"运行级别"（runlevel）。也就是说，启动时根据"运行级别"，确定要运行哪些程序。

<img src="启动过程/runlevel.png">

Linux系统有7个运行级别(runlevel)：    

    运行级别0：系统停机状态，系统默认运行级别不能设为0，否则不能正常启动
    运行级别1：单用户工作状态，root权限，用于系统维护，禁止远程登陆
    运行级别2：多用户状态(没有NFS)
    运行级别3：完全的多用户状态(有NFS)，登陆后进入控制台命令行模式
    运行级别4：系统未使用，保留
    运行级别5：X11控制台，登陆后进入图形GUI模式
    运行级别6：系统正常关闭并重启，默认运行级别不能设为6，否则不能正常启动

#### 系统初始化

在init的配置文件中有这么一行： si::sysinit:/etc/rc.d/rc.sysinit　它调用执行了/etc/rc.d/rc.sysinit，而rc.sysinit是一个bash shell的脚本，它主要是完成一些系统初始化的工作，rc.sysinit是每一个运行级别都要首先运行的重要脚本。    
它主要完成的工作有：激活交换分区，检查磁盘，加载硬件模块以及其它一些需要优先执行任务。        

	l5:5:wait:/etc/rc.d/rc 5

这一行表示以5为参数运行/etc/rc.d/rc，/etc/rc.d/rc是一个Shell脚本，它接受5作为参数，去执行/etc/rc.d/rc5.d/目录下的所有的rc启动脚本，/etc/rc.d/rc5.d/目录中的这些启动脚本实际上都是一些连接文件，而不是真正的rc启动脚本，真正的rc启动脚本实际上都是放在/etc/rc.d/init.d/目录下。

而这些rc启动脚本有着类似的用法，它们一般能接受 `start`、`stop`、`restart`、`status`等参数。

`/etc/rc.d/rc5.d/` 中的rc启动脚本通常是K或S开头的连接文件，对于以 S 开头的启动脚本，将以start参数来运行。

而如果发现存在相应的脚本也存在K打头的连接，而且已经处于运行态了(以/var/lock/subsys/下的文件作为标志)，则将首先以stop为参数停止这些已经启动了的守护进程，然后再重新运行。

这样做是为了保证是当init改变运行级别时，所有相关的守护进程都将重启。

至于在每个运行级中将运行哪些守护进程，用户可以通过chkconfig或setup中的"System Services"来自行设定。

<img src="启动过程/sysinit.png" />

#### 建立终端

rc执行完毕后，返回init。这时基本系统环境已经设置好了，各种守护进程也已经启动了。

init接下来会打开6个终端，以便用户登录系统。在inittab中的以下6行就是定义了6个终端：

	1:2345:respawn:/sbin/mingetty tty1
	2:2345:respawn:/sbin/mingetty tty2
	3:2345:respawn:/sbin/mingetty tty3
	4:2345:respawn:/sbin/mingetty tty4
	5:2345:respawn:/sbin/mingetty tty5
	6:2345:respawn:/sbin/mingetty tty6

#### 用户登录系统

一般来说，用户的登录方式有三种：

1. 命令行登录
2. ssh登录
3. 图形界面登录

<img src="启动过程/userlogin.png" />

对于运行级别为5的图形方式用户来说，他们的登录是通过一个图形化的登录界面。登录成功后可以直接进入 KDE、Gnome 等窗口管理器。

而本文主要讲的还是文本方式登录的情况：当我们看到mingetty的登录界面时，我们就可以输入用户名和密码来登录系统了。

Linux 的账号验证程序是 login，login 会接收 mingetty 传来的用户名作为用户名参数。

然后 login 会对用户名进行分析：如果用户名不是 root，且存在 /etc/nologin 文件，login 将输出 nologin 文件的内容，然后退出。

这通常用来系统维护时防止非root用户登录。只有/etc/securetty中登记了的终端才允许 root 用户登录，如果不存在这个文件，则 root 用户可以在任何终端上登录。

`/etc/usertty` 文件用于对用户作出附加访问限制，如果不存在这个文件，则没有其他限制。

#### 图形模式与文字模式的切换方式

Linux预设提供了六个命令窗口终端机让我们来登录。

默认我们登录的就是第一个窗口，也就是tty1，这个六个窗口分别为tty1,tty2 … tty6，你可以按下Ctrl + Alt + F1 \~ F6 来切换它们。

如果你安装了图形界面，默认情况下是进入图形界面的，此时你就可以按Ctrl + Alt + F1 \~ F6来进入其中一个命令窗口界面。

当你进入命令窗口界面后再返回图形界面只要按下Ctrl + Alt + F7 就回来了。

如果你用的vmware 虚拟机，命令窗口切换的快捷键为 Alt + Space + F1\~F6. 如果你在图形界面下请按Alt + Shift + Ctrl + F1\~F6 切换至命令窗口。 

<img src="启动过程/loginshell.png" />

#### Linux关机

在linux领域内大多用在服务器上，很少遇到关机的操作。毕竟服务器上跑一个服务是永无止境的，除非特殊情况下，不得已才会关机。

正确的关机流程为：`sync > shutdown > reboot > halt`

关机指令为：`shutdown` ，你可以`man shutdown` 来看一下帮助文档。

例如你可以运行如下命令关机：

	sync 将数据由内存同步到硬盘中。

	shutdown 关机指令，你可以man shutdown 来看一下帮助文档。例如你可以运行如下命令关机：

	shutdown –h 10 ‘This server will shutdown after 10 mins’ 这个命令告诉大家，计算机将在10分钟后关机，并且会显示在登陆用户的当前屏幕中。

	shutdown –h now 立马关机

	shutdown –h 20:25 系统会在今天20:25关机

	shutdown –h +10 十分钟后关机

	shutdown –r now 系统立马重启

	shutdown –r +10 系统十分钟后重启

	reboot 就是重启，等同于 shutdown –r now

	halt 关闭系统，等同于shutdown –h now 和 poweroff

### Vim编辑器的使用

Vim键位图：
<img src="vim/vim.gif" /> 

#### 命令模式

| 移动光标快捷方法		| 						| 
|:--------------------	|----------------------	| 
| h 或 向左箭头键(←) 	| 光标向左移动一个字符 	| 
| j 或 向下箭头键(↓) 	| 光标向下移动一个字符 	| 
| k 或 向上箭头键(↑) 	| 光标向上移动一个字符 	| 
| l 或 向右箭头键(→) 	| 光标向右移动一个字符 	| 
| [Ctrl] + [f]     	| 屏幕『向下』移动一页，相当于 [Page Down]按键 (常用)                                                                                     	|
| [Ctrl] + [b]     	| 屏幕『向下』移动一页，相当于 [Page Down]按键(常用)                                                                                      	|
| [Ctrl] + [d]     	| 屏幕『向下』移动半页                                                                                                                    	|
| [Ctrl] + [u]     	| 屏幕『向上』移动半页                                                                                                                    	|
| +                	| 光标移动到非空格符的下一行                                                                                                              	|
| -                	| 光标移动到非空格符的上一行                                                                                                              	|
| n&lt;space&gt;   	| 那个 n 表示『数字』，例如 20 。按下数字后再按空格键，光标会向右移动这一行的 n 个字符。例如 20<space> 则光标会向后面移动 20 个字符距离。 	|
| 0 或功能键[Home] 	| 这是数字『 0 』：移动到这一行的最前面字符处                                                                                             	|
| $ 或功能键[End]  	| 移动到这一行的最后面字符处(常用)                                                                                                        	|
| `H`                	| 光标移动到这个屏幕的最上方那一行的第一个字符                                                                                            	|
| `M`               	| 光标移动到这个屏幕的中央那一行的第一个字符                                                                                              	|
| `L`                	| 光标移动到这个屏幕的最下方那一行的第一个字符                                                                                            	|
| `G`                	| 移动到这个档案的最后一行(常用)                                                                                                          	|
| `nG`               	| n 为数字。移动到这个档案的第 n 行。例如 20G 则会移动到这个档案的第 20                                                                   	|
| `gg`               	| 移动到这个档案的第一行，相当于 1G 啊！(常用)                                                                                            	|
| `n<Enter>`         	| n 为数字。光标向下移动 n 行(常用)                                                                                                       	|

如果你将右手放在键盘上的话，你会发现 `hjkl` 是排列在一起的，因此可以使用这四个按钮来移动光标。 如果想要进行多次移动的话，例如向下移动 `30` 行，可以使用 `30j` 或 `30↓` 的组合按键， 亦即加上想要进行的次数(数字)后，按下动作即可！

| 搜索替换                                    	|                                                                                                                                                                                                                                               	|
|:---------------------------------------------	|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|
| `/word`                                       	| 向光标之下寻找一个名称为 word 的字符串。例如要在档案内搜寻 vbird 这个字符串，就输入 /vbird 即可！(常用)                                                                                                                                       	|
| `?word`                                       	| 向光标之上寻找一个字符串名称为 word 的字符串。                                                                                                                                                                                                	|
| `n`                                           	| 这个 n 是英文按键。代表重复前一个搜寻的动作。举例来说，如果刚刚我们执行 /vbird 去向下搜寻 vbird这个字符串，则按下 n 后，会向下继续搜寻下一个名称为 vbird 的字符串。如果是执行 ?vbird 的话，那么按下 n 则会向上继续搜寻名称为 vbird 的字符串！ 	|
| `N`                                       		| 这个 N 是英文按键。与 n 刚好相反，为『反向』进行前一个搜寻动作。例如 /vbird 后，按下 N 则表示『向上』搜寻 vbird 。                                                                                                                            	|
| `:n1,n2s/word1/word2/g`                       	| n1 与 n2 为数字。在第 n1 与 n2 行之间寻找 word1 这个字符串，并将该字符串取代为word2 ！举例来说，在 100 到 200 行之间搜寻 vbird 并取代为 VBIRD 则：『:100,200s/vbird/VBIRD/g』。(常用)                                                         	|
| `:1,$s/word1/word2/g  或 :%s/word1/word2/g`   	| 从第一行到最后一行寻找 word1 字符串，并将该字符串取代为 		word2 ！(常用)                                                                                                                                                                        	|
| `:1,$s/word1/word2/gc  或 :%s/word1/word2/gc` 	| 从第一行到最后一行寻找 word1 字符串，并将该字符串取代为word2 ！且在取代前显示提示字符给用户确认 (confirm)是否需要取代！(常用)                                                                                                                 	|

| 删除、复制与粘贴 	|                                                                                                                         	|
|------------------	|-------------------------------------------------------------------------------------------------------------------------	|
| x, X             	| 在一行字当中，x 为向后删除一个字符 (相当于 [del] 按键)，X 为向前删除一个字符(相当于 [backspace] 亦即是退格键)(常用)     	|
| nx               	| n 为数字，连续向后删除 n 个字符。举例来说，我要连续删除 10 个字符，『10x』。                                            	|
| dd               	| 删除游标所在的那一整行(常用)                                                                                            	|
| ndd              	| n 为数字。删除光标所在的向下 n 行，例如 20dd 则是删除 20 行(常用)                                                       	|
| d1k              	| 向上删除1行                                                                                          						|
| d1j              	| 向下删除1行                                                                                          						|
| d1h              	| 向左删除1个字符                                                                                         					|
| d1l              	| 向右删除1个字符                                                                                          					|
| dG               	| 删除光标所在到最后一行的所有数据                                                                                        	|
| d$               	| 删除游标所在处，到该行的最后一个字符                                                                                    	|
| d0               	| 那个是数字的 0 ，删除游标所在处，到该行的最前面一个字符                                                                 	|
| yy               	| 复制游标所在的那一行(常用)                                                                                              	|
| nyy              	| n 为数字。复制光标所在的向下 n 行，例如 20yy 则是复制 20 行(常用)                                                       	|
| y1k              	| 向上复制1行                                                                                          						|
| y1j              	| 向下复制1行                                                                                          						|
| y1h              	| 向左复制1个字符                                                                                         					|
| y1l              	| 向右复制1个字符                                                                                          					|
| y1G              	| 复制游标所在行到第一行的所有数据                                                                                        	|
| yG               	| 复制游标所在行到最后一行的所有数据                                                                                      	|
| y0               	| 复制光标所在的那个字符到该行行首的所有数据                                                                              	|
| y$               	| 复制光标所在的那个字符到该行行尾的所有数据                                                                              	|
| p, P             	| p 为将已复制的数据在光标下一行贴上，P 则为贴在游标上一行                                                                	|
| J                	| 将光标所在行与下一行的数据结合成同一行                                                                                  	|
| c                	| 重复删除多个数据，例如向下删除 10 行，[ 10cj ]                                                                          	|
| u                	| 复原前一个动作。(常用)撤销                                                                                              	|
| [Ctrl]+r         	| 重做上一个动作。(常用)恢复撤销                                                                                          	|
| .                	| 不要怀疑！这就是小数点！意思是重复前一个动作的意思。如果你想要重复删除、重复贴上等等动作，按下小数点『.』就好了！(常用) 	|

#### 输入模式

| 进入输入或取代的编辑模式 	|                                                                                                                         	|
|--------------------------	|-------------------------------------------------------------------------------------------------------------------------	|
| i,I                      	| i 为『从目前光标所在处输入』， I 为『在目前所在行的第一个非空格符处开始输入』。                                         	|
| a, A                     	| a 为『从目前光标所在的下一个字符处开始输入』， A 为『从光标所在行的最后一个字符处开始输入』。(常用)                     	|
| o, O                     	| 这是英文字母 o 的大小写。o 为『在目前光标所在的下一行处输入新的一行』；O 为在目前光标所在处的上一行输入新的一行！(常用) 	|
| r, R                     	| r只会取代光标所在的那一个字符一次；R会一直取代光标所在的文字，直到按下 ESC 为止；(常用)                                 	|
| [Esc]                    	| 退出编辑模式，回到一般模式中(常用)                                                                                      	|

#### 底线命令模式

| 进入输入或取代的编辑模式 	|                                                                                                                           	|
|--------------------------	|---------------------------------------------------------------------------------------------------------------------------	|
| :w                       	| 将编辑的数据写入硬盘档案中(常用)                                                                                          	|
| :w!                      	| 若文件属性为『只读』时，强制写入该档案。不过，到底能不能写入，还是跟你对该档案的档案权限有关啊！                          	|
| :q                       	| 离开 vi (常用)                                                                                                            	|
| :q!                      	| 若曾修改过档案，又不想储存，使用 ! 为强制离开不储存档案。                                                                 	|
| :wq                      	| 储存后离开，若为 :wq! 则为强制储存后离开(常用)                                                                            	|
| ZQ                       	| 这是大写的 Z 喔！不存储档案离开！                                         	|
| ZZ                       	| 这是大写的 Z 喔！若档案没有更动，则不储存离开，若档案已经被更动过，则储存后离开！                                         	|
| :w [filename]            	| 将编辑的数据储存成另一个档案（类似另存新档）                                                                              	|
| :r [filename]            	| 在编辑的数据中，读入另一个档案的数据。亦即将 『filename』这个档案内容加到游标所在行后面                                   	|
| :n1,n2 w [filename]      	| 将 n1 到 n2 的内容储存成 filename 这个档案。                                                                              	|
| :! command               	| 暂时离开 vi 到指令行模式下执行 command 的显示结果！例如『:! ls /home』即可在 vi 当中察看 /home 底下以 ls 输出的档案信息！ 	|
|                          	|                                                                                                                           	|

#### 块选择模式

批量注释：
`Ctrl + v` 进入块选择模式，然后移动光标选中你要注释的行，再按大写的 `I` 进入行首插入模式输入注释符号如 `//` 或 `#`，输入完毕之后，按两下 `ESC`，Vim 会自动将你选中的所有行首都加上注释，保存退出完成注释。

取消注释：
`Ctrl + v` 进入块选择模式，选中你要删除的行首的注释符号，注意 `//` 要选中两个，选好之后按 `d` 即可删除注释，`ESC` 保存退出


### 用户管理

#### 添加用户

```
useradd [选项] 用户名
```

使用useradd时，如果后面不添加任何参数选项，
例如：#sudo useradd test创建出来的用户将是默认“三无”用户：一无Home Directory，二无密码，三无系统Shell。
因此利用这个用户登录系统，是登录不了的，为了避免这样的情况出现，可以用 （useradd -m +用户名）的方式创建，
它会在/home目录下创建同名文件夹，然后利用（ passwd + 用户名）为指定的用户名设置密码。

参数说明：

	* 选项：
		* -b, --base-dir BASE_DIR       新账户的主目录的基目录
		* --btrfs-subvolume-home        use BTRFS subvolume for home directory
		* -c, --comment COMMENT         新账户的 GECOS 字段
		* -d, --home-dir HOME_DIR       新账户的主目录
		* -D, --defaults                显示或更改默认的 useradd 配置
		* -e, --expiredate EXPIRE_DATE  新账户的过期日期
		* -f, --inactive INACTIVE       新账户的密码不活动期
		* -g, --gid GROUP               新账户主组的名称或 ID
		* -G, --groups GROUPS           新账户的附加组列表
		* -h, --help                    显示此帮助信息并退出
		* -k, --skel SKEL_DIR           使用此目录作为骨架目录
		* -K, --key KEY=VALUE           不使用 /etc/login.defs 中的默认值
		* -l, --no-log-init             不要将此用户添加到最近登录和登录失败数据库
		* -m, --create-home             创建用户的主目录
		* -M, --no-create-home          不创建用户的主目录
		* -N, --no-user-group           不创建同名的组
		* -o, --non-unique              允许使用重复的 UID 创建用户
		* -p, --password PASSWORD       加密后的新账户密码
		* -r, --system                  创建一个系统账户
		* -R, --root CHROOT_DIR         chroot 到的目录
		* -P, --prefix PREFIX_DIR       prefix directory where are located the /etc/* files
		* -s, --shell SHELL             新账户的登录 shell
		* -u, --uid UID                 新账户的用户 ID
		* -U, --user-group              创建与用户同名的组
		* -Z, --selinux-user SEUSER     为 SELinux 用户映射使用指定 SEUSER
	 
		
	* 用户名：
		* 指定新账号的登录名

实例：

```

# useradd -d /home/sam -m sam 
# 创建一个用户sam,其中 `-d` 和 `-m` 选项用来为登录名sam产生一个主目录 `/home/sam` 。

# useradd -s /bin/sh -g group –G adm,root gem

此命令新建了一个用户gem，该用户的登录Shell是 /bin/sh，它属于group用户组，
同时又属于adm和root用户组，其中group用户组是其主组。
这里可能新建组：#groupadd group及groupadd adm。
增加用户账号就是在/etc/passwd文件中为新用户增加一条记录，
同时更新其他系统文件如/etc/shadow, /etc/group等。
Linux提供了集成的系统管理工具userconf，它可以用来对用户账号进行统一管理。   
```

Linux系统如何添加用户这个问题到网上问一下或者搜一下，很多人可能会说 `useradd` ，实际这是不对的。
useradd只会添加一个用户，没有创建它的主目录，除了添加一个新用户之外什么都没有。这个用户甚至不能登录，因为没有密码。
正确的做法是 `adduser 用户名`，这个命令实际是一个perl脚本，是 `useradd` 等类似底层命令的更友好的前端，
它会用交互性的方式建立新用户，使用它可以指定新用户的家目录，登录密码，是否加密主目录等等，它会：    

1. 建立一个新目录作为家目录
2. 建立同名新组
3. 把用户的主要组设为该组(除非命令选项覆盖以上默认动作，比如–disall-homdirecry之类)
4. 从/etc/SKEL目录下拷贝文件到家目录，完成初始化
5. 建立新用户的密码
6. 如果其存在的话，还会执行一个脚本。

```
# adduser [选项] [用户名]
```
参数说明：
```
添加一个普通用户:
	# adduser [--home 主目录] [--shell SHELL] [--no-create-home]
	   [--uid ID] [--firstuid ID] [--lastuid ID] [--gecos GECOS]
	   [--ingroup 用户组 | --gid ID][--disabled-password] [--disabled-login]
	   [--add_extra_groups] 用户名

添加一个系统用户:
	# adduser --system [--home 主目录] [--shell SHELL] [--no-create-home]
	   [--uid ID] [--gecos GECOS] [--group | --ingroup 用户组 | --gid ID]
	   [--disabled-password] [--disabled-login] [--add_extra_groups] 用户名
	
添加一个用户组:
	# adduser --group [--gid ID] 用户组名
	# addgroup [--gid ID] 用户组名

添加一个系统用户组
	# addgroup --system [--gid ID] 用户组名
	
将一个已存在的用户添加至一个已存在的用户组
	# adduser 用户名 用户组名

常规设置：
  --quiet | -q		不在标准输出中给出进度信息
  --force-badname	允许用户名不匹配：NAME_REGEX[_SYSTEM] 配置变量
  --help | -h		给出本命令用法
  --version | -v	版本号和版权信息
  --conf | -c 文件	使用文件中的配置
```

#### 删除账号

```
	userdel [选项] 用户名   
```

参数说明：

	* 选项：
		* -f, --force                   即使不属于此用户，也强制删除文件
		* -h, --help                    显示此帮助信息并退出
		* -r, --remove                  删除主目录和信件池
		* -R, --root CHROOT_DIR         chroot 到的目录
		* -P, --prefix PREFIX_DIR       prefix directory where are located the /etc/* files
		* -Z, --selinux-user            为用户删除所有的 SELinux 用户映射

实例：

```
# userdel -r sam 
## 删除sam用户包括主目录
```

另一种删除用户方式：

```
删除普通用户
  deluser 用户名
  
  例： deluser mike

  --remove-home	删除用户的主目录和邮箱
  --remove-all-files	删除用户拥有的所有文件
  --backup		删除前将文件备份。
  --backup-to <DIR>	备份的目标目录。
			默认是当前目录。
  --system		只有当该用户是系统用户时才删除。

从系统中删除用户组
  delgroup GROUP
  deluser --group GROUP
  
  例如: deluser --group students

  --system		只有当该用户组是系统用户组时才删除
  --only-if-empty	只有当该用户组中无成员时才删除

将用户从一个组中删除
  deluser USER GROUP
  
  例： deluser mike students

常用选项：
  --quiet | -q			不将进程信息发给 stdout
  --help | -h		帮助信息
  --version | -v	版本号和版权
  --conf | -c 文件	以制定文件作为配置文件
```

#### 修改账号

```
	usermod [选项] 登录名
```

参数说明：

	* 选项：
		* -c, --comment COMMENT         GECOS 字段的新值
		* -d, --home HOME_DIR           用户的新主目录
		* -e, --expiredate EXPIRE_DATE  设定帐户过期的日期为 EXPIRE_DATE
		* -f, --inactive INACTIVE       过期 INACTIVE 天数后，设定密码为失效状态
		* -g, --gid GROUP               强制使用 GROUP 为新主组
		* -G, --groups GROUPS           新的附加组列表 GROUPS
		* -a, --append GROUP            将用户追加至上边 -G 中提到的附加组中，并不从其它组中删除此用户
		* -h, --help                    显示此帮助信息并退出
		* -l, --login NEW_LOGIN         新的登录名称
		* -L, --lock                    锁定用户帐号
		* -m, --move-home               将家目录内容移至新位置 (仅于 -d 一起使用)
		* -o, --non-unique              允许使用重复的(非唯一的) UID
		* -p, --password PASSWORD       将加密过的密码 (PASSWORD) 设为新密码
		* -R, --root CHROOT_DIR         chroot 到的目录
		* -P, --prefix PREFIX_DIR       prefix directory where are located the /etc/* files
		* -s, --shell SHELL             该用户帐号的新登录 shell
		* -u, --uid UID                 用户帐号的新 UID
		* -U, --unlock                  解锁用户帐号
		* -v, --add-subuids FIRST-LAST  添加子 UID 范围
		* -V, --del-subuids FIRST-LAST  移除子 UID 范围
		* -w, --add-subgids FIRST-LAST  添加子 GID 范围
		* -W, --del-subgids FIRST-LAST  移除子 GID 范围
		* -Z, --selinux-user SEUSER     用户的新的 SELinux 用户映射

修改账号参数与添加账号参数意义相同。

#### 用户口令管理

```
	passwd [选项] [登录名]
```

参数说明：   

	* 选项：
		* -a, --all                     报告所有帐户的密码状态
		* -d, --delete                  删除指定帐户的密码
		* -e, --expire                  强制使指定帐户的密码过期
		* -h, --help                    显示此帮助信息并退出
		* -k, --keep-tokens             仅在过期后修改密码
		* -i, --inactive INACTIVE       密码过期后设置密码不活动为 INACTIVE
		* -l, --lock                    锁定指定的帐户
		* -n, --mindays MIN_DAYS        设置到下次修改密码所须等待的最短天数为 MIN_DAYS
		* -q, --quiet                   安静模式
		* -r, --repository REPOSITORY   在 REPOSITORY 库中改变密码
		* -R, --root CHROOT_DIR         chroot 到的目录
		* -S, --status                  报告指定帐户密码的状态
		* -u, --unlock                  解锁被指定帐户
		* -w, --warndays WARN_DAYS      设置过期警告天数为 WARN_DAYS
		* -x, --maxdays MAX_DAYS        设置到下次修改密码所须等待的最多天数为 MAX_DAYS

#### 查询用户

```
	id [选项] [用户名]
```

参数说明：

	* 选项：
		* -a             ignore, for compatibility with other versions
		* -Z, --context  print only the security context of the process
		* -g, --group    print only the effective group ID
		* -G, --groups   print all group IDs
		* -n, --name     print a name instead of a number, for -ugG
		* -r, --real     print the real ID instead of the effective ID, with -ugG
		* -u, --user     print only the effective user ID
		* -z, --zero     delimit entries with NUL characters, not whitespace;not permitted in default format

实例：
<img src="用户/id.PNG" />

```
## 查询当前用户
# whoami
```

#### 切换用户

从高权限用户切换至低权限用户不需要输入密码。
返回原来用户使用 `exit` 即可。

```
	su [options] [-] [<user> [<argument>...]]
```

参数说明：

	* 选项：
		* -m, -p, --preserve-environment      do not reset environment variables
		* -w, --whitelist-environment <list>  don't reset specified variables
		* -g, --group <group>             specify the primary group
		* -G, --supp-group <group>        specify a supplemental group
		* -, -l, --login                  make the shell a login shell
		* -c, --command <command>         pass a single command to the shell with -c
		* --session-command <command>     pass a single command to the shell with -c and do not create a new session
		* -f, --fast                      pass -f to the shell (for csh or tcsh)
		* -s, --shell <shell>             run <shell> if /etc/shells allows it
		* -P, --pty                       create a new pseudo-terminal
		* -h, --help                      display this help
		* -V, --version                   display version

### 用户组管理

#### 添加用户组

```
	groupadd [选项] 组名
```

参数说明：

	* 选项:
		* -f, --force                   如果组已经存在则成功退出，并且如果 GID 已被使用则取消 -g
		* -g, --gid GID                 为新组使用 GID
		* -h, --help                    显示此帮助信息并退出
		* -K, --key KEY=VALUE           不使用 /etc/login.defs 中的默认值
		* -o, --non-unique              允许创建有重复 GID 的组
		* -p, --password PASSWORD       为新组使用此加密过的密码
		* -r, --system                  创建一个系统账户
		* -R, --root CHROOT_DIR         chroot 到的目录
		* -P, --prefix PREFIX_DIR       directory prefix

#### 删除用户组

```
	groupdel [选项] 组名
```

参数说明：

	选项:
		-h, --help                    显示此帮助信息并退出
		-R, --root CHROOT_DIR         chroot 到的目录
		-P, --prefix PREFIX_DIR       prefix directory where are located the /etc/* files
		-f, --force                   即便是用户的主组也继续删除

#### 修改用户组

```
	groupmod [选项] 组名
```

参数说明：

	选项:
		-g, --gid GID                 将组 ID 改为 GID
		-h, --help                    显示此帮助信息并退出
		-n, --new-name NEW_GROUP      改名为 NEW_GROUP
		-o, --non-unique              允许使用重复的 GID
		-p, --password PASSWORD       将密码更改为(加密过的) PASSWORD
		-R, --root CHROOT_DIR         chroot 到的目录
		-P, --prefix PREFIX_DIR       prefix directory where are located the /etc/* files

#### 切换用户组

	newgrp 新用户组

这条命令将当前用户切换到某用户组，前提条件是某用户组确实是该用户的主组或附加组。类似于用户账号的管理，用户组的管理也可以通过集成的系统管理工具来完成。

### 用户配置文件

#### 用户文件

**_/etc/passwd_** 文件是用户管理工作涉及的最重要的一个文件。

例如：
<img src="用户/passwd.PNG" />

/etc/passwd中一行记录对应着一个用户，每行记录又被冒号(:)分隔为7个字段，其格式和具体含义如下：    

	用户名:口令:用户标识号:组标识号:注释性描述:主目录:登录Shell
	
1. "用户名"是代表用户账号的字符串。

> 通常长度不超过8个字符，并且由大小写字母和/或数字组成。登录名中不能有冒号(:)，因为冒号在这里是分隔符。为了兼容起见，登录名中最好不要包含点字符(.)，并且不使用连字符(-)和加号(+)打头。

2. “口令”一些系统中，存放着加密后的用户口令字。

> 虽然这个字段存放的只是用户口令的加密串，不是明文，但是由于/etc/passwd文件对所有用户都可读，所以这仍是一个安全隐患。因此，现在许多Linux 系统（如SVR4）都使用了shadow技术，把真正的加密后的用户口令字存放到/etc/shadow文件中，而在/etc/passwd文件的口令字段中只存放一个特殊的字符，例如“x”或者“*”。

3. “用户标识号”是一个整数，系统内部用它来标识用户。

> 一般情况下它与用户名是一一对应的。如果几个用户名对应的用户标识号是一样的，系统内部将把它们视为同一个用户，但是它们可以有不同的口令、不同的主目录以及不同的登录Shell等。

> 通常用户标识号的取值范围是0～65 535。0是超级用户root的标识号，1～99由系统保留，作为管理账号，普通用户的标识号从100开始。在Linux系统中，这个界限是500。

4. “组标识号”字段记录的是用户所属的用户组。

> 它对应着/etc/group文件中的一条记录。

5. “注释性描述”字段记录着用户的一些个人情况。

> 例如用户的真实姓名、电话、地址等，这个字段并没有什么实际的用途。在不同的Linux 系统中，这个字段的格式并没有统一。在许多Linux系统中，这个字段存放的是一段任意的注释性描述文字，用做finger命令的输出。

6. “主目录”，也就是用户的起始工作目录。

> 它是用户在登录到系统之后所处的目录。在大多数系统中，各用户的主目录都被组织在同一个特定的目录下，而用户主目录的名称就是该用户的登录名。各用户对自己的主目录有读、写、执行（搜索）权限，其他用户对此目录的访问权限则根据具体情况设置。

7. 用户登录后，要启动一个进程，负责将用户的操作传给内核，这个进程是用户登录到系统后运行的命令解释器或某个特定的程序，即Shell。

> Shell是用户与Linux系统之间的接口。Linux的Shell有许多种，每种都有不同的特点。常用的有sh(Bourne Shell), csh(C Shell), ksh(Korn Shell), tcsh(TENEX/TOPS-20 type C Shell), bash(Bourne Again Shell)等。

> 系统管理员可以根据系统情况和用户习惯为用户指定某个Shell。如果不指定Shell，那么系统使用sh为默认的登录Shell，即这个字段的值为/bin/sh。

> 用户的登录Shell也可以指定为某个特定的程序（此程序不是一个命令解释器）。

> 利用这一特点，我们可以限制用户只能运行指定的应用程序，在该应用程序运行结束后，用户就自动退出了系统。有些Linux 系统要求只有那些在系统中登记了的程序才能出现在这个字段中。

8. 系统中有一类用户称为伪用户（pseudo users）

> 这些用户在/etc/passwd文件中也占有一条记录，但是不能登录，因为它们的登录Shell为空。它们的存在主要是方便系统管理，满足相应的系统进程对文件属主的要求。

常见的伪用户如下所示：

	伪 用 户 含 义 
	bin 拥有可执行的用户命令文件 
	sys 拥有系统文件 
	adm 拥有帐户文件 
	uucp UUCP使用 
	lp lp或lpd子系统使用 
	nobody NFS使用

除了上面列出的伪用户外，还有许多标准的伪用户，例如：audit, cron, mail, usenet等，它们也都各自为相关的进程和文件所需要。

#### 密码文件

**_/etc/shadow_** 是密码文件，其中记录行与/etc/passwd中的一一对应，它由pwconv命令根据/etc/passwd中的数据自动产生。

如下图:
<img src="用户/shadow.PNG" />

它的文件格式与/etc/passwd类似，由若干个字段组成，字段之间用":"隔开。这些字段是：

	登录名:加密口令:最后一次修改时间:最小时间间隔:最大时间间隔:警告时间:不活动时间:失效时间:标志

1. "登录名"是与/etc/passwd文件中的登录名相一致的用户账号
2. "口令"字段存放的是加密后的用户口令字，长度为13个字符。如果为空，则对应用户没有口令，登录时不需要口令；如果含有不属于集合 { ./0-9A-Za-z }中的字符，则对应的用户不能登录。
3. "最后一次修改时间"表示的是从某个时刻起，到用户最后一次修改口令时的天数。时间起点对不同的系统可能不一样。例如在SCO Linux 中，这个时间起点是1970年1月1日。
4. "最小时间间隔"指的是两次修改口令之间所需的最小天数。
5. "最大时间间隔"指的是口令保持有效的最大天数。
6. "警告时间"字段表示的是从系统开始警告用户到用户密码正式失效之间的天数。
7. "不活动时间"表示的是用户没有登录活动但账号仍能保持有效的最大天数。
8. "失效时间"字段给出的是一个绝对的天数，如果使用了这个字段，那么就给出相应账号的生存期。期满后，该账号就不再是一个合法的账号，也就不能再用来登录了。

#### 用户组文件

**_/etc/group_** 存放所有的用户组信息
	
如下图：
<img src="用户/group.PNG">
	
用户组的所有信息都存放在/etc/group文件中。此文件的格式也类似于/etc/passwd文件，由冒号(:)隔开若干个字段，这些字段有：

	组名:口令:组标识号:组内用户列表

1. "组名"是用户组的名称，由字母或数字构成。与/etc/passwd中的登录名一样，组名不应重复。
2. "口令"字段存放的是用户组加密后的口令字。一般Linux 系统的用户组都没有口令，即这个字段一般为空，或者是*。
3. "组标识号"与用户标识号类似，也是一个整数，被系统内部用来标识组。
4. "组内用户列表"是属于这个组的所有用户的列表/b]，不同用户之间用逗号(,)分隔。这个用户组可能是用户的主组，也可能是附加组。

#### 批量添加用户

1. 先编辑一个文本用户文件。

> 每一列按照/etc/passwd密码文件的格式书写，要注意每个用户的用户名、UID、宿主目录都不可以相同，其中密码栏可以留做空白或输入x号。一个范例文件user.txt内容如下:
	
	user001::600:100:user:/home/user001:/bin/bash
	user002::601:100:user:/home/user002:/bin/bash
	user003::602:100:user:/home/user003:/bin/bash
	user004::603:100:user:/home/user004:/bin/bash
	user005::604:100:user:/home/user005:/bin/bash
	user006::605:100:user:/home/user006:/bin/bash	
	
2. 以root身份执行命令 \/usr/sbin/newusers，从刚创建的用户文件user.txt中导入数据，创建用户：

```
	# newusers < user.txt
```

> 然后可以执行命令 vipw 或 vi /etc/passwd 检查 /etc/passwd 文件是否已经出现这些用户的数据，并且用户的宿主目录是否已经创建。

3. 执行命令/usr/sbin/pwunconv。     

> 将 /etc/shadow 产生的 shadow 密码解码，然后回写到 /etc/passwd 中，并将/etc/shadow的shadow密码栏删掉。这是为了方便下一步的密码转换工作，即先取消 shadow password 功能。

	# pwunconv

4. 编辑每个用户的密码对照文件。	

格式为： 

	用户名:密码

> 实例文件passwod.txt内容如下：

	user001:123456
	user002:123456
	user003:123456
	user004:123456
	user005:123456
	user006:123456
	
5. 以 root 身份执行命令 /usr/sbin/chpasswd。

> 创建用户密码，chpasswd 会将经过 /usr/bin/passwd 命令编码过的密码写入 /etc/passwd 的密码栏。

	# chpasswd < passwd.txt
	
6. 确定密码经编码写入/etc/passwd的密码栏后。

> 执行命令 /usr/sbin/pwconv 将密码编码为 shadow password，并将结果写入 /etc/shadow。

	# pwconv

7. 验证导入用户。

### 常用命令

#### 文件管理

##### awk

##### cat

连接多个文件并打印到标准输出。

    cat [选项] [文件]
        选项
            -n 或 --number：由 1 开始对所有输出的行数编号。
            -b 或 --number-nonblank：和 -n 相似，只不过对于空白行不编号。
            -s 或 --squeeze-blank：当遇到有连续两行以上的空白行，就代换为一行的空白行。
            -v 或 --show-nonprinting：使用 ^ 和 M- 符号，除了 LFD 和 TAB 之外。
            -E 或 --show-ends : 在每行结束处显示 $。
            -T 或 --show-tabs: 将 TAB 字符显示为 ^I。
            -A, --show-all：等价于 -vET。
            -e：等价于"-vE"选项；
            -t：等价于"-vT"选项
    
        实例
            # 合并显示多个文件
            cat ./1.log ./2.log ./3.log
            # 显示文件中的非打印字符、tab、换行符
            cat -A test.log
            # 压缩文件的空行
            cat -s test.log
            # 显示文件并在所有行开头附加行号
            cat -n test.log
            # 显示文件并在所有非空行开头附加行号
            cat -b test.log
            # 将标准输入的内容和文件内容一并显示
            echo '######' |cat - test.log

##### chgrp

用来变更文件或目录的所属群组。该命令用来改变指定文件所属的用户组。其中，组名可以是用户组的id，也可以是用户组的组名。文件名可以 是由空格分开的要改变属组的文件列表，也可以是由通配符描述的文件集合。如果用户不是该文件的文件主或超级用户(root)，则不能改变该文件的组。

在UNIX系统家族里，文件或目录权限的掌控以拥有者及所属群组来管理。您可以使用chgrp指令去变更文件与目录的所属群组，设置方式采用群组名称或群组识别码皆可。

    chgrp [选项][组群][文件|目录]
        选项
            -R 递归式地改变指定目录及其下的所有子目录和文件的所属的组
            -c或——changes：效果类似“-v”参数，但仅回报更改的部分；
            -f或--quiet或——silent：不显示错误信息；
            -h或--no-dereference：只对符号连接的文件作修改，而不是该其他任何相关文件；
            -H如果命令行参数是一个通到目录的符号链接，则遍历符号链接
            -R或——recursive：递归处理，将指令目录下的所有文件及子目录一并处理；
            -L遍历每一个遇到的通到目录的符号链接
            -P不遍历任何符号链接（默认）
            -v或——verbose：显示指令执行过程；
            --reference=<参考文件或目录>：把指定文件或目录的所属群组全部设成和参考文件或目录的所属群组相同；
        群组
            所属新群组
        文件|目录
            待修改的文件或者目录
            
        实例
            将/usr/meng及其子目录下的所有文件的用户组改为mengxin
            chgrp -R mengxin /usr/meng
            更改文件ah的组群所有者为 newuser
            [root@rhel ~]# chgrp newuser ah
            
##### chmod

用来变更文件或目录的权限

1. 通过符号组合的方式更改目标文件或目录的权限。
2. 通过八进制数的方式更改目标文件或目录的权限。
3. 通过参考文件的权限来更改目标文件或目录的权限。


    chmod [选项]... 模式[,模式]... 文件...
    chmod [选项]... 八进制模式 文件...
    chmod [选项]... --reference=参考文件 文件...
    将每个文件的权限模式变更至指定模式。
    使用 --reference 选项时，把指定文件的模式设置为与参考文件相同。
    
        选项
            -c, --changes          like verbose but report only when a change is made
            -f, --silent, --quiet  suppress most error messages
            -v, --verbose          output a diagnostic for every file processed
              --no-preserve-root  do not treat '/' specially (the default)
              --preserve-root    fail to operate recursively on '/'
              --reference=参考文件  使用参考文件的模式而非给定模式的值
            -R, --recursive        递归修改文件和目录
              --help		显示此帮助信息并退出
              --version		显示版本信息并退出
              
        实例：
            # 添加组用户的写权限。
            chmod g+w ./test.log
            # 删除其他用户的所有权限。
            chmod o= ./test.log
            # 使得所有用户都没有写权限。
            chmod a-w ./test.log
            # 当前用户具有所有权限，组用户有读写权限，其他用户只有读权限。
            chmod u=rwx, g=rw, o=r ./test.log
            # 等价的八进制数表示：
            chmod 754 ./test.log
            # 将目录以及目录下的文件都设置为所有用户拥有读写权限。
            # 注意，使用'-R'选项一定要保留当前用户的执行和读取权限，否则会报错！
            chmod -R a=rw ./testdir/
            # 根据其他文件的权限设置文件权限。
            chmod --reference=./1.log  ./test.log

说明：
    
>   u 符号代表当前用户。  
    g 符号代表和当前用户在同一个组的用户，以下简称组用户。   
    o 符号代表其他用户。    
    a 符号代表所有用户。    
    r 符号代表读权限以及八进制数4。    
    w 符号代表写权限以及八进制数2。    
    x 符号代表执行权限以及八进制数1。    
    X 符号代表如果目标文件是可执行文件或目录，可给其设置可执行权限。    
    s 符号代表设置权限suid和sgid，使用权限组合u+s设定文件的用户的ID位，g+s设置组用户ID位。    
    t 符号代表只有目录或文件的所有者才可以删除目录下的文件。    
    + 符号代表添加目标用户相应的权限。    
    - 符号代表删除目标用户相应的权限。   
    = 符号代表添加目标用户相应的权限，删除未提到的权限。

linux文件的用户权限说明：
    
    # 查看当前目录（包含隐藏文件）的长格式。
    ls -la
      -rw-r--r--   1 user  staff   651 Oct 12 12:53 .gitmodules
    
    # 第1位如果是d则代表目录，是-则代表普通文件。
    # 更多详情请参阅info coreutils 'ls invocation'（ls命令的info文档）的'-l'选项部分。
    # 第2到4位代表当前用户的权限。
    # 第5到7位代表组用户的权限。
    # 第8到10位代表其他用户的权限。

##### chown

用来变更文件或目录的拥有者或所属群组，该命令可以向某个用户授权，使该用户变成指定文件的所有者或者改变文件所属的组。用户可以是用户或者是用户D，用户组可以是组名或组id。文件名可以使由空格分开的文件列表，在文件名中可以包含通配符。

只有文件主和超级用户才可以便用该命令。

    chown [选项] [参数]
        选项
            -c或——changes：效果类似“-v”参数，但仅回报更改的部分；
            -f或--quite或——silent：不显示错误信息；
            -h或--no-dereference：只对符号连接的文件作修改，而不更改其他任何相关文件；
            -R或——recursive：递归处理，将指定目录下的所有文件及子目录一并处理；
            -v或——version：显示指令执行过程；
            --dereference：效果和“-h”参数相同；
            --help：在线帮助；
            --reference=<参考文件或目录>：把指定文件或目录的拥有者与所属群组全部设成和参考文件或目录的拥有者与所属群组相同；
            --version：显示版本信息。
        参数
            用户：组：指定所有者和所属工作组。当省略“：组”，仅改变文件所有者；
            文件：指定要改变所有者和工作组的文件列表。支持多个文件和目标，支持shell通配符。
            
        实例
            将目录/usr/meng及其下面的所有文件、子目录的文件主改成 liu：
            chown -R liu /usr/meng
         
##### cmp

用来比较两个文件是否有差异。当相互比较的两个文件完全一样时，则该指令不会显示任何信息。若发现有差异，预设会标示出第一个不通之处的字符和列数编号。若不指定任何文件名称或是所给予的文件名为“-”，则cmp指令会从标准输入设备读取数据。

    cmp [选项] [参数]
        选项
            -c或--print-chars：除了标明差异处的十进制字码之外，一并显示该字符所对应字符；
            -i<字符数目>或--ignore-initial=<字符数目>：指定一个数目；
            -l或——verbose：标示出所有不一样的地方；
            -s或--quiet或——silent：不显示错误信息；
            -v或——version：显示版本信息；
            --help：在线帮助。
        参数
            目录：比较两个文件的差异。
        
        实例
            使用cmp命令比较文件"testfile"和文件"testfile1"两个文件，则输入下面的命令：
            cmp testfile testfile1            #比较两个指定的文件
            
            在上述指令执行之前，使用cat命令查看两个指定的文件内容，如下所示：
            cat testfile                    #查看文件内容  
            Absncn 50                       #显示文件“testfile”  
            Asldssja 60  
            Jslkadjls 85 
            
            cat testfile1                   #查看文件内容  
            Absncn 50                       #显示文件“testfile1”  
            AsldssjE 62  
            Jslkadjls 85  
            然后，再执行cmp命令，并返回比较结果，具体如下所示：
            
            cmp testfile testfile1       #比较两个文件  
            testfile testfile1           #有差异：第8字节，第2行  
            
            注意：在比较结果中，只能够显示第一比较结果。   
            
##### cut

用来显示行中的指定部分，删除文件中指定字段。cut 经常用来显示文件的内容，类似于 type 命令。

说明：该命令有两项功能，其一是用来显示文件的内容，它依次读取由参数 file 所指 明的文件，将它们的内容输出到标准输出上；其二是连接两个或多个文件，如cut fl f2 > f3将把文件 fl 和 f2 的内容合并起来，然后通过输出重定向符“>”的作用，将它们放入文件 f3 中。

当文件较大时，文本在屏幕上迅速闪过（滚屏），用户往往看不清所显示的内容。因此，一般用 more 等命令分屏显示。为了控制滚屏，可以按 Ctrl+S 键，停止滚屏；按 Ctrl+Q 键可以恢复滚屏。按 Ctrl+C（中断）键可以终止该命令的执行，并且返回 Shell 提示符状态。

    cut（选项）（参数）
    
        选项
            -b：仅显示行中指定直接范围的内容；
            -c：仅显示行中指定范围的字符；
            -d：指定字段的分隔符，默认的字段分隔符为“TAB”；
            -f：显示指定字段的内容；
            -n：与“-b”选项连用，不分割多字节字符；
            --complement：补足被选择的字节、字符或字段；
            --out-delimiter= 字段分隔符：指定输出内容是的字段分割符；
            --help：显示指令的帮助信息；
            --version：显示指令的版本信息。
        参数
            文件：指定要进行内容过滤的文件。

实例

例如有一个学生报表信息，包含 No、Name、Mark、Percent：

    [root@localhost text]# cat test.txt
    No Name Mark Percent
    01 tom 69 91
    02 jack 71 87
    03 alex 68 98

使用 -f 选项提取指定字段（这里的 f 参数可以简单记忆为 --fields的缩写）：

    [root@localhost text]# cut -f 1 test.txt
    No
    01
    02
    03

    [root@localhost text]# cut -f2,3 test.txt
    Name Mark
    tom 69
    jack 71
    alex 68

--complement 选项提取指定字段之外的列（打印除了第二列之外的列）：

    [root@localhost text]# cut -f2 --complement test.txt
    No Mark Percent
    01 69 91
    02 71 87
    03 68 98

使用 -d 选项指定字段分隔符：

    [root@localhost text]# cat test2.txt
    No;Name;Mark;Percent
    01;tom;69;91
    02;jack;71;87
    03;alex;68;98

    [root@localhost text]# cut -f2 -d";" test2.txt
    Name
    tom
    jack
    alex

指定字段的字符或者字节范围

cut 命令可以将一串字符作为列来显示，字符字段的记法：

    N- ：从第 N 个字节、字符、字段到结尾；
    N-M ：从第 N 个字节、字符、字段到第 M 个（包括 M 在内）字节、字符、字段；
    -M ：从第 1 个字节、字符、字段到第 M 个（包括 M 在内）字节、字符、字段。

上面是记法，结合下面选项将摸个范围的字节、字符指定为字段：

    -b 表示字节；
    -c 表示字符；
    -f 表示定义字段。

示例

    [root@localhost text]# cat test.txt
    abcdefghijklmnopqrstuvwxyz
    abcdefghijklmnopqrstuvwxyz
    abcdefghijklmnopqrstuvwxyz
    abcdefghijklmnopqrstuvwxyz
    abcdefghijklmnopqrstuvwxyz
    
打印第 1 个到第 3 个字符：

    [root@localhost text]# cut -c1-3 test.txt
    abc
    abc
    abc
    abc
    abc

打印前 2 个字符：

    [root@localhost text]# cut -c-2 test.txt
    ab
    ab
    ab
    ab
    ab

打印从第 5 个字符开始到结尾：

    [root@localhost text]# cut -c5- test.txt
    efghijklmnopqrstuvwxyz
    efghijklmnopqrstuvwxyz
    efghijklmnopqrstuvwxyz
    efghijklmnopqrstuvwxyz
    efghijklmnopqrstuvwxyz

##### cp

##### diff

##### file

##### find

##### ln

##### less

##### locate

##### lsattr

##### more

##### mv

##### paste 

##### read 

##### rcp
 
##### rm

##### split

##### touch

##### which

##### whereis



#### 文件目录管理

##### cd 

cd --change directory 切换工作目录

    cd [选项] (目录参数)
        常用参数:
            -p 如果要切换到的目标目录是一个符号连接，直接切换到符号连接指向的目标目录
            -L 如果要切换的目标目录是一个符号的连接，直接切换到字符连接名代表的目录，而非符号连接所指向的目标目录。
            - 当仅实用"-"一个选项时，当前工作目录将被切换到环境变量"OLDPWD"所表示的目录。
        实例：
            cd    进入用户主目录；
            cd ~  进入用户主目录；
            cd -  返回进入此目录之前所在的目录；
            cd ..  返回上级目录（若当前目录为“/“，则执行完后还在“/"；".."为上级目录的意思）；
            cd ../..  返回上两级目录；
            cd !$  把上个命令的参数作为cd参数使用。

##### pwd

显示当前工作目录的绝对路径

##### ls

`ls - list directory contents ` 列出当前目录内容

说明：ll = ls -h，开启方式：vim ~/.bashrc 

	常用参数：
		-a 显示所有文件及目录 (ls内定将文件名或目录名称开头为"."的视为隐藏档，不会列出)
		-l 除文件名称外，亦将文件型态、权限、拥有者、文件大小等资讯详细列出
		-r 将文件以相反次序显示(原定依英文字母次序)
		-t 将文件依建立时间之先后次序列出
		-A 同 -a ，但不列出 "." (目前目录) 及 ".." (父目录)
		-F 在列出的文件名称后加一符号；例如可执行档则加 "*", 目录则加 "/"
		-R 若目录下有文件，则以下之文件亦皆依序列出

#### 磁盘管理

##### df 

显示磁盘空间 — 用于显示磁盘分区上的可使用的磁盘空间。默认显示单位为KB。可以利用该命令来获取硬盘被占用了多少空间，目前还剩下多少空间等信息。

    df [选项] [参数]
        选项
            -a或--all：包含全部的文件系统；
            --block-size=<区块大小>：以指定的区块大小来显示区块数目；
            -h或--human-readable：以可读性较高的方式来显示信息；
            -H或--si：与-h参数相同，但在计算时是以1000 Bytes为换算单位而非1024 Bytes；
            -i或--inodes：显示inode的信息；
            -k或--kilobytes：指定区块大小为1024字节；
            -l或--local：仅显示本地端的文件系统；
            -m或--megabytes：指定区块大小为1048576字节；
            --no-sync：在取得磁盘使用信息前，不要执行sync指令，此为预设值；
            -P或--portability：使用POSIX的输出格式；
            --sync：在取得磁盘使用信息前，先执行sync指令；
            -t<文件系统类型>或--type=<文件系统类型>：仅显示指定文件系统类型的磁盘信息；
            -T或--print-type：显示文件系统的类型；
            -x<文件系统类型>或--exclude-type=<文件系统类型>：不要显示指定文件系统类型的磁盘信息；
            --help：显示帮助；
            --version：显示版本信息。
    参数：文件 — 指定文件系统上的文件。

##### fdisk

用于观察硬盘实体使用情况，也可对硬盘分区。它采用传统的问答式界面，而非类似DOS fdisk的cfdisk互动式操作界面，因此在使用上较为不便，但功能却丝毫不打折扣。

    fdisk [选项] [参数]
        选项
             -b <大小>             扇区大小(512、1024、2048或4096)
             -c[=<模式>]           兼容模式：“dos”或“nondos”(默认)
             -h                    打印此帮助文本
             -u[=<单位>]           显示单位：“cylinders”(柱面)或“sectors”(扇区，默认)
             -v                    打印程序版本
             -C <数字>             指定柱面数
             -H <数字>             指定磁头数
             -S <数字>             指定每个磁道的扇区数
        参数
            设备文件：指定要进行分区或者显示分区的硬盘设备文件。
 
 实例
 
 首先选择要进行操作的磁盘：
 
    [root@localhost ~]# fdisk /dev/sdb
    
 输入m列出可以执行的命令：
 
     command (m for help): m
     Command action
        a   toggle a bootable flag
        b   edit bsd disklabel
        c   toggle the dos compatibility flag
        d   delete a partition
        l   list known partition types
        m   print this menu
        n   add a new partition
        o   create a new empty DOS partition table
        p   print the partition table
        q   quit without saving changes
        s   create a new empty Sun disklabel
        t   change a partition's system id
        u   change display/entry units
        v   verify the partition table
        w   write table to disk and exit
        x   extra functionality (experts only)
 
 输入p列出磁盘目前的分区情况：
 
     Command (m for help): p
     
     Disk /dev/sdb: 3221 MB, 3221225472 bytes
     255 heads, 63 sectors/track, 391 cylinders
     Units = cylinders of 16065 * 512 = 8225280 bytes
     
        Device Boot      Start         End      Blocks   Id  System
     /dev/sdb1               1           1        8001   8e  Linux LVM
     /dev/sdb2               2          26      200812+  83  Linux
 
 输入d然后选择分区，删除现有分区：
 
     Command (m for help): d
     Partition number (1-4): 1
     
     Command (m for help): d
     Selected partition 2
 
 查看分区情况，确认分区已经删除：
 
     Command (m for help): print
     
     Disk /dev/sdb: 3221 MB, 3221225472 bytes
     255 heads, 63 sectors/track, 391 cylinders
     Units = cylinders of 16065 * 512 = 8225280 bytes
     
        Device Boot      Start         End      Blocks   Id  System
     
     Command (m for help):
 
 输入n建立新的磁盘分区，首先建立两个主磁盘分区：
 
     Command (m for help): n
     Command action
        e   extended
        p   primary partition (1-4)
     p    //建立主分区
     Partition number (1-4): 1  //分区号
     First cylinder (1-391, default 1):  //分区起始位置
     Using default value 1
     last cylinder or +size or +sizeM or +sizeK (1-391, default 391): 100  //分区结束位置，单位为扇区
     
     Command (m for help): n  //再建立一个分区
     Command action
        e   extended
        p   primary partition (1-4)
     p 
     Partition number (1-4): 2  //分区号为2
     First cylinder (101-391, default 101):
     Using default value 101
     Last cylinder or +size or +sizeM or +sizeK (101-391, default 391): +200M  //分区结束位置，单位为M
 
 确认分区建立成功：
 
     Command (m for help): p
     
     Disk /dev/sdb: 3221 MB, 3221225472 bytes
     255 heads, 63 sectors/track, 391 cylinders
     Units = cylinders of 16065 * 512 = 8225280 bytes
     
        Device Boot      Start         End      Blocks   Id  System
     /dev/sdb1               1         100      803218+  83  Linux
     /dev/sdb2             101         125      200812+  83  Linux
 
 再建立一个逻辑分区：
 
     Command (m for help): n
     Command action
        e   extended
        p   primary partition (1-4)
     e  //选择扩展分区
     Partition number (1-4): 3
     First cylinder (126-391, default 126):
     Using default value 126
     Last cylinder or +size or +sizeM or +sizeK (126-391, default 391):
     Using default value 391
 
 确认扩展分区建立成功：
 
     Command (m for help): p
     
     Disk /dev/sdb: 3221 MB, 3221225472 bytes
     255 heads, 63 sectors/track, 391 cylinders
     Units = cylinders of 16065 * 512 = 8225280 bytes
     
        Device Boot      Start         End      Blocks   Id  System
     /dev/sdb1               1         100      803218+  83  Linux
     /dev/sdb2             101         125      200812+  83  Linux
     /dev/sdb3             126         391     2136645    5  Extended
 
 在扩展分区上建立两个逻辑分区：
 
     Command (m for help): n
     Command action
        l   logical (5 or over)
        p   primary partition (1-4)
     l //选择逻辑分区
     First cylinder (126-391, default 126):
     Using default value 126
     Last cylinder or +size or +sizeM or +sizeK (126-391, default 391): +400M    
     
     Command (m for help): n
     Command action
        l   logical (5 or over)
        p   primary partition (1-4)
     l
     First cylinder (176-391, default 176):
     Using default value 176
     Last cylinder or +size or +sizeM or +sizeK (176-391, default 391):
     Using default value 391
 
 确认逻辑分区建立成功：
 
     Command (m for help): p
     
     Disk /dev/sdb: 3221 MB, 3221225472 bytes
     255 heads, 63 sectors/track, 391 cylinders
     Units = cylinders of 16065 * 512 = 8225280 bytes
     
        Device Boot      Start         End      Blocks   Id  System
     /dev/sdb1               1         100      803218+  83  Linux
     /dev/sdb2             101         125      200812+  83  Linux
     /dev/sdb3             126         391     2136645    5  Extended
     /dev/sdb5             126         175      401593+  83  Linux
     /dev/sdb6             176         391     1734988+  83  Linux
     
     Command (m for help):
 
 从上面的结果我们可以看到，在硬盘sdb我们建立了2个主分区（sdb1，sdb2），1个扩展分区（sdb3），2个逻辑分区（sdb5，sdb6）
 
 注意：主分区和扩展分区的磁盘号位1-4，也就是说最多有4个主分区或者扩展分区，逻辑分区开始的磁盘号为5，因此在这个实验中试没有sdb4的。
 
 最后对分区操作进行保存：
 
     Command (m for help): w
     The partition table has been altered!
     
     Calling ioctl() to re-read partition table.
     Syncing disks.
 
 建立好分区之后我们还需要对分区进行格式化才能在系统中使用磁盘。
 
 在sdb1上建立ext2分区：
 
     [root@localhost ~]# mkfs.ext2 /dev/sdb1
     mke2fs 1.39 (29-May-2006)
     Filesystem label=
     OS type: Linux
     Block size=4096 (log=2)
     Fragment size=4096 (log=2)
     100576 inodes, 200804 blocks
     10040 blocks (5.00%) reserved for the super user
     First data block=0
     Maximum filesystem blocks=209715200
     7 block groups
     32768 blocks per group, 32768 fragments per group
     14368 inodes per group
     Superblock backups stored on blocks:
             32768, 98304, 163840
     
     Writing inode tables: done                           
     Writing superblocks and filesystem accounting information: done
     
     This filesystem will be automatically checked every 32 mounts or
     180 days, whichever comes first.  Use tune2fs -c or -i to override.
 
 在sdb6上建立ext3分区：
 
     [root@localhost ~]# mkfs.ext3 /dev/sdb6
     mke2fs 1.39 (29-May-2006)
     Filesystem label=
     OS type: Linux
     Block size=4096 (log=2)
     Fragment size=4096 (log=2)
     217280 inodes, 433747 blocks
     21687 blocks (5.00%) reserved for the super user
     First data block=0
     Maximum filesystem blocks=444596224
     14 block groups
     32768 blocks per group, 32768 fragments per group
     15520 inodes per group
     Superblock backups stored on blocks:
             32768, 98304, 163840, 229376, 294912
     
     Writing inode tables: done                           
     Creating journal (8192 blocks): done
     Writing superblocks and filesystem accounting information: done
     
     This filesystem will be automatically checked every 32 mounts or
     180 days, whichever comes first.  Use tune2fs -c or -i to override.
     [root@localhost ~]#
 
 建立两个目录/oracle和/web，将新建好的两个分区挂载到系统：
 
     [root@localhost ~]# mkdir /oracle
     [root@localhost ~]# mkdir /web
     [root@localhost ~]# mount /dev/sdb1 /oracle
     [root@localhost ~]# mount /dev/sdb6 /web
 
 查看分区挂载情况：
 
    [root@localhost ~]# df -h
    
    文件系统              容量  已用 可用 已用% 挂载点
     /dev/mapper/VolGroup00-LogVol00
                           6.7G  2.8G  3.6G  44% /
     /dev/sda1              99M   12M   82M  13% /boot
     tmpfs                 125M     0  125M   0% /dev/shm
     /dev/sdb1             773M  808K  733M   1% /oracle
     /dev/sdb6             1.7G   35M  1.6G   3% /web
 
 如果需要每次开机自动挂载则需要修改/etc/fstab文件，加入两行配置：
 
     [root@localhost ~]# vim /etc/fstab
     
     /dev/VolGroup00/LogVol00 /                       ext3    defaults        1 1
     LABEL=/boot             /boot                   ext3    defaults        1 2
     tmpfs                   /dev/shm                tmpfs   defaults        0 0
     devpts                  /dev/pts                devpts  gid=5,mode=620  0 0
     sysfs                   /sys                    sysfs   defaults        0 0
     proc                    /proc                   proc    defaults        0 0
     /dev/VolGroup00/LogVol01 swap                    swap    defaults        0 0
     /dev/sdb1               /oracle                 ext2    defaults        0 0
     /dev/sdb6               /web                    ext3    defaults        0 0
 
 
##### mount 

用于挂载Linux系统外的文件

    mount [-hV]
    mount -a [-fFnrsvw] [-t vfstype]
    mount [-fnrsvw] [-o options [,...]] device | dir
    mount [-fnrsvw] [-t vfstype] [-o options] device dir
    
        选项
            -V：显示程序版本
            -h：显示辅助讯息
            -v：显示较讯息，通常和 -f 用来除错。
            -a：将 /etc/fstab 中定义的所有档案系统挂上。
            -F：这个命令通常和 -a 一起使用，它会为每一个 mount 的动作产生一个行程负责执行。在系统需要挂上大量 NFS 档案系统时可以加快挂上的动作。
            -f：通常用在除错的用途。它会使 mount 并不执行实际挂上的动作，而是模拟整个挂上的过程。通常会和 -v 一起使用。
            -n：一般而言，mount 在挂上后会在 /etc/mtab 中写入一笔资料。但在系统中没有可写入档案系统存在的情况下可以用这个选项取消这个动作。
            -s-r：等于 -o ro
            -w：等于 -o rw
            -L：将含有特定标签的硬盘分割挂上。
            -U：将档案分割序号为 的档案系统挂下。-L 和 -U 必须在/proc/partition 这种档案存在时才有意义。
            -t：指定档案系统的型态，通常不必指定。mount 会自动选择正确的型态。
            -o async：打开非同步模式，所有的档案读写动作都会用非同步模式执行。
            -o sync：在同步模式下执行。
            -o atime、-o noatime：当 atime 打开时，系统会在每次读取档案时更新档案的『上一次调用时间』。当我们使用 flash 档案系统时可能会选项把这个选项关闭以减少写入的次数。
            -o auto、-o noauto：打开/关闭自动挂上模式。
            -o defaults:使用预设的选项 rw, suid, dev, exec, auto, nouser, and async.
            -o dev、-o nodev-o exec、-o noexec允许执行档被执行。
            -o suid、-o nosuid：
            允许执行档在 root 权限下执行。
            -o user、-o nouser：使用者可以执行 mount/umount 的动作。
            -o remount：将一个已经挂下的档案系统重新用不同的方式挂上。例如原先是唯读的系统，现在用可读写的模式重新挂上。
            -o ro：用唯读模式挂上。
            -o rw：用可读写模式挂上。
            -o loop=：使用 loop 模式用来将一个档案当成硬盘分割挂上系统。
   
实例：
    
将 /dev/hda1 挂在 /mnt 之下        

    #mount /dev/hda1 /mnt
    
将 /dev/hda1 用唯读模式挂在 /mnt 之下。

    #mount -o ro /dev/hda1 /mnt
    
将 /tmp/image.iso 这个光碟的 image 档使用 loop 模式挂在 /mnt/cdrom 之下。用这种方法可以将一般网络上可以找到的 Linux 光 碟 ISO 档在不烧录成光碟的情况下检视其内容。
    
    #mount -o loop /tmp/image.iso /mnt/cdrom
    
##### umount   

用于卸载已经加载的文件系统。利用设备名或挂载点都能umount文件系统，不过最好还是通过挂载点卸载，以免使用绑定挂载（一个设备，多个挂载点）时产生混乱。 
    
    umount [选项] (参数)
        选项:
            -a：卸除/etc/mtab中记录的所有文件系统；
            -h：显示帮助；
            -n：卸除时不要将信息存入/etc/mtab文件中；
            -r：若无法成功卸除，则尝试以只读的方式重新挂入文件系统；
            -t<文件系统类型>：仅卸除选项中所指定的文件系统；
            -v：执行时显示详细的信息；
            -V：显示版本信息。
    
实例：

下面两条命令分别通过设备名和挂载点卸载文件系统，同时输出详细信息：

通过设备名卸载

    umount -v /dev/sda1
    /dev/sda1 umounted

通过挂载点卸载

    umount -v /mnt/mymount/
    /tmp/diskboot.img umounted

如果设备正忙，卸载即告失败。卸载失败的常见原因是，某个打开的shell当前目录为挂载点里的某个目录：

    umount -v /mnt/mymount/
    umount: /mnt/mymount: device is busy
    umount: /mnt/mymount: device is busy

有时，导致设备忙的原因并不好找。碰到这种情况时，可以用lsof列出已打开文件，然后搜索列表查找待卸载的挂载点：

    lsof | grep mymount         查找mymount分区里打开的文件
    bash   9341  francois  cwd   DIR   8,1   1024    2 /mnt/mymount

从上面的输出可知，mymount分区无法卸载的原因在于，francois运行的PID为9341的bash进程。

对付系统文件正忙的另一种方法是执行延迟卸载：

    umount -vl /mnt/mymount/     执行延迟卸载

延迟卸载（lazy unmount）会立即卸载目录树里的文件系统，等到设备不再繁忙时才清理所有相关资源。卸载可移动存储介质还可以用eject命令。下面这条命令会卸载cd并弹出CD：

    eject /dev/cdrom      卸载并弹出CD 
    
    
### 参考

[Linux命令查询](https://jaywcjlove.gitee.io/linux-command)    
[菜鸟教程](https://www.runoob.com/linux)    
[视频教程](https://www.bilibili.com/video/av21303002)    

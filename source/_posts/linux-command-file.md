---
title: Linux常用命令-文件管理  
tags:   
  - linux  
categories:
  - linux    
description: 文件管理常用命令....    
date: 2020-02-16 13:19:00
---

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

将源文件或目录复制到目标文件或目录中 - copy files and directories
用来将一个或多个源文件或者目录复制到指定的目的文件或目录。它可以将单个源文件复制成一个指定文件名的具体的文件或一个已经存在的目录下。cp命令还支持同时复制多个文件，当一次复制多个文件时，目标文件参数必须是一个已经存在的目录，否则将出现错误

    cp [选项] [参数]
        选项：
            -a：此参数的效果和同时指定"-dpR"参数相同；
            -d：当复制符号连接时，把目标文件或目录也建立为符号连接，并指向与源文件或目录连接的原始文件或目录；
            -f：强行复制文件或目录，不论目标文件或目录是否已存在；
            -i：覆盖既有文件之前先询问用户；
            -l：对源文件建立硬连接，而非复制文件；
            -p：保留源文件或目录的属性；
            -R/r：递归处理，将指定目录下的所有文件与子目录一并处理；
            -s：对源文件建立符号连接，而非复制文件；
            -u：使用这项参数后只会在源文件的更改时间较目标文件更新时或是名称相互对应的目标文件并不存在时，才复制文件；
            -S：在备份文件时，用指定的后缀“SUFFIX”代替文件的默认后缀；
            -b：覆盖已存在的文件目标前将目标文件备份；
            -v：详细显示命令执行的操作。
        参数：
            源文件：制定源文件列表。默认情况下，cp命令不能复制目录，如果要复制目录，则必须使用-R选项；
            目标文件：指定目标文件。当“源文件”为多个文件时，要求“目标文件”为指定的目录。

实例：

    （-r 是“递归”， -u 是“更新”，-v 是“详细”）
    $ cp -r -u -v /usr/men/tmp ~/men/tmp 

    版本备份 --backup=numbered 参数意思为“我要做个备份，而且是带编号的连续备份”。所以一个备份就是 1 号，第二个就是 2 号，等等。
    $ cp --force --backup=numbered test1.py test1.py
    $ ls
    test1.py test1.py.~1~ test1.py.~2~

    # 将当前目录下所有文件，复制到当前目录的兄弟目录 backup 文件夹中
    cp -rfb ./* ../backup

##### diff

比较给定的两个文件的不同

    diff [选项] [参数]
        选项：
            -<行数>：指定要显示多少行的文本。此参数必须与-c或-u参数一并使用；
            -a或——text：diff预设只会逐行比较文本文件；
            -b或--ignore-space-change：不检查空格字符的不同；
            -B或--ignore-blank-lines：不检查空白行；
            -c：显示全部内容，并标出不同之处；
            -C<行数>或--context<行数>：与执行“-c-<行数>”指令相同；
            -d或——minimal：使用不同的演算法，以小的单位来做比较；
            -D<巨集名称>或ifdef<巨集名称>：此参数的输出格式可用于前置处理器巨集；
            -e或——ed：此参数的输出格式可用于ed的script文件；
            -f或-forward-ed：输出的格式类似ed的script文件，但按照原来文件的顺序来显示不同处；
            -H或--speed-large-files：比较大文件时，可加快速度；
            -l<字符或字符串>或--ignore-matching-lines<字符或字符串>：若两个文件在某几行有所不同，而之际航同时都包含了选项中指定的字符或字符串，则不显示这两个文件的差异；
            -i或--ignore-case：不检查大小写的不同；
            -l或——paginate：将结果交由pr程序来分页；
            -n或——rcs：将比较结果以RCS的格式来显示；
            -N或--new-file：在比较目录时，若文件A仅出现在某个目录中，预设会显示：Only in目录，文件A 若使用-N参数，则diff会将文件A 与一个空白的文件比较；
            -p：若比较的文件为C语言的程序码文件时，显示差异所在的函数名称；
            -P或--unidirectional-new-file：与-N类似，但只有当第二个目录包含了第一个目录所没有的文件时，才会将这个文件与空白的文件做比较；
            -q或--brief：仅显示有无差异，不显示详细的信息；
            -r或——recursive：比较子目录中的文件；
            -s或--report-identical-files：若没有发现任何差异，仍然显示信息；
            -S<文件>或--starting-file<文件>：在比较目录时，从指定的文件开始比较；
            -t或--expand-tabs：在输出时，将tab字符展开；
            -T或--initial-tab：在每行前面加上tab字符以便对齐；
            -u，-U<列数>或--unified=<列数>：以合并的方式来显示文件内容的不同；
            -v或——version：显示版本信息；
            -w或--ignore-all-space：忽略全部的空格字符；
            -W<宽度>或--width<宽度>：在使用-y参数时，指定栏宽；
            -x<文件名或目录>或--exclude<文件名或目录>：不比较选项中所指定的文件或目录；
            -X<文件>或--exclude-from<文件>；您可以将文件或目录类型存成文本文件，然后在=<文件>中指定此文本文件；
            -y或--side-by-side：以并列的方式显示文件的异同之处；
            --left-column：在使用-y参数时，若两个文件某一行内容相同，则仅在左侧的栏位显示该行内容；
            --suppress-common-lines：在使用-y参数时，仅显示不同之处。
        参数：
            文件1：指定要比较的第一个文件；
            文件2：指定要比较的第二个文件。

实例：

    将目录/usr/li下的文件"test.txt"与当前目录下的文件"test.txt"进行比较，输入如下命令：

    diff /usr/li test.txt     #使用diff指令对文件进行比较

    上面的命令执行后，会将比较后的不同之处以指定的形式列出，如下所示：
    n1 a n3,n4  
    n1,n2 d n3  
    n1,n2 c n3,n4 

其中，字母"a"、"d"、"c"分别表示添加、删除及修改操作。而"n1"、"n2"表示在文件1中的行号，"n3"、"n4"表示在文件2中的行号。

注意：以上说明指定了两个文件中不同处的行号及其相应的操作。在输出形式中，每一行后面将跟随受到影响的若干行。其中，以<开始的行属于文件1，以>开始的行属于文件2。

##### file

用来探测给定文件的类型

    file [选项] [参数]
        -b：列出辨识结果时，不显示文件名称；
        -c：详细显示指令执行过程，便于排错或分析程序执行的情形；
        -f<名称文件>：指定名称文件，其内容有一个或多个文件名称时，让file依序辨识这些文件，格式为每列一个文件名称；
        -L：直接显示符号连接所指向的文件类别；
        -m<魔法数字文件>：指定魔法数字文件；
        -v：显示版本信息；
        -z：尝试去解读压缩文件的内容。
实例：    

显示文件类型

    [root@localhost ~]# file install.log
    install.log: UTF-8 Unicode text

    [root@localhost ~]# file -b install.log      <== 不显示文件名称
    UTF-8 Unicode text

    [root@localhost ~]# file -i install.log      <== 显示MIME类别。
    install.log: text/plain; charset=utf-8

    [root@localhost ~]# file -b -i install.log
    text/plain; charset=utf-8
    
显示符号链接的文件类型

    [root@localhost ~]# ls -l /var/mail
    lrwxrwxrwx 1 root root 10 08-13 00:11 /var/mail -> spool/mail

    [root@localhost ~]# file /var/mail
    /var/mail: symbolic link to `spool/mail'

    [root@localhost ~]# file -L /var/mail
    /var/mail: directory

    [root@localhost ~]# file /var/spool/mail
    /var/spool/mail: directory

    [root@localhost ~]# file -L /var/spool/mail
    /var/spool/mail: directory


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



  
### 参考

[Linux命令查询](https://jaywcjlove.gitee.io/linux-command)    
[菜鸟教程](https://www.runoob.com/linux)    
[视频教程](https://www.bilibili.com/video/av21303002)    


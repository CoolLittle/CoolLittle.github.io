---
title: Linux常用命令-文件管理  
tags:   
  - linux  
categories:
  - linux    
description: 文件管理常用命令....    
date: 2020-02-16 13:19:00
---

#### awk

--------------------------------------------------------------------

#### ar

ar命令 是一个建立或修改备存文件，或是从备存文件中抽取文件的工具，ar可让您集合许多文件，成为单一的备存文件。在备存文件中，所有成员文件皆保有原来的属性与权限

类似命令：[tar](#tar)

    ar [选项] [参数]
        选项：
            d            - 从归档文件中删除文件
            m[ab]        - 在归档文件中移动文件
            p            - 打印在归档文件中找到的文件
            q[f]         - 将文件快速追加到归档文件中
            r[ab][f][u]  - 替换归档文件中已有的文件或加入新文件
            s            - 作为 ranlib 工作
            t[O][v]      - display contents of the archive
            x[o]         - 从归档文件中分解文件
            特定命令修饰符：
            [a]          - 将文件置于 [成员名] 之后
            [b]          - 将文件置于 [成员名] 之前 (于 [i] 相同)
            [D]          - 将 0 用于时间戳和 uid/gid（默认）
            [D]          - 使用实际时间戳和 uid/gid
            [N]          - 使用名称的实例 [数量]
            [f]          - 截去插入的文件名称
            [P]          - 在匹配时使用完整的路径名
            [o]          - 保留原来的日期
            [O]          - display offsets of files in the archive
            [u]          - 只替换比当前归档内容更新的文件
            通用修饰符：
            [c]          - 不在必须创建库的时候给出警告
            [s]          - 创建归档索引 (cf. ranlib)
            [S]          - 不要创建符号表
            [T]          - 产生一个简单归档
            [v]          - 输出较多信息
            [V]          - 显示版本号

实例：

打包文件

    [root@localhost ~]# ar rv one.bak a.c b.c  # 打包 a.c b.c文件 
    ar: 正在创建 one.bak
    a - a.c
    a - b.c

打包多个文件

    [root@localhost ~]# ar rv two.bak *.c  // 打包以.c结尾的文件  
    ar: 正在创建 two.bak
    a - a.c
    a - b.c
    a - c.c
    a - d.c

显示打包文件的内容

    [root@localhost ~]# ar t two.bak    
    a.c
    b.c
    c.c
    d.c

删除打包文件的成员文件

    [root@localhost ~]# ar d two.bak a.c b.c c.c  
    [root@localhost ~]# ar t two.bak       
    d.c

--------------------------------------------------------------------

#### bzip2

将文件压缩成bz2格式,bzip2 采用 Burrows-Wheeler 块排序文本压缩算法和 Huffman 编码方式压缩文件。 压缩率一般比基于 LZ77/LZ78 的压缩软件好得多，其性能接近 PPM 族统计类压缩软件。

**压缩文件后源文件会消失加 _k_ 选项后会保留源文件解压缩同理，没有保留文件权限、属组功能**

bzip2 从命令行读入文件名和参数。 每个文件被名为 "原始文件名.bz2" 的压缩文件替换。 每个压缩文件具有与原文件相同的修改时间、 权限， 如果可能的话，还具有相同的属主， 因此在解压缩时这些特性将正确地恢复。 在某些文件系统中， 没有权限、 属主或时间的概念， 或者对文件名的长度有严格限制， 例如 MSDOS，在这种情况下，bzip2 没有保持原文件名、 属主、 权限以及时间的机制， 从这个意义上说，bzip2 对文件名的处理是幼稚的

    bzip2 [选项] [参数]
        选项：
            -c --stdout # 将数据压缩或解压缩至标准输出。
            -d --decompress # 强制解压缩。 bzip2, bunzip2 以及 bzcat 实际上是同一个程序，进行何种操作将根据程序名确定。  指定该选项后将不考虑这一机制，强制 bzip2 进行解压缩。
            -z --compress # -d 选项的补充：强制进行压缩操作，而不管执行的是哪个程序。
            -t --test # 检查指定文件的完整性，但并不对其解压缩。 实际上将对数据进行实验性的解压缩操作，而不输出结果。
            -f --force # 强制覆盖输出文件。通常 bzip2 不会覆盖已经存在的文件。该选项还强制 bzip2 打破文件的硬连接，缺省情况下 bzip2 不会这么做。
            -k --keep # 在压缩或解压缩时保留输入文件（不删除这些文件）。
            -s --small # 在压缩、解压缩及检查时减少内存用量。采用一种修正的算法进行压缩和测试，每个数据块仅需要 2.5 个字节。这意味着任何文件都可以在 2300k 的内存中进行解压缩， 尽管速度只有通常情况下的一半。在压缩时，-s将选定 200k 的块长度，内存用量也限制在 200k 左右， 代价是压缩率会降低。 总之，如果机器的内存较少（8兆字节或更少）， 可对所有操作都采用-s选项。参见下面的内存管理。
            -q --quiet  压制不重要的警告信息。属于 I/O 错误及其它严重事件的信息将不会被压制。
            -v --verbose # 详尽模式 -- 显示每个被处理文件的压缩率。 命令行中更多的 -v 选项将增加详细的程度， 使 bzip2 显示出许多主要用于诊断目的信息。
            -1 to -9 # 在压缩时将块长度设为 100 k、200 k ..  900 k。 对解压缩没有影响。参见下面的内存管理。
            -- # 将所有后面的命令行变量看作文件名，即使这些变量以减号"-"打头。 可用这一选项处理以减号"-"打头的文件名， 例如：bzip2 -- -myfilename.
        参数：
            文件：指定要压缩的文件

实例：

压缩指定文件filename:

    bzip2 filename
    或
    bzip2 -z filename

这里，压缩的时候不会输出，会将原来的文件filename给删除，替换成filename.bz2.如果以前有filename.bz2则不会替换并提示错误（如果想要替换则指定-f选项，例如bzip2 -f filename；如果filename是目录则也提醒错误不做任何操作；如果filename已经是压过的了有bz2后缀就提醒一下，不再压缩，没有bz2后缀会再次压缩。

解压指定的文件filename.bz2:

    bzip2 -d filename.bz2
    或
    bunzip2 filename.bz2

这里，解压的时候没标准输出，会将原来的文件filename.bz2给替换成filename。如果以前有filename则不会替换并提示错误（如果想要替换则指定-f选项，例如bzip2 -df filename.bz2。

模拟解压实际并不解压：

    bzip2 -tv filename.bz2

使用bzip2的时候将所有后面的看作文件(即使文件名以'-'开头)：

    bzip2 -- -myfilename

这里主要是为了防止文件名中-产生以为是选项的歧义。

--------------------------------------------------------------------

#### cat

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
    echo '#####' |cat - test.log

--------------------------------------------------------------------

#### chattr

修改文件属性;查看文件属性：[lsattr](#lsattr)

    chattr [选项] [参数]
        选项:
            -R     递归处理
            -f     Suppress most error messages.
            -p     查看项目编号

文件有如下属性：

    A：即Atime，告诉系统不要修改对这个文件的最后访问时间。
    S：即Sync，一旦应用程序对这个文件执行了写操作，使系统立刻把修改的结果写到磁盘。
    a：即Append Only，系统只允许在这个文件之后追加数据，不允许任何进程覆盖或截断这个文件。如果目录具有这个属性，系统将只允许在这个目录下建立和修改文件，而不允许删除任何文件。
    b：不更新文件或目录的最后存取时间。
    c：将文件或目录压缩后存放。
    d：当dump程序执行时，该文件或目录不会被dump备份。
    D:检查压缩文件中的错误。
    i：即Immutable，系统不允许对这个文件进行任何的修改。如果目录具有这个属性，那么任何的进程只能修改目录之下的文件，不允许建立和删除文件。
    s：彻底删除文件，不可恢复，因为是从磁盘上删除，然后用0填充文件所在区域。
    u：当一个应用程序请求删除这个文件，系统会保留其数据块以便以后能够恢复删除这个文件，用来防止意外删除文件或目录。
    t:文件系统支持尾部合并（tail-merging）。
    X：可以直接访问压缩文件的内容。

实例：

用chattr命令防止系统中某个关键文件被修改：

    chattr +i /etc/fstab

然后试一下rm、mv、rename等命令操作于该文件，都是得到Operation not permitted的结果。

让某个文件只能往里面追加内容，不能删除，一些日志文件适用于这种操作：

    chattr +a /data1/user_act.log


--------------------------------------------------------------------

#### chgrp

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

 --------------------------------------------------------------------       

#### chmod

用来变更文件或目录的权限

1. 通过符号组合的方式更改目标文件或目录的权限。
2. 通过八进制数的方式更改目标文件或目录的权限。
3. 通过参考文件的权限来更改目标文件或目录的权限。      

```
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
```

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

--------------------------------------------------------------------

#### chown

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
         
--------------------------------------------------------------------   

#### cmp

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

--------------------------------------------------------------------

#### cut

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

--------------------------------------------------------------------

#### cp

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

--------------------------------------------------------------------

#### diff

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

--------------------------------------------------------------------

#### file

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

--------------------------------------------------------------------

#### find

用来在指定目录下查找文件。任何位于参数之前的字符串都将被视为欲查找的目录名。如果使用该命令时，不设置任何参数，则find命令将在当前目录下查找子目录与文件。并且将查找到的子目录和文件全部进行显示。

    find [选项] [参数]
        选项
            -amin<分钟>：查找在指定时间曾被存取过的文件或目录，单位以分钟计算；
            -anewer<参考文件或目录>：查找其存取时间较指定文件或目录的存取时间更接近现在的文件或目录；
            -atime<24小时数>：查找在指定时间曾被存取过的文件或目录，单位以24小时计算；
            -cmin<分钟>：查找在指定时间之时被更改过的文件或目录；
            -cnewer<参考文件或目录>查找其更改时间较指定文件或目录的更改时间更接近现在的文件或目录；
            -ctime<24小时数>：查找在指定时间之时被更改的文件或目录，单位以24小时计算；
            -daystart：从本日开始计算时间；
            -depth：从指定目录下最深层的子目录开始查找；
            -expty：寻找文件大小为0 Byte的文件，或目录下没有任何子目录或文件的空目录；
            -exec<执行指令>：假设find指令的回传值为True，就执行该指令；
            -false：将find指令的回传值皆设为False；
            -fls<列表文件>：此参数的效果和指定“-ls”参数类似，但会把结果保存为指定的列表文件；
            -follow：排除符号连接；
            -fprint<列表文件>：此参数的效果和指定“-print”参数类似，但会把结果保存成指定的列表文件；
            -fprint0<列表文件>：此参数的效果和指定“-print0”参数类似，但会把结果保存成指定的列表文件；
            -fprintf<列表文件><输出格式>：此参数的效果和指定“-printf”参数类似，但会把结果保存成指定的列表文件；
            -fstype<文件系统类型>：只寻找该文件系统类型下的文件或目录；
            -gid<群组识别码>：查找符合指定之群组识别码的文件或目录；
            -group<群组名称>：查找符合指定之群组名称的文件或目录；
            -help或——help：在线帮助；
            -ilname<范本样式>：此参数的效果和指定“-lname”参数类似，但忽略字符大小写的差别；
            -iname<范本样式>：此参数的效果和指定“-name”参数类似，但忽略字符大小写的差别；
            -inum<inode编号>：查找符合指定的inode编号的文件或目录；
            -ipath<范本样式>：此参数的效果和指定“-path”参数类似，但忽略字符大小写的差别；
            -iregex<范本样式>：此参数的效果和指定“-regexe”参数类似，但忽略字符大小写的差别；
            -links<连接数目>：查找符合指定的硬连接数目的文件或目录；
            -iname<范本样式>：指定字符串作为寻找符号连接的范本样式；
            -ls：假设find指令的回传值为Ture，就将文件或目录名称列出到标准输出；
            -maxdepth<目录层级>：设置最大目录层级；
            -mindepth<目录层级>：设置最小目录层级；
            -mmin<分钟>：查找在指定时间曾被更改过的文件或目录，单位以分钟计算；
            -mount：此参数的效果和指定“-xdev”相同；
            -mtime<24小时数>：查找在指定时间曾被更改过的文件或目录，单位以24小时计算；
            -name<范本样式>：指定字符串作为寻找文件或目录的范本样式；
            -newer<参考文件或目录>：查找其更改时间较指定文件或目录的更改时间更接近现在的文件或目录；
            -nogroup：找出不属于本地主机群组识别码的文件或目录；
            -noleaf：不去考虑目录至少需拥有两个硬连接存在；
            -nouser：找出不属于本地主机用户识别码的文件或目录；
            -ok<执行指令>：此参数的效果和指定“-exec”类似，但在执行指令之前会先询问用户，若回答“y”或“Y”，则放弃执行命令；
            -path<范本样式>：指定字符串作为寻找目录的范本样式；
            -perm<权限数值>：查找符合指定的权限数值的文件或目录；
            -print：假设find指令的回传值为Ture，就将文件或目录名称列出到标准输出。格式为每列一个名称，每个名称前皆有“./”字符串；
            -print0：假设find指令的回传值为Ture，就将文件或目录名称列出到标准输出。格式为全部的名称皆在同一行；
            -printf<输出格式>：假设find指令的回传值为Ture，就将文件或目录名称列出到标准输出。格式可以自行指定；
            -prune：不寻找字符串作为寻找文件或目录的范本样式;
            -regex<范本样式>：指定字符串作为寻找文件或目录的范本样式；
            -size<文件大小>：查找符合指定的文件大小的文件；
            -true：将find指令的回传值皆设为True；
            -type<文件类型>：只寻找符合指定的文件类型的文件；
            -uid<用户识别码>：查找符合指定的用户识别码的文件或目录；
            -used<日数>：查找文件或目录被更改之后在指定时间曾被存取过的文件或目录，单位以日计算；
            -user<拥有者名称>：查找符和指定的拥有者名称的文件或目录；
            -version或——version：显示版本信息；
            -xdev：将范围局限在先行的文件系统中；
            -xtype<文件类型>：此参数的效果和指定“-type”参数类似，差别在于它针对符号连接检查。
        参数
            起始目录：查找文件的起始目录。

实例

1. **根据文件或者正则表达式进行匹配**

在/home目录下查找以.txt结尾的文件名

    find /home -name "*.txt"

基于正则表达式匹配文件路径

    find . -regex ".*\(\.txt\|\.pdf\)$"

当前目录及子目录下查找所有以.txt和.pdf结尾的文件

    find . \( -name "*.txt" -o -name "*.pdf" \)
    或
    find . -name "*.txt" -o -name "*.pdf"

匹配文件路径或者文件

    find /usr/ -path "*local*"

2. **否定参数**

找出/home下不是以.txt结尾的文件

    find /home ! -name "*.txt"

根据文件类型进行搜索

    find . -type 类型参数

类型参数列表：

    f 普通文件
    l 符号连接
    d 目录
    c 字符设备
    b 块设备
    s 套接字
    p Fifo

3. **基于目录深度搜索**

向下最大深度限制为3

    find . -maxdepth 3 -type f

搜索出深度距离当前目录至少2个子目录的所有文件

    find . -mindepth 2 -type f

4. **根据文件时间戳进行搜索**

```
find . -type f 时间戳 （'-'之内   '+'表示之前 无表示当前时间）
```

UNIX/Linux文件系统每个文件都有三种时间戳：

    访问时间 （-atime/天，-amin/分钟）：用户最近一次访问时间。
    修改时间 （-mtime/天，-mmin/分钟）：文件最后一次修改时间。
    变化时间 （-ctime/天，-cmin/分钟）：文件数据元（例如权限等）最后一次修改时间。

搜索最近七天内被访问过的所有文件

    find . -type f -atime -7

搜索恰好在第七天被访问过的所有文件

    find . -type f -atime 7

搜索超过七天内被访问过的所有文件

    find . -type f -atime +7

搜索访问时间超过10分钟的所有文件

    find . -type f -amin +10

找出比file.log修改时间更长的所有文件

    find . -type f -newer file.log

5. **根据文件大小进行匹配**   
     
```
    find . -type f -size 文件大小单元 (-表示小于  +表示大于  无表示等于)
```

文件大小单元：

    b —— 块（512字节）
    c —— 字节
    w —— 字（2字节）
    k —— 千字节
    M —— 兆字节
    G —— 吉字节

搜索大于10KB的文件

    find . -type f -size +10k

搜索小于10KB的文件

    find . -type f -size -10k

搜索等于10KB的文件

    find . -type f -size 10k

6. **删除匹配文件**

删除当前目录下所有.txt文件

    find . -type f -name "*.txt" -delete

7. **根据文件权限/所有权进行匹配**

当前目录下搜索出权限为777的文件

    find . -type f -perm 777

找出当前目录下权限不是644的php文件

    find . -type f -name "*.php" ! -perm 644

找出当前目录用户tom拥有的所有文件

    find . -type f -user tom

找出当前目录用户组sunk拥有的所有文件

    find . -type f -group sunk

8. **借助-exec选项与其他命令结合使用**

找出当前目录下所有root的文件，并把所有权更改为用户tom

    find .-type f -user root -exec chown tom {} \; 

上例中， {} 用于与 -exec 选项结合使用来匹配所有文件，然后会被替换为相应的文件名，反斜杠必须加。

查找当前目录下所有.txt文件并把他们拼接起来写入到all.txt文件中

    find . -type f -name "*.txt" -exec cat {} \;> /all.txt

将30天前的.log文件移动到old目录中

    find . -type f -mtime +30 -name "*.log" -exec cp {} old \;

找出当前目录下所有.txt文件并以“File:文件名”的形式打印出来

    find . -type f -name "*.txt" -exec printf "File: %s\n" {} \;

因为单行命令中-exec参数中无法使用多个命令，以下方法可以实现在-exec之后接受多条命令

    -exec ./text.sh {} \;

9. **搜索但跳出指定的目录**

查找当前目录或者子目录下所有.txt文件，但是跳过子目录sk

    find . -path "./sk" -prune -o -name "*.txt" -print

10. **find其他技巧收集**

要列出所有长度为零的文件

    find . -empty

统计代码行数

    find . -name "*.java"|xargs cat|grep -v ^$|wc -l # 代码行数统计, 排除空行

在主目录中找到对所有人可读的文件。

    find ~ -perm -o=r

删除 mac 下自动生成的文件

    find ./ -name '__MACOSX' -depth -exec rm -rf {} \;

 -----------------------------------------------------------------

#### ln

ln命令 用来为文件创建链接，链接类型分为硬链接和符号链接两种，默认的链接类型是硬链接。如果要创建符号链接必须使用"-s"选项。

注意：符号链接文件不是一个独立的文件，它的许多属性依赖于源文件，所以给符号链接文件设置存取权限是没有意义的。

    ln [选项]... [-T] 目标 链接名    (第一种格式)
　  ln [选项]... 目标        (第二种格式)
　  ln [选项]... 目标... 目录    (第三种格式)
　  ln [选项]... -t 目录 目标...    (第四种格式)
        选项：
            --backup[=CONTROL]  为每个已存在的目标文件创建备份文件
            -b        类似--backup，但不接受任何参数
            -d, -F, --directory   创建指向目录的硬链接(只适用于超级用户)
            -f, --force     强行删除任何已存在的目标文件
            -i, --interactive           覆盖既有文件之前先询问用户；
            -L, --logical               取消引用作为符号链接的目标
            -n, --no-dereference        把符号链接的目的目录视为一般文件；
            -P, --physical              直接将硬链接到符号链接
            -r, --relative              创建相对于链接位置的符号链接
            -s, --symbolic              对源文件建立符号链接，而非硬链接；
            -S, --suffix=SUFFIX         用"-b"参数备份目标文件后，备份文件的字尾会被加上一个备份字符串，预设的备份字符串是符号“~”，用户可通过“-S”参数来改变它；
            -t, --target-directory=DIRECTORY  指定要在其中创建链接的DIRECTORY
            -T, --no-target-directory   将“LINK_NAME”视为常规文件
            -v, --verbose               打印每个链接文件的名称
        参数：
            源文件：指定链接的源文件。如果使用-s选项创建符号链接，则“源文件”可以是文件或者目录。创建硬链接时，则“源文件”参数只能是文件；
            目标文件：指定源文件的目标链接文件。

实例：

将目录/usr/mengqc/mub1下的文件m2.c链接到目录/usr/liu下的文件a2.c

    cd /usr/mengqc
    ln /mub1/m2.c /usr/liu/a2.c

在执行ln命令之前，目录/usr/liu中不存在a2.c文件。执行ln之后，在/usr/liu目录中才有a2.c这一项，表明m2.c和a2.c链接起来（注意，二者在物理上是同一文件），利用ls -l命令可以看到链接数的变化。

在目录/usr/liu下建立一个符号链接文件abc，使它指向目录/usr/mengqc/mub1

    ln -s /usr/mengqc/mub1 /usr/liu/abc

执行该命令后，/usr/mengqc/mub1代表的路径将存放在名为/usr/liu/abc的文件中。

**_解释：_**

Linux具有为一个文件起多个名字的功能，称为链接。被链接的文件可以存放在相同的目录下，但是必须有不同的文件名，而不用在硬盘上为同样的数据重复备份。另外，被链接的文件也可以有相同的文件名，但是存放在不同的目录下，这样只要对一个目录下的该文件进行修改，就可以完成对所有目录下同名链接文件的修改。对于某个文件的各链接文件，我们可以给它们指定不同的存取权限，以控制对信息的共享和增强安全性。

文件链接有两种形式，即硬链接和符号链接。

**硬链接**

建立硬链接时，在另外的目录或本目录中增加目标文件的一个目录项，这样，一个文件就登记在多个目录中。如上所示的m2.c文件就在目录mub1和liu中都建立了目录项。

创建硬链接后，己经存在的文件的I节点号（Inode）会被多个目录文件项使用。一个文件的硬链接数可以在目录的长列表格式的第二列中看到，无额外链接的文件的链接数为l。

在默认情况下，ln命令创建硬链接。ln命令会增加链接数，rm命令会减少链接数。一个文件除非链接数为0，否则不会从文件系统中被物理地删除。

对硬链接有如下限制：

    不能对目录文件做硬链接。
    不能在不同的文件系统之间做硬链接。就是说，链接文件和被链接文件必须位于同一个文件系统中。

**符号链接**

符号链接也称为软链接，是将一个路径名链接到一个文件。这些文件是一种特别类型的文件。事实上，它只是一个文本文件，其中包含它提供链接的另一个文件的路径名。另一个文件是实际包含所有数据的文件。所有读、写文件内容的命令被用于符号链接时，将沿着链接方向前进来访问实际的文件。

!符号连接

与硬链接不同的是，符号链接确实是一个新文件，当然它具有不同的I节点号；而硬链接并没有建立新文件。

符号链接没有硬链接的限制，可以对目录文件做符号链接，也可以在不同文件系统之间做符号链接。

用ln -s命令建立符号链接时，源文件最好用绝对路径名。这样可以在任何工作目录下进行符号链接。而当源文件用相对路径时，如果当前的工作路径与要创建的符号链接文件所在路径不同，就不能进行链接。

符号链接保持了链接与源文件或目录之间的**区别**：

    删除源文件或目录，只删除了数据，不会删除链接。一旦以同样文件名创建了源文件，链接将继续指向该文件的新数据。
    在目录长列表中，符号链接作为一种特殊的文件类型显示出来，其第一个字母是l。
    符号链接的大小是其链接文件的路径名中的字节数。
    当用ln -s命令列出文件时，可以看到符号链接名后有一个箭头指向源文件或目录，例如lrwxrwxrwx … 14 jun 20 10:20 /etc/motd->/original_file其中，表示“文件大小”的数字“14”恰好说明源文件名original_file由14个字符构成。

 -----------------------------------------------------------------

#### less

less命令 的作用与 [more](#more) 十分相似，都可以用来浏览文字档案的内容，不同的是less命令允许用户向前或向后浏览文件，而more命令只能向前浏览。用less命令显示文件时，用PageUp键向上翻页，用PageDown键向下翻页。要退出less程序，应按Q键。

less每次只加载文件显示部分内容。

    less [选项] [参数]
        选项：
            -e：文件内容显示完毕后，自动退出；
            -f：强制显示文件；
            -g：不加亮显示搜索到的所有关键词，仅显示当前显示的关键字，以提高显示速度；
            -l：搜索时忽略大小写的差异；
            -N：每一行行首显示行号；
            -s：将连续多个空行压缩成一行显示；
            -S：在单行显示较长的内容，而不换行显示；
            -x<数字>：将TAB字符显示为指定个数的空格字符。
        参数：
            文件：指定要分屏显示内容的文件。    

快捷操作：

| 按键       | 说明        |
|------------|------------|
| b（back）  | 向上翻一页  |
| u（undo）  | 向上翻半页  |
| 空格、z     | 向下翻一页  |
| d（down）  | 向下翻半页  |
| 回车、e、j   | 向下一行    |
| y、k        | 向上一行    |
| g\[n]       | 跳转到第n行,默认第一行 |
| G          | 跳转到结尾  |
| p\[n]       |  跳转到百分之n行 |
| /          | 向下匹配     |
| ?          | 向上匹配     |

实例：

    less a.md

 -----------------------------------------------------------------

#### locate

通过名字查找文件

    locate [选项] [样式]
        选项：
            -A, --all              打印所有匹配条目
            -b, --basename         打印匹配名字的路径
            -c, --count            打印匹配数量
            -d, --database DBPATH  使用 DBPATH 替换 默认 database 
            -e, --existing         打印当前存在的文件的条目
            -L, --follow           检查文件是否存在时，跟随后面的符号链接(默认)
            -i, --ignore-case      忽略大小写
            -l, --limit, -n LIMIT  输出限制条数
            -m, --mmap             ignored, for backward compatibility
            -P, --nofollow, -H     检查文件时不要跟随后面的符号链接存在
            -0, --null             在输出中使用NUL分隔条目
            -S, --statistics       数据库不要搜索条目，打印每个条目的统计数据
            -q, --quiet            安静模式，不打印读取数据库的错误
            -r, --regexp REGEXP    基于正则匹配搜索
                --regex            patterns are extended regexps
            -s, --stdio            ignored, for backward compatibility
            -w, --wholename       输出全路径名 (默认)

实例：

搜索etc目录下，所有以m开头的文件

    root ~ # locate /etc/m
    /etc/magic
    /etc/magic.mime
    /etc/mailcap
    /etc/mailcap.order
    /etc/manpath.config
    /etc/mate-settings-daemon

 -----------------------------------------------------------------

#### lsattr

查看文件的第二扩展文件系统属性；修改文件属性：[chattr](#chattr)

    lsattr [选项] [参数]
        选项：
            -R     Recursively list attributes of directories and their contents.
            -a    列出所有文件包括隐藏文件
            -d    列出目录，不列出它们的内容。
            -l     使用长名称而不是单个字符缩写来打印选项。
            -p     列出文件的项目编号。
        
实例

查看文件属性
    
    lsattr -l a.md

 -----------------------------------------------------------------

#### more

一个基于vi编辑器文本过滤器，它以全屏幕的方式按页显示文本文件的内容，支持vi中的关键字定位操作。more名单中内置了若干快捷键，常用的有H（获得帮助信息），Enter（向下翻滚一行），空格（向下滚动一屏），Q（退出命令）

    more [选项] [参数]
        选项：
            -d          显示“[press space to continue,'q' to quit.]”和“[Press 'h' for instructions]”
            -f          count logical rather than screen lines
            -l          suppress pause after form feed
            -c          不进行滚屏操作，每次刷新这个屏幕
            -p          do not scroll, clean screen and display text
            -s          将多个空行压缩至一行
            -u          禁止下划线
            -<number>   每屏显示的行数
            +<number>   从指定行数开始显示
            +/<string>  从搜索字符开始显示
        参数：
            文件：指定分页显示内容的文件

实例：

显示文件file的内容，但在显示之前先清屏，并且在屏幕的最下方显示完核的百分比。

    more -dc file

显示文件file的内容，每10行显示一次，而且在显示之前先清屏。

    more -c -10 file

 -----------------------------------------------------------------

#### mv

移动或者重命名文件

    mv [选项] [参数]
        选项：
            --backup=<备份模式>：若需覆盖文件，则覆盖前先行备份；
            -b：当文件存在时，覆盖前，为其创建一个备份；
            -f：若目标文件或目录与现有的文件或目录重复，则直接覆盖现有的文件或目录；
            -i：交互式操作，覆盖前先行询问用户，如果源文件与目标文件或目标目录中的文件同名，则询问用户是否覆盖目标文件。用户输入”y”，表示将覆盖目标文件；输入”n”，表示取消对源文件的移动。这样可以避免误将文件覆盖。
            --strip-trailing-slashes：删除源文件中的斜杠“/”；
            -S<后缀>：为备份文件指定后缀，而不使用默认的后缀；
            --target-directory=<目录>：指定源文件要移动到目标目录；
            -u：当源文件比目标文件新或者目标文件不存在时，才执行移动操作。
        参数：
            源文件：源文件列表。
            目标文件：如果"目标文件"是文件名则在移动文件的同时，将其改名为“目标文件”；如果“目标文件”是目录名则将源文件移动到“目标文件”下。

实例

将目录/usr/men中的所有文件移到当前目录（用.表示）中：

    mv /usr/men/* .

移动文件

    mv file_1.txt /home/office/

移动多个文件

    mv file_2.txt file_3.txt file_4.txt /home/office/
    mv *.txt /home/office/

移动目录

    mv directory_1/ /home/office/

重命名文件或目录

    mv file_1.txt file_2.txt # 将文件file_1.txt改名为file_2.txt

重命名目录

    mv directory_1/ directory_2/

打印移动信息

    mv -v *.txt /home/office

提示是否覆盖文件

        mv -i file_1.txt /home/office

源文件比目标文件新时才执行更新

    mv -uv *.txt /home/office

不要覆盖任何已存在的文件

    mv -vn *.txt /home/office

复制时创建备份

    mv -bv *.txt /home/office

无条件覆盖已经存在的文件

    mv -f *.txt /home/office



 -----------------------------------------------------------------

#### paste 

将多个文件按列合并 - 即第一个文件显示到前面一列，第二个显示到后面一列

    paste [选项] [参数]
        选项：
            -d<间隔字符>或--delimiters=<间隔字符>：用指定的间隔字符取代跳格字符；
            -s或——serial串列进行而非平行处理。
        参数：
            文件列表：指定需要合并的文件列表

实例：

显示a.txt与b.txt文件内容，并且以 '-' 间隔
    
    paste -d '-' a.txt b.txt

显示a.txt与b.txt文件内容，将b文件显示到a文件末尾
    
    paste -d '-' a.txt b.txt


 -----------------------------------------------------------------

#### read 

从键盘读取变量，通常用在shell脚本中与用户进行交互的场合。该命令可以一次读取多个变量的值，变量和输入的值都需要使用空格隔开。在read命令后面，如果没有指定变量名，读取的数据将被自动赋值给特定的变量REPLY

    read [选项] [参数]
        选项：
                -a 数组	
                -d 自定义定界符结束输入行，用于代替 enter 结束
                -e	use Readline to obtain the line in an interactive shell
                -i text	use TEXT as the initial text for Readline
                -n 从输入中读取指定字符并存入变量，不需要按回车读取。
                -N 用于限定最多可以有多少字符可以作为有效读入。例
                -p 指定读取值时的提示符；
                -r 允许反斜杠、转义及?等特殊字符
                -s 将标准输入内容隐藏，多用于输入密码。
                -t 用于表示等待输入的时间，单位为秒，等待时间超过，将继续执行后面的脚本，注意不作为null输入，参数将保留原有的值
                -u fd	read from file descriptor FD instead of the standard input

        参数：
            变量：指定读取值的变量名。

实例：

从标准输入读取输入并赋值给变量1987name。

    #read 1987name        #等待读取输入，直到回车后表示输入完毕，并将输入赋值给变量answer
    HelloWorld            #控制台输入Hello

    #echo $1987name       #打印变量
    HelloWorld

等待一组输入，每个单词之间使用空格隔开，直到回车结束，并分别将单词依次赋值给这三个读入变量。

    #read one two three
    1 2 3                   #在控制台输入1 2 3，它们之间用空格隔开。

    #echo "one = $one, two = $two, three = $three"
    one = 1, two = 2, three = 3

REPLY示例

    #read                  #等待控制台输入，并将结果赋值给特定内置变量REPLY。
    This is REPLY          #在控制台输入该行。 

    #echo $REPLY           #打印输出特定内置变量REPLY，以确认是否被正确赋值。

    This is REPLY

-p选项示例

    #read -p "Enter your name: "            #输出文本提示，同时等待输入，并将结果赋值给REPLY。
    Enter you name: stephen                 #在提示文本之后输入stephen

    #echo $REPLY
    stephen

等待控制台输入，并将输入信息视为数组，赋值给数组变量friends，输入信息用空格隔开数组的每个元素。

    #read -a friends
    Tim Tom Helen

    #echo "They are ${friends[0]}, ${friends[1]} and ${friends[2]}."
    They are Tim, Tom and Helen.

 -----------------------------------------------------------------

#### rm

remove files or directories - 可以删除一个目录中的一个或多个文件或目录，也可以将某个目录及其下属的所有文件及其子目录均删除掉。对于链接文件，只是删除整个链接文件，而原有文件保持不变。

注意：使用rm命令要格外小心。因为一旦删除了一个文件，就无法再恢复它。所以，在删除文件之前，最好再看一下文件的内容，确定是否真要删除。rm命令可以用-i选项，这个选项在使用文件扩展名字符删除多个文件时特别有用。使用这个选项，系统会要求你逐一确定是否要删除。这时，必须输入y并按Enter键，才能删除文件。如果仅按Enter键或其他字符，文件不会被删除。

    rm [选项] [参数]
        选项：
            -d：直接把欲删除的目录的硬连接数据删除成0，删除该目录；
            -f：强制删除文件或目录；
            -i：删除已有文件或目录之前先询问用户；
            -r或-R：递归处理，将指定目录下的所有文件与子目录一并处理；
            --preserve-root：不对根目录进行递归操作；
            -v：显示指令的详细执行过程。
        参数：
            文件：指定被删除的文件列表，如果参数中含有目录，则必须加上-r或者-R选项。

实例：

rm 命令删除文件

    # rm 文件1 文件2 ...
    rm testfile.txt

rm 命令删除目录

> rm -r [目录名称] -r 表示递归地删除目录下的所有文件和目录。 -f 表示强制删除

    rm -rf testdir
    rm -r testdir

删除操作前有确认提示

> rm -i [文件/目录]

    rm -r -i testdir

rm 忽略不存在的文件或目录

> -f 选项（LCTT 译注：即 “force”）让此次操作强制执行，忽略错误提示

    rm -f [文件...]

仅在某些场景下确认删除

> 选项 -I，可保证在删除超过 3 个文件时或递归删除时（LCTT 译注： 如删除目录）仅提示一次确认。

    rm -I file1 file2 file3

删除根目录

> 当然，删除根目录（/）是 Linux 用户最不想要的操作，这也就是为什么默认 rm 命令不支持在根目录上执行递归删除操作。 然而，如果你非得完成这个操作，你需要使用 --no-preserve-root 选项。当提供此选项，rm 就不会特殊处理根目录（/）了。


rm 显示当前删除操作的详情

    rm -v [文件/目录]

 -----------------------------------------------------------------

#### rsync

a fast, versatile, remote (and local) file-copying tool - 一个远程数据同步工具，可通过LAN/WAN快速同步多台主机间的文件。rsync使用所谓的“rsync算法”来使本地和远程两个主机之间的文件达到同步，这个算法只传送两个文件的不同部分，而不是每次都整份传送，因此速度相当快。

    rsync [选项]... 源地址 目标地址
    rsync [选项]... SRC [USER@]host:DEST
    rsync [选项]... [USER@]HOST:SRC DEST
    rsync [选项]... [USER@]HOST::SRC DEST
    rsync [选项]... SRC [USER@]HOST::DEST
    rsync [选项]... rsync://[USER@]HOST[:PORT]/SRC [DEST]

对应于以上六种命令格式，rsync有六种不同的工作模式：

1. 拷贝本地文件。当SRC和DES路径信息都不包含有单个冒号":"分隔符时就启动这种工作模式。如：rsync -a /data /backup
2. 使用一个远程shell程序(如rsh、ssh)来实现将本地机器的内容拷贝到远程机器。当DST路径地址包含单个冒号":"分隔符时启动该模式。如：rsync -avz *.c foo:src
3. 使用一个远程shell程序(如rsh、ssh)来实现将远程机器的内容拷贝到本地机器。当SRC地址路径包含单个冒号":"分隔符时启动该模式。如：rsync -avz foo:src/bar /data
4. 从远程rsync服务器中拷贝文件到本地机。当SRC路径信息包含"::"分隔符时启动该模式。如：rsync -av root@192.168.78.192::www /databack
5. 从本地机器拷贝文件到远程rsync服务器中。当DST路径信息包含"::"分隔符时启动该模式。如：rsync -av /databack root@192.168.78.192::www
6. 列远程机的文件列表。这类似于rsync传输，不过只要在命令中省略掉本地机信息即可。如：rsync -v rsync://192.168.78.192/www

    选项：
        -v, --verbose 详细模式输出。
        -q, --quiet 精简输出模式。
        -c, --checksum 打开校验开关，强制对文件传输进行校验。
        -a, --archive 归档模式，表示以递归方式传输文件，并保持所有文件属性，等于-rlptgoD。
        -r, --recursive 对子目录以递归模式处理。
        -R, --relative 使用相对路径信息。
        -b, --backup 创建备份，也就是对于目的已经存在有同样的文件名时，将老的文件重新命名为~filename。可以使用--suffix选项来指定不同的备份文件前缀。
        --backup-dir 将备份文件(如~filename)存放在在目录下。
        -suffix=SUFFIX 定义备份文件前缀。
        -u, --update 仅仅进行更新，也就是跳过所有已经存在于DST，并且文件时间晚于要备份的文件，不覆盖更新的文件。
        -l, --links 保留软链结。
        -L, --copy-links 想对待常规文件一样处理软链结。
        --copy-unsafe-links 仅仅拷贝指向SRC路径目录树以外的链结。
        --safe-links 忽略指向SRC路径目录树以外的链结。
        -H, --hard-links 保留硬链结。
        -p, --perms 保持文件权限。
        -o, --owner 保持文件属主信息。
        -g, --group 保持文件属组信息。
        -D, --devices 保持设备文件信息。
        -t, --times 保持文件时间信息。
        -S, --sparse 对稀疏文件进行特殊处理以节省DST的空间。
        -n, --dry-run现实哪些文件将被传输。
        -w, --whole-file 拷贝文件，不进行增量检测。
        -x, --one-file-system 不要跨越文件系统边界。
        -B, --block-size=SIZE 检验算法使用的块尺寸，默认是700字节。
        -e, --rsh=command 指定使用rsh、ssh方式进行数据同步。
        --rsync-path=PATH 指定远程服务器上的rsync命令所在路径信息。
        -C, --cvs-exclude 使用和CVS一样的方法自动忽略文件，用来排除那些不希望传输的文件。
        --existing 仅仅更新那些已经存在于DST的文件，而不备份那些新创建的文件。
        --delete 删除那些DST中SRC没有的文件。
        --delete-excluded 同样删除接收端那些被该选项指定排除的文件。
        --delete-after 传输结束以后再删除。
        --ignore-errors 及时出现IO错误也进行删除。
        --max-delete=NUM 最多删除NUM个文件。
        --partial 保留那些因故没有完全传输的文件，以是加快随后的再次传输。
        --force 强制删除目录，即使不为空。
        --numeric-ids 不将数字的用户和组id匹配为用户名和组名。
        --timeout=time ip超时时间，单位为秒。
        -I, --ignore-times 不跳过那些有同样的时间和长度的文件。
        --size-only 当决定是否要备份文件时，仅仅察看文件大小而不考虑文件时间。
        --modify-window=NUM 决定文件是否时间相同时使用的时间戳窗口，默认为0。
        -T --temp-dir=DIR 在DIR中创建临时文件。
        --compare-dest=DIR 同样比较DIR中的文件来决定是否需要备份。
        -P 等同于 --partial。
        --progress 显示备份过程。
        -z, --compress 对备份的文件在传输时进行压缩处理。
        --exclude=PATTERN 指定排除不需要传输的文件模式。
        --include=PATTERN 指定不排除而需要传输的文件模式。
        --exclude-from=FILE 排除FILE中指定模式的文件。
        --include-from=FILE 不排除FILE指定模式匹配的文件。
        --version 打印版本信息。
        --address 绑定到特定的地址。
        --config=FILE 指定其他的配置文件，不使用默认的rsyncd.conf文件。
        --port=PORT 指定其他的rsync服务端口。
        --blocking-io 对远程shell使用阻塞IO。
        -stats 给出某些文件的传输状态。
        --progress 在传输时现实传输过程。
        --log-format=formAT 指定日志文件格式。
        --password-file=FILE 从FILE中得到密码。
        --bwlimit=KBPS 限制I/O带宽，KBytes per second。

实例：

**普通rsync方式**

    rsync -vzrtopg --progress -e ssh --delete work@172.16.78.192:/www/* /databack/experiment/rsync

**后台服务方式**

1、启动rsync服务，编辑/etc/xinetd.d/rsync文件，将其中的disable=yes改为disable=no，并重启xinetd服务，如下：

```shell
#default: off
# description: The rsync server is a good addition to an ftp server, as it \
# allows crc checksumming etc.
service rsync {
disable = no
socket_type = stream
wait = no
user = root
server = /usr/bin/rsync
server_args = --daemon
log_on_failure += USERID
}
```

```shell

/etc/init.d/xinetd restart
停止 xinetd： [确定]
启动 xinetd： [确定]

```

2、创建配置文件，默认安装好rsync程序后，并不会自动创建rsync的主配置文件，需要手工来创建，其主配置文件为“/etc/rsyncd.conf”，创建该文件并插入如下内容：

```shell
uid=root
gid=root
max connections=4
log file=/var/log/rsyncd.log
pid file=/var/run/rsyncd.pid
lock file=/var/run/rsyncd.lock
secrets file=/etc/rsyncd.passwd
hosts deny=172.16.78.0/22

[www]
comment= backup web
path=/www
read only = no
exclude=test
auth users=work
```
3、创建密码文件，采用这种方式不能使用系统用户对客户端进行认证，所以需要创建一个密码文件，其格式为“username:password”，用户名可以和密码可以随便定义，最好不要和系统帐户一致，同时要把创建的密码文件权限设置为600，这在前面的模块参数做了详细介绍。

```shell
echo "work:abc123" > /etc/rsyncd.passwd
chmod 600 /etc/rsyncd.passwd
```

 -----------------------------------------------------------------

#### scp

scp - secure copy (remote file copy program) - 用于在Linux下进行远程拷贝文件的命令，和它类似的命令有[cp](#cp)，不过cp只是在本机进行拷贝不能跨服务器，而且scp传输是加密的。可能会稍微影响一下速度。当你服务器硬盘变为只读read only system时，用scp可以帮你把文件移出来。另外，scp还非常不占资源，不会提高多少系统负荷，在这一点上，[rsync](#rsync)就远远不及它了。虽然 rsync比scp会快一点，但当小文件众多的情况下，rsync会导致硬盘I/O非常高，而scp基本不影响系统正常使用。

    scp [选项] [参数]
        选项：
            -1：使用ssh协议版本1；
            -2：使用ssh协议版本2；
            -4：使用ipv4；
            -6：使用ipv6；
            -B：以批处理模式运行；
            -C：使用压缩；
            -F：指定ssh配置文件；
            -i：identity_file 从指定文件中读取传输时使用的密钥文件（例如亚马逊云pem），此参数直接传递给ssh；
            -l：指定宽带限制；
            -o：指定使用的ssh选项；
            -P：指定远程主机的端口号；
            -p：保留文件的最后修改时间，最后访问时间和权限模式（即用户读写权限）；
            -q：不显示复制进度；
            -r：以递归方式复制。
        参数：
            源文件 - 指定要复制的源文件.
            目标文件 - 目标文件. 格式为user@host：filename（文件名为目标文件的名称）。
实例：

从远程复制到本地descp命令与上面的命令相同，只要将从本地复制到远程的命令后面2个参数互换位置即可

**从远程复制文件到本地目录**

    scp root@10.10.10.10:/opt/soft/nginx-0.5.38.tar.gz /opt/soft/

从10.10.10.10机器上的/opt/soft/的目录中下载nginx-0.5.38.tar.gz 文件到本地/opt/soft/目录中。

**从亚马逊云复制OpenVPN到本地目录**

    scp -i amazon.pem ubuntu@10.10.10.10:/usr/local/openvpn_as/etc/exe/openvpn-connect-2.1.3.110.dmg openvpn-connect-2.1.3.110.dmg

从10.10.10.10机器上下载openvpn安装文件到本地当前目录来。

**从远处复制到本地**

    scp -r root@10.10.10.10:/opt/soft/mongodb /opt/soft/

从10.10.10.10机器上的/opt/soft/中下载mongodb目录到本地的/opt/soft/目录来。

**上传本地文件到远程机器指定目录**

    scp /opt/soft/nginx-0.5.38.tar.gz root@10.10.10.10:/opt/soft/scptest
    # 指定端口 2222

    scp -rp -P 2222 /opt/soft/nginx-0.5.38.tar.gz root@10.10.10.10:/opt/soft/scptest

复制本地/opt/soft/目录下的文件nginx-0.5.38.tar.gz到远程机器10.10.10.10的opt/soft/scptest目录。

**上传本地目录到远程机器指定目录**

    scp -r /opt/soft/mongodb root@10.10.10.10:/opt/soft/scptest

上传本地目录/opt/soft/mongodb到远程机器10.10.10.10上/opt/soft/scptest的目录中去。

 -----------------------------------------------------------------

#### split

split a file into pieces - 分割任意大小的文件。可以将一个大文件分割成很多个小文件，有时需要将文件分割成更小的片段，比如为提高可读性，生成日志等。

    split [选项]... [文件 [前缀]]
        选项：
            -b：值为每一输出档案的大小，单位为 byte。
            -C：每一输出档中，单行的最大 byte 数。
            -d：使用数字作为后缀。
            -l：值为每一输出档的行数大小。
            -a：指定后缀长度(默认为2)。
        参数：
            文件

使用split命令将文件分割成大小为10KB的小文件：

    [root@localhost split]# split -b 10k date.file 
    [root@localhost split]# ls
    date.file  xaa  xab  xac  xad  xae  xaf  xag  xah  xai  xaj

文件被分割成多个带有字母的后缀文件，如果想用数字后缀可使用-d参数，同时可以使用-a length来指定后缀的长度：

    [root@localhost split]# split -b 10k date.file -d -a 3
    [root@localhost split]# ls
    date.file  x000  x001  x002  x003  x004  x005  x006  x007  x008  x009

为分割后的文件指定文件名的前缀：

    [root@localhost split]# split -b 10k date.file -d -a 3 split_file
    [root@localhost split]# ls
    date.file  split_file000  split_file001  split_file002  split_file003  split_file004  split_file005  split_file006  split_file007  split_file008  split_file009

使用-l选项根据文件的行数来分割文件，例如把文件分割成每个包含10行的小文件：

    split -l 10 date.file

 -----------------------------------------------------------------

#### tar

Linux下的归档使用工具，用来打包和备份

tar命令 可以为linux的文件和目录创建档案，类似命令有 [ar](#ar) 。利用tar，可以为某一特定文件创建档案（备份文件），也可以在档案中改变文件，或者向档案中加入新的文件。tar最初被用来在磁带上创建档案，现在，用户可以在任何设备上创建档案。利用tar命令，可以把一大堆的文件和目录全部打包成一个文件，这对于备份文件或将几个文件组合成为一个文件以便于网络传输是非常有用的。

首先要弄清两个概念：打包和压缩。打包是指将一大堆文件或目录变成一个总的文件；压缩则是将一个大的文件通过一些压缩算法变成一个小文件。

为什么要区分这两个概念呢？这源于Linux中很多压缩程序只能针对一个文件进行压缩，这样当你想要压缩一大堆文件时，你得先将这一大堆文件先打成一个包（tar命令），然后再用压缩程序进行压缩 ( [gzip](#gzip) [bzip2](#bzip2) 命令）。

    tar [选项] [参数]
        选项：
          主操作模式：
            -A或--catenate：新增文件到以存在的备份文件；
            -c或--create：建立新的备份文件；
            -d：记录文件的差别；
            -r, --append               追加文件至归档结尾
            -t或--list：列出备份文件的内容；
            -u：添加改变了和现有的文件到已经存在的压缩文件；
            -x或--extract或--get：从归档文件中提取文件，可以搭配-C（大写）在特定目录解开，需要注意的是-c、-t、-x不可同时出现在一串命令中；

          重写控制:
            -k, --keep-old-files       解压时不替换存在的文件,而将其认为是错误
                --keep-directory-symlink   解压时保留已存在的目录符号链接
                --keep-newer-files  不要替换比归档中副本更新的已存在的文件
                --no-overwrite-dir     保留已存在目录的元数据
                --one-top-level[=DIR]  创建子目录以避免解压松散文件
                --overwrite            解压时重写存在的文件
                --overwrite-dir        解压时重写已存在目录的元数据(默认)
                --recursive-unlink     解压目录之前先清除目录层次
                --remove-files         在添加文件至归档后删除它们
                --skip-old-files       解压时不替换存在的文件，而是自动忽略
            -U, --unlink-first         在解压要重写的文件之前先删除它们
            -W, --verify               在写入以后尝试校验归档
          
          设备分块:
            -b, --blocking-factor=BLOCKS   每个记录 BLOCKS x 512 字节
            -B, --read-full-records    读取时重新分块(只对 4.2BSD 管道有效)
            -i, --ignore-zeros         忽略归档中的零字节块(即文件结尾)
                --record-size=NUMBER   每个记录的字节数 NUMBER，乘以512
                
          选择归档格式:
            -H, --format=FORMAT        创建指定格式的归档
            FORMAT 是以下格式中的一种:
                gnu                      GNU tar 1.13.x 格式
                oldgnu                   GNU 格式 as per tar <= 1.12
                pax                      POSIX 1003.1-2001 (pax) 格式
                posix                    等同于 pax
                ustar                    POSIX 1003.1-1988 (ustar) 格式
                v7                       old V7 tar 格式
            
          压缩类型：
            -a, --auto-compress        使用归档后缀名来决定压缩程序
            -I, --use-compress-program=PROG 通过 PROG 过滤(必须是能接受 -d 选项的程序)
            -j, --bzip2                通过 bzip2 过滤归档
            -J, --xz                   通过 xz 过滤归档
                --lzip                 通过 lzip 过滤归档
                --lzma                 通过 xz 过滤归档
                --lzop                 通过 lzop 过滤归档
                --no-auto-compress     不使用归档后缀名来决定压缩程序
            -z, --gzip, --gunzip, --ungzip   通过 gzip 过滤归档
                --zstd                 通过 zstd 过滤归档
            -Z, --compress, --uncompress   通过 compress 过滤归档
        
        参数：
            文件或者目录：指定要打包的文件或目录列表

实例：

```shell
1、zip格式:

压缩： zip -r [目标文件名].zip [原文件/目录名]
解压： unzip [原文件名].zip
注：-r参数代表递归

2、tar格式（该格式仅仅打包，不压缩）:

打包：tar -cvf [目标文件名].tar [原文件名/目录名]
解包：tar -xvf [原文件名].tar
注：c参数代表create（创建），x参数代表extract（解包），v参数代表verbose（详细信息），f参数代表filename（文件名），所以f后必须接文件名。
tar.gz格式

方式一：利用前面已经打包好的tar文件，直接用压缩命令。

压缩：gzip [原文件名].tar
解压：gunzip [原文件名].tar.gz

方式二：一次性打包并压缩、解压并解包

打包并压缩： tar -zcvf [目标文件名].tar.gz [原文件名/目录名]
解压并解包： tar -zxvf [原文件名].tar.gz
注：z代表用gzip算法来压缩/解压。

3、tar.bz2格式:

方式一：利用已经打包好的tar文件，直接执行压缩命令：

压缩：bzip2 [原文件名].tar
解压：bunzip2 [原文件名].tar.bz2
方式二：一次性打包并压缩、解压并解包

打包并压缩： tar -jcvf [目标文件名].tar.bz2 [原文件名/目录名]
解压并解包： tar -jxvf [原文件名].tar.bz2
注：小写j代表用bzip2算法来压缩/解压。
tar.xz格式

方式一：利用已经打包好的tar文件，直接用压缩命令：

压缩：xz [原文件名].tar
解压：unxz [原文件名].tar.xz
方式二：一次性打包并压缩、解压并解包

打包并压缩： tar -Jcvf [目标文件名].tar.xz [原文件名/目录名]
解压并解包： tar -Jxvf [原文件名].tar.xz
注：大写J代表用xz算法来压缩/解压。

4、tar.Z格式（已过时）

方式一：利用已经打包好的tar文件，直接用压缩命令：

压缩：compress [原文件名].tar
解压：uncompress [原文件名].tar.Z
方式二：一次性打包并压缩、解压并解包

打包并压缩： tar -Zcvf [目标文件名].tar.Z [原文件名/目录名]
解压并解包： tar -Zxvf [原文件名].tar.Z
注：大写Z代表用ncompress算法来压缩/解压。另，ncompress是早期Unix系统的压缩格式，但由于ncompress的压缩率太低，现已过时。

5、jar格式

压缩：jar -cvf [目标文件名].jar [原文件名/目录名]
解压：jar -xvf [原文件名].jar

注：如果是打包的是Java类库，并且该类库中存在主类，那么需要写一个META-INF/MANIFEST.MF配置文件，内容如下：

Manifest-Version: 1.0
Created-By: 1.6.0_27 (Sun Microsystems Inc.)
Main-class: the_name_of_the_main_class_should_be_put_here

然后用如下命令打包：

jar -cvfm [目标文件名].jar META-INF/MANIFEST.MF [原文件名/目录名]
这样以后就能用“java -jar [文件名].jar”命令直接运行主类中的public static void main方法了。
7z格式

压缩：7z a [目标文件名].7z [原文件名/目录名]
解压：7z x [原文件名].7z
注：这个7z解压命令支持rar格式，即：

6、7z x [原文件名].rar

7、其它例子

将文件全部打包成tar包 ：

tar -cvf log.tar log2012.log    仅打包，不压缩！
tar -zcvf log.tar.gz log2012.log   打包后，以 gzip 压缩
tar -jcvf log.tar.bz2 log2012.log  打包后，以 bzip2 压缩

在选项f之后的文件档名是自己取的，我们习惯上都用 .tar 来作为辨识。 如果加z选项，则以.tar.gz或.tgz来代表gzip压缩过的tar包；如果加j选项，则以.tar.bz2来作为tar包名。

解压目录

去掉第一层目录结构，要出除第二层，--strip-components 2

tar -xvf portal-web-v2.0.0.tar --strip-components 1  -C 指定目录

查阅上述tar包内有哪些文件 ：

tar -ztvf log.tar.gz

由于我们使用 gzip 压缩的log.tar.gz，所以要查阅log.tar.gz包内的文件时，就得要加上z这个选项了。

将tar包解压缩 ：

tar -zxvf /opt/soft/test/log.tar.gz

在预设的情况下，我们可以将压缩档在任何地方解开的

只将tar内的部分文件解压出来 ：

tar -zxvf /opt/soft/test/log30.tar.gz log2013.log

我可以透过tar -ztvf来查阅 tar 包内的文件名称，如果单只要一个文件，就可以透过这个方式来解压部分文件！

文件备份下来，并且保存其权限 ：

tar -zcvpf log31.tar.gz log2014.log log2015.log log2016.log

这个-p的属性是很重要的，尤其是当您要保留原本文件的属性时。

在文件夹当中，比某个日期新的文件才备份 ：

tar -N "2012/11/13" -zcvf log17.tar.gz test

备份文件夹内容是排除部分文件：

tar --exclude scf/service -zcvf scf.tar.gz scf/*

打包文件之后删除源文件：

tar -cvf test.tar test --remove-files

其实最简单的使用 tar 就只要记忆底下的方式即可：

压　缩：tar -jcv -f filename.tar.bz2 要被压缩的文件或目录名称
查　询：tar -jtv -f filename.tar.bz2
解压缩：tar -jxv -f filename.tar.bz2 -C 欲解压缩的目录
```

-----------------------------------------------------------------

#### touch


 -----------------------------------------------------------------

#### which


-----------------------------------------------------------------

#### whereis



-----------------------------------------------------------------

#### 参考

[Linux命令查询](https://jaywcjlove.gitee.io/linux-command)    
[菜鸟教程](https://www.runoob.com/linux)    
[视频教程](https://www.bilibili.com/video/av21303002)    

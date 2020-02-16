---
title: Linux常用命令-磁盘管理
tags:   
  - linux  
categories:
  - linux    
description: 磁盘管理命令命令详解.....    
date: 2020-02-16 13:19:00
---

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


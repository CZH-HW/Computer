[toc]
## Linux 简介
---
### Linux 系统目录结构

![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/linux_contents_1.png)

在 Linux 或 Unix 操作系统中，所有的文件和目录都被组织成以一个根节点开始的倒置的树状结构。

文件系统的最顶层是由根目录开始的，系统使用`/`来表示根目录。在根目录之下的既可以是目录，也可以是文件，而每一个目录中又可以包含子目录文件。如此反复就可以构成一个庞大的文件系统。

在 Linux 文件系统中有两个特殊的目录：
一个用户所在的工作目录，也叫当前目录，可以使用一个点`.`来表示；
另一个是当前目录的上一级目录，也叫父目录，可以使用两个点`..`来表示。

`.`：代表当前的目录，也可以使用 ./ 来表示；
`..`：代表上一层目录，也可以 ../ 来代表。
如果一个目录或文件名以一个点`.`开始，表示这个目录或文件是一个隐藏目录或文件(如:`.bashrc`)。
即以默认方式查找时，不显示该目录或文件。


- /bin：
bin 是 Binary 的缩写, 这个目录存放着最经常使用的命令。

- /boot：
这里存放的是启动 Linux 时使用的一些核心文件，包括一些连接文件以及镜像文件。

- /dev ：
dev 是 Device (设备)的缩写, 该目录下存放的是 Linux 的外部设备，在 Linux 中访问设备的方式和访问文件的方式是相同的。
这里并不是放的外部设备的驱动程序，这一点和 windows, dos 操作系统不一样。它**实际上是一个访问这些外部设备的端口**。
我们可以非常方便地去访问这些外部设备，和访问一个文件，一个目录没有任何区别。

- /etc：
这个目录用来存放所有的系统管理所需要的配置文件和子目录。

- /home：
用户的主目录，在 Linux 中，每个用户都有一个自己的目录，一般该目录名是以用户的账号命名的。

- /lib：
这个目录里存放着系统最基本的动态连接共享库，其作用类似于 Windows 里的 DLL 文件。几乎所有的应用程序都需要用到这些共享库。

- /lost+found：
这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件。

- /media 
linux 系统会自动识别一些设备，例如U盘、光驱等等，当识别后，linux 会把识别的设备挂载到这个目录下。

- /mnt：
系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将光驱挂载在 /mnt/ 上，然后进入该目录就可以查看光驱里的内容了。

- /opt：
这是给主机额外安装软件所摆放的目录。比如你安装一个 ORACLE 数据库则就可以放到这个目录下。默认是空的。

- /proc：
这个目录是一个虚拟的目录，它是系统内存的映射，我们可以通过直接访问这个目录来获取系统信息。
这个目录的内容不在硬盘上而是在内存里，我们也可以直接修改里面的某些文件。

```shell
比如可以通过下面的命令来屏蔽主机的 ping 命令，使别人无法 ping 你的机器：
echo 1 > /proc/sys/net/ipv4/icmp_echo_ignore_all
```

- /root：
该目录为系统管理员，也称作超级权限者的用户主目录。

- /sbin：
s 就是 Super User 的意思，这里存放的是系统管理员使用的系统管理程序。

- /selinux：
 这个目录是 Redhat/CentOS 所特有的目录，Selinux 是一个安全机制，类似于 windows 的防火墙，但是这套机制比较复杂，这个目录就是存放 selinux 相关的文件的。

- /srv：
 该目录存放一些服务启动之后需要提取的数据。

- /sys：
这是 linux2.6 内核的一个很大的变化。该目录下安装了 2.6 内核中新出现的一个文件系统 sysfs 。

sysfs 文件系统集成了下面3种文件系统的信息：
针对进程信息的 proc 文件系统、针对设备的 devfs 文件系统以及针对伪终端的 devpts 文件系统。
该文件系统是内核设备树的一个直观反映。

当一个内核对象被创建的时候，对应的文件和目录也在内核对象子系统中被创建。

- /tmp：
这个目录是用来存放一些临时文件的。

- /usr：
 这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似与 windows 下的 program files 目录。

- /usr/bin：
系统用户使用的应用程序。

- /usr/sbin：
超级用户使用的比较高级的管理程序和系统守护程序。

- /usr/src：内核源代码默认的放置目录。

- /var：
这个目录中存放着在不断扩充着的东西，我们习惯将那些经常被修改的目录放在这个目录下。包括各种日志文件。


在linux系统中，有几个目录是比较重要的，平时需要注意不要误删除或者随意更改内部文件。

1. /etc： 上边也提到了，这个是系统中的配置文件，如果你更改了该目录下的某个文件可能会导致系统不能启动。

2. /bin, /sbin, /usr/bin, /usr/sbin: 这是系统预设的执行文件的放置目录，比如 ls 就是在/bin/ls 目录下的。
值得提出的是，/bin, /usr/bin 是给系统用户使用的指令（除root外的通用户），而 /sbin, /usr/sbin 则是给 root 使用的指令。

3. /var: 这是一个非常重要的目录，系统上跑了很多程序，那么每个程序都会有相应的日志产生，而这些日志就被记录到这个目录下，具体在 /var/log 目录下，另外 mail 的预设放置也是在这里。

|目录|应放置文件内容|
|----|----|
|/var/cache/|应用程序本身运作过程中会产生的一些暂存档|
|/var/lib/|程序本身执行的过程中，需要使用到的数据文件放置的目录。在此目录下各自的软件应该要有各自的目录。<br>举例来说，MySQL的数据库放置到/var/lib/mysql/而rpm的数据库则放到/var/lib/rpm去|
|/var/lock/|某些装置或者是文件资源一次只能被一个应用程序所使用，如果同时有两个程序使用该装置时， 就可能产生一些错误的状况，因此就得要将该装置上锁（lock），以确保该装置只会给单一软件所使用。<br>举例来说，刻录机正在刻录一块光盘，你想一下，会不会有两个人同时在使用一个刻录机烧片？ 如果两个人同时刻录，那片子写入的是谁的数据？所以当第一个人在刻录时该刻录机就会被上锁， 第二个人就得要该装置被解除锁定(就是前一个人用完了)才能够继续使用|
|/var/log/|非常重要。这是登录文件放置的目录。里面比较重要的文件如/var/log/messages, /var/log/wtmp(记录登入者的信息)等。|
|/var/mail/|放置个人电子邮件信箱的目录，不过这个目录也被放置到/var/spool/mail/目录中，通常这两个目录是互为链接文件。|
|/var/run/|某些程序或者是服务启动后，会将他们的PID放置在这个目录下|
|/var/spool/|这个目录通常放置一些队列数据，所谓的“队列”就是排队等待其他程序使用的数据。这些数据被使用后通常都会被删除。举例来说，系统收到新信会放置到/var/spool/mail/中， 但使用者收下该信件后该封信原则上就会被删除。信件如果暂时寄不出去会被放到/var/spool/mqueue/中， 等到被送出后就被删除。如果是工作排程数据(crontab)，就会被放置到/var/spool/cron/目录中。|

---
### Linux 远程登录

远程登录 Linux 服务器来管理维护系统。CentOS系统默认安装了openssh

如果没有安装可以使用命令进行安装：
```shell
yum install openssh-server -y 
```

Linux 系统中是通过 ssh 服务实现的远程登录功能，默认 ssh 服务端口号为 22。

Window 系统上 Linux 远程登录客户端有 SecureCRT, Putty, SSH Secure Shell, Xshell 等。

---

### Linux 文件基本属性

#### Linux 文件基本属性
Linux 系统是一种典型的多用户系统，不同的用户处于不同的地位，拥有不同的权限。
为了保护系统的安全性，Linux 系统对不同的用户访问同一文件（包括目录文件）的权限做了不同的规定。

在 Linux 中我们可以使用 ll 或者 ls –l 命令来显示一个文件的属性以及文件所属的用户和组，如：
```shell
[root@www /]# ls -l
total 64
dr-xr-xr-x   2 root root 4096 Dec 14  2012 bin
dr-xr-xr-x   4 root root 4096 Apr 19  2012 boot
……
```
实例中，bin 文件的第一个属性用`d`表示。`d`在 Linux 中代表该文件是一个目录文件。

**在Linux中第一个字符代表这个文件是目录、文件或链接文件等等**。
- [`d`]则是目录；
- [`-`]则是文件；
- [`l`]则表示为链接文档(link file)；
- [`b`]则表示为装置文件里面的可供储存的接口设备(可随机存取装置)；
- [`c`]则表示为装置文件里面的串行端口设备，例如键盘、鼠标(一次性读取装置)。

接下来的字符中，以三个为一组，且均为`rwx`的三个参数的组合。
- [`r`]代表可读(read)、
- [`w`]代表可写(write)、
- [`x`]代表可执行(execute)。 
要注意的是，这三个权限的位置不会改变，如果没有权限，就会出现减号[`-`]而已。

总结：
每个文件的属性由左边第一部分的10个字符来确定，如下图所示：

![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/linux_contents_2.png)

从左至右用 0-9 这些数字来表示：
- 第 0 位确定文件类型；
- 第 1-3 位确定属主（该文件的所有者）拥有该文件的权限；
- 第 4-6 位确定属组（所有者的同组用户）拥有该文件的权限；
- 第 7-9 位确定其他用户拥有该文件的权限。

图中，第1、4、7位表示读权限，如果用`r`字符表示，则有读权限，如果用`-`字符表示，则没有读权限；
第2、5、8位表示写权限，如果用`w`字符表示，则有写权限，如果用`-`字符表示没有写权限；
第3、6、9位表示可执行权限，如果用`x`字符表示，则有执行权限，如果用`-`字符表示，则没有执行权限。

#### Linux 文件属主和属组

对于文件来说，它都有一个特定的所有者，也就是对该文件具有所有权的用户。

同时，**在 Linux 系统中，用户是按组分类的，一个用户属于一个或多个组**。

文件所有者以外的用户又可以分为文件所有者的同组用户和其他用户。

因此，Linux 系统按文件所有者、文件所有者同组用户和其他用户来规定了不同的文件访问权限。

```shell
[root@www /]# ls -l
total 64
dr-xr-xr-x   2 root root 4096 Dec 14  2012 bin
dr-xr-xr-x   4 root root 4096 Apr 19  2012 boot
……
```
在以上实例中，bin文件是一个目录文件，属主和属组都为root，属主有可读、可写、可执行的权限；与属主同组的其他用户有可读和可执行的权限；其他用户也有可读和可执行的权限。

#### 更改文件属性
1.`chgrp`：更改文件属组

```shell
chgrp [-R] 属组名 文件名
```
参数选项`-R`：递归更改文件属组，就是在更改某个目录文件的属组时，如果加上-R的参数，那么该目录下的所有文件的属组都会更改。
<br>

2.`chown`：更改文件属主，也可以同时更改文件属组
```shell
chown [–R] 属主名 文件名
chown [-R] 属主名：属组名 文件名
```
 <br>

3.`chmod`：更改文件9个属性
**Linux 文件属性有两种设置方法，一种是数字，一种是符号**。
Linux 文件的基本权限就有九个，分别是 owner/group/others 三种身份各有自己的 read/write/execute 权限。

(1) 符号类型改变文件权限
先复习一下刚刚上面提到的数据：文件的权限字符为：`-rwxrwxrwx`， 这九个权限是三个三个一组的！其中，我们可以使用数字来代表各个权限，各权限的分数对照表如下：
- `r`:`4`
- `w`:`2`
- `x`:`1`

每种身份（owner/group/others）各自的三个权限（r/w/x）分数是需要累加的。
例如当权限为： [-rwxrwx---] 分数则是：
owner = `rwx` = `4+2+1` = `7`
group = `rwx` = `4+2+1` = `7`
others = `---` = `0+0+0` = `0`

所以等一下我们设定权限的变更时，该文件的权限数字就是770啦！
变更权限的指令chmod的语法是这样的：
```shell
chmod [-R] xyz 文件或目录
```
选项与参数：
xyz : 就是刚刚提到的数字类型的权限属性，为`rwx`属性数值的相加
-R : 进行递归（recursive）的持续变更，亦即连同此目录下的所有文件都会变更

举例来说，如果要将.bashrc这个文件所有的权限都设定启用，那么命令如下：
```shell
[root@www ~]# ls -al .bashrc
-rw-r--r--  1 root root 395 Jul  4 11:45 .bashrc
[root@www ~]# chmod 777 .bashrc
[root@www ~]# ls -al .bashrc
-rwxrwxrwx  1 root root 395 Jul  4 11:45 .bashrc
```

(2) 符号类型改变文件权限
九个权限分别是 user、group、others 三种身份，那么我们就可以藉由 `u`, `g`, `o` 来代表三种身份的权限！
此外，`a`则代表 all 亦即全部的身份！那么读写的权限就可以写成`r`, `w`, `x`，也就是可以使用底下的方式来看：

|`chmod`|`u`<br>`g`<br>`o`<br>`a`|`+`(加入)<br>`-`(除去)<br>`=`(设定)|`r`<br>`w`<br>`x`|文件或目录|
|----|----|----|----|----|

```shell
如果我们需要将文件权限设置为-rwxr-xr--，可以使用 chmod u=rwx,g=rx,o=r 文件名 来设定:

[root@www ~]# ls -al .bashrc
-rwxr-xr-x  1 root root 395 Jul  4 11:45 .bashrc
[root@www ~]# chmod  a+w  .bashrc
[root@www ~]# ls -al .bashrc
-rwxrwxrwx  1 root root 395 Jul  4 11:45 .bashrc

而如果是要将权限去掉而不改变其他已存在的权限呢？例如要拿掉全部人的可执行权限，则：

[root@www ~]# chmod  a-x  .bashrc
[root@www ~]# ls -al .bashrc
-rw-rw-rw-  1 root root 395 Jul  4 11:45 .bashrc
```
---

### Linux 文件与目录管理

我们知道 Linux 的目录结构为树状结构，最顶级的目录为根目录`/`。

其他目录通过挂载可以将它们添加到树中，通过解除挂载可以移除它们。

>Linux 中的挂载是指将一个设备（通常是存储设备）挂接到一个已存在的目录上。
这个目录可以不为空，但挂载后这个目录下以前的内容将不可用。
需要理解的是，**Linux 操作系统将所有的设备都看作文件，它将整个计算机的资源都整合成一个大的文件目录**。
我们要访问存储设备中的文件，必须将文件所在的分区挂载到一个已存在的目录上，然后通过访问这个目录来访问存储设备。


绝对路径与相对路径
- 绝对路径：
路径的写法，由根目录`/`写起，例如：`/usr/share/doc`这个目录。
- 相对路径：
路径的写法，不是由`/`写起，例如由`/usr/share/doc`要到`/usr/share/man`底下时，可以写成：`cd ../man` 

#### 处理目录的常用命令

常见的处理目录的命令：

`ls`: 列出目录
`cd`：切换目录
`pwd`：显示目前的目录
`mkdir`：创建一个新的目录
`rmdir`：删除一个空的目录
`cp`: 复制文件或目录
`rm`: 移除文件或目录
`mv`: 移动文件与目录、文件重命名

可以使用`man [命令]`来查看各个命令的使用文档，如：`man cp`


1.`ls` (列出目录)
```shell
[root@www ~]# ls [-aAdfFhilnrRSt] 目录名称
[root@www ~]# ls [--color={never,auto,always}] 目录名称
[root@www ~]# ls [--full-time] 目录名称
```
选项与参数：
`-a`：全部的文件，连同隐藏档（开头为`.`的文件）一起列出来(常用)
`-d`：仅列出目录本身，而不是列出目录内的文件数据(常用)
`-l`：长数据串列出，包含文件的属性与权限等等数据(常用)


2.`cd` (切换目录)
`cd`是 Change Directory 的缩写，是用来变换工作目录的命令。

```shell
cd [相对路径或绝对路径]
```
举例：
```
使用 mkdir 命令创建school.cn目录
[root@www ~]# mkdir school.cn

使用绝对路径切换到school.cn目录
[root@www ~]# cd /root/school.cn/

使用相对路径切换到school.cn目录
[root@www ~]# cd ./school.cn/

表示回到自己的家目录，亦即是 /root 这个目录
[root@www school.cn]# cd ~

表示去到目前的上一级目录，亦即是 /root 的上一级目录的意思；
[root@www ~]# cd ..
```

3.`pwd` (显示目前所在的目录)
`pwd`是 Print Working Directory 的缩写，也就是显示目前所在目录的命令。

```shell
[root@www ~]# pwd [-P]
选项与参数：
-P：显示出确实的路径，而非使用连结 (link) 路径。

范例：单纯显示出目前的工作目录：
[root@www ~]# pwd
/root   <== 显示出目录啦～  

范例：显示出实际的工作目录，而非连结档本身的目录名而已 
[root@www ~]# cd /var/mail   <==注意，/var/mail是一个连结档 
[root@www mail]# pwd 
/var/mail         <==列出目前的工作目录 
[root@www mail]# pwd -P 
/var/spool/mail   <==怎么回事？有没有加 -P 差很多～ 
[root@www mail]# ls -ld /var/mail 
lrwxrwxrwx 1 root root 10 Sep  4 17:54 /var/mail -> spool/mail
# 看到这里应该知道为啥了吧？因为 /var/mail 是连结档，连结到 /var/spool/mail 
# 所以，加上 pwd -P 的选项后，会不以连结档的数据显示，而是显示正确的完整路径啊！
```

4.`mkdir` (创建新目录)
创建新的目录使用`mkdir`（make directory）。

```shell
mkdir [-mp] 目录名称
```

选项与参数：
`-m`：直接配置文件的权限，不需要看默认权限 (umask) 的脸色～
`-p` ：帮助你直接将所需要的目录(包含上一级目录)递回创建起来！

```shell
范例：请到/tmp底下尝试创建数个新目录看看：
[root@www ~]# cd /tmp
[root@www tmp]# mkdir test    <==创建一名为 test 的新目录 
[root@www tmp]# mkdir test1/test2/test3/test4 
mkdir: cannot create directory 'test1/test2/test3/test4':  
No such file or directory     <==没办法直接创建此目录啊！ 
[root@www tmp]# mkdir -p test1/test2/test3/test4 
加了这个 -p 的选项，可以自行帮你创建多层目录！

范例：创建权限为rwx--x--x的目录
[root@www tmp]# mkdir -m 711 test2
[root@www tmp]# ls -l
drwxr-xr-x  3 root  root 4096 Jul 18 12:50 test
drwxr-xr-x  3 root  root 4096 Jul 18 12:53 test1
drwx--x--x  2 root  root 4096 Jul 18 12:54 test2
上面的权限部分，如果没有加上 -m 来强制配置属性，系统会使用默认属性。
如果我们使用 -m ，如上例我们给予 -m 711 来给予新的目录 drwx--x--x 的权限。
```

5.`rmdir` (删除空的目录)

```
 rmdir [-p] 目录名称
```
选项与参数：
`-p` ：连同上一级『空的』目录也一起删除

```shell
将mkdir范例中创建的目录(/tmp底下)删除掉！

[root@www tmp]# ls -l   <==看看有多少目录存在？ 
drwxr-xr-x  3 root  root 4096 Jul 18 12:50 test 
drwxr-xr-x  3 root  root 4096 Jul 18 12:53 test1 
drwx--x--x  2 root  root 4096 Jul 18 12:54 test2 
[root@www tmp]# rmdir test   <==可直接删除掉，没问题 
[root@www tmp]# rmdir test1  <==因为尚有内容，所以无法删除！ 
rmdir: 'test1': Directory not empty 
[root@www tmp]# rmdir -p test1/test2/test3/test4 
[root@www tmp]# ls -l        <==您看看，底下的输出中test与test1不见了！ 
drwx--x--x  2 root  root 4096 Jul 18 12:54 test2 
利用 -p 这个选项，立刻就可以将 test1/test2/test3/test4 一次删除。
不过要注意的是，这个 rmdir 仅能删除空的目录，你可以使用 rm 命令来删除非空目录。
```

6.`cp` (复制文件或目录)
即拷贝文件和目录。

```
[root@www ~]# cp [-adfilprsu] 来源档(source) 目标档(destination)
[root@www ~]# cp [options] source1 source2 source3 .... directory
```
选项与参数：
`-a`：相当于 -pdr 的意思，至于 pdr 请参考下列说明(常用)；
`-d`：若来源档为连结档的属性(link file)，则复制连结档属性而非文件本身；
`-f`：为强制(force)的意思，若目标文件已经存在且无法开启，则移除后再尝试一次；
`-i`：若目标档(destination)已经存在时，在覆盖时会先询问动作的进行(常用)；
`-l`：进行硬式连结(hard link)的连结档创建，而非复制文件本身；
`-p`：连同文件的属性一起复制过去，而非使用默认属性(备份常用)；
`-r`：递回持续复制，用于目录的复制行为；(常用)
`-s`：复制成为符号连结档 (symbolic link)，亦即『捷径』文件；
`-u`：若 destination 比 source 旧才升级 destination ！

```shell
用root身份，将家目录下的 .bashrc 复制到 /tmp 下，并更名为 bashr

[root@www ~]# cp ~/.bashrc /tmp/bashrc
[root@www ~]# cp -i ~/.bashrc /tmp/bashrc
cp: overwrite '/tmp/bashrc'? n    <== n不覆盖，y为覆盖 
```


7.`rm` (移除文件或目录)

```shell
rm [-fir] 文件或目录
```

选项与参数：
`-f` ：就是 force 的意思，忽略不存在的文件，不会出现警告信息；
`-i` ：互动模式，在删除前会询问使用者是否动作
`-r` ：递回删除啊！最常用在目录的删除了！这是非常危险的选项！！！


```shell
将刚刚在 cp 的范例中创建的 bashrc 删除掉！
[root@www tmp]# rm -i bashrc
rm: remove regular file 'bashrc'? y
如果加上 -i 的选项就会主动询问喔，避免你删除到错误的档名！
```

8.`mv` (移动文件与目录，或修改名称)

```
[root@www ~]# mv [-fiu] source destination
[root@www ~]# mv [options] source1 source2 source3 .... directory
```

选项与参数：
`-f`：force 强制的意思，如果目标文件已经存在，不会询问而直接覆盖；
`-i`：若目标文件 (destination) 已经存在时，就会询问是否覆盖！
`-u`：若目标文件已经存在，且 source 比较新，才会升级 (update)

```shell
复制一文件，创建一目录，将文件移动到目录中
[root@www ~]# cd /tmp
[root@www tmp]# cp ~/.bashrc bashrc
[root@www tmp]# mkdir mvtest
[root@www tmp]# mv bashrc mvtest
将某个文件移动到某个目录去，就是这样做！

将刚刚的目录名称更名为 mvtest2
[root@www tmp]# mv mvtest mvtest2
```

#### Linux 文件内容查看

Linux系统中使用以下命令来查看文件的内容：

`cat`  由第一行开始显示文件内容
`tac`  从最后一行开始显示，可以看出 tac 是 cat 的倒着写！
`nl`   显示的时候，顺道输出行号！
`more` 一页一页的显示文件内容
`less` 与 more 类似，但是比 more 更好的是，他可以往前翻页！
`head` 只看头几行
`tail` 只看尾巴几行

你可以使用 man [命令]来查看各个命令的使用文档，如 ：man cp。

1.`cat`
由第一行开始显示文件内容

```shell
cat [-AbEnTv]
```

选项与参数：
`-A` ：相当于 -vET 的整合选项，可列出一些特殊字符而不是空白而已；
`-b` ：列出行号，仅针对非空白行做行号显示，空白行不标行号！
`-E` ：将结尾的断行字节 $ 显示出来；
`-n` ：列印出行号，连同空白行也会有行号，与 -b 的选项不同；
`-T` ：将 [tab] 按键以 ^I 显示出来；
`-v` ：列出一些看不出来的特殊字符

```shell
检看 /etc/issue 这个文件的内容：

[root@www ~]# cat /etc/issue
CentOS release 6.4 (Final)
Kernel \r on an \m
```

2.`tac`
`tac`与`cat`命令刚好相反，文件内容从最后一行开始显示，可以看出`tac`是`cat`的倒着写！如：

```shell
[root@www ~]# tac /etc/issue
Kernel \r on an \m
CentOS release 6.4 (Final)
```

3.nl
显示行号

```shell
nl [-bnw] 文件
```

选项与参数：
`-b`：指定行号指定的方式，主要有两种：
`-b a`：表示不论是否为空行，也同样列出行号(类似 cat -n)；
`-b t`：如果有空行，空的那一行不要列出行号(默认值)；

`-n`：列出行号表示的方法，主要有三种：
`-n ln`：行号在萤幕的最左方显示；
`-n rn`：行号在自己栏位的最右方显示，且不加 0 ；
`-n rz`：行号在自己栏位的最右方显示，且加 0 ；

`-w`：行号栏位的占用的位数。

```shell
范例一：用 nl 列出 /etc/issue 的内容
[root@www ~]# nl /etc/issue
     1  CentOS release 6.4 (Final)
     2  Kernel \r on an \m
```

4.`more`
一页一页翻动

```shell
more [-dlfpcsu] [-num] [+/pattern] [+linenum] [fileNames..]
```

参数：
`-num`：一次显示的行数
`-d`：提示使用者，在画面下方显示 [Press space to continue, 'q' to quit.] ，如果使用者按错键，则会显示 [Press 'h' for instructions.] 而不是 '哔' 声
`-l`：取消遇见特殊字元 ^L（送纸字元）时会暂停的功能
`-f`：计算行数时，以实际上的行数，而非自动换行过后的行数（有些单行字数太长的会被扩展为两行或两行以上）
`-p`：不以卷动的方式显示每一页，而是先清除萤幕后再显示内容
`-c`：跟`-p`相似，不同的是先显示内容再清除其他旧资料
`-s`：当遇到有连续两行以上的空白行，就代换为一行的空白行
`-u`：不显示下引号 （根据环境变数 TERM 指定的 terminal 而有所不同）
`+/pattern`：在每个文档显示前搜寻该字串（pattern），然后从该字串之后开始显示
`+num`：从第 num 行开始显示
`fileNames`：欲显示内容的文档，可为复数个数

```shell
[root@www ~]# more /etc/man.config
#
# Generated automatically from man.conf.in 
# 
# by the configure script.
#
# man.conf from man-1.6d
....(中间省略)....
--More--(28%)  <== 重点在这一行喔！你的光标也会在这里等待你的命令 
```

在 more 这个程序的运行过程中，你有几个按键可以按的：
- 空白键（space） ：代表向下翻一页；
- Enter         ：代表向下翻『一行』；
- /字串         ：代表在这个显示的内容当中，向下搜寻『字串』这个关键字；
- :f            ：立刻显示出档名以及目前显示的行数；
- q             ：代表立刻离开 more ，不再显示该文件内容。
- b 或 [ctrl]-b ：代表往回翻页，不过这动作只对文件有用，对管线无用。


5.`less`
一页一页翻动


以下实例输出/etc/man.config文件的内容：
```shell
[root@www ~]# less /etc/man.config
#
# Generated automatically from man.conf.in by the
# configure script.
#
# man.conf from man-1.6d
....(中间省略)....
:   <== 这里可以等待你输入命令！
```

less运行时可以输入的命令有：

- 空白键    ：向下翻动一页；
- [pagedown]：向下翻动一页；
- [pageup]  ：向上翻动一页；
- /字串     ：向下搜寻『字串』的功能；
- ?字串     ：向上搜寻『字串』的功能；
- n         ：重复前一个搜寻 (与 / 或 ? 有关！)
- N         ：反向的重复前一个搜寻 (与 / 或 ? 有关！)
- q         ：离开 less 这个程序；


6.`head`
取出文件前面几行

```shell
head [-n number] 文件 
```

选项与参数：
-n ：后面接数字，代表显示几行的意思

```shell
[root@www ~]# head /etc/man.config
# 默认的情况中，显示前面 10 行！
# 若要显示前 20 行，就得要这样：
[root@www ~]# head -n 20 /etc/man.config
```

7.`tail`
取出文件后面几行

```shell
tail [-n number] 文件 
```

选项与参数：
`-n`：后面接数字，代表显示几行的意思
`-f`：表示持续侦测后面所接的档名，要等到按下[ctrl]-c才会结束tail的侦测

```shell
[root@www ~]# tail /etc/man.config
# 默认的情况中，显示最后的十行！
# 若要显示最后的 20 行，就得要这样：
[root@www ~]# tail -n 20 /etc/man.config
```
---

### Linux 用户和用户组管理

Linux 系统是一个多用户多任务的分时操作系统，任何一个要使用系统资源的用户，都必须首先向系统管理员申请一个账号，然后以这个账号的身份进入系统。

用户的账号一方面可以帮助系统管理员对使用系统的用户进行跟踪，并控制他们对系统资源的访问；另一方面也可以帮助用户组织文件，并为用户提供安全性保护。

每个用户账号都拥有一个惟一的用户名和各自的口令。

用户在登录时键入正确的用户名和口令后，就能够进入系统和自己的主目录。

实现用户账号的管理，要完成的工作主要有如下几个方面：
- 用户账号的添加、删除与修改
- 用户口令的管理
- 用户组的管理

#### Linux 系统用户账号的管理
用户账号的管理工作主要涉及到用户账号的添加、修改和删除。

添加用户账号就是在系统中创建一个新账号，然后为新账号分配用户号、用户组、主目录和登录 Shell 等资源。刚添加的账号是被锁定的，无法使用。

1.添加新的用户账号使用 useradd 命令，其语法如下：

```shell
useradd 选项 用户名
```

选项与参数:
`-c`：comment 指定一段注释性描述。
`-d`：目录 指定用户主目录，如果此目录不存在，则同时使用`-m`选项，可以创建主目录。
`-g`：用户组 指定用户所属的用户组。
`-G`：用户组 指定用户所属的附加组。
`-s`：Shell 文件 指定用户的登录 Shell。
`-u`：用户号 指定用户的用户号，如果同时有`-o`选项，则可以重复使用其他用户的标识号。


```shell
实例1
useradd –d /usr/sam -m sam
此命令创建了一个用户 sam，其中 -d 和 -m 选项用来为登录名sam产生一个主目录 /usr/sam（ /usr 为默认的用户主目录所在的父目录）。

实例2
useradd -s /bin/sh -g group –G adm,root gem
此命令新建了一个用户 gem，该用户的登录Shell是 /bin/sh，它属于 group 用户组，同时又属于 adm 和 root 用户组，其中 group 用户组是其主组。

这里可能新建组：# groupadd group 及 groupadd adm
增加用户账号就是在 /etc/passwd 文件中为新用户增加一条记录，同时更新其他系统文件如 /etc/shadow， /etc/group 等。

实例3
#添加一个不能登录的用户
useradd -d /usr/local/apache -g apache -s /bin/false apache
要拒绝系统用户登录，可以将其 shell 设置为 /usr/sbin/nologin 或者 /bin/false。

usermod -s | --shell /usr/sbin/nologin username
或者
usermod -s | -shell /bin/false username

说明及比较：
/bin/false
/bin/false 什么也不做只是返回一个错误状态，然后立即退出。将用户的 shell 设置为 /bin/false，用户会无法登录，并且不会有任何提示。

/usr/sbin/nologin
nologin 会礼貌的向用户显示一条信息，并拒绝用户登录：
This account is currently not available.

有一些软件，比如一些 ftp 服务器软件，对于本地非虚拟账户，只有用户有有效的 shell 才能使用 ftp 服务。这时候就可以使用 nologin 使用户即不能登录系统，还能使用一些系统服务，比如 ftp 服务。/bin/false 则不行，这是二者的重要区别之一。

/etc/nologin
如果存在 /etc/nologin 文件，则系统只允许 root 用户登录，其他用户全部被拒绝登录，并向他们显示 /etc/nologin 文件的内容。

Linux提供了集成的系统管理工具userconf，它可以用来对用户账号进行统一管理。
```

2.删除帐号
如果一个用户的账号不再使用，可以从系统中删除。**删除用户账号就是要将 /etc/passwd 等系统文件中的该用户记录删除，必要时还删除用户的主目录**。

删除一个已有的用户账号使用`userdel`命令，其格式如下：

```shell
userdel 选项 用户名
```

常用的选项是 -r，它的作用是把用户的主目录（ /home/USERNAME，/var/spool/mail/USERNAME）一起删除。

```shell
userdel -r sam
一般都加上 -r 参数
此命令删除用户sam在系统文件中（主要是/etc/passwd, /etc/shadow, /etc/group等）的记录，同时删除用户的主目录。
```

3.修改帐号
修改用户账号就是根据实际情况更改用户的有关属性，如用户号、主目录、用户组、登录 Shell 等。

修改已有用户的信息使用`usermod`命令，其格式如下：

```shell
usermod 选项 用户名
```

常用的选项包括`-c`, `-d`, `-m`, `-g`, `-G`, `-s`, `-u`以及`-o`等，这些选项的意义与useradd命令中的选项一样，可以为用户指定新的资源值。

另外，有些系统可以使用选项：`-l` 新用户名
这个选项指定一个新的账号，即将原来的用户名改为新的用户名。

```shell
usermod -s /bin/ksh -d /home/z –g developer sam
此命令将用户 sam 的登录 Shell 修改为 ksh，主目录改为 /home/z，用户组改为 developer。
```

4.用户口令的管理
用户管理的一项重要内容是用户口令的管理。**用户账号刚创建时没有口令，但是被系统锁定，无法使用，必须为其指定口令后才可以使用，即使是指定空口令**。

指定和修改用户口令的 Shell 命令是 passwd。超级用户可以为自己和其他用户指定口令，普通用户只能用它修改自己的口令。命令的格式为：

```shell
passwd 选项 用户名
```

选项与参数：
`-l`：锁定口令，即禁用账号。
`-u`：口令解锁。
`-d`：使账号无口令。
`-f`：强迫用户下次登录时修改口令。
如果默认用户名，则修改当前用户的口令。

```shell
例如，假设当前用户是 sam，则下面的命令修改该用户自己的口令：
# passwd 
Old password:****** 
New password:******* 
Re-enter new password:*******

如果是超级用户，可以用下列形式指定任何用户的口令：
# passwd sam 
New password:******* 
Re-enter new password:*******
```

- 普通用户修改自己的口令时，`passwd`命令会先询问原口令，验证后再要求用户输入两遍新口令，如果两次输入的口令一致，则将这个口令指定给用户；
- 超级用户为用户指定口令时，就不需要知道原口令。

为了系统安全起见，用户应该选择比较复杂的口令，例如最好使用8位长的口令，口令中包含有大写、小写字母和数字，并且应该与姓名、生日等不相同。

```shell
为用户指定空口令时，执行下列形式的命令：
# passwd -d sam
此命令将用户sam的口令删除，这样用户sam下一次登录时，系统就不再询问口令。

passwd命令还可以用-l(lock)选项锁定某一用户，使其不能登录，例如：
# passwd -l sam
```

#### Linux 系统用户组的管理
每个用户都有一个用户组，系统可以对一个用户组中的所有用户进行集中管理。

不同 Linux 系统对用户组的规定有所不同，如 Linux 下的用户属于与它同名的用户组，这个用户组在创建用户时同时创建。

用户组的管理涉及用户组的添加、删除和修改。组的增加、删除和修改实际上就是对 /etc/group 文件的更新。

1.增加一个新的用户组使用`groupadd`命令。其格式如下：
```shell
groupadd 选项 用户组
```

选项与参数：
`-g`：GID 指定新用户组的组标识号（GID）。
`-o`：一般与-g选项同时使用，表示新用户组的GID可以与系统已有用户组的GID相同。

```shell
实例1：
# groupadd group1
此命令向系统中增加了一个新组 group1，新组的组标识号是在当前已有的最大组标识号的基础上加1。

实例2：
# groupadd -g 101 group2
此命令向系统中增加了一个新组group2，同时指定新组的组标识号是101。
```

2.如果要删除一个已有的用户组，使用`groupdel`命令，其格式如下：

```
groupdel 用户组
```

```
实例：
# groupdel group1
此命令从系统中删除组group1。
```

3.修改用户组的属性使用`groupmod`命令。其语法如下：

```shell
groupmod 选项 用户组
```

选项与参数：
`-g`：GID 为用户组指定新的组标识号。
`-o`：与`-g`选项同时使用，用户组的新 GID 可以与系统已有用户组的 GID 相同。
`-n`：新用户组 将用户组的名字改为新名字

```shell
实例1：
# groupmod -g 102 group2
此命令将组group2的组标识号修改为102。

实例2：
# groupmod –g 10000 -n group3 group2
此命令将组group2的标识号改为10000，组名修改为group3。
```

4.如果一个用户同时属于多个用户组，那么用户可以在用户组之间切换，以便具有其他用户组的权限。

用户可以在登录后，使用命令`newgrp`切换到其他用户组，这个命令的参数就是目的用户组。例如：

```shell
# newgrp root
这条命令将当前用户切换到root用户组，前提条件是 root 用户组确实是该用户的主组或附加组。
类似于用户账号的管理，用户组的管理也可以通过集成的系统管理工具来完成。
```

#### 与用户账号有关的系统文件
完成用户管理的工作有许多种方法，但是每一种方法实际上都是对有关的系统文件进行修改。

与用户和用户组相关的信息都存放在一些系统文件中，这些文件包括 /etc/passwd、/etc/shadow、/etc/group等。

下面分别介绍这些文件的内容。

1.`/etc/passwd`文件是用户管理工作涉及的最重要的一个文件。
Linux 系统中的每个用户都在 /etc/passwd 文件中有一个对应的记录行，它记录了这个用户的一些基本属性。

这个文件对所有用户都是可读的。它的内容类似下面的例子：

```shell
＃ cat /etc/passwd
root:x:0:0:Superuser:/:
daemon:x:1:1:System daemons:/etc:
bin:x:2:2:Owner of system commands:/bin:
sys:x:3:3:Owner of system files:/usr/sys:
adm:x:4:4:System accounting:/usr/adm:
uucp:x:5:5:UUCP administrator:/usr/lib/uucp:
auth:x:7:21:Authentication administrator:/tcb/files/auth:
cron:x:9:16:Cron daemon:/usr/spool/cron:
listen:x:37:4:Network daemon:/usr/net/nls:
lp:x:71:18:Printer administrator:/usr/spool/lp:
sam:x:200:50:Sam san:/usr/sam:/bin/sh
```

从上面的例子我们可以看到，/etc/passwd 中一行记录对应着一个用户，每行记录又被冒号`:`分隔为 7 个字段，其格式和具体含义如下：

```shell
用户名:口令:用户标识号:组标识号:注释性描述:主目录:登录Shell
```
- **用户名**是代表用户账号的字符串。
通常长度不超过8个字符，并且由大小写字母和/或数字组成。登录名中不能有冒号`:`，因为冒号在这里是分隔符。
为了兼容起见，登录名中最好不要包含点字符`.`，并且不使用连字符`-`和加号`+`打头。
<br>

- **口令**在一些系统中，存放着加密后的用户口令字。
虽然这个字段存放的只是用户口令的加密串，不是明文，但是由于 /etc/passwd 文件对所有用户都可读，所以这仍是一个安全隐患。
因此，**现在许多 Linux 系统（ 如 SVR4 ）都使用了 shadow 技术，把真正的加密后的用户口令字存放到 /etc/shadow 文件中，而在 /etc/passwd 文件的口令字段中只存放一个特殊的字符，例如`x`或者`*`**。
<br>

- **用户标识号**是一个整数，系统内部用它来标识用户。
一般情况下它与用户名是一一对应的。如果几个用户名对应的用户标识号是一样的，系统内部将把它们视为同一个用户，但是它们可以有不同的口令、不同的主目录以及不同的登录 Shell 等。
    + 通常用户标识号的取值范围是 0～65535。
    + 0 是超级用户 root 的标识号，1～99 由系统保留，作为管理账号，
    + 普通用户的标识号从 100 开始。在 Linux 系统中，这个界限是 500。
<br>

- **组标识号**字段记录的是用户所属的用户组。
它对应着 /etc/group 文件中的一条记录。
<br>

- **注释性描述**字段记录着用户的一些个人情况。
例如用户的真实姓名、电话、地址等，这个字段并没有什么实际的用途。在不同的 Linux 系统中，这个字段的格式并没有统一。在许多 Linux 系统中，这个字段存放的是一段任意的注释性描述文字，用做 finger 命令的输出。
<br>

- **主目录**，也就是用户的起始工作目录。
它是用户在登录到系统之后所处的目录。在大多数系统中，各用户的主目录都被组织在同一个特定的目录下，而用户主目录的名称就是该用户的登录名。各用户对自己的主目录有读、写、执行（搜索）权限，其他用户对此目录的访问权限则根据具体情况设置。
<br>

- 用户登录后，要启动一个进程，负责将用户的操作传给内核，这个进程是用户登录到系统后运行的命令解释器或某个特定的程序，即 Shell。
**Shell 是用户与 Linux 系统之间的接口**。
    + Linux 的 Shell 有许多种，每种都有不同的特点。常用的有 sh(Bourne Shell), csh(C Shell), ksh(Korn Shell), tcsh(TENEX/TOPS-20 type C Shell), bash(Bourne Again Shell)等。
    + 系统管理员可以根据系统情况和用户习惯为用户指定某个 Shell。如果不指定 Shell，那么系统使用 sh 为默认的登录 Shell，即这个字段的值为 /bin/sh。
    + 用户的登录S hell 也可以指定为某个特定的程序（此程序不是一个命令解释器）。利用这一特点，我们可以限制用户只能运行指定的应用程序，在该应用程序运行结束后，用户就自动退出了系统。有些 Linux 系统要求只有那些在系统中登记了的程序才能出现在这个字段中。
<br>

- 系统中有一类用户称为伪用户（psuedo users）。
这些用户在 /etc/passwd 文件中也占有一条记录，但是不能登录，因为它们的登录 Shell 为空。它们的存在主要是方便系统管理，满足相应的系统进程对文件属主的要求。
    > 常见的伪用户如下所示：
    > 1. `bin`：拥有可执行的用户命令文件 
    > 2. `sys`：拥有系统文件 
    > 3. `adm`：拥有帐户文件 
    > 4. `uucp`：UUCP使用 
    > 5. `lp`：lp或lpd子系统使用 
    > 6. `nobody`：NFS使用
    除了上面列出的伪用户外，还有许多标准的伪用户，例如：audit, cron, mail, usenet等，它们也都各自为相关的进程和文件所需要。

<br>

2.`/etc/shadow`拥有帐户文件
由于 /etc/passwd 文件是所有用户都可读的，如果用户的密码太简单或规律比较明显的话，一台普通的计算机就能够很容易地将它破解，因此对安全性要求较高的 Linux 系统都把加密后的口令字分离出来，单独存放在一个文件中，这个文件是 /etc/shadow 文件。 有超级用户才拥有该文件读权限，这就保证了用户密码的安全性。

/etc/shadow 中的记录行与 /etc/passwd 中的一一对应，它由`pwconv`命令根据 /etc/passwd 中的数据自动产生，它的文件格式与 /etc/passwd 类似，由若干个字段组成，字段之间用`:`隔开。这些字段是：

```shell
登录名:加密口令:最后一次修改时间:最小时间间隔:最大时间间隔:警告时间:不活动时间:失效时间:标志
```
- **登录名**是与 /etc/passwd 文件中的登录名相一致的用户账号
- **口令**字段存放的是加密后的用户口令字，长度为13个字符。如果为空，则对应用户没有口令，登录时不需要口令；如果含有不属于集合 `{ ./0-9A-Za-z }`中的字符，则对应的用户不能登录。
- **最后一次修改时间**表示的是从某个时刻起，到用户最后一次修改口令时的天数。时间起点对不同的系统可能不一样。例如在SCO Linux 中，这个时间起点是1970年1月1日。
- **最小时间间隔**指的是两次修改口令之间所需的最小天数。
- **最大时间间隔**指的是口令保持有效的最大天数。
- **警告时间**字段表示的是从系统开始警告用户到用户密码正式失效之间的天数。
- **不活动时间**表示的是用户没有登录活动但账号仍能保持有效的最大天数。
- **失效时间**字段给出的是一个绝对的天数，如果使用了这个字段，那么就给出相应账号的生存期。期满后，该账号就不再是一个合法的账号，也就不能再用来登录了。

下面是/etc/shadow的一个例子：
```shell
＃ cat /etc/shadow
root:Dnakfw28zf38w:8764:0:168:7:::
daemon:*::0:0::::
bin:*::0:0::::
sys:*::0:0::::
adm:*::0:0::::
uucp:*::0:0::::
nuucp:*::0:0::::
auth:*::0:0::::
cron:*::0:0::::
listen:*::0:0::::
lp:*::0:0::::
sam:EkdiSECLWPdSa:9740:0:0::::
```
<br>

3.用户组的所有信息都存放在`/etc/group`文件中。
将用户分组是 Linux 系统中对用户进行管理及控制访问权限的一种手段。

每个用户都属于某个用户组；一个组中可以有多个用户，一个用户也可以属于不同的组。

当一个用户同时是多个组中的成员时，在 /etc/passwd 文件中记录的是用户所属的主组，也就是登录时所属的默认组，而其他组称为附加组。

用户要访问属于附加组的文件时，必须首先使用`newgrp`命令使自己成为所要访问的组中的成员。

用户组的所有信息都存放在 /etc/group 文件中。此文件的格式也类似于 /etc/passwd 文件，由冒号`:`隔开若干个字段，这些字段有：

```shell
组名:口令:组标识号:组内用户列表
```
- **组名**是用户组的名称，由字母或数字构成。与 /etc/passwd 中的登录名一样，组名不应重复。
- **口令**字段存放的是用户组加密后的口令字。一般 Linux 系统的用户组都没有口令，即这个字段一般为空，或者是`*`。
- **组标识号**与用户标识号类似，也是一个整数，被系统内部用来标识组。
- **组内用户列表**是属于这个组的所有用户的列表，不同用户之间用逗号`,`分隔。这个用户组可能是用户的主组，也可能是附加组。

/etc/group文件的一个例子如下：
```shell
# cat /etc/group
root::0:root
bin::2:root,bin
sys::3:root,uucp
adm::4:root,adm
daemon::5:root,daemon
lp::7:root,lp
users::20:root,sam
```

#### 批量添加或删除用户
添加和删除用户对每位 Linux 系统管理员都是轻而易举的事，比较棘手的是如果要添加几十个、上百个甚至上千个用户时，我们不太可能还使用 useradd 一个一个地添加，必然要找一种简便的创建大量用户的方法。Linux 系统提供了创建大量用户的工具，可以让您立即创建大量用户，方法如下：

1. 先编辑一个文本用户文件。
每一列按照 /etc/passwd 密码文件的格式书写，要注意每个用户的用户名、UID、宿主目录都不可以相同，其中密码栏可以留做空白或输入`*`号。一个范例文件 user.txt 内容如下：
    ```
    user001::600:100:user:/home/user001:/bin/bash
    user002::601:100:user:/home/user002:/bin/bash
    user003::602:100:user:/home/user003:/bin/bash
    user004::603:100:user:/home/user004:/bin/bash
    user005::604:100:user:/home/user005:/bin/bash
    user006::605:100:user:/home/user006:/bin/bash
    ```

2. 以 root 身份执行命令`/usr/sbin/newusers`，从刚创建的用户文件 user.txt 中导入数据，创建用户：
    ```shell
    # newusers < user.txt 
    ```
    然后可以执行命令`vipw`或 `vi /etc/passwd`检查 /etc/passwd 文件是否已经出现这些用户的数据，并且用户的宿主目录是否已经创建。
<br>

3. 执行命令`/usr/sbin/pwunconv`。
将 /etc/shadow 产生的 shadow 密码解码，然后回写到 /etc/passwd 中，并将 /etc/shadow 的 shadow 密码栏删掉。这是为了方便下一步的密码转换工作，即先取消 shadow password 功能。
    ```
    # pwunconv
    ```

4. 编辑每个用户的密码对照文件。
范例文件 passwd.txt 内容如下：
    ```
    user001:密码
    user002:密码
    user003:密码
    user004:密码
    user005:密码
    user006:密码
    ```

5. 以 root 身份执行命令`/usr/sbin/chpasswd`。
创建用户密码，`chpasswd`会将经过 `/usr/bin/passwd` 命令编码过的密码写入 /etc/passwd 的密码栏。
    ```
    # chpasswd < passwd.txt
    ```

6. 确定密码经编码写入 /etc/passwd 的密码栏后。
执行命令`/usr/sbin/pwconv`将密码编码为 shadow password，并将结果写入 /etc/shadow。
    ```
    # pwconv
    ```
    这样就完成了大量用户的创建了，之后您可以到 /home 下检查这些用户宿主目录的权限设置是否都正确，并登录验证用户密码是否正确。

---

### Linux 磁盘管理

#### 磁盘管理
Linux 磁盘管理好坏管理直接关系到整个系统的性能问题。

Linux 磁盘管理常用三个命令为`df`、`du`和`fdisk`。
- `df`：列出文件系统的整体磁盘使用量
- `du`：检查磁盘空间使用量
- `fdisk`：用于磁盘分区 

1.`df`
`df`命令参数功能：**检查文件系统的磁盘空间占用情况**。可以利用该命令来获取硬盘被占用了多少空间，目前还剩下多少空间等信息。

```shell
df [-ahikHTm] [目录或文件名]
```

选项与参数：
`-a` ：列出所有的文件系统，包括系统特有的 /proc 等文件系统；
`-k` ：以 KBytes 的容量显示各文件系统；
`-m` ：以 MBytes 的容量显示各文件系统；
`-h` ：以人们较易阅读的 GBytes, MBytes, KBytes 等格式自行显示；
`-H` ：以 M=1000K 取代 M=1024K 的进位方式；
`-T` ：显示文件系统类型, 连同该 partition 的 filesystem 名称 (例如 ext3) 也列出；
`-i` ：不用硬盘容量，而以 inode 的数量来显示


2.`du`
`du`命令也是查看使用空间的，但是与`df`命令不同的是：**`du`命令是对文件和目录磁盘使用的空间的查看**，还是和`df`命令有一些区别的，这里介绍Linux du命令。

```shell
du [-ahskm] 文件或目录名称
```

选项与参数：
`-a` ：列出所有的文件与目录容量，因为默认仅统计目录底下的文件量而已。
`-h` ：以人们较易读的容量格式 (G/M) 显示；
`-s` ：列出总量而已，而不列出每个各别的目录占用容量；
`-S` ：不包括子目录下的总计，与 -s 有点差别。
`-k` ：以 KBytes 列出容量显示；
`-m` ：以 MBytes 列出容量显示；

与`df`不一样的是，`du`这个命令其实会直接到文件系统内去搜寻所有的文件数据。

3.`fdisk`
`fdisk`是 Linux 的磁盘分区表操作工具。

```shell
fdisk [-l] 装置名称
```

选项与参数：
`-l`：输出后面接的装置所有的分区内容。若仅有 fdisk -l 时， 则系统将会把整个系统内能够搜寻到的装置的分区均列出来。

实例：
```shell
找出你系统中的根目录所在磁盘，并查阅该硬盘内的相关信息

[root@www ~]# df /            <==注意：重点在找出磁盘文件名而已 
Filesystem           1K-blocks      Used Available Use% Mounted on 
/dev/hdc2              9920624   3823168   5585388  41% /  
[root@www ~]# fdisk /dev/hdc  <==仔细看，不要加上数字喔！ 
The number of cylinders for this disk is set to 5005. 
There is nothing wrong with that, but this is larger than 1024, 
and could in certain setups cause problems with: 
1) software that runs at boot time (e.g., old versions of LILO) 
2) booting and partitioning software from other OSs   
    (e.g., DOS FDISK, OS/2 FDISK)  

Command (m for help):     <==等待你的输入！ 
输入 m 后，就会看到底下这些命令介绍

Command (m for help): m   <== 输入 m 后，就会看到底下这些命令介绍 
Command action    
   a   toggle a bootable flag   
   b   edit bsd disklabel    
   c   toggle the dos compatibility flag    
   d   delete a partition            <==删除一个partition    
   l   list known partition types    
   m   print this menu    
   n   add a new partition           <==新增一个partition    
   o   create a new empty DOS partition table    
   p   print the partition table     <==在屏幕上显示分割表    
   q   quit without saving changes   <==不储存离开fdisk程序    
   s   create a new empty Sun disklabel    
   t   change a partition's system id    
   u   change display/entry units    
   v   verify the partition table    
   w   write table to disk and exit  <==将刚刚的动作写入分割表    
   x   extra functionality (experts only) 
```   

离开 fdisk 时按下 q，那么所有的动作都不会生效！相反的， 按下 w 就是动作生效的意思。

```
Command (m for help): p  <== 这里可以输出目前磁盘的状态  

Disk /dev/hdc: 41.1 GB, 41174138880 bytes        <==这个磁盘的文件名与容量 
255 heads, 63 sectors/track, 5005 cylinders      <==磁头、扇区与磁柱大小 
Units = cylinders of 16065 * 512 = 8225280 bytes <==每个磁柱的大小    
   Device Boot      Start         End      Blocks   Id  System 
/dev/hdc1   *           1          13      104391   83  Linux 
/dev/hdc2              14        1288    10241437+  83  Linux 
/dev/hdc3            1289        1925     5116702+  83  Linux 
/dev/hdc4            1926        5005    24740100    5  Extended 
/dev/hdc5            1926        2052     1020096   82  Linux swap / Solaris 
# 装置文件名 启动区否 开始磁柱    结束磁柱  1K大小容量 磁盘分区槽内的系统  

Command (m for help): q 
```
想要不储存离开吗？按下 q 就对了！不要随便按 w 啊！
使用 p 可以列出目前这颗磁盘的分割表信息，这个信息的上半部在显示整体磁盘的状态。

#### 磁盘格式化
磁盘分割完毕后自然就是要进行文件系统的格式化，格式化的命令非常的简单，使用`mkfs（make filesystem）`命令。

```shell
mkfs [-t 文件系统格式] 装置文件名
```

选项与参数：
`-t` ：可以接文件系统格式，例如 ext3, ext2, vfat 等(系统有支持才会生效)

```shell
查看 mkfs 支持的文件格式
[root@www ~]# mkfs[tab][tab]
mkfs         mkfs.cramfs  mkfs.ext2    mkfs.ext3    mkfs.msdos   mkfs.vfat
按下两个[tab]，会发现 mkfs 支持的文件格式如上所示。
```
```shell
将分区 /dev/hdc6（可指定你自己的分区） 格式化为 ext3 文件系统：

[root@www ~]# mkfs -t ext3 /dev/hdc6
mke2fs 1.39 (29-May-2006)
Filesystem label=                <==这里指的是分割槽的名称(label) 
OS type: Linux Block size=4096 (log=2)          <==block 的大小配置为 4K  
Fragment size=4096 (log=2) 
251392 inodes, 502023 blocks     <==由此配置决定的inode/block数量 
25101 blocks (5.00%) reserved for the super user 
First data block=0 
Maximum filesystem blocks=515899392 
16 block groups 32768 blocks per group, 
32768 fragments per group 
15712 inodes per group 
Superblock backups stored on blocks:         
        32768, 98304, 163840, 229376, 294912  

Writing inode tables: done 
Creating journal (8192 blocks): done <==有日志记录 
Writing superblocks and filesystem accounting information: done  

This filesystem will be automatically checked every 34 mounts or 
180 days, whichever comes first.  Use tune2fs -c or -i to override. 
# 这样就创建起来我们所需要的 Ext3 文件系统了！简单明了！ 
```

#### 磁盘检验
`fsck（file system check）`用来检查和维护不一致的文件系统。
若系统掉电或磁盘发生问题，可利用`fsck`命令对文件系统进行检查。

```shell
fsck [-t 文件系统] [-ACay] 装置名称
```

选项与参数：
`-t` : 给定档案系统的型式，若在 /etc/fstab 中已有定义或 kernel 本身已支援的则不需加上此参数
`-s` : 依序一个一个地执行 fsck 的指令来检查
`-A` : 对/etc/fstab 中所有列出来的 分区（partition）做检查
`-C` : 显示完整的检查进度
`-d` : 打印出 e2fsck 的 debug 结果
`-p` : 同时有 -A 条件时，同时有多个 fsck 的检查一起执行
`-R` : 同时有 -A 条件时，省略 / 不检查
`-V` : 详细显示模式
`-a` : 如果检查有错则自动修复
`-r` : 如果检查有错则由使用者回答是否修复
`-y` : 选项指定检测每个文件是自动输入yes，在不确定那些是不正常的时候，可以执行 # fsck -y 全部检查修复。

```shell
强制检测 /dev/hdc6 分区:

[root@www ~]# fsck -C -f -t ext3 /dev/hdc6 
fsck 1.39 (29-May-2006)
e2fsck 1.39 (29-May-2006)
Pass 1: Checking inodes, blocks, and sizes
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information
vbird_logical: 11/251968 files (9.1% non-contiguous), 36926/1004046 blocks
```
如果没有加上`-f`的选项，则由于这个文件系统不曾出现问题，检查的经过非常快速！
若加上`-f`强制检查，才会一项一项的显示过程。


#### 磁盘挂载与卸除
Linux 的磁盘挂载使用`mount`命令，卸载使用`umount`命令。

1.磁盘挂载语法：
```shell
mount [-t 文件系统] [-L Label名] [-o 额外选项] [-n]  装置文件名  挂载点
```

用默认的方式，将刚刚创建的 /dev/hdc6 挂载到 /mnt/hdc6 上面！
```
[root@www ~]# mkdir /mnt/hdc6
[root@www ~]# mount /dev/hdc6 /mnt/hdc6
[root@www ~]# df
Filesystem           1K-blocks      Used Available Use% Mounted on
.....中间省略.....
/dev/hdc6              1976312     42072   1833836   3% /mnt/hdc6
```

2.磁盘卸载语法：
```shell
umount [-fn] 装置文件名或挂载点
```

选项与参数：
`-f` ：强制卸除！可用在类似网络文件系统 (NFS) 无法读取到的情况下；
`-n` ：不升级 /etc/mtab 情况下卸除。

卸载/dev/hdc6
```shell
[root@www ~]# umount /dev/hdc6    
```

---
### Linux vi/vim
所有的 Unix Like 系统都会内建 vi 文书编辑器，其他的文书编辑器则不一定会存在。

但是目前我们使用比较多的是 vim 编辑器，vim 具有程序编辑的能力，可以主动的以字体颜色辨别语法的正确性，方便程序设计。





































---
### Linux yum命令
yum（ Yellow dog Updater, Modified ）是一个在 Fedora 和 RedHat 以及 SUSE 中的 Shell 前端软件包管理器。

基于 RPM 包管理，能够从指定的服务器自动下载 RPM 包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软体包，无须繁琐地一次次下载、安装。

yum提供了查找、安装、删除某一个、一组甚至全部软件包的命令，而且命令简洁而又好记。

```shell
yum [options] [command] [package ...]
```
- `options`：可选，选项包括`-h`（帮助），`-y`（当安装过程提示选择全部为"yes"），`-q`（不显示安装的过程）等等。
- `command`：要进行的操作。
- `package`：操作的对象。

#### yum常用命令
1. 列出所有可更新的软件清单命令：`yum check-update`
2. 更新所有软件命令：`yum update`
3. 仅安装指定的软件命令：`yum install <package_name>`
4. 仅更新指定的软件命令：`yum update <package_name>`
5. 列出所有可安裝的软件清单命令：`yum list`
6. 删除软件包命令：`yum remove <package_name>`
7. 查找软件包命令：`yum search <keyword>`
8. 清除缓存命令:
    `yum clean packages`：清除缓存目录下的软件包
    `yum clean headers`：清除缓存目录下的 headers
    `yum clean oldheaders`：清除缓存目录下旧的 headers
    `yum clean, yum clean all (= yum clean packages; yum clean oldheaders)`：清除缓存目录下的软件包及旧的 headers

实例 1：
安装 pam-devel
```shell
[root@www ~]# yum install pam-devel
Setting up Install Process
Parsing package install arguments
Resolving Dependencies  <==先检查软件的属性相依问题 --> Running transaction check
---> Package pam-devel.i386 0:0.99.6.2-4.el5 set to be updated
--> Processing Dependency: pam = 0.99.6.2-4.el5 for package: pam-devel
--> Running transaction check
---> Package pam.i386 0:0.99.6.2-4.el5 set to be updated
filelists.xml.gz          100% |=========================| 1.6 MB    00:05
filelists.xml.gz          100% |=========================| 138 kB    00:00
-> Finished Dependency Resolution
……(省略)
```

实例 2：
移除 pam-devel
```shell
[root@www ~]# yum remove pam-devel
Setting up Remove Process
Resolving Dependencies  <==同样的，先解决属性相依的问题 --> Running transaction check
---> Package pam-devel.i386 0:0.99.6.2-4.el5 set to be erased
--> Finished Dependency Resolution

Dependencies Resolved

=============================================================================
 Package                 Arch       Version          Repository        Size
=============================================================================
Removing:
 pam-devel               i386       0.99.6.2-4.el5   installed         495 k

Transaction Summary
=============================================================================
Install      0 Package(s)
Update       0 Package(s)
Remove       1 Package(s)  <==还好，并没有属性相依的问题，单纯移除一个软件  Is this ok [y/N]: y Downloading Packages: Running rpm_check_debug Running Transaction Test Finished Transaction Test Transaction Test Succeeded Running Transaction   Erasing   : pam-devel                    ######################### [1/1]  Removed: pam-devel.i386 0:0.99.6.2-4.el5 Complete! 
```

实例 3：
利用 yum 的功能，找出以 pam 为开头的软件名称有哪些？
```shell
[root@www ~]# yum list pam*
Installed Packages
pam.i386                  0.99.6.2-3.27.el5      installed
pam_ccreds.i386           3-5                    installed
pam_krb5.i386             2.2.14-1               installed
pam_passwdqc.i386         1.0.2-1.2.2            installed
pam_pkcs11.i386           0.5.3-23               installed
pam_smb.i386              1.1.7-7.2.1            installed
Available Packages <==底下则是『可升级』的或『未安装』的 pam.i386                  0.99.6.2-4.el5         base pam-devel.i386            0.99.6.2-4.el5         base pam_krb5.i386             2.2.14-10              base
```
---


## Shell 教程





[toc]
## Linux 基础

### 1. Linux 系统目录结构

在 Linux 或 Unix 操作系统中，**所有的文件和目录都被组织成以一个根节点开始的倒置的树状结构**。

![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/linux_contents_1.png)

1. 文件系统的最顶层是由根目录开始的，系统使用`/`来表示根目录。在根目录之下的既可以是目录，也可以是文件，而每一个目录中又可以包含子目录文件。如此反复就可以构成一个庞大的文件系统。

2. 在 Linux 文件系统中有两个特殊的目录：
   - 一个用户所在的工作目录，也叫当前目录，可以使用一个点`.`来表示，也可以使用`./`来表示；
   - 另一个是当前目录的上一级目录，也叫父目录，可以使用两个点`..`来表示，也可以`../`来代表。

3. 如果一个目录或文件名以一个点`.`开始，表示这个目录或文件是一个隐藏目录或文件(如:`.bashrc`)。
即以默认方式查找时，不显示该目录或文件。


Linux 文件系统的常用目录如下表所示：


| 目录 | 存放内容 |
|----|----|
| `/bin` | bin 是 Binary 的缩写, 此目录存放着最经常使用的命令 |
| `/boot` | 此目录存放的是启动 Linux 时使用的一些核心文件，包括一些连接文件以及镜像文件 |
| `/dev` | dev 是 Device 的缩写, 该目录下存放的是 Linux 的外部设备（每个设备都被认为是一个文件）<br> 设备文件分为块设备文件和字符设备文件 |
| `/etc` | 存放所有的系统管理所需要的配置文件和子目录 |
| `/home` | 用户的主目录，在 Linux 中每个用户都有一个自己的目录，一般该目录名是以用户的账号命名的 |
| `/lib` | 存放着系统最基本的动态连接共享库，其作用类似于 Windows 里的 DLL 文件。几乎所有的应用程序都需要用到这些共享库 |
| `/media` | Linux 系统会自动识别一些设备，例如 U 盘、光驱等等，当识别后会把识别的设备挂载到这个目录下 |
| `/mnt` | Linux 系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将光驱挂载在`/mnt`上，然后进入该目录就可以查看光驱里的内容了 |
| `/opt` | 额外安装软件所放置的目录。比如安装一个 ORACLE 数据库则就可以放到这个目录下（默认是空的） |
| `/root` | 该目录为系统管理员，也称作超级权限者的用户主目录 |
| `sbin` | s 就是 Super User 的意思，这里存放的是系统管理员使用的系统管理程序 | 
| `/proc` | proc 被称为虚拟文件系统，它是系统内存的映射，这个目录的内容不在硬盘上而是在内存里 <br> 它是一个控制中心，可以通过更改其中某些文件改变内核运行状态 <br> 它也是内核提空给我们的查询中心，用户可以通过它查看系统硬件及当前运行的进程信息 |
| `/sys` | 存放硬件设备的驱动程序信息 |
| `/tmp` | 存放一些临时文件 |
| `/srv` | 存放一些服务启动之后需要提取的数据 |
| `/usr` | 非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似与 windows 下的 program files 目录 |
| `/var` | 存放着在不断扩充着的东西，我们习惯将那些经常被修改的目录放在这个目录下。包括各种日志文件 |


- 备注1. `/sys/fs/`目录 
  + fs 文件系统集成了下面 3 种文件系统的信息：
    - 针对进程信息的 proc 文件系统
    - 针对设备的 devfs 文件系统
    - 针对伪终端的 devpts 文件系统
  + 该文件系统是内核设备树的一个直观反映，当一个内核对象被创建的时候，对应的文件和目录也在内核对象子系统中被创建。
<br>

- 备注2. `/usr/` 目录
  + `/usr/bin`：系统用户使用的应用程序
  + `/usr/sbin`：超级用户使用的比较高级的管理程序和系统守护程序
  + `/usr/src`：内核源代码默认的放置目录
<br>

- 备注3. `/var/` 目录
  + `/var/cache/`：应用程序本身运作过程中会产生的一些暂存档
  + `/var/lib/`：程序本身执行的过程中，需要使用到的数据文件的放置目录。在此目录下各自的软件应该要有各自的目录
  + `/var/lock/`：某些装置或者是文件资源一次只能被一个应用程序所使用，如果同时有两个程序使用该装置时，就可能产生一些错误的状况，因此就得要将该装置上锁（lock），以确保该装置只会给单一软件所使用
  + `/var/log/`：非常重要。这是登录文件放置的目录。里面比较重要的文件如`/var/log/messages`, `/var/log/wtmp`（记录登入者的信息）
  + `/var/mail`：放置个人电子邮件信箱的目录，不过这个目录也被放置到`/var/spool/mail/`目录中，通常这两个目录是互为链接文件
  + `/var/run/`：某些程序或者是服务启动后，会将他们的 PID 放置在这个目录下
  + `/var/spool/`：这个目录通常放置一些队列数据（等待其他程序使用的数据），这些数据被使用后通常都会被删除

---

### 2. Linux 文件属性

#### 2.1 Linux 文件基本属性

Linux 系统是一种典型的多用户系统，不同的用户处于不同的地位，拥有不同的权限。为了保护系统的安全性，Linux 系统对不同的用户访问同一文件（包括目录文件）的权限做了不同的规定。

在 Linux 中我们可以使用`ll`或者`ls –l`命令来显示一个目录下所有文件的属性以及文件所属的用户和组，如：
```shell
ls -l

# output
total 64
dr-xr-xr-x   2 root root 4096 Dec 14  2012 bin
dr-xr-xr-x   4 root root 4096 Apr 19  2012 boot
……
```

1. **在Linux中第一个字符代表这个文件是目录、文件或链接文件等等**，具体如下表所示：

| 属性标识 | 含义 |
|:----:|:----:|
| `d` | 目录 |
| `-` | 文件 |
| `l` | 链接文件（link file）|
| `b` | 装置文件里面的可供储存的接口设备（可随机存取装置） |
| `c` | 装置文件里面的串行端口设备，例如键盘、鼠标（一次性读取装置） |


2. **接下来的字符中，以三个为一组，且均为`rwx`的三个参数的组合，来表示此文件的读、写、执行的权限**。

| 权限标识 | 含义 |
|:----:|:----:|
| `r` | 可读（read）|
| `w` | 可写（write）|
| `x` | 可执行（execute）|
| `-` | 在相应位置上没有相应的权限 |

> 注意：这三个权限的位置不会改变，如果没有权限，就会出现减号`-`而已。

3. **每个文件的属性由左边第一部分的 10 个字符来确定**，如下图所示：

![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/linux_contents_2.png)

从左至右用 0-9 这些数字来表示：
- 第 0 位确定文件类型；
- 第 1-3 位确定属主（该文件的所有者）拥有该文件的权限；
- 第 4-6 位确定属组（所有者的同组用户）拥有该文件的权限；
- 第 7-9 位确定其他用户拥有该文件的权限。

> 图中，第1、4、7位表示读权限，如果用`r`字符表示，则有读权限，如果用`-`字符表示，则没有读权限；
> 第2、5、8位表示写权限，如果用`w`字符表示，则有写权限，如果用`-`字符表示没有写权限；
> 第3、6、9位表示可执行权限，如果用`x`字符表示，则有执行权限，如果用`-`字符表示，则没有执行权限。

<br>

#### 2.2 Linux 文件属主和属组

对于文件来说，它都有一个特定的所有者，也就是拥有该文件所有权的用户。**在 Linux 系统中，用户是按组分类的，一个用户属于一个或多个组**。

文件所有者以外的用户又可以分为文件所有者的同组用户和其他用户。因此，**Linux 系统按文件所有者、文件所有者同组用户和其他用户来规定了不同的文件访问权限**。

```shell
ls -l

# output
total 64
dr-xr-xr-x   2 root root 4096 Dec 14  2012 bin
dr-xr-xr-x   4 root root 4096 Apr 19  2012 boot
……
```

解读：在以上实例中，bin 文件是一个目录文件
- 属主和属组都为 root，属主有可读、可写、可执行的权限；
- 与属主同组的其他用户有可读和可执行的权限；
- 其他用户也有可读和可执行的权限。

<br>

#### 2.3 更改文件属主和属组

1. `chown`：更改文件属主，也可以同时更改文件属组
```shell
# 更改文件属主，参数 -R 表示递归更改
chown [–R] 属主名 文件名/目录名

# 更改文件属主和属组，参数 -R 表示递归更改
chown [-R] 属主名:属组名 文件名/目录名
```

2. `chgrp`：更改文件属组

```shell
# 更改文件属组，参数 -R 表示递归更改
chgrp [-R] 属组名 文件名/目录名
```

#### 2.4 更改文件权限属性

`chmod`：更改文件 9 个属性

- Linux 文件的基本权限就有九个，分别是 owner/group/others 三种身份各有自己的 read/write/execute 权限。
- **Linux 文件属性有两种设置方法，一种是数字，一种是符号**。

##### 2.4.1 数字类型改变文件权限
文件的权限字符为：`-rwxrwxrwx`， 这九个权限是三个三个一组的！其中，我们可以使用数字来代表各个权限，各权限的分数对照表如下：

| 权限字符 | 分数 |
|:----:|:----:|
| `r` | `4` |
| `w` | `2` |
| `x` | `1` |


每种身份（owner/group/others）各自的三个权限（r/w/x）分数是需要累加的。例如当权限为：`-rwxrwx---` 分数则是`770`：
- owner = `rwx` = `4+2+1` = `7`
- group = `rwx` = `4+2+1` = `7`
- others = `---` = `0+0+0` = `0`

变更权限的指令`chmod`的语法是这样的：
```shell
chmod [-R] xyz 文件名/目录名

# 选项与参数：
xyz : 就是刚刚提到的数字类型的权限属性，为`rwx`属性数值的相加
-R : 进行递归（recursive）的持续变更，亦即连同此目录下的所有文件都会变更
```

举例：将`.bashrc`这个文件所有的权限都设定启用，那么命令如下：
```shell
# 查看文件修改前的权限
ls -al .bashrc
# output
-rw-r--r--  1 root root 395 Jul  4 11:45 .bashrc

# 修改文件权限为 777
chmod 777 .bashrc

# 查看文件修改后的权限
ls -al .bashrc
# output
-rwxrwxrwx  1 root root 395 Jul  4 11:45 .bashrc
```

##### 2.4.2 符号类型改变文件权限
九个权限分别是 user、group、others 三种身份，可以藉由 `u`, `g`, `o` 来代表三种身份的权限！
此外，`a`则代表 all 亦即全部的身份！那么读写的权限就可以写成`r`, `w`, `x`：

|`chmod`|`u`<br>`g`<br>`o`<br>`a`|`+`(加入)<br>`-`(除去)<br>`=`(设定)|`r`<br>`w`<br>`x`|文件或目录|
|:----:|:----:|:----:|:----:|:----:|

如果需要将文件权限设置为`-rwxr-xr--`，可以使用`chmod u=rwx,g=rx,o=r 文件名`来设定

```shell
# 查看文件修改前的权限
ls -al .bashrc
# output
-rwxr-xr-x  1 root root 395 Jul  4 11:45 .bashrc

# 修改文件权限为
chmod  a+w  .bashrc

# 查看文件修改后的权限
ls -al .bashrc
# output
-rwxrwxrwx  1 root root 395 Jul  4 11:45 .bashrc
```

---

### 3. 文件的隐藏属性


Linux系统中的文件除了具备一般权限和特殊权限之外，还有一种隐藏权限，即被隐藏起来的权限，默认情况下不能直接被用户发觉。明明权限充足但却无法删除某个文件的情况，或者仅能在日志文件中追加内容而不能修改或删除内容，这在一定程度上阻止了黑客篡改系统日志的图谋














---
### 4. Linux 文件与目录管理

Linux 的目录结构为树状结构，最顶级的目录为根目录`/`。其他目录通过挂载可以将它们添加到树中，通过解除挂载可以移除它们。

> - Linux 中的挂载是指将一个设备（通常是存储设备）挂接到一个已存在的目录上。
> - 这个目录可以不为空，但挂载后这个目录下以前的内容将不可用。
> - 我们要访问存储设备中的文件，必须将文件所在的分区挂载到一个已存在的目录上，然后通过访问这个目录来访问存储设备。


绝对路径与相对路径
- 绝对路径：由根目录`/`写起，例如`/usr/share/doc`这个目录。
- 相对路径：不由根目录`/`写起，例如由`/usr/share/doc`要到`/usr/share/man`底下时，可以写成：`cd ../man` 


#### 4.1 Linux 处理目录

Linux 中常见的处理目录的命令：

| 命令 | 功能 | 基本方法 | 常用参数 | 
|----|----|----|---|
| `ls` | 列出目录 | `ls`或`ls 绝对路径/相对路径` | `-a`：列出全部的目录和文件，包括隐藏文件（开头为`.`的文件） <br> `-l`：长数据串列出，包含目录和文件的属性与权限等等数据 | 
| `cd` | 切换目录（Change Directory）| `cd 绝对路径/相对路径` | |
| `pwd` | 显示当前用户目前所在目录 | `pwd` | `-P`：显示确实的路径，而非使用连结 (link) 的路径 |
| `mkdir` | 创建一个新的目录（make directory）| `mkdir [-mp] 目录名称` | `-p`：直接将所需要的目录(包含上一级目录)递回创建起来 <br> `-m`：直接配置文件的权限，例如`mkdir -m 755 目录` |
| `rmdir` | 删除一个空的目录 | `rmdir [-p] 目录` | `-p`：连同上一级空目录也一起删除 |
| `cp` | 复制文件或目录 | `cp [-dfpru] 源路径 目标路径` | `-r`：递回持续复制，用于目录的复制 <br> `-p`：连同文件的属性一起复制过去，而非使用默认属性（备份常用）<br>  `-d`：若来源文件的属性为链接文件(link file)，则复制连结文件属性而非链接到的文件本身 <br> `-f`：为强制（force）的意思 <br> `-u`：若目标文件已经存在，且源文件相对目标文件比较新，才会升级覆盖 (update)  |
| `rm` | 移除文件或目录 | `rm [-fir] 文件/目录` | `-f`：是 force 的意思，忽略不存在的文件，不会出现警告信息 <br> `-i`：互动模式，在删除前会询问使用者是否动作 <br> `-r`：递回删除！常用在删除非空目录 |
| `mv` | 移动文件与目录、文件重命名 | `mv [-fiu] 源路径 目标路径` | `-f`：是 force 强制的意思，如果目标文件已经存在，不会询问而直接覆盖 <br> `-i`：若目标文件 (destination) 已经存在时，就会询问是否覆盖！<br> `-u`：若目标文件已经存在，且源文件相对目标文件比较新，才会升级覆盖 (update) |



#### 4.2 Linux 文件内容查看

Linux系统中使用以下命令来查看文件的内容：

| 命令 | 功能 | 基本方法 | 常用参数 |  
|----|----|----|----|
| `cat` | 由第一行开始显示文件内容 | `cat [-bnETv] 文件名` | `-b`：列出行号，仅针对非空白行做行号显示，空白行不标行号！<br> `-n`：列印出行号，连同空白行也会有行号 <br> `-E`：将结尾的断行字节`$`显示出来 <br> `-T`：将 tab 按键以`^I`显示出来 <br> `-v`：列出一些看不出来的特殊字符 |
| `tac` | 从最后一行开始显示（`tac`是`cat`的倒写）| `tac [-bnETv] 文件名` | `-b`：列出行号，仅针对非空白行做行号显示，空白行不标行号！<br> `-n`：列印出行号，连同空白行也会有行号 <br> `-E`：将结尾的断行字节`$`显示出来 <br> `-T`：将 tab 按键以`^I`显示出来 <br> `-v`：列出一些看不出来的特殊字符 |
| `nl` | 从第一行开始显示内容及行号（对非空白行做行号显示）| `nl [-bnw] 文件名` | `-b a`：表示不论是否为空行，也同样列出行号(类似`cat -n`) <br> `-b t`：如果有空行，空的那一行不要列出行号(默认值) <br> `-n`：列出行号表示的方法（`-n ln`、`-n rn`、`-n rz`）<br> `-w`：行号栏位的占用的位数（例如占用三位`-w 3`）|
| `more` | 一页一页的显示文件内容 | `more [-dfpsu] [-num] [+lineNum] 文件名` | `-num`：一次显示的行数 <br> `-d`：提示使用者，在画面下方显示 [Press space to continue, 'q' to quit.] ，如果使用者按错键，则会显示 [Press 'h' for instructions.] <br>  `+lineNum`：从第 num 行开始显示 <br> `-f`：计算行数时，以实际上的行数，而非自动换行过后的行数（有些单行字数太长的会被扩展为两行或两行以上）<br> `-p`：不以卷动的方式显示每一页，而是先清除屏幕后再显示内容 <br> `-s`：当遇到有连续两行以上的空白行，就代换为一行的空白行 <br> `-u`：不显示下引号（根据环境变数 TERM 指定的 terminal 而有所不同）|
| `less` | 与 more 类似，但是比 more 更好的是，他可以往前翻页！| `less [参数] 文件名`| `-i`：忽略搜索时的大小写 <br> `-b`：<缓冲区大小> 设置缓冲区的大小 <br> `-e`：当文件显示结束后，自动离开 |
| `head` | 只看头几行 | `head [-n number] 文件名` | `-n`：后面接数字，代表显示开头相应的行数 |
| `tail` | 只看尾巴几行 | `tail [-n number] 文件名` | `-n`：后面接数字，代表显示最后相应的行数 <br> `-f`：表示持续侦测文件的内容，要等到按下 ctrl + c 才会结束 tail 的侦测 |
<br>

备注1. 在`more`运行时可以使用的常用按键命令有：

| 按键命令 | 功能 |
|:----:|----|
| `空格键（space）` | 代表向下翻"一页" |
| `Enter` | 代表向下翻"一行" |
| `/字串` | 代表在这个显示的内容当中，向下搜寻"字串"这个关键字 |
| `:f` | 立刻显示文件名以及目前显示的行数 |
| `q` | 代表立刻离开 more ，不再显示该文件内容 |
| `b`或`[ctrl]+b` | 代表往回翻页，不过这动作只对文件有用，对管线无用 |
<br>

备注2. 在`less`运行时可以输入的常用按键命令有：

| 按键命令 | 功能 |
|:----:|----|
| `空格键（space）` | 代表向下翻"一页" |
| `方向键[pageDown]` | 代表向下翻"一行" |
| `方向键[pageUp]` | 代表向上翻"一行" |
| `/字串` | 向下搜寻"字串"的功能 | 
| `?字串` | 向上搜寻"字串"的功能 | 
| `n` | 重复前一个搜寻（与`/`或`?`有关） |
| `N` | 反向的重复前一个搜寻（与`/`或`?`有关）|
| `q` | 离开 less 这个程序 | 
<br>


---


### 5. Linux 用户和用户组管理

Linux 系统是一个多用户多任务的分时操作系统，任何一个要使用系统资源的用户，都必须首先向系统管理员申请一个账号，然后以这个账号的身份进入系统。

用户的账号一方面可以帮助系统管理员对使用系统的用户进行跟踪，并控制他们对系统资源的访问；另一方面也可以帮助用户组织文件，并为用户提供安全性保护。

每个用户账号都拥有一个惟一的用户名和各自的口令。用户在登录时键入正确的用户名和口令后，就能够进入系统和自己的主目录。实现用户账号的管理，要完成的工作主要有如下几个方面：
  - 用户账号的添加、删除与修改
  - 用户口令的管理
  - 用户组的管理

#### 5.1 Linux 系统用户账号的管理

用户账号的管理工作主要涉及到用户账号的添加、修改和删除。

添加用户账号就是在系统中创建一个新账号，然后为新账号分配用户号、用户组、主目录和登录 Shell 等资源。刚添加的账号是被锁定的，无法使用。

##### 5.1.1 添加用户账号

添加新的用户账号使用`useradd`命令，其语法如下：

```shell
useradd [参数] 用户名
```

参数:
`-c`：comment --- 指定一段注释性描述。
`-d`：目录 --- 指定用户主目录，如果此目录不存在，则同时使用`-m`选项，可以创建主目录。
`-g`：用户组 --- 指定用户所属的用户组。
`-G`：用户组 --- 指定用户所属的附加组。
`-s`：Shell 文件 --- 指定用户的登录 Shell。
`-u`：用户号 --- 指定用户的用户号，如果同时有`-o`选项，则可以重复使用其他用户的标识号。

举例
```shell
# 创建了一个用户 sam，其中 -d 和 -m 选项用来为登录名 sam 产生一个主目录 /usr/sam（ /usr 为默认的用户主目录所在的父目录）
useradd –d /usr/sam -m sam

# 新建了一个用户 gem，该用户的登录 Shell 是 /bin/sh，它属于 group 用户组，同时又属于 adm 和 root 用户组，其中 group 用户组是其主组
useradd -s /bin/sh -g group –G adm,root gem

# 添加一个不能登录的用户，要拒绝系统用户登录，可以将其 shell 设置为 /usr/sbin/nologin 或者 /bin/false
useradd -d /usr/local/apache -g apache -s /bin/false apache
或
useradd -d /usr/local/apache -g apache -s /usr/sbin/nologin apache

# usermod 可用来修改用户帐号的各项设定
usermod -s | --shell /usr/sbin/nologin username

```


备注：/bin/false 和 /usr/sbin/nologin 的比较

> - `/bin/false` 是最严格的禁止 login 选项，一切服务都不能用。将用户的 shell 设置为 /bin/false，用户会无法登录，并且不会有任何提示。
> - `/usr/sbin/nologin` 只是不允许 login 系统（即使给了密码也不行）。"无法登陆"指的仅是这个用户无法使用 bash 或其他 shell 来登陆系统而已，并不是说这个账号就无法使用系统资源
> 举例来说，各个系统账号中，打印作业有 lp 这个账号管理，www 服务器有 apache 这个账号管理，他们都可以进行系统程序的工作，但就是无法登陆主机而已

<br>


##### 5.1.2 删除用户帐号

如果一个用户的账号不再使用，可以从系统中删除。**删除用户账号就是要将`/etc/passwd`等系统文件中的该用户记录删除，必要时还删除用户的主目录**。

删除一个已有的用户账号使用`userdel`命令，其格式如下：

```shell
userdel 选项 用户名
# 此命令删除用户在系统文件中（主要是/etc/passwd, /etc/shadow, /etc/group等）的记录
```

常用参数是`-r`，它的作用是把用户的主目录（`/home/USERNAME`，`/var/spool/mail/USERNAME`）一起删除。

<br>


##### 5.1.3 修改用户帐号

修改用户账号就是根据实际情况更改用户的有关属性，如用户号、主目录、用户组、登录 Shell 等。

修改已有用户的信息使用`usermod`命令，其格式如下：

```shell
usermod 选项 用户名
```

参数:
`-c`：comment --- 指定一段注释性描述。
`-d`：目录 --- 指定用户主目录，如果此目录不存在，则同时使用`-m`选项，可以创建主目录。
`-g`：用户组 --- 指定用户所属的用户组。
`-G`：用户组 --- 指定用户所属的附加组。
`-s`：Shell 文件 --- 指定用户的登录 Shell。
`-u`：用户号 --- 指定用户的用户号，如果同时有`-o`选项，则可以重复使用其他用户的标识号。
`-l`：新用户名 --- 指定一个新的用户名，即将原来的用户名改为新的用户名

举例：
```shell
# 将用户 sam 的登录 Shell 修改为 ksh，主目录改为 /home/z，用户组改为 developer
usermod -s /bin/ksh -d /home/z –g developer sam
```

##### 5.1.4 用户口令的管理
用户管理的一项重要内容是用户口令的管理。**用户账号刚创建时没有口令，但是被系统锁定，无法使用，必须为其指定口令后才可以使用，即使是指定空口令**。

指定和修改用户口令的 Shell 命令是`passwd`。超级用户可以为自己和其他用户指定口令，普通用户只能用它修改自己的口令。命令的格式为：


```shell
# 未指出用户名，则默认修改当前用户的口令密码
passwd [参数] [用户名]
```

参数：
`-l`：锁定口令，不能修改密码。
`-u`：口令解锁。
`-d`：使账号无口令，可使用空密码登录。
`-f`：强迫用户下次登录时修改口令。
`-S`：显示用户的密码是否被锁定，以及密码所采用的加密算法名称

举例：
```shell
# 假设当前用户是 sam，则下面的命令修改该用户自己的口令
passwd 
# output
Old password:****** 
New password:******* 
Re-enter new password:*******

# 如果是超级用户，可以用下列形式指定任何用户的口令
passwd sam 
# output
New password:******* 
Re-enter new password:*******

# 为用户指定空口令
passwd -d sam
```

#### 5.2 Linux 系统用户组的管理

每个用户都有一个用户组，系统可以对一个用户组中的所有用户进行集中管理。

用户组的管理涉及用户组的添加、删除和修改，实际上就是对`/etc/group`文件的更新。

1.增加一个新的用户组使用`groupadd`命令。其格式如下：
```shell
groupadd [参数] [用户组名]
```
参数：
`-g`：GID 指定新用户组的组标识号（GID）。
`-o`：一般与`-g`选项同时使用，表示新用户组的 GID 可以与系统已有用户组的 GID 相同。

举例：
```shell
# 增加了一个新组 group1，新组的组标识号是在当前已有的最大组标识号的基础上加 1
groupadd group1

# 增加了一个新组 group2，同时指定新组的组标识号是 101。
groupadd -g 101 group2
```

2.删除一个已有的用户组，使用`groupdel`命令，其格式如下：
```shell
groupdel [用户组名]
```

3.修改用户组的属性使用`groupmod`命令。其语法如下：
```shell
groupmod [参数] [用户组名]
```
参数：
`-g`：GID 为用户组指定新的组标识号。
`-o`：与`-g`选项同时使用，用户组的新 GID 可以与系统已有用户组的 GID 相同。
`-n`：新用户组 将用户组的名字改为新名字

```shell
# 此命令将组 group2 的组标识号修改为 102
groupmod -g 102 group2

# 此命令将组 group2 的标识号改为 10000，组名修改为 group3
groupmod –g 10000 -n group3 group2
```

4.如果一个用户同时属于多个用户组，那么用户可以在用户组之间切换，以便具有其他用户组的权限。

用户可以在登录后，使用命令`newgrp`切换到其他用户组，这个命令的参数就是目的用户组。例如：

```shell
# 将当前用户切换到 root 用户组，前提条件是 root 用户组确实是该用户的主组或附加组。
newgrp root
```

#### 5.3 Linux 中与用户账号有关的系统文件

完成用户管理的工作有许多种方法，但是每一种方法实际上都是对有关的系统文件进行修改。

与用户和用户组相关的信息都存放在一些系统文件中，这些文件包括`/etc/passwd`、`/etc/shadow`、`/etc/group`等。

##### 5.3.1 `/etc/passwd`文件

`/etc/passwd`文件是用户管理工作涉及的最重要的一个文件，Linux 系统中的每个用户都在`/etc/passwd`文件中有一个对应的记录行，它记录了这个用户的一些基本属性。

```shell
# 查看文件内容
cat /etc/passwd

# output
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

可以看到，`/etc/passwd`中一行记录对应着一个用户，每行记录又被冒号`:`分隔为 7 个字段，其格式和具体含义如下：

```shell
用户名:口令:用户标识号:组标识号:注释性描述:主目录:登录Shell
```

| 字段 |具体含义| 备注 |
|:----:|----|----|
| 用户名 | 代表用户账号的字符串 | 1. 通常长度不超过8个字符，并且由大小写字母和/或数字组成。登录名中不能有冒号`:`。<br> 2. 为了兼容起见，登录名中最好不要包含点字符`.`，并且不使用连字符`-`和加号`+`打头 |
| 口令 | 存放着加密后的用户口令字段 | 1. 这个字段存放的只是用户口令的加密串，不是明文。 <br> 2. 许多 Linux 系统都使用了 shadow 技术，把真正的加密后的用户口令字存放到`/etc/shadow`文件中，而在`/etc/passwd`文件的口令字段中只存放一个特殊的字符，例如`x`或者`*`。|
| 用户标识号 | 一个整数，系统内部用它来标识用户 | 一般情况下它与用户名是一一对应的。如果几个用户名对应的用户标识号是一样的，系统内部将把它们视为同一个用户，但是它们可以有不同的口令、不同的主目录以及不同的登录 Shell 等。<br> - 通常用户标识号的取值范围是 0～65535。<br> - 0 是超级用户 root 的标识号，1～99 由系统保留，作为管理账号。<br> - 普通用户的标识号从 100 开始。在 Linux 系统中，这个界限是 500。|
| 组标识号 | 一个整数，系统内部用它来标识用户所属的用户组 | 对应着`/etc/group`文件中的一条记录 |
| 注释性描述 | 记录着用户的一些个人情况 | 1. 记录例如用户的真实姓名、电话、地址等，这个字段并没有什么实际的用途。在不同的 Linux 系统中，这个字段的格式并没有统一。<br> 2. 在许多 Linux 系统中，这个字段存放的是一段任意的注释性描述文字，用做 finger 命令的输出 |
| 主目录 | 用户的起始工作目录 | 1. 它是用户在登录到系统之后所处的目录。 <br> 2. 在大多数系统中，各用户的主目录都被组织在同一个特定的目录下，而用户主目录的名称就是该用户的登录名。各用户对自己的主目录有读、写、执行（搜索）权限，其他用户对此目录的访问权限则根据具体情况设置。|
| 登录 Shell | Shell 是用户与 Linux 系统之间的接口 | 1. 用户登录后，要启动一个进程，负责将用户的操作传给内核，这个进程是用户登录到系统后运行的命令解释器或某个特定的程序，即 Shell。 <br> 2. Linux 的 Shell 有许多种，每种都有不同的特点。常用的有 sh(Bourne Shell), csh(C Shell), ksh(Korn Shell), tcsh(TENEX/TOPS-20 type C Shell), bash(Bourne Again Shell)等。 <br> 3. 用户的登录 Shell 也可以指定为某个特定的程序（此程序不是一个命令解释器）。利用这一特点，我们可以限制用户只能运行指定的应用程序，在该应用程序运行结束后，用户就自动退出了系统。有些 Linux 系统要求只有那些在系统中登记了的程序才能出现在这个字段中。 |

附加：
- 系统中有一类用户称为伪用户（psuedo users）。
这些用户在`/etc/passwd`文件中也占有一条记录，但是不能登录，因为它们的登录 Shell 为空。它们的存在主要是方便系统管理，满足相应的系统进程对文件属主的要求。
- 常见的伪用户如下所示：
    > 1. `bin`：拥有可执行的用户命令文件 
    > 2. `sys`：拥有系统文件 
    > 3. `adm`：拥有帐户文件 
    > 4. `uucp`：UUCP使用 
    > 5. `lp`：lp或lpd子系统使用 
    > 6. `nobody`：NFS使用
- 除了上面列出的伪用户外，还有许多标准的伪用户，例如：audit, cron, mail, usenet等，它们也都各自为相关的进程和文件所需要。
<br>


##### 5.3.2 `/etc/shadow`文件
由于`/etc/passwd`文件是所有用户都可读的，如果用户的密码太简单或规律比较明显的话，一台普通的计算机就能够很容易地将它破解，因此对安全性要求较高的 Linux 系统都把加密后的口令字分离出来，单独存放在一个文件中，这个文件是`/etc/shadow`文件。 有超级用户才拥有该文件读权限，这就保证了用户密码的安全性。

`/etc/shadow`中的记录行与`/etc/passwd`中的一一对应，它由`pwconv`命令根据`/etc/passwd`中的数据自动产生，它的文件格式与`/etc/passwd`类似，由若干个字段组成，字段之间用`:`隔开。这些字段是：

```shell
登录名:加密口令:最后一次修改时间:最小时间间隔:最大时间间隔:警告时间:不活动时间:失效时间:标志
```
- **登录名**：与`/etc/passwd`文件中的登录名相一致的用户账号。
- **口令**：存放的是加密后的用户口令字，长度为13个字符。如果为空，则对应用户没有口令，登录时不需要口令；如果含有不属于集合 `{ ./0-9A-Za-z }`中的字符，则对应的用户不能登录。
- **最后一次修改时间**：表示的是从某个时刻起，到用户最后一次修改口令时的天数。时间起点对不同的系统可能不一样。例如在SCO Linux 中，这个时间起点是1970年1月1日。
- **最小时间间隔**：指的是两次修改口令之间所需的最小天数。
- **最大时间间隔**：指的是口令保持有效的最大天数。
- **警告时间**：字段表示的是从系统开始警告用户到用户密码正式失效之间的天数。
- **不活动时间**：表示的是用户没有登录活动但账号仍能保持有效的最大天数。
- **失效时间**：字段给出的是一个绝对的天数，如果使用了这个字段，那么就给出相应账号的生存期。期满后，该账号就不再是一个合法的账号，也就不能再用来登录了。

下面是/etc/shadow的一个例子：
```shell
cat /etc/shadow

# output
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


##### 5.3.3 `/etc/group`文件

用户组的所有信息都存放在`/etc/group`文件中。将用户分组是 Linux 系统中对用户进行管理及控制访问权限的一种手段。每个用户都属于某个用户组；一个组中可以有多个用户，一个用户也可以属于不同的组。

当一个用户同时是多个组中的成员时，在`/etc/passwd`文件中记录的是用户所属的主组，也就是登录时所属的默认组，而其他组称为附加组。

用户要访问属于附加组的文件时，必须首先使用`newgrp`命令使自己成为所要访问的组中的成员。

用户组的所有信息都存放在`/etc/group`文件中。此文件的格式也类似于`/etc/passwd`文件，由冒号`:`隔开若干个字段，这些字段有：

```shell
组名:口令:组标识号:组内用户列表
```
- **组名**：是用户组的名称，由字母或数字构成。与 /etc/passwd 中的登录名一样，组名不应重复。
- **口令**：存放的是用户组加密后的口令字。一般 Linux 系统的用户组都没有口令，即这个字段一般为空，或者是`*`。
- **组标识号**：与用户标识号类似，也是一个整数，被系统内部用来标识组。
- **组内用户列表**： 是属于这个组的所有用户的列表，不同用户之间用逗号`,`分隔。这个用户组可能是用户的主组，也可能是附加组。

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

#### 5.4 批量添加或删除用户
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
    newusers < user.txt 
    ```
    然后可以执行命令`vipw`或 `vi /etc/passwd`检查 /etc/passwd 文件是否已经出现这些用户的数据，并且用户的宿主目录是否已经创建。
<br>

3. 执行命令`/usr/sbin/pwunconv`。
将 /etc/shadow 产生的 shadow 密码解码，然后回写到 /etc/passwd 中，并将 /etc/shadow 的 shadow 密码栏删掉。这是为了方便下一步的密码转换工作，即先取消 shadow password 功能。
    ```
    pwunconv
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
    ```shell
    chpasswd < passwd.txt
    ```

6. 确定密码经编码写入 /etc/passwd 的密码栏后。
执行命令`/usr/sbin/pwconv`将密码编码为 shadow password，并将结果写入 /etc/shadow。
    ```shell
    pwconv
    ```
    这样就完成了大量用户的创建了，之后您可以到 /home 下检查这些用户宿主目录的权限设置是否都正确，并登录验证用户密码是否正确。

---

### Linux 磁盘管理

#### Linux 下的磁盘分区和逻辑卷

##### 硬盘接口类型

硬盘的接口主要有IDE、SATA、SCSI 、SAS和光纤通道等五种类型。

其中IDE和SATA接口硬盘多用于家用产品中，也有部分应用于服务器，SATA是一种新生的硬盘接口类型，已经取代了大部分IDE接口应用。

SCSI 、SAS主要应用于服务器上，普通家用设备一般不支持SCSI和SAS接口。SAS也是是一种新生的硬盘接口类型，可以和SATA以及部分SCSI设备无缝结合。

光纤通道最初设计也不是为了硬盘设计开发的接口，是专门为网络系统设计的，但随着存储系统对速度的需求，才逐渐应用到硬盘系统中，并且其只应用在高端服务器上价格昂贵。

##### 硬盘和分区

Linux 中主要有两种分区类型，分别为 MBR（Master Boot Record）和 GPT（GUID Partition Table），是在磁盘上存储分区信息的两种不同方式。

这些分区信息包含了分区从哪里开始的信息，这样操作系统才知道哪个扇区是属于哪个分区的，以及哪个分区是可以启动的。在磁盘上创建分区时，你必须在 MBR 和 GPT 之间做出选择。

在 Linux 中会把设备映射成为一个 /dev 目录下的系统文件。
- **IDE 接口类型的硬盘设备映射的文件名称前缀为 “hd”**
- **SCSI、SATA、SAS 等接口的硬盘设备映射的文件名称前缀为 “sd”（部分虚拟机或者云主机的名称可能是其他的，比如 “vd”）**
- **后面拼接从 “a” 开始一直到 “z” 用来区分不同的硬盘设备**
- **在硬盘名称后面拼接数字形式的分区号用来区分不同的分区**


1. MBR 分区
> MBR 的意思是“主引导记录”，它是存在于驱动器开始部分的一个特殊的启动扇区。
> 
> 这个扇区包含了已安装的操作系统的启动加载器和驱动器的逻辑分区信息。MBR 支持最大 2TB 磁盘，它无法处理大于2TB容量的磁盘。
> 
> MBR 格式的磁盘分区主要分为基本分区（primary partion）和扩展分区（extension partion）两种主分区。
> 
> 扩展分区下则分为各个逻辑分区。
> 
> 主分区总数不能大于4个，其中最多只能有一个扩展分区。且基本分区可以马上被挂载使用但不能再分区，扩展分区必须再进行二次分区后才能挂载。扩展分区下的二次分区被称之为逻辑分区，逻辑分区数量限制视磁盘类型而定。
> 
> MBR 的主分区号为 1-4，逻辑分区号为从 5 开始累加的数字。

举例：
比如设备主板上装了 4 块硬盘，分别为 2 块 IDE 接口硬盘，一块 SCSI 接口硬盘和一块 SATA 接口硬盘。

其中 2 块 IDE 接口硬盘的分区策略为 2 个主分区和 2 个逻辑分区；

SCSI 分区策略为 3 个主分区和 3 个逻辑分区；

SATA 分区策略为 4 个主分区。

硬盘文件和分区名称如下：

||硬盘|主分区1|主分区2|主分区3|主分区4|逻辑分区1|逻辑分区2|逻辑分区3|...|逻辑分区n|
|----|----|----|----|----|----|----|----|----|----|----|
|IDE1| /dev/hda| /dev/hda1(p)| /dev/hda2(p)| /dev/hda3(e)|	/|	/dev/hda5(l)| /dev/hda6(l)|	/|...| /|
|IDE2| /dev/hdb| /dev/hdb1(p)| /dev/hdb2(p)| /dev/hdb3(e)|	/|	/dev/hdb5(l)| /dev/hdb6(l)|	/|...| /|
|SCSI| /dev/sda| /dev/sda1(p)| /dev/sda2(p)| /dev/sda3(p)| /dev/sda4(e)| /dev/sda5(l)| /dev/sda6(l)| /dev/sda7(l)|...| /|
|SATA| /dev/sdb| /dev/sdb1(p)| /dev/sdb2(p)| /dev/sdb3(p)| /dev/sdb4(p)| /| /| /|...| /|

> 1. 其中分区名称后面的（p）代表基本分区，（e）代表扩展分区，（l）代表逻辑分区。
>
> 2. 需要注意的是，如果分区策略中存在逻辑分区，则说明一定会有扩展分区，那么基本分区数则最多只能有 3 个，扩展分区数最多只能是 1 个，如果没有扩展分区则可以创建 4 个基本分区。
>
> 3. 想要创建逻辑分区，则必须先将唯一的扩展分区创建出来，并且如果删除了扩展分区，那么它下面的所有逻辑分区也会被自动删除。
>
> 4. 如果是 SCSI 接口硬盘则最多只能有 15（其中扩展分区不能直接使用所以不计算）个分区，其中主分区最多 4 个，逻辑分区最多 12 个。
>
> 5. IDE接口硬盘最多只能有 63（其中扩展分区不能直接使用所以不计算）个分区，其中主分区最多 4 个，逻辑分区最多 60 个。

<br>

2. GPT 分区

GPT 意为 GUID 分区表，驱动器上的每个分区都有一个全局唯一的标识符（globally unique identifier，GUID）。支持的最大磁盘可达 18EB。

它没有主分区和逻辑分区之分，每个硬盘最多可以有 128 个分区，具有更强的健壮性与更大的兼容性，并且将逐步取代 MBR 分区方式。

GPT 分区的命名和 MBR 类似，只不过没有主分区、扩展分区和逻辑分区之分，分区号直接从 1 开始累加一直到 128。


##### 逻辑卷

LVM（逻辑卷）的产生是因为传统的分区一旦分区好后就无法在线扩充空间，也存在一些工具能实现在线扩充空间但是还是会面临数据损坏的风险。

传统的分区当分区空间不足时，一般的解决办法是再创建一个更大的分区将原分区卸载然后将数据拷贝到新分区，但是在企业的生产系统往往不允许停机或者允许停机的时间很短，LVM 就能很好的解决在线扩充空间的问题，而且不会对数据造成影响，LVM 还能通过快照在备份的过程中保证日志文件和表空间文件在同一时间点的一致性。

在 LVM 中 PE(Physical Extend) 是卷的最小单位，默认 4M 大小，就像我们的数据是以页的形式存储一样，卷就是以 PE 的形式存储。PV(Physical Volume) 是物理卷，如果要使用逻辑卷，首先第一步操作就是将物理磁盘或者物理分区格式化成PV，格式化之后 PV 就可以为逻辑卷提供 PE 了。VG(Volume Group) 是卷组，VG 就是将很多 PE 组合在一起生成一个卷组，当然这里的 PE 是可以跨磁盘的，如果当前服务器磁盘空间不足就可以增加一个新磁盘对当前系统不会产生任何影响。LV(Logical Volume) 是逻辑卷，逻辑卷最终是给用户使用的，前面几个都是为创建逻辑卷做的准备，创建逻辑卷的大小只要不超过 VG 剩余空间就可以。

##### 文件系统

当硬盘分区被创建完成之后，还并不能直接挂载到目录上存储文件，需要选择合适的文件系统进行格式化。
常见的分区类型有 FAT32、FAT16、NTFS、HP-UX 等，而专供 Linux 使用的主流的一些分区有 ext2/3/4、physical volume (LVM) 、softwareRAID、swap、vfat、xfs 等。其中：

1. ext2/3/4：是适合 Linux 的文件系统类型，由于 ext3 文件系统多了日志记录功能，因此系统恢复起来更加快速，ext4 是 ext3 的升级，效率更加高，因此建议使用默认类型 ext4 类型，而不要使用 ext2/3；

2. physical volume (LVM)：这是一种弹性调整文件系统大小的机制，即可以让文件系统变大或变小，而不改变原文件数据的内容，功能不错，但性能不佳。

3. softwareRAID：利用 Linux 系统的特性，用软件仿真出磁盘阵列功能。

4. swap：就是内存交换空间。由于 swap 并不会使用到目录树的挂载，因此用 swap 就不需要指定挂载点。

5. vfat：同时被 Linux 与 windows 所支持的文件系统类型。如果主机硬盘同事存在 windows 和 linux 两种操作系统，为了进行数据交换，可以使用该文件系统。

6. xfs：也是一个文件系统类型，在 centos7 中将被作为默认的文件系统类型，替换 ext4。


---

#### 磁盘管理
Linux 磁盘管理好坏管理直接关系到整个系统的性能问题。

Linux 磁盘管理常用三个命令为`df`、`du`和`fdisk`。
- `df`：列出文件系统的整体磁盘使用量
- `du`：检查磁盘空间使用量
- `fdisk`：用于磁盘分区 

1.`df`
`df`命令参数功能：**检查文件系统的磁盘空间占用情况**。可以 利用该命令来获取硬盘被占用了多少空间，目前还剩下多少空间等信息。

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
`-p` : 同时有 -A 条件时，同时�����������多个 fsck 的检查一起执行
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






































---
### Linux yum命令
yum（ Yellow dog Updater, Modified ）是一个在 Fedora 和 RedHat 以及 SUSE 中的 Shell 前端软件包管理器。

基于 RPM 包管理，能够从指定的服务器自动下载 RPM 包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软体包，无须繁琐地一次次下载、安装。

yum提供了查找、安装、删除某一个、一组甚至全部软件包的命令，而且命令简洁而又好记。

```shell
yum [options] [command] [package ...]
```
`options`：可选，选项包括`-h`（帮助），`-y`（当安装过程提示选择全部为"yes"），`-q`（不显示安装的过程）等等。
`command`：要进行的操作。
`package`：操作的对象。

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

### Linux 常用命令

#### tar 命令

tar 命令是类 Linux 中比较常用的解压与压缩命令。

```shell
tar [-cxtzjvfpPN] 文件与目录 ...
```

必选参数 —— **一次只能使用其中一个，不能同时使用多个**：
`-c`：建立一个压缩文件的参数指令（create 的意思）；  
`-x`：解开一个压缩文件的参数指令！  
`-t`：查看 tarfile 里面的文件！  
`-r`：向压缩归档文件末尾追加文件  
`-u`：更新原压缩包中的文件 
<br>

可选参数 —— 根据需要在压缩或解压文档时可选的
`-z`：有 gzip 属性，即需要用 gzip 压缩  
`-j`：有 bz2 属性，即需要用 bzip2 压缩  
`-Z`：有 compress 属性的  
`-v`：**压缩的过程中显示文件(显示所有过程)**。(这个常用，但不建议用在背景执行过程!)  
`-O`：将文件解开到标准输出  
`-f`：使用文档名，**请留意，在`f`之后要立即接文档名! 不要再加参数! 切记，这个参数是最后一个参数!**
`-p`：使用原文件的原来属性（属性不会依据使用者而变）  
`-P`：可以使用绝对路径来压缩！  
`-N`：打包文件时间约束，比`N`后面接的日期(yyyy/mm/dd)还要新的才会被打包进新建的文件中! 
`--exclude FILE`：在压缩的过程中，不要将 FILE 打包!
<br>

举例：
```shell

tar -cf all.tar *.jpg      // 这条命令是将所有 .jpg 的文件打成一个名为 all.tar 的包。-c 是表示产生新的包，-f 指定包的文件名。  
tar -rf all.tar *.gif      // 这条命令是将所有 .gif 的文件增加到 all.tar 的包里面去。-r 是表示增加文件的意思。   
tar -uf all.tar logo.gif   // 这条命令是更新原来 tar 包 all.tar 中 logo.gif 文件，-u 是表示更新文件的意思。   
tar -tf all.tar            // 这条命令是列出 all.tar 包中所有文件，-t 是列出文件的意思   
tar -xf all.tar            // 这条命令是解出 all.tar 包中所有文件，-x 是解开的意思  

# 压缩
tar –cvf jpg.tar *.jpg      // 将目录里所有 .jpg 文件打包成 tar.jpg  
tar –czf jpg.tar.gz *.jpg   // 将目录里所有 .jpg 文件打包成 jpg.tar 后，并且将其用 gzip 压缩，生成一个 gzip 压缩过的包，命名为 jpg.tar.gz  
tar –cjf jpg.tar.bz2 *.jpg  // 将目录里所有 .jpg 文件打包成 jpg.tar 后，并且将其用 bzip2 压缩，生成一个 bzip2 压缩过的包，命名为 jpg.tar.bz2  
tar –cZf jpg.tar.Z *.jpg    // 将目录里所有 .jpg 文件打包成 jpg.tar 后，并且将其用 compress 压缩，生成一个 umcompress 压缩过的包，命名为 jpg.tar.Z  

# 解压
tar –xvf file.tar         // 解压 tar 包  
tar -zxvf file.tar.gz     // 解压 tar.gz  
tar -jxvf file.tar.bz2    // 解压 tar.bz2  
tar –Zxvf file.tar.Z      // 解压 tar.Z  

1、*.tar                  用 tar –xvf 解压
2、*.gz                   用 gzip -d 或者 gunzip 解压
3、*.tar.gz 和 *.tgz      用 tar –xzf 解压
4、*.bz2                  用 bzip2 -d 或者用 bunzip2 解压
5、*.tar.bz2              用 tar –xjf 解压
6、*.Z                    用 uncompress 解压
7、*.tar.Z                用 tar –xZf 解压
```
<br>

```shell
范例一：将整个 /etc 目录下的文件全部打包成为 /tmp/etc.tar  
[root@linux ~]# tar -cvf /tmp/etc.tar /etc      <==仅打包，不压缩！  
[root@linux ~]# tar -zcvf /tmp/etc.tar.gz /etc  <==打包后，以 gzip 压缩  
[root@linux ~]# tar -jcvf /tmp/etc.tar.bz2 /etc <==打包后，以 bzip2 压缩  
# 特别注意，在参数 f 之后的文件档名是自己取的，我们习惯上都用 .tar 来作为辨识。  
# 如果加 z 参数，则以 .tar.gz 或 .tgz 来代表 gzip 压缩过的 tar file ～  
# 如果加 j 参数，则以 .tar.bz2 来作为附档名啊～  
# 上述指令在执行的时候，会显示一个警告讯息：  
# 『tar: Removing leading `/' from member names』那是关于绝对路径的特殊设定。


范例二：查阅上述 /tmp/etc.tar.gz 文件内有哪些文件？  
[root@linux ~]# tar -ztvf /tmp/etc.tar.gz  
# 由于我们使用 gzip 压缩，所以要查阅该 tar file 内的文件时，  
# 就得要加上 z 这个参数了！这很重要的！  


范例三：将 /tmp/etc.tar.gz 文件解压缩在 /usr/local/src 底下  
[root@linux ~]# cd /usr/local/src  
[root@linux src]# tar -zxvf /tmp/etc.tar.gz  
# 在预设的情况下，我们可以将压缩档在任何地方解开的！以这个范例来说，  
# 我先将工作目录变换到 /usr/local/src 底下，并且解开 /tmp/etc.tar.gz ，  
# 则解开的目录会在 /usr/local/src/etc 呢！另外，如果您进入 /usr/local/src/etc  
# 则会发现，该目录下的文件属性与 /etc/ 可能会有所不同喔！ 


范例四：在 /tmp 底下，我只想要将 /tmp/etc.tar.gz 内的 etc/passwd 解开而已  
[root@linux ~]# cd /tmp  
[root@linux tmp]# tar -zxvf /tmp/etc.tar.gz etc/passwd  
# 我可以透过 tar -ztvf 来查阅 tarfile 内的文件名称，如果单只要一个文件，  
# 就可以透过这个方式来下达！注意到！ etc.tar.gz 内的根目录 / 是被拿掉了！


范例五：将 /etc/ 内的所有文件备份下来，并且保存其权限！  
[root@linux ~]# tar -zxvpf /tmp/etc.tar.gz /etc  
# 这个 -p 的属性是很重要的，尤其是当您要保留原本文件的属性时！ 


范例六：在 /home 当中，比 2005/06/01 新的文件才备份  
[root@linux ~]# tar -N '2005/06/01' -zcvf home.tar.gz /home  


范例七：我要备份 /home, /etc ，但不要 /home/dmtsai  
[root@linux ~]# tar --exclude /home/dmtsai -zcvf myfile.tar.gz /home/* /etc  


范例八：将 /etc/ 打包后直接解开在 /tmp 底下，而不产生文件！  
[root@linux ~]# cd /tmp  
[root@linux tmp]# tar -cvf - /etc | tar -xvf -  
# 这个动作有点像是 cp -r /etc /tmp 啦～依旧是有其有用途的！  
# 要注意的地方在於输出档变成 - 而输入档也变成 - ，又有一个 | 存在～  
# 这分别代表 standard output, standard input 与管线命令啦！  
# 这部分我们会在 Bash shell 时，再次提到这个指令跟大家再解释啰！ 
```


#### echo 命令

echo 命令用于在终端输出字符串或输出变量提取后的值，格式为`echo [字符串|$变量]`

```shell
# 在终端输出字符 HelloWorld
echo HelloWord

# 提取变量 SHELL 的值，并将其输出到屏幕上
echo $SHELL
# 一般输出 /bin/bash
```

#### ps 命令

ps 命令用于查看系统中的进程状态，格式为`ps [参数]`

参数：
`-a`：显示所有进程（包括其他用户进程）
`-u`：用户以及其他详细信息
`-x`：显示没有控制终端的进程


#### top 命令

top 命令用于动态的监视进程活动与系统负载等信息，相当于“强化版的 Windows 任务管理器”，其格式为`top`



#### pidof 命令

pidof 命令用于查询某个指定服务进程的PID值，格式为`pidof [参数] [服务名称]`


#### kill 命令

kill 命令用于终止某个指定PID的服务进程，格式为`kill [参数] [进程PID]`

#### killall 命令

killall 命令用于终止某个指定名称的服务所对应的全部进程，格式为`killall [参数] [服务名称]`

#### tr 命令

tr 命令用于替换文本文件中的字符，格式为`tr [原始字符] [目标字符]`

一般情况下与cat命令、管道符一起使用，比如先使用 cat 命令读取待处理文本，然后通过管道符把这些文本内容传递给 tr 命令进行替换操作
```
# 将待处理文本中小写字母全部替换为大写字母
cat [文本文件名] | tr [a-z] [A-Z]
```

#### wc 命令

wc 命令用于统计指定文本的行数、字数、字节数、格式为`wc [参数] 文本名`

参数：
`-l`：只显示行数
`-w`：只显示单词数
`-c`：只显示字节数

#### stat 命令

stat 命令用于查看文件的具体存储信息和时间等信息，格式为`stat 文件名称`

#### cut 命令

cut 命令用于按列提取文本字符，格式为`cut [参数] 文本名`

```shell
# 只查询用户名信息，即提取以冒号为间隔符号的第一列内容
# -f 参数设置需要看的列数，-d 参数来设置间隔符号
cut -d: -f1 /etc/passwd
```

#### diff 命令

diff 命令用于比较多个文本文件的差异，格式为`diff [参数] 文件名1 文件名2`

参数
`--bref`：确认两个文件是否不同
`-c`：详细比较多个文件的差异之处

#### file 命令

file 命令用于查看文件的类型，格式为`file 文件名`

#### grep 命令

 grep 命令用于在文本中执行关键词搜索，并显示匹配结果，格式为`grep [参数] [关键词] [文件名]`

 `-n`；用于显示搜索到的信息的行号
 `-v`：用于反选信息，即没有包含关键词的所有信息行

#### find 命令

find 命令用于按照指定条件来查找文件，格式为`find [查找路径] [寻找条件] [操作]`

find 命令可以使用不同的文件特性作为寻找条件（如文件名、大小、修改时间、权限等信息），一旦匹配成功则默认将信息显示到屏幕上

参数：
`-name`：匹配名称
`-perm`：匹配权限（mode为完全匹配，-mode为包含即可）
`-user`：匹配所有者
`group`：匹配所有组
`-mtime -n +n`：匹配修改内容的时间（-n指n填以内，+n指n天以前）
`-atime -n +n`：匹配访问文件的时间（-n指n填以内，+n指n天以前）
`-ctime -n +n`：匹配修改文件权限的时间（-n指n填以内，+n指n天以前）
`-nouser`：匹配无所有者的文件
`-nogroup`：匹配无所有组的文件
`-newer f1 !f2`：匹配比文件f1新但是比f2旧的文件
`--type b/d/c/p/l/f`：匹配文件类型，后面的字母依次表示块设备、目录、字符设备、管道、链接文件、文本文件
`-size`：匹配文件的大小（+50KB为查找超过50KB的文件，而-50KB为查找小于50KB的文件）
`-prune`：忽略某个目录
`-exec ... {} \;`：后面可跟用于进一步处理搜索结果的命令，用于把 find 命令搜索到的结果交给紧随其后的命令做进一步处理

```shell
# 查找 /etc 目录下所有文件名以 host 开头的文件，并打印出来
find /etc -name "host*" -print

# 在整个文件系统中找出所有归属于 linuxprobe 用户的文件，并复制这些文件到 /root/findresults 目录
find / -user linuxprobe -exec cp -a {} /root/findresults/ \;
```


---


### Linux hosts

#### 什么是 hosts

hosts 的全称是 the static table lookup for host name（主机名查询静态表）

hosts 文件是一个用于**储存计算机网络中各节点信息的计算机文件**。**这个文件负责将主机名映射到相应的 IP 地址**。hosts文件通常用于补充或取代网络中 DNS 的功能。和 DNS 不同的是，计算机的用户可以直接对 hosts 文件进行控制。

hosts 是一个没有扩展名的系统文件，其作用就是将一些常用的网址域名或主机名与其对应的 IP 地址建立一个关联“数据库”。

- 当用户在浏览器中输入一个需要登录的网址时，系统会首先自动从 hosts 文件中寻找对应的 IP 地址，一旦找到，系统会立即打开对应网页，如果没有找到，则系统会再将网址提交 DNS 域名解析服务器进行 IP 地址的解析。
  
- 服务优先级 ： dns 缓存 > hosts > dns 服务

> Linux 系统 hosts 位于 /etc/hosts
> Windows 系统 hosts 位于 C:\Windows\System32\drivers\etc\hosts

#### hosts 的内容和作用

##### hosts 的内容

在一个局域网中，每台机器都有一个主机名，用于区分主机，便于相互访问。

一般`/etc/hosts`的内容一般有如下类似内容：
```shell
127.0.0.1  localhost.localdomain   localhost
192.168.1.100   linumu100.com   linumu100
192.168.1.120  ftpserver   ftp120
```

一般情况下 hosts 文件的每行尾一个主机，每行由三部分组成，每个部分由空格隔开。

- 第一部分：网络IP地址；
- 第二部分：主机名或域名；
- 第三部分：主机名别名；

当然每行也可以是两部分，即主机 IP 地址和主机名。


主机名（hostname）和域名（domain）的区别：

1. 主机名通常在局域网内使用，通过 hosts 文件，主机名就被解析到对应IP;
2. 域名通常在 internet 上使用，但如果本机不想使用 internet 上的域名解析，这时就可以更改 hosts 文件，加入自己的域名解析


##### hosts 的作用

hosts 的作用：
1. 加快域名解析
2. 方便局域网用户
   （在很多局域网中，会有服务器提供给用户使用。但由于局域网中一般很少架设 DNS 服务器，访问这些服务器时，要输入难记的 IP 地址。这对不少人来说相当麻烦。可以分别给这些服务器取个容易记住的名字，然后在 Hosts 中建立 IP 映射，这样以后访问的时候，只要输入这个服务器的名字就行了）
3. 屏蔽网站（域名重定向）
4. 顺利连接系统
   （对于 Lotus 的服务器和一些数据库服务器，在访问时如果直接输入 IP 地址那是不能访问的，只能输入服务器名才能访问。那么我们配置好 hosts 文件，这样输入服务器名就能顺利连接了）
5. 虚拟域名
   （很多时候，网站建设者需要把“软环境”搭建好，再进行上传调试。但类似于邮件服务，则需要使用域名来辅助调试，这时就可以将本地 IP 地址与一个“虚拟域名”做地址指向，就可以达到要求的效果，且无需花费）


##### 修改 hosts 后生效方法

Linux：重启网络
```shell
sudo /etc/init.d/networking restart
# desktop 版可以重启服务
sudo service network-manager restart
```

Windows：
```shell
ipconfig/flushdns
```

---


### 管道符、重定向与环境变量

#### 输入输出重定向

- 输入重定向：把文件导入到命令中

- 输出重定向：把原本要输出到屏幕的数据信息写入到指定文件中
  
  + 标准输出重定向：文件描述符为1
    - 清空写入
    - 追加写入
  
  + 错误输出重定向：文件描述符为2
    - 清空写入
    - 追加写入

1. 输入重定向：
`命令 < 文件`：将文件作为命令的标准输入
`命令 << 分界符`：从标准输入中读入，直到遇见分界符才停止
`命令 < 文件1 > 文件2`：将文件1作为命令的标准输入，并将标准输出输出到文件2
<br>

2. 输出重定向：
`命令 > 文件`：将标准输出重定向到一个文件中（清空写入）
`命令 2> 文件`：将错误输出重定向到一个文件中（清空写入）
`命令 >> 文件`：将标准输出重定向到一个文件中（追加写入）
`命令 2>> 文件`：将错误输出重定向到一个文件中（追加写入）
`命令 &>> 文件`或`命令 >> 文件 2>&1`：将标准输出与错误输出共同写入到文件中（追加写入）

```shell
# 修改配置文件
cat >> /etc/yum.repos.d/cm6.repo << EOF
[cm-local]
name=cm6-local
baseurl=http://cdh-port001/cm6
enabled=1
gpgcheck=0
EOF
```

#### 管道命令符

管道符的执行格式为`命令A | 命名B`
管道符的作用：**把一个命令原本要输出到屏幕的标准正常数据当作是后一个命令的标准输入**

```shell
# 统计被限制登录的用户数量
grep "/sbin/nologin" /etc/passwd | wc -l
```

#### 命令行的通配符

通配符就是通用的匹配信息的符号
- `*`：匹配 0 个或多个字符
- `?`：匹配单个字符
- `[]`:匹配[]内范围字符（正则表达）

```shell
# 通配符举例
ls -l /dev/sda*
ls -l /dev/sda?
ls -l /dev[0-9]
ls -l /dev[135]
```

#### 常用的转义字符

4个最常用的转义字符
- `\`：使反斜杠后面的一个变量变为单纯的字符串
- `''`：单引号转义其中所有的变量为单纯的字符串
- `""`：双引号保留其中的变量属性，不进行转义处理
- ````：反引号把其中的命令执行后返回结果

```shell
# 先命名一个变量
PRICE=5
----------------

echo "Price is $PRICE"
# 输出 Price is 5

echo `uname -a`
# 输出本机的 Linux 版本和内核信息

```

#### 重要的环境变量

在 Linux 系统中，变量的名称一般都是大写的，这是一种约定俗成的规范

命令在 Linux 系统中执行分为 4 个步骤：
1. 判断用户是否以绝对路径或相对路径的方式输入命令（如`/bin/ls`），如果是的话则直接执行
2. Linux 系统检查用户输入的命令是否为“别名命令”，可以使用`alias 别名=命令`创建一个属于自己的命令别名，也可以使用`unalias 别名`取消一个命令别名
3. Bash 解释器判断用户输入的是内部命令（解释器内部的指令）还是外部命令，可使用`type 命令名称`判断命令为内部命令还是外部命令
4. 系统在多个路径中查找用户输入的命令文件，而定义这些路径的变量就叫做PATH，PATH是由多个路径值组成的变量，每个路径值之间用冒号间隔，可使用`echo $PATH`查看PATH的路径

Linux 系统中最重要的 10 个环境变量
|变量名称|作用|
|----|----|
|HOME| 用户的主目录（家目录）|
|SHELL| 用户在使用的 Shell 解释器名称|
|HISTSIZE| 输出的历史命令记录条数|
|HISTFILESIZE| 保存的历史命令记录条数|
|MAIL| 邮件保存路径|
|LANG| 系统语言、语系名称|
|RANDOM| 生成一个随机数字|
|PS1| Bash 解释器的提示符|
|PATH| 定义解释器搜索用户执行命令的路径|
|EDITOR| 用户默认的文本编辑器|


---


### Shell



---


### ssh 服务管理远程主机

#### 配置网络服务

配置网络参数：使用`nmtui`命令来配置网络

- 输入`nmtui`命令进行 NetworkManager TUI 界面，选择 Edit a connection 进入到网卡编辑界面，界面显示已存在的网卡及新增、编辑、删除等操作
- 选中要编辑的网卡，然后按下编辑按钮进入配置修改界面修改 IP 地址等参数

创建网络会话：使用`nmcli`命令来管理 NetworkManager 服务

- 查看网络信息与网络状态 
![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/linux_contents_3.png)

- 创建网络会话（只需在不同的）

### Linux 远程登录

远程登录 Linux 服务器来管理维护系统。CentOS系统默认安装了openssh

如果没有安装可以使用命令进行安装：
```shell
yum install openssh-server -y 
```

Linux 系统中是通过 ssh 服务实现的远程登录功能，默认 ssh 服务端口号为 22。

Window 系统上 Linux 远程登录客户端有 SecureCRT, Putty, SSH Secure Shell, Xshell 等。

---























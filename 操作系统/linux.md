## 什么是用户和组，他们两者有什么关系
Linux内核（2.6.x）已经可以支持到2^32 - 1 个标识符。

- 用户分类
  - 普通用户： 1 - 65535，拥有对系统的最高管理权限
  - 系统用户： 1-499（CentOS 6） ;  1-999（CentOS 7）
  - 登录用户： 500 - 60000（CentOS 6） ； 1000 - 60000 （CentOS 7），只能对自己目录下的文件进行访问和修改，具有登录系统的权限
  
### Q: What's the difference between a normal user and a system user?

A: When you are creating an account to run a daemon, service, or other system software, rather than an account for interactive use.

Technically, it makes no difference, but in the real world it turns out there are long term benefits in keeping user and software accounts in separate parts of the numeric space.

- 用户组分类
  - 普通用户组:可以加入多个用户
  - 系统组:一般加入一些系统用户
  - 私有组(也称基本组):当创建用户时,如果没有为其指明所属组,则就为其定义一个私有的用户组,起名称与用户名同名.
注:私有组可以变成普通用户组,当把其他用户加入到该组中,则其就变成了普通组

## 什么是文件权限，如何修改文件权限？

ls

 -l中显示的内容如下：

-rwxrw-r‐-1 root root 1213 Feb 2 09:39 abc

- 10个字符确定不同用户能对文件干什么

- 第一个字符代表文件（-）、目录（d），链接（l）

- 其余字符每3个一组（rwx），读（r）、写（w）、执行（x）

- 第一组rwx：文件所有者的权限是读、写和执行

- 第二组rw-：与文件所有者同一组的用户的权限是读、写但不能执行

- 第三组r--：不与文件所有者同组的其他用户的权限是读不能写和执行

也可用数字表示为：r=4，w=2，x=1  因此rwx=4+2+1=7

- 1 表示连接的文件数

- root 表示用户

- root表示用户所在的组

- 1213 表示文件大小（字节）

- Feb 2 09:39 表示最后修改日期

- abc 表示文件名

 

改变权限的命令

chmod 改变文件或目录的权限

chmod 755 abc：赋予abc权限rwxr-xr-x

chmod u=rwx，g=rx，o=rx abc：同上u=用户权限，g=组权限，o=不同组其他用户权限

chmod u-x，g+w abc：给abc去除用户执行的权限，增加组写的权限

chmod a+r abc：给所有用户添加读的权限

## 什么是shell

Shell 是一个用 C 语言编写的程序，它是用户使用 Linux 的桥梁。Shell 既是一种命令语言，又是一种程序设计语言。

Shell 是指一种应用程序，这个应用程序提供了一个界面，用户通过这个界面访问操作系统内核的服务。

## shell下怎样查看一个环境变量

-bash-3.00# env

如果只想看指定的变量设置，如路径PATH的设置，可以用 "echo $PATH"或 “ env | grep PATH"或” env | grep -i path"来查询。前面的适合知道全名的，后面2种适合只知道部分字段或者部分关键字母（甚至不确定字符大小写）的。

## 如何查看文件内容

- cat 由第一行开始显示内容，并将所有内容输出
 
- tac 从最后一行倒序显示内容，并将所有内容输出
 
- more 根据窗口大小，一页一页的现实文件内容
 
- less 和more类似，但其优点可以往前翻页，而且进行可以搜索字符
 
- head 只显示头几行
 
- tail 只显示最后几行

## 阐述sudo，su指令

通过su可以在用户之间切换，如果超级权限用户root向普通或虚拟用户切换不需要密码，而普通用户切换到其它任何用户都需要密码验证。

由于 su 对切换到超级权限用户root后，权限的无限制性，所以su并不能担任多个管理员所管理的系统。如果用su 来切换到超级用户来管理系统，也不能明确哪些工作是由哪个管理员进行的操作。特别是对于服务器的管理有多人参与管理时，最好是针对每个管理员的技术特长和管理范围，并且有针对性的下放给权限，并且约定其使用哪些工具来完成与其相关的工作，这时我们就有必要用到 sudo。

通过sudo，我们能把某些超级权限有针对性的下放，并且不需要普通用户知道root密码，所以sudo 相对于权限无限制性的su来说，还是比较安全的，所以sudo 也能被称为受限制的su ；另外sudo 是需要授权许可的，所以也被称为授权许可的su；

sudo 执行命令的流程是当前用户切换到root（或其它指定切换到的用户），然后以root（或其它指定的切换到的用户）身份执行命令，执行完成后，直接退回到当前用户；而这些的前提是要通过sudo的配置文件/etc/sudoers来进行授权。

## 怎样复制和移动文件，怎样重命名一个文件
### 重命名
使用mv命令就可以了。
例：要把名为：abc 重命名为：123  
mv abc 123
mv命令既可以重命名，又可以移动文件或文件夹。

### 拷贝复制
将文件file1复制成文件file2
cp file1 file2

- 参数说明：
  - -f 或 --force　　　　 强行复制文件或目录， 不论目的文件或目录是否已经存在
  - -p 或 --preserve      保留源文件或目录的属性，包括所有者、所属组、权限与时间
  - -P 或 --parents        保留源文件或目录的路径，此路径可以是绝对路径或相对路径，且目的目录必须已经存在
  - -R 或 --recursive  　 递归处理，将指定目录下的文件及子目录一并处理
  - -s 或 --symbolic-link  对源文件建立符号链接，而非复制文件
  
### 移动
命令格式：mv [-fiv] source destination
- 参数说明：
  - -f:force，强制直接移动而不询问
  - -i:若目标文件(destination)已经存在，就会询问是否覆盖
  - -u:若目标文件已经存在，且源文件比较新，才会更新

## 怎样从一个目录下所有文件中搜索关键字
### 在某目录下查找名为“elm.cc”的文件

find /home/lijiajia/ -name elm.cc

### 查找文件名中包含某字符（如"elm"）的文件

find /home/lijiajia/ -name '*elm*'
find /home/lijiajia/ -name 'elm*'
find /home/lijiajia/ -name '*elm'

### 根据文件的特征进行查询

find /home/lijiajia/ -amin -10        #查找在系统中最后10分钟访问的文件  
find /home/lijiajia/ -atime -2        #查找在系统中最后48小时访问的文件  
find /home/lijiajia/ -empty           #查找在系统中为空的文件或者文件夹  
find /home/lijiajia/ -mmin -5         # 查找在系统中最后5 分钟里修改过的文件
find /home/lijiajia/ -mtime -1        #查找在系统中最后24 小时里修改过的文件  
find /home/lijiajia/ -nouser          #查找在系统中属于作废用户的文件  
find /home/ftp/pub -user lijiajia     #查找在系统中属于lijiajia这个用户的文件  

## 怎样安装软件

通常Linux应用软件的安装包有三种：
- tar包，如software-1.2.3-1.tar.gz。它是使用UNIX系统的打包工具tar打包的。
- rpm包，如software-1.2.3-1.i386.rpm。它是RedHat Linux提供的一种包封装格式。
- dpkg包，如software-1.2.3-1.deb。它是Debain Linux提供的一种包封装格式。

整个安装过程可以分为以下几步：
- 取得应用软件：通过下载、购买光盘的方法获得；
- 解压缩文件：一般tar包，都会再做一次压缩，如gzip、bz2等，所以你需要先解压。如果是最常见的gz格式，则可以执行：“tar –xvzf 软件包名”，就可以一步完成解压与解包工作。如果不是，则先用解压软件，再执行“tar –xvf 解压后的tar包”进行解包；
- 阅读附带的INSTALL文件、README文件；
- 执行“./configure”命令为编译做好准备；
- 执行“make”命令进行软件编译；
- 执行“make install”完成安装；
- 执行“make clean”删除安装时产生的临时文件。

### 使用 rpm 安装
rpm -ivh your-package.rpm
ivh：安装显示安装进度--install--verbose--hash

常用命令组合：
－ivh：安装显示安装进度--install--verbose--hash
－Uvh：升级软件包--Update；
－qpl： 列出RPM软件包内的文件信息[Query Package list]；
－qpi：列出RPM软件包的描述信息[Query Package install package(s)]；
－qf：查找指定文件属于哪个RPM软件包[Query File]；
－Va：校验所有的 RPM软件包，查找丢失的文件[View Lost]；
－e：删除包

## 什么是链接，什么是符号链接，什么是软连接
### 硬链接
硬链接指通过索引节点来进行连接，在Linux为文件系统中，保存在磁盘分区中的文件不管是什么类型都给它分配一个编号，称为索引节点号；
硬链接指的就是在linux中，多个文件名指向同一索引节点；
常见用途：通过建立硬链接到重要文件，防止误删，删除其实对应的是删除其中的一个硬链接，当文件对应的硬链接都被删除了，该文件才真正被删除。

### 软链接
也称为符号链接（Symbolic Link），类似于Windows的快捷方式，其中包含的是另一个文件的位置信息。

## 什么是make
在 Linux环境下使用 GNU 的 make工具能够比较容易的构建一个属于你自己的工程，整个工程的编译只需要一个命令就可以完成编译、连接以至于最后的执行。不过这需要我们投入一些时间去完成一个或者多个称之为 Makefile 文件的编写。此文件正是 make 正常工作的基础。

make 是一个命令工具，它解释 Makefile 中的指令（应该说是规则）。在 Makefile文件中描述了整个工程所有文件的编译顺序、编译规则。

编译：把高级语言所书写的代码转换成机器可识别的指令，此时还不能够被执行，编译器通过检查高级语言的语法，函数和变量的声明是否正确！如果正确则产生中间目标文件（目标文件在Liunx中默认后缀为“.o”）

链接：将多.o 文件，或者.o 文件和库文件链接成为可被操作系统执行的可执行程序

静态库：又称为文档文件（Archive File） 。它是多个.o文件的集合。Linux中静态库文件的后缀为“.a”

共享库：也是多个.o 文件的集合，但是这些.o 文件时有编译器按照一种特殊的方式生成（共享库已经具备了可执行条件）

在执行 make  之前，需要一个命名为 Makefile  的特殊文件（本文的后续将使用Makefile 作为这个特殊文件的文件名）来告诉 make 需要做什么（完成什么任务），该怎么做。

    当使用make 工具进行编译时，工程中以下几种文件在执行make 时将会被编译（重新编译）：

1.所有的源文件没有被编译过，则对各个 C 源文件进行编译并进行链接，生成最后的可执行程序；

2.每一个在上次执行 make 之后修改过的 C 源代码文件在本次执行make 时将会被重新编译；

3.头文件在上一次执行make 之后被修改。则所有包含此头文件的 C 源文件在本次执make 时将会被重新编译。

## 什么是FIFO
无名管道应用的一个重大限制是它没有名字，因此，只能用于具有亲缘关系的进程间通信，在有名管道（named pipe或FIFO）提出后，该限制得到了克服。FIFO不同于管道之处在于它提供一个路径名与之关联，以FIFO的文件形式存在于文件系统中。这样，即使与FIFO的创建进程不存在亲缘关系的进程，只要可以访问该路径，就能够彼此通过FIFO相互通信（能够访问该路径的进程以及FIFO的创建进程之间），因此，通过FIFO不相关的进程也能交换数据。值得注意的是，FIFO严格遵循先进先出（first in first out），对管道及FIFO的读总是从开始处返回数据，对它们的写则把数据添加到末尾。它们不支持诸如lseek()等文件定位操作。
管道的缓冲区是有限的（管道制存在于内存中，在管道创建时，为缓冲区分配一个页面大小）
管道所传送的是无格式字节流，这就要求管道的读出方和写入方必须事先约定好数据的格式，比如多少字节算作一个消息（或命令、或记录）等等

FIFO往往都是多个写进程，一个读进程。



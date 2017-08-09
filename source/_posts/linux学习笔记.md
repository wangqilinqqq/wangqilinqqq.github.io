---
title: Linux学习笔记
date: 2017-07-17 15:16:46
categories: Linux
tags: Linux
---

**Linux简介**

相对于现在的 Windows 系统，UNIX/Linux 本身是没有图形界面的，我们通常在 UNIX/Linux 发行版上看到的图形界面实际都只是运行在 Linux 系统之上的一套软件，类似 Windows95 之前的 Windows 的图形界面实则也只是运行在 DOS 环境的一套软件。<!--more-->而 Linux 上的这套软件以前是 XFree86，现在则是 xorg（X.Org），而这套软件又是通过 X 窗口系统（X Window System，也常被称为 X11 或 X）实现的，X 本身只是工具包及架构协议，而 xorg 便是 X 架构规范的一个实现体，也就是说它是实现了 X 协议规范的一个提供图形界面服务的服务器，就像实现了 http 协议提供 web 服务的 Apache 。如果只有服务器也是不能实现一个完整的桌面环境的，当然还需要一个客户端，我们称为 X Client，像如下几个大家熟知也最流行的实现了客户端功能的桌面环境 KDE，GNOME，XFCE，LXDE 。其中就有你看到的，实验楼目前使用的 XFCE 桌面环境，部分老用户可能可以回想起，实验楼之前使用的环境是 LXDE 。这也意味着在 Linux 上你可以自己选择安装不同的桌面环境，甚至可以定制自己的专属桌面。

**笔记开始**

## 快捷键

>Ctrl+d ---- 键盘输入结束或退出终端
Ctrl+s ---- 暂停当前程序，暂停后按下任意键恢复运行
Ctrl+z ---- 将当前程序放到后台运行，恢复到前台为命令fg
Ctrl+a ---- 将光标移至输入行头，相当于Home键
Ctrl+e ---- 将光标移至输入行末，相当于End键
Ctrl+k ---- 删除从光标所在位置到行末
Alt+Backspace ---- 向前删除一个单词
Shift+PgUp ---- 将终端显示向上滚动
Shift+PgDn ---- 将终端显示向下滚动



## 通配符

>\* ---- 匹配 0 或多个字符
? ---- 匹配任意一个字符
[list] ---- 匹配 list 中的任意单一字符
[!list] ---- 匹配 除list 中的任意单一字符以外的字符
[c1-c2] ---- 匹配 c1-c2 中的任意单一字符 如：[0-9] [a-z]
{string1,string2,...} ---- 匹配 string1 或 string2 (或更多)其一字符串
{c1..c2} ---- 匹配 c1-c2 中全部字符 如{1..10}

## 用户及文件权限管理

### 查看用户

``` bash
$ who am i
```


who 命令其它常用参数

>参数 ---- 说明
-a ---- 打印能打印的全部
-d ---- 打印死掉的进程
-m ---- 同am i,mom likes
-q ---- 打印当前登录用户数及用户名
-u ---- 打印当前登录用户登录信息
-r ---- 打印运行等级

### 创建用户

sudo su su-  介绍

` su <user `可以切换到用户 user，执行时需要输入目标用户的密码，` sudo <cmd `可以以特权级别运行 cmd 命令，需要当前用户属于 sudo 组，且需要输入当前用户的密码。` su - <user `命令也是切换用户，同时环境变量也会跟着改变成目标用户的环境变量。

**注:所有使用 ` sudo `和` su ` 命令的用户都必须在 sudo用户组才能正常使用sudo命令**

新建用户

```bash
$ sudo adduser <user name>
```

切换用户

```bash
$ su -l <user name>
```

查看用户组

```bash
$ groups <user name>
```

将其他用户加入到sudo组

```bash
$ sudo usermod -G sudo <user name>
```

**只有在sudo组内的用户才有权限拉人**

删除用户

```bash
$ sudo deluser <user name> --remove-home
```


### 文件权限

#### 查看文件

查看文件的详细信息

```bash
$ ls -l 
```

输出示例

```bash
	drwxrwxrwx 2 shiyanlou shiyanlou 4096 7月 17 14:00 1.txt
	# 文件类型和权限 链接数 所有者 所属组 文件大小 最后修改日期 文件名
```

文件类型和权限详解

```bash
	drwxrwxrwx  
```

可分解为四组 
第一个字母为一组 
第二至第四为一组 
第五至第七位一组 
第八至第十位一组	

第一组:
'd'表示目录
'l'表示软链接
'b'表示块设备
'c'表示字符设备
's'socket
'p'管道
'-'普通文件

第二组:表示的是所有者权限
第三组:表示用户组权限
第四组:表示其他用户权限

r-代表读权限
w-代表写权限
x-代表执行权限



#### 修改文件所有者

```bash
$ sudo chown <user> <file name>
```

**注:所有使用 sudo 指令的用户都必须在sudo组才能使用 sudo 指令**

#### 修改文件权限

```bash
$ chmod 700 <file name>
```

一个完整rwx 为 7

只可执行 1
只可写 2
可执行可写 3
只可读 4
可执行可读 5
可写可读 6
可写可读可执行 7	

## Linux 目录结构及文件基本操作

### Linux 目录结构

#### FHS 标准

**如果觉得图片不清晰，建议另存为到本地放大查看：**

![Alt text](/images/linux/logoblackfont.png "FHS 标准")

#### 绝对路径和相对路径

这个不需要记,记几个linux命令即可

>打印当前绝对路径

```bash
$ pwd
```

>输出隐藏文件

```bash
$ ls -a
```

>进入home目录

```bash
$ ~
```

### linux文件的基本操作

#### 新建

>创建文件

```bash
$ touch <file name>
```

>新建目录

```bash
$ mkdir  <catalogue name>
```

>带 ` -p ` 参数可同时创建父目录

```bash
$ mkdir -p father/son/grandson
```

#### 复制

>>复制文件
	```bash
	$ cp <file name> <path>
	```

>>复制目录
	```bash
	# 需要加上 -r 或者 -R 参数，表示递归复制，注：复制 father目录 到 family目录下
	$ cp -r father family
	```

#### 删除

```bash
$ rm <file name>
```

>强制删除

```bash
$ rm -f <file name>
```

>删除目录

	```bash
	# 跟复制目录一样，要删除一个目录，也需要加上 -r 或 -R 参数：
	$ rm -r <catalogue name>
	
	# 当目录为非空的时候 -r 参数就很苦恼了 使用 -rf 可快速删除
	# rm -rf <catalogue name>
	```


#### 移动与重命名

>移动文件

```bash
$ mv <源目录文件> <目的目录>
```

>文件重命名

```bash
$ mv <旧的文件名> <新的文件名>
```

>批量重命名

```bash
# 使用通配符批量创建 5 个文件:
$ touch file{1..5}.txt

# 批量将这 5 个后缀为 .txt 的文本文件重命名为以 .c 为后缀的文件:
$ rename 's/\.txt/\.c/' *.txt

# 批量将这 5 个文件，文件名改为大写:
$ rename 'y/a-z/A-Z/' *.c
```

#### 查看文件

```bash
$ cat <file name>
```

>查看并显示行号

```bash
$ cat -n <file name>
```

> ` nl ` 命令。添加行号并打印

``` bash
# -b : 指定添加行号的方式，主要有两种：
#     -b a:表示无论是否为空行，同样列出行号("cat -n"就是这种方式)
#     -b t:只列出非空行的编号并列出（默认为这种方式）
# -n : 设置行号的样式，主要有三种：
#     -n ln:在行号字段最左端显示
#     -n rn:在行号字段最右边显示，且不加 0
#     -n rz:在行号字段最右边显示，且加 0
# -w : 行号字段占用的位数(默认为 6 位)

$ nl -b -a <file name>
```

>使用 `more` 查看文件

```bash
$ more <file name>
# 打开后默认只显示一屏内容，终端底部显示当前阅读的进度。
# 可以使用 Enter 键向下滚动一行，使用 Space 键向下滚动一屏，
# 按下 h 显示帮助，q 退出。
```

>使用 `head` 和 `tail` 命令查看文件

```bash
# 只查看文件的头几行和尾几行
$ tail <file name>

# 甚至更直接的只看一行， 加上 -n 参数，后面紧跟行数：
$ tail -n 1 <file name>

# 关于 tail 命令，不得不提的还有它一个很牛的参数 -f，
# 这个参数可以实现不停地读取某个文件的内容并显示。
# 这可以让我们动态查看日志，达到实时监视的目的。

```

#### 编辑文件

```bash
$ vim <file name>
```

## 环境变量与文件查找

### 变量

>创建变量

```bash
$ declare tmp
```

>赋值变量

```bash
$ tmp=shiyanlou
```

>读取变量

```bash
$ echo $tmp
```

**可以既用既创建， ` declare ` 在声明其他类型的变量时会用到。读取变量 ` $ ` 符号不要忘记， ` $ ` 表示引用一个变量**

**注意：并不是任何形式的变量名都是可用的，变量名只能是英文字母、数字或者下划线，且不能以数字作为开头。**

### 环境变量

通常我们会涉及到的变量类型有三种：

*	当前 Shell 进程私有用户自定义变量，如上面我们创建的 tmp 变量，只在当前 Shell 中有效。
*	Shell 本身内建的变量。
*	从自定义变量导出的环境变量。

>也有三个与上述三种环境变量相关的命令：set，env，export。这三个命令很相似，都是用于打印环境变量信息，区别在于涉及的变量范围不同。详见下表：

<table><tr><th>命令</th><th>说明</th></tr><tr><td>set</td><td>显示当前 Shell 所有变量，包括其内建环境变量（与 Shell 外观等相关），用户自定义变量及导出的环境变量。</td></tr><tr><td>env</td><td>显示与当前用户相关的环境变量，还可以让命令在指定环境中运行。</td></tr><tr><td>export</td><td>显示从 Shell 中导出成环境变量的变量，也能通过它将自定义变量导出为环境变量。</td></tr></table>

>关于哪些变量是环境变量，可以简单地理解成在当前进程的子进程有效则为环境变量，否则不是（有些人也将所有变量统称为环境变量，只是以全局环境变量和局部环境变量进行区分，我们只要理解它们的实质区别即可）。我们这里用 export 命令来体会一下，先在 Shell 中设置一个变量 temp=shiyanlou，然后再新创建一个子 Shell 查看 temp 变量的值：

![Alt text](/images/linux/logoblackfont_1.png "")

**永久生效**

但是问题来了，当你关机后，或者关闭当前的 shell 之后，环境变量就没了啊。怎么才能让环境变量永久生效呢？

按变量的生存周期来划分，Linux 变量可分为两类：

*	永久的：需要修改配置文件，变量永久生效；

*	临时的：使用 export 命令行声明即可，变量在关闭 shell 时失效。

这里介绍两个重要文件 /etc/bashrc（有的 Linux 没有这个文件） 和 /etc/profile ，它们分别存放的是 shell 变量和环境变量。还有要注意区别的是每个用户目录下的一个隐藏文件：

这个 .profile 只对当前用户永久生效。而写在 /etc/profile 里面的是对所有用户永久生效，所以如果想要添加一个永久生效的环境变量，只需要打开 /etc/profile，在最后加上你想添加的环境变量就好啦。

### 命令的查找路径与顺序

你可能很早之前就有疑问，我们在 Shell 中输入一个命令，Shell 是怎么知道去哪找到这个命令然后执行的呢？这是通过环境变量 PATH 来进行搜索的，熟悉 Windows 的用户可能知道 Windows 中的也是有这么一个 PATH 环境变量。这个 PATH 里面就保存了 Shell 中执行的命令的搜索路径

>查看 ` PATH `变量

```bash
$ echo $PATH
```

>下面我们将练习创建一个最简单的可执行 Shell 脚本和一个使用 C 语言创建的“ hello world ”程序，如果这两部分内容你之前没有学习过，那么你可以进行一个入门学习：

```bash
# gedit 是linux里面的一个文本编辑器
$ gedit hello_shell.sh
```

在脚本中添加如下内容，保存并退出（注意不要省掉第一行，这不是注释，论坛有用户反映有语法错误，就是因为没有了第一行）：

```bash
#!/bin/bash

for ((i=0; i<10; i++));do
    echo "hello shell"
done

exit 0
```

为文件添加可执行权限：

```bash
$ chmod 755 hello_shell.sh
```

执行脚本

```bash
$ ./hello_shell.sh
```

创建一个 C 语言“ hello world ”程序：

```bash
$ gedit hello_world.c
```

编辑脚本

```bash
#include <stdio.h>

int main(void)
{
    printf("hello world!\n");
    return 0;
}
```

保存后使用 gcc 生成可执行文件：

```bash
$ gcc -o hello_world hello_world.c
```

>gcc 生成二进制文件默认具有可执行权限，不需要修改

在 shiyanlou 家目录创建一个 mybin 目录，并将上述 hello_shell.sh 和 hello_world 文件移动到其中：

```bash
$ mkdir mybin
$ mv hello_shell.sh hello_world mybin/
```

现在你可以在 mybin 目录中分别运行你刚刚创建的两个程序：

```bash
$ cd mybin
$ ./hello_shell.sh
$ ./hello_world
```

回到上一级目录，也就是 shiyanlou 家目录，当再想运行那两个程序时，会发现提示命令找不到，除非加上命令的完整路径，但那样很不方便，如何做到想使用系统命令一样执行自己创建的脚本文件或者程序呢？那就要将命令所在路径添加到 PATH 环境变量了。

### 添加自定义路径到 "PATH" 环境变量

在前面我们应该注意到 PATH 里面的路径是以 : 作为分割符的，所以我们可以这样添加自定义路径：

```bash
$ PATH=$PATH:/home/shiyanlou/mybin
```

**注意这里一定要使用绝对路径。**

现在你就可以在任意目录执行那两个命令了（注意需要去掉前面的 ./）。你可能会意识到这样还并没有很好的解决问题，因为我给 PATH 环境变量追加了一个路径，它也只是在当前 Shell 有效，我一旦退出终端，再打开就会发现又失效了。有没有方法让添加的环境变量全局有效？或者每次启动 Shell 时自动执行上面添加自定义路径到 PATH 的命令？下面我们就来说说后一种方式——让它自动执行。

在每个用户的 home 目录中有一个 Shell 每次启动时会默认执行一个配置脚本，以初始化环境，包括添加一些用户自定义环境变量等等。zsh 的配置文件是 .zshrc，相应 Bash 的配置文件为 .bashrc 。它们在 etc 下还都有一个或多个全局的配置文件，不过我们一般只修改用户目录下的配置文件。

我们可以简单地使用下面命令直接添加内容到 .zshrc 中：

```bash
$ echo "PATH=$PATH:/home/shiyanlou/mybin" >> .zshrc
```

**上述命令中` >>` 表示将标准输出以追加的方式重定向到一个文件中，注意前面用到的 `>` 是以覆盖的方式重定向到一个文件中，使用的时候一定要注意分辨。在指定文件不存在的情况下都会创建新的文件。**

### 修改和删除已有变量

#### 变量修改

变量的修改有以下几种方式：


<table><tr><th>变量设置方式</th><th>说明</th></tr><tr><td>`${变量名#匹配字串}`</td><td>从头向后开始匹配，删除符合匹配字串的最短数据</td></tr><tr><td>`${变量名##匹配字串}`</td><td>从头向后开始匹配，删除符合匹配字串的最长数据</td></tr><tr><td>`${变量名%匹配字串}`</td><td>从尾向前开始匹配，删除符合匹配字串的最短数据</td></tr><tr><td>`${变量名%%匹配字串}`</td><td>从尾向前开始匹配，删除符合匹配字串的最长数据</td></tr><tr><td>`${变量名/旧的字串/新的字串}`</td><td>将符合旧字串的第一个字串替换为新的字串</td></tr><tr><td>`${变量名//旧的字串/新的字串}`</td><td>将符合旧字串的全部字串替换为新的字串</td></tr></table>

比如修改自定义变量

```bash
# 创建变量
$ w=wangqilin
# 输出变量
$ echo $w
# 匹配字母w替换为大写W
$ w=${w/w/W/}
```

#### 变量删除

可以使用 unset 命令删除一个环境变量：

```bash
$ unset temp
```

### 如何让环境变量立即生效

前面我们在 Shell 中修改了一个配置脚本文件之后（比如 zsh 的配置文件 home 目录下的 .zshrc），每次都要退出终端重新打开甚至重启主机之后其才能生效，很是麻烦，我们可以使用 source 命令来让其立即生效，如：

```bash
$ source .zshrc
```

### 搜索文件

* whereis 简单快速

```bash
$ whereis who
```

whereis 只能搜索二进制文件(-b)，man 帮助文件(-m)和源代码文件(-s)。

*	locate 快而全

```bash
# 寻找etc目录下sh开头的文件
$ locate /etc/sh

# 使用通配符寻找
$ locate /usr/share/\*.jpg
```

**如果想只统计数目可以加上 `-c` 参数，`-i` 参数可以忽略大小写进行查找，`whereis` 的 `-b`、`-m`、 `-m`同样可以使用。**

*	which 小而精

`which` 本身是 `Shell` 内建的一个命令，我们通常使用 `which` 来确定是否安装了某个指定的软件，因为它只从 `PATH` 环境变量指定的路径中去搜索命令：

```bash
$ which man
```

*	find 精而细
find 应该是这几个命令中最强大的了，它不但可以通过文件类型、文件名进行查找而且可以根据文件的属性（如文件的时间戳，文件的权限等）进行搜索。find 命令强大到，要把它讲明白至少需要单独好几节课程才行，我们这里只介绍一些常用的内容。

这条命令表示去 /etc/ 目录下面 ，搜索名字叫做 interfaces 的文件或者目录。这是 find 命令最常见的格式，千万记住 find 的第一个参数是要搜索的地方：

```bash
$ sudo find /etc/ -name interfaces
```

>**注意 find 命令的路径是作为第一个参数的， 基本命令格式为 find [path] [option] [action] 。**

与时间相关的命令参数：

<table><tr><th>参数</th><th>说明</th></tr><tr><td>`-atime`</td><td>最后访问时间</td></tr><tr><td>`-ctime`</td><td>最后修改文件内容的时间</td></tr><tr><td>`-mtime`</td><td>最后修改文件属性的时间</td></tr></table>

下面以 -mtime 参数举例：

*	-mtime n：n 为数字，表示为在 n 天之前的“一天之内”修改过的文件
*	-mtime +n：列出在 n 天之前（不包含 n 天本身）被修改过的文件
*	-mtime -n：列出在 n 天之内（包含 n 天本身）被修改过的文件
*	-newer file：file 为一个已存在的文件，列出比 file 还要新的文件名

![Alt text](/images/linux/5-8.png)

列出 home 目录中，当天（24 小时之内）有改动的文件：

```bash
$ find ~ -mtime 0
```

列出用户家目录下比 Code 文件夹新的文件

```bash
$ find ~ -newer /home/shiyanlou/Code
```

列出在三天前做过改动的文件

```bash
$ find ~ -mtime +3
```

## 文件打包与压缩

### 概念讲解

在讲 Linux 上的压缩工具之前，有必要先了解一下常见常用的压缩包文件格式。在 Windows 上最常见的不外乎这三种 *.zip，*.rar，*.7z 后缀的压缩文件，而在 Linux 上面常见常用的除了以上三种外，还有 *.gz，*.xz，*.bz2，*.tar，*.tar.gz，*.tar.xz，*.tar.bz2，简单介绍如下：

<table><tr><th>文件后缀名</th><th>说明</th></tr><tr><td>`*.zip`</td><td>zip 程序打包压缩的文件</td></tr><tr><td>`*.rar`</td><td>rar 程序压缩的文件</td></tr><tr><td>`*.7z`</td><td>7zip 程序压缩的文件</td></tr><tr><td>`*.tar`</td><td>tar 程序打包，未压缩的文件</td></tr><tr><td>`*.gz`</td><td>gzip 程序（GNU zip）压缩的文件</td></tr><tr><td>`*.xz`</td><td>xz 程序压缩的文件</td></tr><tr><td>`*.bz2`</td><td>bzip2 程序压缩的文件</td></tr><tr><td>`*.tar.gz`</td><td>tar 打包，gzip 程序压缩的文件</td></tr><tr><td>`*.tar.bz2`</td><td>ar 打包，bzip2 程序压缩的文件</td></tr><tr><td>`*.tar.7z`</td><td>tar 打包，7z 程序压缩的文件</td></tr></table>

讲了这么多种压缩文件，这么多个命令，不过我们一般只需要掌握几个命令即可，包括 zip，rar，tar。下面会依次介绍这几个命令及对应的解压命令。

### 实战

#### zip打包文件与解压文件

*	zip：
	*	打包 ：zip something.zip something （目录请加 -r 参数）
	*	解包：unzip something
	*	指定路径：-d 参数


```bash
# 打包文件
$ zip <打包文件名> <打包文件>

# 打包文件夹 `-r` 表示递归打包
$ zip -r <打包文件名> <打包文件夹>

# `-q`安静模式打包
$ zip -q <打包文件名> <打包文件>

# `-o`表示输出文件
$ zip -r -q -o mZip.zip ~

# 打包加密文件
$ zip -e -r -q -o mZip.zip ~

# 如果你想让你在 Linux 创建的 zip 压缩文件在 Windows 上解压后没有任何问题
# 需要加上 -l 参数将 LF 转换为 CR+LF 来达到以上目的。
$ zip -r -l -o shiyanlou.zip /home/shiyanlou
```

**解压文件**
```bash
# 将 shiyanlou.zip 解压到当前目录：
$ unzip shiyanlou.zip

# 使用安静模式，将文件解压到指定目录：
$ unzip -q shiyanlou.zip -d ziptest

#指定编码类型,Linux默认的是UTF-8吗，如果有中文的话，会出现中文乱码的现象。 `-O` 参数指定解压编码
$ unzip -O GBK 中文压缩文件.zip
```

#### rar 打包压缩命令

rar 也是 Windows 上常用的一种压缩文件格式，在 Linux 上可以使用 rar 和 unrar 工具分别创建和解压 rar 压缩包。

* 安装 rar 和 unrar 工具：

```bash
$ sudo apt-get update
$ sudo apt-get install rar unrar
```

* 从指定文件或目录创建压缩包或添加文件到压缩包：

```bash
$ rm *.zip
$ rar a shiyanlou.rar .
```

上面的命令使用 a 参数添加一个目录 ～ 到一个归档文件中，如果该文件不存在就会自动创建。

**注意：rar 的命令参数没有 -，如果加上会报错。**

* 从指定压缩包文件中删除某个文件：

```bash
$ rar d shiyanlou.rar .zshrc
```

* 查看不解压文件：

```bash
$ rar l shiyanlou.rar
```

* 使用 unrar 解压 rar 文件

全路径解压：

```bash
$ unrar x shiyanlou.rar
```

去掉路径解压：

```bash
$ mkdir tmp
$ unrar e shiyanlou.rar tmp/
```

**rar 命令参数非常多，上面只涉及了一些基本操作。**

#### tar 打包工具
*	tar：
	*	打包：tar -zcvf something.tar something
	*	解包：tar -zxvf something.tar
	*	指定路径：-C 参数

```bash
# tar 打包文件
$ tar -zcvf myTar.tar ~

# tar 解包文件在当前目录
$tar -zxvf myTar.tar

# tar 解包文件在指定目录
$tar -zxvf myTar.tar -C myHome/
```

`-c` 表示创建一个tar包文件,`-f`用于指定创建的文件名，文件名必须紧跟在`-f`后面,`-x`表示解包一个tar文件、
#### 查看文件大小

用 du 命令分别查看默认压缩级别、最低、最高压缩级别及未压缩的文件的大小：

```bash
# 查看文件大小
$ du mytext.txt

# 查看目录大小
$ du ~

# `-h` 带单位输出
$ du -h mttext.txt

# 同时查看文件大小和目录大小,并进行排序操作
$ du -h myTest.zip ~ |sort
```



**学累了，学疲了。休息···等我有心情之后在来更新笔记哈**
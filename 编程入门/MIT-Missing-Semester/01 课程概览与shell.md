## 概况

[The Missing Semester of Your CS Education 中文版](https://missing-semester-cn.github.io/)


## 课程概览与shell

### The Shell

#### shell是什么?
计算机文字指令

#### 使用shell

**命令解释**
当您打开终端时，您会看到一个提示符，它看起来一般是这个样子的：

```
missing:~$ 
```

这是 shell 最主要的文本接口.

* 主机名是 `missing` 
* 当前的工作目录（”current working directory”）或者说您当前所在的位置是 `~` (表示 “home”)
* `$` 符号表示您现在的身份不是 root 用户

**执行命令**
在这个提示符中，您可以输入 _命令_ ，命令最终会被 shell 解析。最简单的命令是执行一个程序：
```sh
missing:~$ date
Fri 10 Jan 2020 11:49:31 AM EST
missing:~$ 
```


以在执行命令的同时向程序传递 _参数_:
```sh
missing:~$ echo hello
hello
```

如果传递的参数中包含空格（例如一个名为 My Photos 的文件夹），您要么用使用单引号，双引号将其包裹起来，要么使用转义符号 `\` 进行处理（`My\ Photos`）。

**解释**
shell是如何知道去哪里寻找 `date` 或 `echo` 的呢？其实，类似于Python或Ruby，shell是一个编程环境，所以它具备变量、条件、循环和函数.

当你在 shell 中执行命令时，您实际上是在执行一段 shell 可以解释执行的简短代码。如果你要求 shell 执行某个指令，但是该指令并不是 shell 所了解的编程关键字，那么它会去咨询 _环境变量_ `$PATH`，它会列出当 shell 接到某条指令时，进行程序搜索的路径：
```
missing:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
missing:~$ which echo
/bin/echo
missing:~$ /bin/echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```
当我们执行 `echo` 命令时，shell 了解到需要执行 `echo` 这个程序，随后它便会在 `$PATH` 中搜索由 `:` 所分割的一系列目录，基于名字搜索该程序。当找到该程序时便执行（假定该文件是 _可执行程序_）。确定某个程序名代表的是哪个具体的程序，可以使用 `which` 程序。我们也可以绕过 `$PATH`，通过直接指定需要执行的程序的路径来执行该程序


#### 在shell中导航
shell 中的路径是一组被分割的目录，在 Linux 和 macOS 上使用 `/` 分割，而在Windows上是 `\`。
==根目录==: 路径 `/` 代表的是系统的根目录，所有的文件夹都包括在这个路径之下，在Windows上每个盘都有一个根目录（例如： `C:\`）。 
==绝对/相对路径==: 如果某个路径以 `/` 开头，那么它是一个 _绝对路径_，其他的都是 _相对路径_ 。相对路径是指相对于当前工作目录的路径，当前工作目录可以使用 `pwd` 命令来获取(printworkingdirectory)
==切换目录==: 切换目录需要使用 `cd` 命令。在路径中，`.` 表示的是当前目录，而 `..` 表示上级目录

```
missing:~$ pwd
/home/missing
missing:~$ cd /home
missing:/home$ pwd
/home
missing:/home$ cd ..
missing:/$ pwd
/
missing:/$ cd ./home
missing:/home$ pwd
/home
missing:/home$ cd missing
missing:~$ pwd
/home/missing
missing:~$ ../../bin/echo hello
hello
```

一般来说，当我们运行一个程序时，如果我们没有指定路径，则该程序会在当前目录下执行。

为了==查看指定目录==下包含哪些文件，我们使用 `ls` 命令
```
missing:~$ ls
missing:~$ cd ..
missing:/home$ ls
missing
missing:/home$ cd ..
missing:/$ ls
bin
boot
dev
etc
home
...
```
除非我们利用第一个参数指定目录，否则 `ls` 会打印当前目录下的文件。

命令的参数:
大多数的命令接受标记和选项（带有值的标记），它们以 `-` 开头，并可以改变程序的行为。通常，在执行程序时使用 `-h` 或 `--help` 标记可以打印帮助信息，以便了解有哪些可用的标记或选项。
```
  -l                         use a long listing format
```

```
// $ 命令名称 命令参数 命令对象
missing:~$ ls -l /home
drwxr-xr-x 1 missing  users  4096 Jun 15  2019 missing
```

这个参数可以更加详细地列出目录下文件或文件夹的信息。
本行第一个字符 `d` 表示 `missing` 是一个目录。
然后接下来的九个字符，每三个字符构成一组,3组分别表示文件所有者（`missing`），用户组（`users`） 以及其他所有人具有的权限。(r表示读, w表示写, x表示执行, -表示不具有相应权限)

例如上面的信息可以看出, 只有文件所有者可以修改（`w`），`missing` 文件夹 （例如，添加或删除文件夹中的文件）。
为了进入某个文件夹，用户需要具备该文件夹以及其父文件夹的“搜索”权限（“可执行”：`x`）表示。
为了列出它的包含的内容，用户必须对该文件夹具备读权限（`r`）

注意，`/bin` 目录下的程序在最后一组，即表示所有人的用户组中，均包含 `x` 权限，也就是说任何人都可以执行这些程序。

==重要的命令==
* mv 重命名或移动文件
* cp 拷贝文件
* mkdir 新建文件夹
* man 展示程序名的文档,使用q退出该程序
* 



#### 在程序间创建连接
> 在 shell 中，程序有两个主要的“流”：它们的输入流和输出流。 当程序尝试读取信息时，它们会从输入流中进行读取，当程序打印信息时，它们会将信息输出到输出流中。 通常，一个程序的输入输出流都是您的终端。也就是，您的键盘作为输入，显示器作为输出。 但是，我们也可以重定向这些流！

最简单的重定向是 `< file` 和 `> file`。这两个命令可以将程序的输入输出流分别重定向到文件
```
missing:~$ echo hello > hello.txt
missing:~$ cat hello.txt
hello
missing:~$ cat < hello.txt
hello
missing:~$ cat < hello.txt > hello2.txt
missing:~$ cat hello2.txt
hello
```

您还可以使用 `>>` 来向一个文件追加内容。
使用管道（ _pipes_ ），我们能够更好的利用文件重定向。 `|` 操作符允许我们将一个程序的输出和另外一个程序的输入连接起来：
```
missing:~$ ls -l / | tail -n1
drwxr-xr-x 1 root  root  4096 Jun 20  2019 var
missing:~$ curl --head --silent google.com | grep --ignore-case content-length | cut --delimiter=' ' -f2
219
```


#### sudo
> 对于大多数的类 Unix 系统，有一类用户是非常特殊的，那就是：根用户（root user）。
> 根用户几乎不受任何限制，他可以创建、读取、更新和删除系统中的任何文件。我们并不会以根用户的身份直接登录系统，因为这样可能会因为某些错误的操作而破坏系统。 取而代之的是我们会在需要的时候使用 `sudo` 命令. 



### 课后练习

使用 `|` 和 `>` ，将 `semester` 文件输出的最后更改日期信息，写入主目录下的 `last-modified.txt` 的文件中
```sh
/home/missing/semester | sed -n '2p' > ../../last-modified.txt
```



   
写一段命令来从 `/sys` 中获取笔记本的电量信息，或者台式机 CPU 的温度。注意：macOS 并没有 sysfs，所以 Mac 用户可以跳过这一题。
```sh
cat /sys/class/power_supply/BAT1/power_now
```



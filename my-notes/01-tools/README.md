
## README

> <https://missing-semester-cn.github.io/>



### 1.课程概览与shell

#### win系统安装WSL

在win11系统上安装和配置子系统Linux，以便能使用可能推荐的bash版本。可以参考微软官网上这两篇文章:

* [安装WSL](https://learn.microsoft.com/zh-cn/windows/wsl/install)
* 安装发行版
  * [检查发行版](https://learn.microsoft.com/zh-cn/windows/wsl/install#check-which-version-of-wsl-you-are-running): 在安装WSL成功以后，通过命令行检查是否安装了发行版本： `wsl.exe --list --verbose`
  * 如果发行版未安装成功,[需要手动安装](https://learn.microsoft.com/zh-cn/windows/wsl/install#:~:text=%E8%AF%B7%E5%B0%9D%E8%AF%95%E8%BF%90%E8%A1%8C%20wsl%20%2D%2Dlist%20%2D%2Donline%20%E4%BB%A5%E6%9F%A5%E7%9C%8B%E5%8F%AF%E7%94%A8%E5%8F%91%E8%A1%8C%E7%89%88%E5%88%97%E8%A1%A8%E5%B9%B6%E8%BF%90%E8%A1%8C%20wsl%20%2D%2Dinstall%20%2Dd%20%3CDistroName%3E%20%E4%BB%A5%E5%AE%89%E8%A3%85%E5%8F%91%E8%A1%8C%E7%89%88): `wsl --install -d Ubuntu-22.04`
* [设置 WSL 开发环境指南的最佳做法](https://learn.microsoft.com/zh-cn/windows/wsl/setup/environment#set-up-your-linux-username-and-password)
  * 设置用户名和密码
  * [密码重置](https://learn.microsoft.com/zh-cn/windows/wsl/setup/environment#set-up-your-linux-username-and-password:~:text=%E7%A1%AE%E8%AE%A4%E6%96%B0%E5%AF%86%E7%A0%81%E3%80%82-,%E5%A6%82%E6%9E%9C%E5%BF%98%E8%AE%B0%E4%BA%86%20Linux%20%E5%88%86%E5%8F%91%E7%89%88%E7%9A%84%E5%AF%86%E7%A0%81%EF%BC%9A,-%E8%AF%B7%E6%89%93%E5%BC%80%20PowerShell)



#### Shell概述

* 是什么:  是一个编程环境, 是一个文字接口

* 基本概念:

  * 主机名
  
* 当前工作目录
  * `~`的含义
  
* `$`的含义
  * 输入命令解析: `missing:~$ echo $PATH`

    * shell会基于名字(如`echo`)搜索这个指令,如果这个指令不是shell关键字,会去咨询环境变量`$PATH`

* 在shell中导航

    * 分隔符: `/, \`
    * 相对路径和绝对路径
    * cd命令, `.`和`..`的含义
    * ls命令, 参数 `-l`
      * 理解`drwxr-xr-x`
    * 帮助信息的查看, `-h, -help`
    * mv命令(重命名或移动文件)
    * 创建文件
      * 创建空文件 touch name
      * `> 文件名`
      * `echo "文件内容" > 文件名`
      * `cat > 文件名`
    * cp命令(拷贝文件)
    * mkdir命令(新建文件夹)
    * man命令(展示命令参数的文档/用户手册)
    
* 在程序间创建链接

    * 重定向程序的输入和输出流: `< file`, `> file`

    * 使用`>>`来向文件追加内容

    * 使用管道符`|`,可以将一个程序的输出和另一个程序的输入连接起来

    * ```js
        missing: ~$ ls -l / | tail -nl
        
        ```

* sudo命令

#### 练习

* 在/tmp下新建一个名为'missing'的文件夹: `mkdir -p tmp/missing`
* 将如下内容一行一行写入`semester`文件夹

```bash
#!/bin/sh
curl --head --silent https://missing.csail.mit.edu
```



```bash
// 方案1
echo '#!/bin/sh' > semester
echo 'curl --head --silent https://missing.csail.mit.edu' >> semester


//方案2
nano semester

```

* 尝试执行`./semester`这个文件

```bash
./semester
ls -l ./semester
-rwxr-xr-x 1 kk kk 61 Sep 14 21:21 semester
chmod +x ./semester

```



* 查看`chmod`文档(例如, 使用`man chmod`)
* 使用`|, >`, 将semester文件输出的最后更改日期信息, 写入主目录下的`last-modified.txt`文件中

```bash
 ./semester | grep 'last-modified' | sudo tee /last-modified.txt > /dev/null
 
 cat /last-modified.txt
```



* 写一段命令来从 `/sys` 中获取笔记本的电量信息，或者台式机 CPU 的温度。注意：macOS 并没有 sysfs，所以 Mac 用户可以跳过这一题。

```bash
xxxx:~$ echo 'cat /sys/class/power_supply/BAT1/capacity' >> ./tmp/tmpcc

echo 'echo "the power is: $(cat /sys/class/power_supply/BAT1/capacity)"' >> your_script.sh
```


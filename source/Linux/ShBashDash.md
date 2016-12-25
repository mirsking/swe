# sh & bash & dash

所谓shell，就是命令行解释器(command-line interpreter)。
将shell作为操作系统内核之外的用户程序，这个概念最初是由Unix先驱Multics提出的。

## Thompson Shell
Thompson shell是第一个Unix shell，并用于Unix Version 1(1971)中。

## Bourne Shell (sh)
Bourne shell，也就是所谓的sh，是由Stephen Bourne在Bell实验室开发的，并用来代替Thompson shell。并用于Unix Version 7(1977)。

在Thompson shell基础上，sh开始用于命令行交互（interactive command interpreter）。而且引入了很多结构化编程的特性，从而成为一种脚本语言。

## Bourne-Again Shell (bash)
Bash是使用最广的unix shell，大多数Linux发行版的默认shell都是bash。
Bash是POSIX shell，同时加入了一些扩展feature。



### Bash语法的扩展（部分从ksh(Korn Shell)和csh(C shell)中借鉴）
* command line editing
* command line completion：当用户按下Tab键是，进行补全的就是这个
* command history
* directory stack
* `$RANDOM`和`$PPID`变量
* 命令替换（command substitution）语法：`$(...)`
* 数值计算（arithmetic evaluation），用`((...))` command或者 `$((...))`variable来实线evaluation
* 简化IO重定向：`$>`同时重定向stdout和stderr。
* 进程替换(process substitution): 通过`>`和`<`重定向输入输出。这个是通过/proc/fd匿名管道或者临时具名管道实现的。
* Bash的function关键词是和sh/ksh/POSIX不兼容的。
* Bash 2.05b开始支持[here documents](https://en.wikipedia.org/wiki/Here_document)，可以用`<<<`操作符，从一个“here string"重定向stdin。
* Bash 3.0支持类似Perl语法的正则表达式语法
* Bash 4.0支持关联数组(associative arrays)。关联数组提供了一种多维索引的fake support，这种方法很类似与AWK中用的方法。
* 括号表达式(Brace expansion)：从C shell中借鉴的feature
	```bash
	$ echo a{p,c,d,b}e
	ape ace ade abe
	$ echo {a,b,c}{d,e,f}
	ad ae af bd be bf cd ce cf

	ls *.{jpg,jpeg,png}    # expands to *.jpg *.jpeg *.png - after which,
						   # the wildcards are processed
	echo *.{png,jp{e,}g}   # echo just show the expansions -
						   # and braces in braces are possible.
	```

### Bash启动过程

1. 作为interactive login shell启动：
	* `/etc/profile`：这个脚本通常会调用`/etc/bash.bashrc`
	* 按以下顺序找到第一个可读的脚本并执行`~/.bash_profile, ~/.bash_login, ~/.profile`
2. 作为interactive not login shell启动：
	* `~/.bashrc`，当已经存在login shell，会先调用`~/.bashlogout`


## Debian Almquist Shell (Dash)
现有的大部分linux发行版本，将`/usr/bin`软链接到`/usr/bash`。
而Ubuntu从6.10开始将这一行为改变为`/usr/bin`->`/usr/dash`。
Dash主要是为了执行脚本而出现，而不是交互，它速度更快，但功能相比bash要少很多，语法严格遵守POSIX标准

**注意**：默认的login shell仍然是bash，从meau或者[ctrl-alt-t]打开的终端，默认事interactive bash。桌面或者文件管理器中运行的脚本，或者右键run in termial，则是POSIX dash。

## 恢复bash，而不使用dash的方法
sudo dpkg-reconfigure dash

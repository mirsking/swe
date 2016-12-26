# Ubuntu用户管理

## Ubuntu账户：

Ubuntu有三类账户：超级用户、普通用户以及系统用户。

每一个用户在ubuntu中都必须拥有一种账户，在Ubuntu中， /etc/passwd用来保存每个账户的信息。实际密码保存在 /etc/shadow中。

/etc/passwd文件每行基本格式：
```
username:password:uid:gid:gecos:homedir:shell
```
大多数都顾名思义，值得一提的是gecos是用于存放杂项信息的，一般不适用。

在Ubuntu中，普通用户可以使用sudo命令来完成系统管理员的操作。若想激活root用户，只需在命令行中输入sudo passwd root，并键入密码即可。

若想使用root终端，而不必每次键入sudo，则只需要输入sudo -i，便可看到“#”符号，完成工作后输入exit退出回到普通用户状态即可。

Ubuntu通过UID（user id）和GID（group id）区分不同的用户及用户组，root用户的UID和GID都为0.

1~499以及65534是系统用户的UID，普通用户的UID从1000开始递加。Ubuntu为每个大于1000的UID建立一个私有GID，管理员可以添加一个用户到某个GID，不允许一个用户分处多组。

## 文件访问权限：

Ubuntu用户访问权限分为3类，一次为Owner，Group，Other。每一类又分为r(read), w(write), x(execute)，对应一个三位二进制码。
例如：777表示Owner,Group,Other都具有r, w, x权限。
其中，改变权限的命令为：
chgrp： 改变用户组权限
chown： 改变所有者
chmod： 改变访问权限

## 用户组管理

用户组管理内容存储于/etc/group 文件中。
用于构成一个新用户的主目录所需的文件都位于/etc/skel中。这就为系统管理员提供了很多方便，因为任何需要大范围使用的文件，联系或目录都可以存放到/etc/skel中，创建新用户时系统会自动以合适权限分配给每一个新用户。
用户组管理命令：
1. groupadd
2. groupdel
3. groupmod：改变用户组名称或GID，不会更改其中用户。
4. gpasswd：创建用户组的口令。每个用户组都可以设置用户组口令和用户组管理员。-A参数可将一个用户指派为用户组管理员
5. useradd -G，使用-G参数可以在用户最初创建的时候将它添加到某用户组
6. grpck： 检查/etc/group文件的格式
7. usermod -G：在用户没有登录到系统时将它添加到某用户组
useradd bbb -p abc -s j/bin/zsh -u 507：其中-p指定密码 -s指定shell，-u指定UID

## 权限提升

命令 su - 将用户转变为root用户，并继承root用户环境变量等信息。（su的意义为substitute user）
命令su    将用户转变为root用户，不继承其他信息。
权限提升一般形式为sudo command。
使用了速冻命令后，速冻会核对/etc/sudoers文件，sudoers文件每行基本格式如下：
user  host_computer=command
user字段可以是单独的用户或用户组（用%前缀表示用户组）
host——computer字段一般表示网络中所有主机ALL或本机localhost（也可以指定为单独一台机器）
command可以是ALL，指定的命令或者受限命令（即不能使用该命令，用！前缀表示）

# 交换Capslock和Esc按键

### windows
写一个文件：locktoesc.reg
文件包含如下内容：

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layout]
"Scancode Map"=hex:00,00,00,00,00,00,00,00,03,00,00,00,3a,00,01,00,01,00,3a,00,00,00,00,00
```

然后保存，并且双击写入注册表，重新登录计算机

### linux
对于ubuntu有直接的可以设置；
对于有些不能直接设置的linux发行版：
新建文件locktoesc
文件内容包含：
```
remove Lock = Caps_Lock
keysym Escape = Caps_Lock
keysym Caps_Lock = Escape
add Lock = Caps_Lock
```

之后， xmodmap ~/locktoesc，使得上面的更改生效
把 xmodmap ~/locktoesc 这一句加到~/.bash_profile 文件里使得开机自动生效，而且不影响其他用户，不要root权限即可以使用。

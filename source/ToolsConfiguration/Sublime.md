# Sublime安装配置

## Package Control安装

按Ctrl+`调出console
粘贴以下代码到底部命令行并回车：
```
import urllib2,os;pf='Package Control.sublime-package';ipp=sublime.installed_packages_path();os.makedirs(ipp) if not os.path.exists(ipp) else None;open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read())
```
重启Sublime Text 2。
如果在Perferences->package settings中看到package control这一项，则安装成功。
如果这种方法不能安装成功，可以到这里下载文件手动安装。

## Package Control安装插件

按下Ctrl+Shift+P调出命令面板
输入install 调出 Install Package 选项并回车，然后在列表中选中要安装的插件。
不爽的是，有的网络环境可能会不允许访问陌生的网络环境从而设置一道防火墙，而Sublime Text 2貌似无法设置代理，可能就获取不到安装包列表了。

## 推荐插件

* SublimeLinter

一个支持lint语法的插件，可以高亮linter认为有错误的代码行，也支持高亮一些特别的注释，比如“TODO”，这样就可以被快速定位。（IntelliJ IDEA的TODO功能很赞，这个插件虽然比不上，但是也够用了吧）

* Sublime Alignment

用于代码格式的自动对齐。传说最新版Sublime 已经集成。

* Clipboard History

粘贴板历史记录，方便使用复制/剪切的内容。

* DetectSyntax

这是一个代码检测插件。

* Sublime CodeIntel

代码自动提示

* Bracket Highlighter

类似于代码匹配，可以匹配括号，引号等符号内的范围。

* Hex to HSL

自动转换颜色值，从16进制到HSL格式，快捷键 Ctrl+Shift+U


* GBK to UTF8

将文件编码从GBK转黄成UTF8，快捷键Ctrl+Shift+C

* Git

该插件基本上实现了git的所有功能。

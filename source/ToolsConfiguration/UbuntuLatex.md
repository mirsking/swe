# Ubuntu安装Latex

三个比较重要的命令:
```
apt-cache search
apt-cache show
sudo apt-get install
```

## 基本组件安装
```
sudo apt-get install texlive-latex-base        #Tex Live: Basic LaTex packages.
sudo apt-get install latex-cjk-all                #Installs all LaTex CJK packages
```

## latex相关库的安装
有些.sty文件可能没有安装，例如：lastpage.sty. 这个时候不要到网络上去询问是因为什么， Latex的输出错误信息已经很明显了。
使用下面的命令来查找相应的包：
apt-cache search lastpage (注意不要加.sty文件后缀)
可以看到需要下面的包，以及对这个包的描述：
texlive-latex-extra - TeX Live: LaTeX supplementary packages
选择安装即可：
```
sudo apt-get install texlive-latex-extra
```
完成上面的这三步，就可以完全满足我平时的应用需求了。 如果以后需要使用到新的包，可以使用上面第三步的方法来查找相应的安装包，并选择安装即可。

## texmaker安装
```
sudo apt-get install texmaker
```
texmaker是一个图形化界面的Tex书写，编译，生成，预览集合为一体的程序。 与Windows操作系统中的WinTex界面很相似。
Texlive-publishers包也可以安装一下， support for publishers, theses, standards, conferences, etc.
```
sudo apt-get install texlive-publishers
```
使用apt-cache show texlive-publishers命令可以看到它所支持的CTAN包的信息。

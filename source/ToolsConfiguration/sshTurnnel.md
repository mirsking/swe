# SSH端口转发

SSH的的Port Forward，中文可以称为端口转发，是SSH的一项非常重要的功能。它可以建立一条安全的SSH通道，并把任意的TCP连接放到这条通道中。下面仔细就仔细讨论SSH的这种非常有用的功能。


SSH Tunnel有三种，分别是本地Local（ssh -NfL），远程Remote（ssh -NfR），动态Dynamic（ssh -NfD）。（含义参考man ssh）


说明：在我们举例说明用法之前，先假设你有一台SSH机器，它的IP是a.b.c.d。

## 本地Local（ssh -NfL）
```
ssh -L <local port>:<remote host>:<remote port> <SSH hostname>
ssh -NfL a.b.c.d:1234:www.google.com:80 a.b.c.d
```

此时，在浏览器里键入：http://a.b.c.d:1234，就会看到Google的页面了。

在绑定1234端口的时候，可以省略前面的ip，如此一来，1234端口就仅仅绑定在localhost地址上，更安全：

```
ssh -NfL 1234:www.google.com:80 a.b.c.d
```

此时浏览的话就要在a.b.c.d机器上使用http://localhost:1234了。

何时使用本地Tunnel？

比如说你在本地访问不了某个网络服务（如www.google.com），而有一台机器（如：a.b.c.d）可以，那么你就可以通过这台机器来访问。

## 远程Remote（ssh -NfR）
```
ssh -R <local port>:<remote host>:<remote port> <SSH hostname>
```
在需要被访问的内网机器上运行： `ssh -NfR 1234:localhost:22 a.b.c.d`

登录到a.b.c.d机器，使用如下命令连接内网机器：

```
ssh -p 1234 localhost
```

需要注意的是上下两个命令里的localhost不是同一台。这时你会发现自己已经连上最开始命令里的localhost机器了，也就是执行“ssh -NfR”的那台机器。

何时使用远程Tunnel？

比如当你下班回家后就访问不了公司内网的机器了，遇到这种情况可以事先在公司内网的机器上执行远程Tunnel，连上一台公司外网的机器，等你下班回家后 就可以通过公司外网的机器去访问公司内网的机器了。


## 动态Dynamic（ssh -NfD）-Socket代理

```
ssh -D
ssh -NfD 1234 a.b.c.d
a.b.c.c 是server 地址
```

如此一来就建立了一台Socket代理机器，接着在浏览器上设置Socket代理：地址是localhost，端口是1234，从此以后，你的访问都是加 密的了！你可以通过访问WhatIsMyIP来 确认自己现在的IP，看看是不是已经变成a.b.c.d了。

测试阶段，也可以把端口绑定在外网地址上，如此一来，你在浏览器上就可以使用外网地址设置Socket代理，但这仅限于测试，否则，你的机器就不安全了， 随时可能成为肉鸡。对于Windows用户来说，如果讨厌命令行，还可以使用MyEnTunnel来实现同样的功能，配合Firefox的 FoxyPorxy，基本就无敌了，至于具体的配置方法，小崔已经写好了：使用Firefox+foxyProxy+SSH翻山越岭。如果你使用的是Chrome的话，则可以选择 Proxy Switchy!来实现同样的效果，恕不多言。
顶

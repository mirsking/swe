# Ubuntu局域网单网卡Nat共享上网

1、设置好主机的上网，下面的例子供参考：
代码:
IP地址 192.168.1.250
网关   192.168.1.1
DNS   61.128.114.166


2、进入终端，输入命令 sudo su 进入管理员模式；

3、接着输入命令，开启路由功能：

代码:
echo "1">/proc/sys/net/ipv4/ip_forward


4、接着依次输入下列命令：

```bash
sudo iptables -F
sudo iptables -P INPUT ACCEPT
sudo iptables -P FORWARD ACCEPT
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

这样主机就设好了，如果想自动运行，可以如下操作：

1）在文件 /etc/sysctl.conf 最后加上一行 net.ipv4.ip_forward = 1

2）在文件 /etc/rc.local 里加入下面的几行（注意，加在 exit 0 的前面）：

```bash
sudo iptables -F
sudo iptables -P INPUT ACCEPT
sudo iptables -P FORWARD ACCEPT
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

设好以后，重新启动一下计算机；

5、客户机设置好DNS，网关为主机的IP地址，下面的例子供参考：

代码:
IP地址 192.168.1.78
网关   192.168.1.250
DNS   61.128.114.166


重启后，客户机就可以直接上网、上QQ了；

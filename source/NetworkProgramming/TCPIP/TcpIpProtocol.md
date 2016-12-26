# TCP/IP五层协议
| Layer             | Protocols                                                     | 备注                                                                                             |
|-------------------|---------------------------------------------------------------|--------------------------------------------------------------------------------------------------|
| 应用层            | 使用TCP：Http/Telnet/FTP/SMTP/POP3; 使用UDP：DNS/TFTP/BOOTP/SNMP； | SMTP：简单邮件传送协议； TFTP：简单文件传送协议； BOOTP：引导程序协议； SNMP：简单网络管理协议； |
| 传输层            | TCP/UDP                                                       | TCP：传输层控制协议 UDP：用户数据报协议                                                          |
| 网络层            | IP/ICMP/IGMP                                                  | ICMP：Internet互联网控制报文协议； IGMP：Internet组管理协议                                      |
| 数据链路层（MAC） | ARP/RARP                                                      | ARP：地址解析协议（将IP动态映射到硬件地址）； RARP：拟地址解析协议；                             |
| 物理层            |                                                               |                                                                                                  |


## TCP UDP区别
[TCP与UDP的区别](http://blog.csdn.net/yipiankongbai/article/details/24435977)
* TCP面向连接， UDP是无连接的;
* 每一条TCP连接只能是点到点的; UDP支持一对一，一对多，多对一和多对多的交互通信
* TCP提供可靠的服务（通过TCP连接传送的数据，无差错，不丢失，不重复，且按序到达）; UDP尽最大努力交付（不保证可靠交付）。TCP的逻辑通信信道是全双工的可靠信道，UDP则是不可靠信道
* TCP首部开销20字节;UDP的首部开销小，只有8个字节
* TCP面向字节流，实际上是TCP把数据看成一连串无结构的字节流; UDP是面向报文的； UDP没有拥塞控制，因此网络出现拥塞不会使源主机的发送速率降低


## UDP协议的使用领域
注重速度，安全性要求不高的领域，如视频会议，流媒体

## ARP
ARP的报文格式如下： ![ARP_defination]( http://7xnluw.com1.z0.glb.clouddn.com/career/ARP_defination.png)
当主机需要与目的主机进行通信，但没有目的主机的MAC地址是使用ARP协议，将自己的IP、MAC地址以及目的主机的IP封装到ARP包中进行广播，目的主机收到该包后发现自己的IP地址和请求的目的IP地址相同，则会将自己的MAC地址封装在ARP包中并返回。

## PING的工作原理
* 参考博客
    + [ping过程原理详解](http://www.360doc.com/content/10/0804/20/1278923_43700893.shtml)
    +  [简述Ping的工作原理]( http://mp.weixin.qq.com/s?__biz=MzIxMTE3MDQwNQ==&mid=401893633&idx=1&sn=39c3d853b203fbf577206e42960ab78e#rd)

* ICMP
ICMP全称Internet Control Message Protocol，Internet控制报文协议。它通过让主机或路由器发送ICMP报文，将网络传输过程中的差错和异常情况报告给参与数据通信的相关主机。ICMP报文作为IP数据报的数据部分，被封装在IP数据报中。ICMP报文有两种类型：CIMP差错报告报文和ICMP询问报文。ICMP报文格式为： ![ICMP_defination]( http://7xnluw.com1.z0.glb.clouddn.com/career/ICMP_defination.png)

* Ping
Ping命令利用了ICMP的询问报文的0和8两种报文类型。Ping的过程最核心的过程是ping主机发送类型为8的echo request ICMP报文，目标主机如果收到，则会发一个类型为0的echo answer ICMP报文。事实上这其中还用到了ARP协议获取目标主机的MAC地址。


## DNS
DNS全称是Domain Name System，负责将域名解析为主机IP地址。


## Http状态码
![http_code](http://7xnluw.com1.z0.glb.clouddn.com/career/http_code.png)


 

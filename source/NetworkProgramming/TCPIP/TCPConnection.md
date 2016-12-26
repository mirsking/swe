# TCP建立连接的三次握手与四次挥手
## 参考博客
[TCP的三次握手(建立连接）和四次挥手(关闭连接）](http://www.cnblogs.com/Jessy/p/3535612.html)

## TCP连接建立与终止的时间系列
![TCP连接建立与终止的时间系列][1]

* 三次握手的具体过程
> （1）第一次握手：建立连接时，客户端A发送SYN包（SYN=j）到服务器B，并进入SYN_SEND状态，等待服务器B确认。
（2）第二次握手：服务器B收到SYN包，必须确认客户A的SYN（ACK=j+1），同时自己也发送一个SYN包（SYN=k），即SYN+ACK包，此时服务器B进入SYN_RECV状态。
（3）第三次握手：客户端A收到服务器B的SYN＋ACK包，向服务器B发送确认包ACK（ACK=k+1），此包发送完毕，客户端A和服务器B进入ESTABLISHED状态，完成三次握手。


* 四次挥手的具体过程
> （1）客户端A发送一个FIN，用来关闭客户A到服务器B的数据传送。
（2）服务器B收到这个FIN，它发回一个ACK，确认序号为收到的序号加1。和SYN一样，一个FIN将占用一个序号。
（3）服务器B关闭与客户端A的连接，发送一个FIN给客户端A。
（4）客户端A发回ACK报文确认，并将确认序号设置为收到序号加1。


## TCP连接建立与终止的状态变迁图
![TCP的状态变迁图][2]
![TCP的状态变迁图 简略版][3]
注意`FIN_WAIT_1`、`FIN_WAIT_2`和`TIME_WAIT`都是主动关闭方会进入的状态。


* 其中`TIME_WAIT`也称为2MSL(Maximum Segment Lifetime, 报文段最大生存时间)，它是报文段被丢弃前在网络内的最长时间。MSL在RFC793中规定为2min。
* TCP连接在2MSL等待时间时，定义这个连接的socket不能再被使用。


* `TIME_WAIT`时间的意义：
  1. 保证TCP协议的全双工连接能够可靠关闭
  如果Client直接CLOSED了，那么由于IP协议的不可靠性或者是其它网络原因，导致Server没有收到Client最后回复的ACK。那么Server就会在超时之后继续发送FIN，此时由于Client已经CLOSED了，就找不到与重发的FIN对应的连接，最后Server就会收到RST而不是ACK，Server就会以为是连接错误把问题报告给高层。这样的情况虽然不会造成数据丢失，但是却导致TCP协议不符合可靠连接的要求。所以，Client不是直接进入CLOSED，而是要保持TIME_WAIT，当再次收到FIN的时候，能够保证对方收到ACK，最后正确的关闭连接。
  2. 保证这次连接的重复数据段从网络中消失
  如果Client直接CLOSED，然后又再向Server发起一个新连接，我们不能保证这个新连接与刚关闭的连接的端口号是不同的。也就是说有可能新连接和老连接的端口号是相同的。一般来说不会发生什么问题，但是还是有特殊情况出现：假设新连接和已经关闭的老连接端口号是一样的，如果前一次连接的某些数据仍然滞留在网络中，这些延迟数据在建立新连接之后才到达Server，由于新连接和老连接的端口号是一样的，又因为TCP协议判断不同连接的依据是socket pair，于是，TCP协议就认为那个延迟的数据是属于新连接的，这样就和真正的新连接的数据包发生混淆了。所以TCP连接还要在TIME_WAIT状态等待2倍MSL，这样可以保证本次连接的所有数据都从网络中消失。

* [TIME_WAIT](http://blog.itpub.net/15480802/viewspace-1334381/)
TIME_WAIT状态也成为2MSL[^MSL]等待状态。
当TCP执行一个主动关闭，并发回最后一个ACK之后，该连接必须在TIME_WAIT状态停留2MSL时间。
这样可以让主动关闭端再次发送最后的ACK以防这个ACK丢失（另一端超时并重传FIN）。
* 2MSL状态
    + 处于2MSL的、由该socket对定义的连接在这段时间内不能再被使用。
    + 处于2MSL等待时，任何迟到的报文段将被丢弃。

* 没有TIME_WAIT的危害
    + 假设最终的 ACK 丢失 ， server 将重发 FIN ， client 必须维护 TCP 状态信息以便可以重发最终的 ACK ，否则会发送RST，结果 server 认为发生错误。若要TCP可靠地终止连接的两个方向 ( 全双工关闭 ) ， client 必须进 TIME_WAIT状态。
    + 数据包混淆
    + 如果没有TIME_WAIT的话，假设连接1已经断开，然而其被动方最后重发的那个FIN(或者FIN之前发送的任何TCP分段)还在网络上，然而连接2重用了连接1的所有的5元素(源IP，目的IP，TCP，源端口，目的端口)，刚刚将建立好连接，连接1迟到的FIN到达了，这个FIN将以比较低但是确实可能的概率终止掉连接2.

* 如何解决TIME_WAIT过多
    +  可以改为长连接，但代价较大，长连接太多会导致服务器性能问题，而且PHP等脚本语言，需要通过proxy之类的软件才能实现长连接；
    +  修改ipv4.ip_local_port_range，增大可用端口范围，但只能缓解问题，不能根本解决问题；
    +  客户端程序中设置socket的SO_LINGER选项；
    +  客户端机器打开tcp_tw_recycle和tcp_timestamps选项；
    +  客户端机器打开tcp_tw_reuse和tcp_timestamps选项；
    +  客户端机器设置tcp_max_tw_buckets为一个很小的值

## socket编程中服务端和客户端的流程
0. 参考博客[socket通信简介](http://blog.csdn.net/xiaoweige207/article/details/6211577)
1. socket的基本操作
  * socket()
  ```
  int socket(int domain, int type, int protocol);
  // domain: 协议域： AF_INET、AF_INET6、AF_LOCAL(AF_UNIX)、AF_ROUTE
  // type: socket类型： SOCK_STREAM、 SOCK_DGRAM、SOCK_RAW、SOCK_PACKET、SOCK_SEQPACKET等
  // protocol: 协议： IPPROTO_TCP、IPPROTO_UDP、IPPROTO_SCTP、IPPROTO_TIPC
  ```
  * bind()
  ```
  int bind(int sockfd, const struct sockaddr* addr, socklen_t addrlen);
  // sockfd: socket()返回的socket描述字
  // addr： 协议地址， IPv4 IPv6等都不同
  // addrlen: addr的长度
  ```
  * listen()/connect()
  ```
  int listen(int sockfd, in backlog);//服务端阻塞监听
  int connect(int sockfd, const struct sockaddr *addr, socklen_t addren);//客户端建立连接
  ```
  * accept()
  ```
  int accept(int sockfd, struct sockaddr* addr, socklen_t* addrlen);
  // 服务端接受连接，返回sockaddr和socklen_t
  ```
  * read()/write()
  ```
  read()/write()
  recv()/send()
  readv()/writev()
  recvmsg()/sendmsg()
  recvfrom()/sendto()
  ```
  * close()
  ```
  int close(int fd);

  ```


2. socket三次握手和四次挥手
  * 三次握手建立连接
  ![三次握手][4]

  * 四次挥手断开连接
  ![四次挥手][5]




  [1]: http://7xnluw.com1.z0.glb.clouddn.com/computer_network/tcp_ip/connection_disconnection_in_tcp.png
  [2]: http://7xnluw.com1.z0.glb.clouddn.com/computer_network/tcp_ip/state_machine_of_tcp.png
  [3]: http://7xnluw.com1.z0.glb.clouddn.com/computer_network/tcp_ip/state_machine_of_tcp_2.png
  [4]: http://7xnluw.com1.z0.glb.clouddn.com/computer_network/tcp_ip/socket_tcp_connection.png
  [5]: http://7xnluw.com1.z0.glb.clouddn.com/computer_network/tcp_ip/socket_tcp_disconnection.png

[^MSL]: Maximum Segment Lifetime，报文段最大生存时间，任何报文段被丢弃前在网络内的最长生存时间。RFC 793指出MSL为2分钟，实现中常用值是30s，1分钟，2分钟。

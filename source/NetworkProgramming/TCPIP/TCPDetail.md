# TCP细节

## TCP报文

![TCP报文头](http://7xnluw.com1.z0.glb.clouddn.com/Network/tcp_header2.png)

![TCP/IP封包大小]( http://7xnluw.com1.z0.glb.clouddn.com/Network/tcp_ip_package_size.png)

![TCP/IP封包大小]( http://7xnluw.com1.z0.glb.clouddn.com/Network/tcp_ip_package_size2.png)

## 滑动窗口机制

![slide_window](http://7xnluw.com1.z0.glb.clouddn.com/computer_network/transport/slide_window.png)

A发送11个字节之后
![slide_window_after_A_send_11_byte](http://7xnluw.com1.z0.glb.clouddn.com/slide_window_after_A_send_11_byte.png)

* TCP协议的两端分别为发送者A和接收者B，由于是全双工协议，因此A和B应该分别维护着一个独立的发送缓冲区和接收缓冲区，由于对等性（A发B收和B发A收），我们以A发送B接收的情况作为例子；
* 发送窗口是发送缓存中的一部分，是可以被TCP协议发送的那部分，其实应用层需要发送的所有数据都被放进了发送者的发送缓冲区；
* 发送窗口中相关的有四个概念：已发送并收到确认的数据（不再发送窗口和发送缓冲区之内）、已发送但未收到确认的数据（位于发送窗口之中）、允许发送但尚未发送的数据以及发送窗口外发送缓冲区内暂时不允许发送的数据；
* 每次成功发送数据之后，发送窗口就会在发送缓冲区中按顺序移动，将新的数据包含到窗口中准备发送；



## 拥塞控制

### 参考博客： [计算机网络【七】：可靠传输的实现 ](http://blog.chinaunix.net/uid-26275986-id-4109679.html)

### 1. 慢启动、拥塞控制
目的是在拥塞发生时循序减少主机发送到网络中的分组数，使得发生拥塞的路由器有足够的时间把队列中积压的分组处理完毕。
![slow_start_congestion_window]( http://7xnluw.com1.z0.glb.clouddn.com/computer_network/transport/slow_start_congestion_window.png)
慢启动的思路：
+ 发送方维持一个叫做“拥塞窗口(cwnd)”的变量，改变量和接受窗口共同决定了发送者的发送窗口；
+ 当主机开始发送数据时，避免一下子将大量字节注入到网络，造成或者增加拥塞，选择发送一个1字节的试探报文；
+ 当收到第一个字节的数据确认后，发送2个字节的报文；
+ 当再次收到2字节的确认，则发送四个字节（之后按照2的指数递增）
+ 最后达到一个提前预设的“慢启动门限(ssthresh)”，如上图，此时遵循以下条件判定。
    - cwnd < ssthresh，继续使用慢启动算法；
    - cwnd > ssthresh，停止慢启动，使用拥塞避免算法；
    - cwnd = ssthresh，既可以使用慢启动，也可以使用拥塞避免算法；
+ 所谓拥塞避免算法就是：每经过一个往返时间RRT，就把发送方的拥塞窗口+1，即让拥塞窗口线性增长；
+ 当出现网络拥塞时（比如丢包），把慢启动门限ssthresh设为原来的一半，然后将cwnd设为1，执行慢启动算法（低起点，指数增）


### 2. 快重传、快恢复
为了减少因为拥塞导致的数据包丢失带来的重传时间，从而避免传递无用的数据到网络。
![quick_recovery](http://7xnluw.com1.z0.glb.clouddn.com/computer_network/transport/quick_recovery.png)
快重传的机制是：
+ 接收方建立这样的机制，如果一个包丢失，则对后续的包继续发送针对该包的重传请求；
+ 一旦发送方接收到三个一样的确认，就知道该包之后出现了错误，立刻重传该包；
+ 此时发送方开始执行“快恢复”算法：
    - 慢开始门限减半；
    - cwnd设为慢开始门限减半后的数值；
    - 执行拥塞避免算法（高起点，线性增长）；

### 3. 总结
TCP 对于拥塞的控制总结为**“慢启动、加性增、乘性减”**，如图所示：

![tcp窗口大小变换]( http://7xnluw.com1.z0.glb.clouddn.com/Network/tcp_window_change.png)

(1) 慢启动 ：初始的窗口值很小，但是按指数规律渐渐增长，直到达到慢开始门限(ssthresh)。

(2) 加性增 ：窗口值达到慢开始门限后，每发送一个报文段，窗口值增加一个单位

(3) 乘性减 ：无论什么阶段，只要出现超时，则把窗口值减小一半。


## PAWS

## 几种定时器

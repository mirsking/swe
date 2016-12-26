# IO多路复用

[知乎上对IO多路复用的解释，讲的非常好](https://www.zhihu.com/question/32163005)

## 概念
I/O multiplexing指在单个线程通过记录跟踪每一个Sock(I/O流)的状态来同时管理多个I/O流。
![IO_multiplexing](http://7xnluw.com1.z0.glb.clouddn.com/operator_system/IO_multiplexing.jpg)

## select/poll/epoll的区别；
+ 前两个遍历后者回调（内核注册回调函数），所以后者效率高
+ select和poll不是线程安全的，epoll是线程安全的
+ select内部是数组实现、poll内部是链表实现，所以select有最大fd限制，poll没有限制（系统资源假设无穷大的话），epoll是内部是一棵红黑树；
+ select和poll都有用户态到内核态拷贝的过程，两者的切换和数据拷贝都很消耗性能，epoll没有内核和用户态的切换；
+ epoll内部采用了共享内存机制

## epoll中两种触发方式：边缘ET和水平LT的区别
+ 边缘只通知一次，效率高
+ 水平触发会把没处理的事件一直通知，效率低

# UDP细节

## UDP报文
![UDP header](http://7xnluw.com1.z0.glb.clouddn.com/Network/udp_header.jpg)

![UDP封装包大小]( http://7xnluw.com1.z0.glb.clouddn.com/Network/udp_package_size.png)

![UDP头]( http://7xnluw.com1.z0.glb.clouddn.com/Network/udp_header.png)

## 伪首部
伪首部字段：**32位源IP地址、32位目的IP地址、8位协议、16位UDP长度。**

## 伪首部的作用
1. 伪首部是**不占地址空间**的，在实际传输中不存在这样的字段。只是在使用的时候把它拿出来一下。
2. 伪首部的作用是让UDP**两次**检查数据是否已经正确到达目的地”：
* 第一次，通过伪首部的IP地址检验，UDP可以确认该数据报是不是发送给本机IP地址的；
* 第二次，通过伪首部的协议字段检验，UDP可以确认IP有没有把不应该传给UDP而应该传给别的高层的数据报传给了UDP。

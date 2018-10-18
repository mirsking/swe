# 微信红包架构总结

根据目前网络上现有微信架构文章总结成文。参考文献见文末链接。

## SET化

红包订单生成后，最后三位用于分SET，将该红包stick到一组SERVER上，然后根据订单找到对应的进程，该红包的所有请求在该进程上串行处理。

这里同一个进程的请求数，又由memcache做限流，防止db压力过大。

### 问题： 
1. 这组Server中的处理某个红包的进程挂了怎么办？    也即这个处理红包的队列是怎么维护的？如何故障恢复？

2. 这组Server整个挂了怎么办？   也即这组Server故障恢复如何处理？

3. 如何扩容缩容？
    * 分SET的规则是根据订单号自主生成的末三位来生成的，所以不需要考虑扩容缩容后的一致性hash问题


## 双维度分库分表

按照红包生成时间，分31天进行按天分库分表。冷热数据分离，提高db查询性能。


## 红包拆分
在线拆分，拆分算法是：
* 每次拆分金额是 random(0.01, min(2*total/n, total-(n-1)*0.01))
* 最后一个人，take all

每抢到一个红包：
* 更新红包剩余金额和剩余个数
* 插入一条领取记录
* 异步进行入账

微信从财付通拉取金额数据过来，生成个数/红包类型/金额放到redis集群里，app端将红包ID的请求放入请求队列中，如果发现超过红包的个数，直接返回。根据红包的逻辑处理成功得到令牌请求，则由财付通进行一致性调用，通过像比特币一样，两边保存交易记录，交易后交给第三方服务审计，如果交易过程中出现不一致就强制回归。

## 参考文献

### 方乐明在2017年10月InfoQ举办的QCon上的演讲
* 演讲内容文字版： [微信红包后台系统可用性设计实践](http://www.infoq.com/cn/articles/2017hongbao-weixin)
* 演讲内容PPT： [微信红包系统可用性设计实践](WeChatRedEnvelop.pdf)

### 方乐明在InfoQ上发表的文章
* [百亿级微信红包的高并发资金交易系统设计方案](http://www.infoq.com/cn/articles/2017hongbao-weixin)

### 其他人整理的QCon微信群讨论内容
* [微信红包金额分配的算法](https://timyang.net/architecture/wechat-red-packet/)
* [微信红包实现原理](http://www.phppan.com/2015/04/weixin-hongbao/)
* [微信红包的架构设计简介](https://colobu.com/2015/05/04/weixin-red-packets-design-discussion/)

### 其他
* [揭秘微信红包：架构、抢红包算法、高并发和降级方案](https://www.sohu.com/a/236239138_756465)
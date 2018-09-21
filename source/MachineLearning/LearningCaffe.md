# Caffe学习笔记

## 文档
[Caffe Tutorial](https://pan.baidu.com/s/1midja1i) 密码: u4je

## Caffe网络参数学习
```
weight_filler {
      type: "xavier"
}
```
xavier algorithm automatically determines the scale of initialization based on the number of input and output neurons

## Caffe可视化
[特征与滤波器可视化](https://github.com/gustavla/caffe-weighted-samples/blob/master/examples/filter_visualization.ipynb)

## Caffe例子
[Brewing Logistic Regression then Going Deeper](http://blog.csdn.net/thystar/article/details/50669615)

[官方CaffeTutorial PPT（附件中PPT）的笔记](http://blog.sciencenet.cn/blog-1583812-844177.html)

[Layors](http://caffe.berkeleyvision.org/tutorial/layers.html)

[python读写LMDB](http://deepdish.io/2015/04/28/creating-lmdb-in-python/)


## caffe 学习率
```
 // The learning rate decay policy. The currently implemented learning rate
  // policies are as follows:
  //    - fixed: always return base_lr.
  //    - step: return base_lr * gamma ^ (floor(iter / step))
  //    - exp: return base_lr * gamma ^ iter
  //    - inv: return base_lr * (1 + gamma * iter) ^ (- power)
  //    - multistep: similar to step but it allows non uniform steps defined by
  //      stepvalue
  //    - poly: the effective learning rate follows a polynomial decay, to be
  //      zero by the max_iter. return base_lr (1 - iter/max_iter) ^ (power)
  //    - sigmoid: the effective learning rate follows a sigmod decay
  //      return base_lr ( 1/(1 + exp(-gamma * (iter - stepsize))))
  //
  // where base_lr, max_iter, gamma, step, stepvalue and power are defined
  // in the solver parameter protocol buffer, and iter is the current iteration.
  optional string lr_policy = 8;
```

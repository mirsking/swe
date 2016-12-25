# Cuda并行编程

## 基本概念
* SM(streaming multiprocessor)
* SP(streaming processor)

## GPU架构

![GPU架构](http://7xnluw.com1.z0.glb.clouddn.com/GPU/gpu-computing-applications.png)

目前实验室已有GPU：
1. Kepler架构
    * GTX750（自己电脑上）
    * Tesla K40（服务器上）

2. Maxwell架构
    * GTX980Ti（唐立电脑上）
    * GTX980（楼下丁夏清电脑上）

## Thread模型

### 软件层面上

1. thread组合概念

    * 众多threads构成一个block，一个block拥有的threads数，目前所有架构都是最多不超过1024
    * 众多blocks构成一个grid

    ![thread](http://7xnluw.com1.z0.glb.clouddn.com/GPU/grid-of-thread-blocks.png)

2. thread/block/grid组合关系

![relationship](http://7xnluw.com1.z0.glb.clouddn.com/GPU/relationship-between-thread-block-grid-new.jpg)


3. kernel函数在`<<<a, b>>>`中的两个参数
    1) 参数含义
        * a, b都即可能是int，也可能是dim3
        * a代表grid包含blocks的维度信息
        * b代表block包含thread的维度信息
    2) 参数与thread的idx的关系
        * The index of a thread and its thread ID relate to each other in a straightforward way: For a one-dimensional block, they are the same; for a two-dimensional block of size (Dx, Dy),the thread ID of a thread of index (x, y) is (x + y Dx); for a three-dimensional block of size (Dx, Dy, Dz), the thread ID of a thread of index (x, y, z) is (x + y Dx + z Dx Dy).


### 硬件层面上

1. 一个SM上可以分配多个blocks，其数量由①kernel函数占用的register和shared memory大小以及②SM拥有的register和shared memory大小共同决定。

2. 一个block中的threads会按照thread id连续递增的顺序以32个thread为一组分成多个warps。warp是SM调度的基本单元。
blocks切分成warp的方式都是相同的，而每个block能够被分成的最大warp数目是$\lceil{\frac{T}{W_{size}}}\rceil$，其中T是blocks中$T$的大小，$W_{size}$是warp的大小，目前所有架构的warp大小都是32。

3. warp中的thread可以分成active和inactive两种状态。进入inactive状态的原因有三种：
    * 该thread比其他threads提前结束
    * 该thread在当前执行branch相不同的branch上（由于GPU的SIMT架构，一个warp中的kernel一次只能执行一个branch）
    * 该thread是$\frac{T}{W_{size}}$不能整除，为了补齐$W_{size}$而占位的那部分thread


## 内存模型

* 一个SM有一组32bit的registers，由warps瓜分
* 一个SM有一个parallel data cache和shared memory，由blocks瓜分

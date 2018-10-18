# Java 垃圾回收
[toc]

## 一些概念

### 新生代和老年代
JVM将堆分成了新生代(Young Generation)和老年代(Tenured Generation)两个区域。

其中新生代又分为Eden和两个Survivor区域。一般两个Survivor又可以分别叫做From和To区域。

### 永久代
上述新生代和老年代共同构成了JVM的堆区，而永久代是指JVM中的方法区。永久代的垃圾回收主要回收两部分内容：
* 废弃常量：比如常量池中的字面量，其他类（接口）、方法、字段的符号引用等。
* 无用的类：
    * 该类所有实例都已被回收，也即堆中不存在该类的任何实例。
    * 加载该类的ClassLoader已被回收。
    * 该类对应的`java.lang.Class`对象没有任何地方被引用，无法在任何地方通过反射访问该类的方法。

注： 在大量使用反射、动态代理、CGLib等bytecode框架的场景，动态生成JSP和OSGi这类频繁自定义ClassLoader的场景需要进行类卸载，保证永久代不会溢出。

### 垃圾回收中的并行与并发
描述垃圾回收算法时：
* 并行是指算法本身是多线程并行执行的；
* 并发是指算法的执行过程和用户线程是一起执行的。

### 根搜索算法 GC Roots Tracing 

从GC Roots节点出发，搜索走过的路径成为引用链(Reference Chain)，当一个对象到GC Roots没有任何引用链时，该对象可以被垃圾收集器回收。

可以作为GC Roots的对象：
* 虚拟机栈中的引用对象（栈帧中的本地变量表）
* 本地方法栈JNI引用的对象
* 方法区中的类静态属性应用的对象
* 方法区中的常量引用的对象


## 对象内存分配策略

### 对象优先在Eden分配
大多数情况，对象在新生代的Eden区分配；当Eden区没有足够的空间进行分配时，触发一次Minor GC。

### 大对象直接进入老年代
大对象(需要大量连续内存空间的Java对象，比如长字符串或数组)，直接在老年代分配内存。防止新生代到老年代无谓的复制。

### 长期存活的对象进入老年代
JVM会给每个对象定义对象年龄(Age)计数器：
* 对象在Eden出生并经过第一次Minor GC后仍然存活，并能被Survivor容纳的话，将被移动到Survivor区，并将对象年龄设为1
* 对象在Survivor区中每熬过一次Minor GC，年龄就增加1岁。
* 当年龄增长到一定值（默认15）后，对象被晋升到老年代。

### 动态对象年龄判定
如果在Survivor空间中相同年龄所有对象大小的总和大于Survivor空间的一半，则年龄大于等于该年龄的对象就可以直接进入老年代。

### 空间分配担保
新生代的由Eden和From、To组成，每次使用Eden和From，当发生MinorGC时，将Eden和From中仍然存活的对象复制到To中。由于Eden和Survivor的比例比较大，有可能出现To的空间不够情况，此时由老年代的内存进行担保。

发生Minor GC时，虚拟机会检测之前每次晋升到老年代的平均大小是否大于老年代的剩余空间大小：
* 如果大于：直接进行一次FullGC
* 如果小于：检测HandlePromotionFailure是否允许担保失败
    * 允许：跳过
    * 不允许：进行一次FullGC

如果出现了老年代担保失败(Handle Promotion Failure)，则会进行一次Full GC。


## 垃圾回收算法
JVM将堆分成新生代和老年代，从而可以在不同代中使用不同的算法。主要的垃圾回收算法如下图，连线表示能搭配使用的收集器：
![JavaGC](JavaGC/JavaGC.png)

### 新生代算法
#### Serial收集器
* Eden-Survivor复制算法
* 单线程收集器
* 需要Stop the world
* 简单高效
* 一般Client端的程序会用

#### ParNew收集器
* Serial的并行版本
* 一般Server端的程序会用

#### Parallel Scavenge收集器
* 复制算法
* 并行收集器

与ParNew的区别：
1. 目标不同：Parallel Scanvenge的目标是达到一个可控制的吞吐量(Throughput = 运行用户代码时间/(运行用户代码时间+垃圾收集时间))
2. 自适应调节(GC Ergonomics)：-XX:+UseAdaptiveSizePolicy打开后，会自适应调节Eden与Survivor比例、晋升老年代对象年龄等，以提供最合适的停顿时间或最大的吞吐量。

另，之所以Parallel Scavenge不能喝Serial Old以及CMS收集器结合使用。是因为Parallel Scavenge和G1都没有使用传统的GC收集器代码框架，而是单独实现的。

### 老年代算法

#### Serial Old收集器
* 标记-整理算法
* 一般Client端的程序会用
* 用在Server模式下，用于：
    * 与Parallel Scavenge收集器搭配使用
    * 作为CMS收集器发生Concurrent Mode Failure的时候的后背预案。

#### Parallel Old收集器
* 标记-清除算法
* Parallel Scanvenge的老年代版本
* 并行收集

#### CMS收集器 (Concurrent Mark Sweep)
* 目标：获取最短回收停顿时间
* 一般用于Server端应用
* 并发的标记-清除算法，分四个步骤
    * 1. 初始标记（CMS initial mark）
    * 2. 并发标记 (CMS concurrent mark)
    * 3. 重新标记 (CMS remark)
    * 4. 并发清除 (CMS concurrent sweep)

其中1和3两个步骤需要stop the world，另外两个步骤可以与用户程序并发执行。每个步骤的主要工作如下：
* 初始标记只标记GC Roots能直接关联的对象，速度最快；
* 并发标记是进行GC Roots Tracing的过程。
* 重新标记是修正并发标记期间，用户程序运行而导致标记记录变动的。

### G1收集器


## 一些JVM参数

* -XX:SurvivorRatio=8 控制新生代中Eden区和Survivor区的大小比例。配置为8表示，Eden:From:To=8:1:1

* -XX:PretensureSizeThreshhold 控制超过某大小的内存分配直接在老年代中进行。该参数只对Serial和ParNew两款收集器有效。

* -XX:MaxTenuringThreshold 对象从新生代晋升到老年代的阈值年龄。


## Java GC问题处理

[JVM问题分析处理手册](https://zhuanlan.zhihu.com/p/43435903)
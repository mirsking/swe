# C++中内存分配与回收

## malloc/free和new/delete的区别
1.malloc/free是C/C++语言的标准库函数，new/delete是C++的运算符

2.new能够自动分配空间大小

3.对于用户自定义的对象而言，用maloc/free无法满足动态管理对象的要求。对象在创建的同时要自动执行构造函数，对象在消亡之前要自动执行析构函数。由于malloc/free是库函数而不是运算符，不在编译器控制权限之内，不能够把执行构造函数和析构函数的任务强加于malloc/free。因此C++需要一个能对对象完成动态内存分配和初始化工作的运算符new，以及一个能对对象完成清理与释放内存工作的运算符delete---简而言之 new/delete能进行对对象进行构造和析构函数的调用进而对内存进行更加详细的工作，而malloc/free不能。

## malloc/free函数原型

```cpp
void * malloc(size_t size);
void free( void * memblock );
```

## new/delete时的[]
注 对于无构造函数直接内存初始化类型，以下操作都不会有问题。
但是对于有构造函数的类型，以下问题会出现：

1. new数组，delete时少了[]

    > VisualStudio会assert failed.
    Gcc会Segmentation Falt.

2. new 一个对象，delete时多了[]

    > VisualStudio 越界访问冲突
    Gcc一直free到一个invalid指针

3. 注意
对象数组的构造和析构过程相反，比如

```cpp
#include <iostream>
using namespace std;
class Test
{
public:
    Test()
    {
        object_count = count++;
        cout << "ctor: " << object_count << endl;
    }
    ~Test()
    {
        cout << "dtor: " << object_count << endl;
    }
private:
    static int count;
    int object_count;
};
int Test::count = 0;

int main()
{
    Test* t = new Test[3];
    delete[] t;
    return 0;
}
```

输出：
```
ctor: 0
ctor: 1
ctor: 2
dtor: 2
dtor: 1
dtor: 0
```

## 混用
1.  new/free

    > 这个在vs和gcc上都可以过，2333
    测试gcc版本4.8.4

2. malloc/delete

    > 这个在vs和gcc上也都可以过，2333

##  能否malloc(1.2G)
1. 32位win下，内核用2G，能分配2G的内存；
32位linux下，内核用1G，能分配3G的内存；

# struct的前置声明


## a.h:
```cpp
typedef struct my_time_t
{
    int hour, minute, second;
} MY_TIME;
```

## b.h
```cpp
struct my_time_t;
typedef struct my_time_t MY_TIME;

void func(MY_TIME* mt) {}
```

## main.cpp
```cpp
#include "a.h"
#include "b.h"

int main()
{
    MY_TIME mt;
    func(&mt);

    return 0;
}
```

这样就可以成功了. 在b.h中做前置声明时, 先声明有my_time_t这样一个struct, 然后说明MY_TIME是由那个结构体typedef出来的,

这样void func(MY_TIME* mt);这个函数声明就能编译通过了.   直接做struct MY_TIME;这样的前置声明是不被接受的.

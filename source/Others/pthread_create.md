## pthread_create

### 头文件
```cpp
#include <pthread.h>
```

### 函数声明
```cpp
int pthread_create (pthread_t *__restrict __newthread,
                           const pthread_attr_t *__restrict __attr,
                           void *(*__start_routine) (void *),
                           void *__restrict __arg)
```

### 返回值
成功返回0。

### 参数
1. `__newthread`
  指向线程标识符的指针。成功返回时，`__newthread`被设置为新创建线程的线程ID。
2. `__attr`
  用于设置线程属性。
3. `__start_routine`
  线程运行函数的起始地址。
4. `__arg`
  运行函数需要的参数，如果`__start_routine`需要的参数不只一个，则需要将参数放到结构体中，将结构体指针传入。

### 其他
`restrict`是C99添加的用于编译器优化的关键字，可以无视。

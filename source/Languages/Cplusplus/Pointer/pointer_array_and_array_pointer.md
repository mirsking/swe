# 指针数组与数组指针的区别

```cpp
#include <iostream>
using namespace std;

int main()
{
    short p0[16];    // short数组的指针，数组中每个元素存储的是一个short，sizeof大小是16*2 = 32
    short *p1[16];    // short指针 数组， 数组中每一个元素存储的是一个short*指针，sizeof大小是数组元素大小*指针大小 = 16*4 = 64
    short(*p2)[32]; // 指向数组第一个元素的头指针，是一个指针，sizeof 返回的是指针的大小

    cout << sizeof(p0) << endl;
    cout << sizeof(p1) << endl;
    cout << sizeof(p2) << endl;
    return 0;
}
```

# 大小端与网络字节序

## 大端（高尾端）
数据尾端放在高地址 (网络字节序是大端)

## 小端（低尾端）
数据尾端放在低地址

## 代码测试大端小端

在一个短整数变量中存放2字节的值0x1122，然后查看它的连续字节c[0]（对应上图地址A）和c[1]，以此确定字节序。
```cpp
#include <stdlib.h>
#include <stdio.h>
int main(int argc, char **argv)
{
    union {
        short s;
        char c[sizeof(short)];
    } un;
    un.s = 0x1122;
    if(sizeof(short)==2) {
        if(un.c[0]==0x11 && un.c[1] == 0x22)
            printf("big-endian\n");
        else if (un.c[0] == 0x22 && un.c[1] == 0x11)
            printf("little-endian\n");
        else
            printf("unknown\n");
    } else
        printf("sizeof(short)= %d\n",sizeof(short));
    exit(0);
}
```

## 网络字节序
网络字节序是大端模式

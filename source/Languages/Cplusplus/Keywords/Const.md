# const关键字在C/C++中的用法

## 参考博客:
    * [在C和C++里const的用法异同总结](http://blog.sina.com.cn/s/blog_6f808e510100qb1d.html)
    * [C/C++中const用法小结](http://blog.csdn.net/huangxy10/article/details/8087194)

## C中const应用

1. 在定义变量时使用（由于const常量在定义后不能被修改，所以在定义时一定要进行初始化操作）：
    * 最简单的用法，说明变量为一个常变量（在以下例子里，int 和const的先后顺序可以改变的，这无所谓）：
    `const int a=100; int const b=100 `
    * 说明指针为指向常数的指针，即指针本身的值是可以改变的：
    `const int *a=&b`
    * 说明指针本身的值不可改变，但指向的内容可改变(如果const位于星号的左侧，则const就是用来修饰指针所指向的变量，即指针指向为常量；如果const位于星号的右侧，const就是修饰指针本身，即指针本身是常量。)：
    `int * const a = &b`
    * 说明指针为指向常数的常指针,即指针本身与指针指向的内容都不可改变：
    `const int * const a = &b`
    * 说明引用为常数引用,即不能改变引用的值(被说明的引用为常引用，该引用所引用的对象不能被更新)：
    `const int &a=100`
2. 在定义函数时使用：
    * 作为参数使用，说明函数体内是不能修改该参数的：
    `void func(const int a)`
    * 作为返回值使用，说明函数的返回值是不能被修改的：
    `const int func()`
    * 在函数中使用const,情况与定义变量的情况基本一致

## C++中多于C的const应用

1. const类成员

const类成员在对象构造期间允许被初始化并且在以后不允许被改变。const类成员和一般的const 变量有所不同。(由于const类型对象必须被初始化，并且不能更新，因此，在类中说明了const数据成员时，只能通过成员初始化列表的方式来生成构造函数对数据成员初始化。)

2. const 成员函数

const成员函数不允许在此函数体内对此函数对应的类的所有成员变量进行修改，这样可以提高程序的健壮性。


## C/C++中const的区别

1. 在C中const默认是外部链接，而C++中则是内部链接。所以只能在定义const常量的文件中使用该常量时；
2. c++不给const常量分配空间，此时const int c = 0；相当于#define c 0；而在C中，它会给每个const常量分配内存空间。


## 宏和全局变量区别
宏在编译器直接做文本替换，不在变量区分配内存空间。
全局变量在变量区分配内存空间，如果不是const修饰，在程序运行期可以修改变量值。
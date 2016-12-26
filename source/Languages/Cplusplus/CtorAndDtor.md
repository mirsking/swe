# 构造函数与析构函数

## 构造/析构函数相关

1. 编译器什么时候隐式声明默认构造函数？
有两个条件：
     * 该类没有显式声明任何构造函数。--既然你都定义了，系统就不给你生成了。
     * 数据成员中没有const和reference。--因为要初始化。


2. 什么时候编译器会**真正**生成默认构造函数
[博客链接](http://www.cnblogs.com/fanzhidongyzby/archive/2013/01/12/2858040.html)
只有编译器不得不为这个类生成函数的时候（nontrival），编译器才会真正的生成它。
     * nontrival四种情况：
        * 类内数据成员是对象，并且该对象的类提供了一个默认构造函数；
        * 类的基类提供了默认的构造函数；
        * 类内定义了虚函数；
        * 类使用了虚继承。


3. 什么时候必须要定义默认构造函数，不能使用编译器提供的构造函数？
     * 参考博客： [默认构造函数的常见问题](http://www.cnblogs.com/gnuhpc/archive/2012/12/10/2811920.html)
     * 数据成员中有const和reference（需要初始化）
     * 数据成员中有无默认构造函数的对象需要显示构造
     * 基类无默认构造函数，需要显示构造
     * 类中定义了其他构造函数，但是需要不带参数的默认构造函数


4. 虚析构函数怎么调用的？

最底层的派生类（most derived class）的析构函数最先被调用，然后调用每一个基类的析构函数。


5. 为什么析构函数要是虚函数？

析构函数执行时先调用派生类的析构函数，其次才调用基类的析构函数。如果析构函数不是虚函数，而程序执行时又要通过基类的指针去销毁派生类的动态对象，那么用delete销毁对象时，只调用了基类的析构函数，未调用派生类的析构函数。这样会造成销毁对象不完全。

* 注意：
     * 应该为多态基类声明虚析构器。一旦一个类包含虚函数，它就应该包含一个虚析构器。
     * 如果一个类不用作基类或者不需具有多态性，便不应该为它声明虚析构器。


6.  为什么构造函数不能设成虚函数
虚函数对应一个vtable和virtual指针（VPTR），如果构造函数是虚的，就需要通过virtual指针调用vtable。但是此时对象没有构造完成，不存在virtual指针。


## 拷贝（复制）构造函数
0. 该部分参考博客
     * [拷贝构造函数和赋值运算符重载](http://blog.csdn.net/gxtdjh/article/details/6653499)
     * [赋值运算符与拷贝构造函数的区别与联系]( http://blog.sina.com.cn/s/blog_725dd1010100t826.html)


1. 类的深浅拷贝

浅拷贝：对被拷贝对象按位拷贝
深拷贝：对被拷贝对象中的指针对象等进行深层拷贝


2. 复制构造函数定义中的const是否必须？引用是否必须？

const不是必须，引用是必须的，否则在拷贝构造函数传入参数时引起循环调用。


3. 复制构造函数什么时候被调用？

复制构造函数在用一个已初始化的对象去初始化一个新建对象。
     * 对象的直接赋值初始化会调用拷贝构造函数；
     * 函数参数传递只要是按值传递调用拷贝构造函数；
     * 函数返回只要是按值返回调用拷贝构造函数。


4. 复制构造函数和赋值操作符=的区别：

* 拷贝构造函数，用已创建的对象来初始化新建对象，对于一个已经被初始化的对象则进行operator=赋值操作。
* =赋值操作符重载比拷贝构造函数做得要多，它除了完成拷贝构造函数所完成的拷贝动态申请的内存的数据之外，还释放了原本自己申请的内存空间。

```cpp
class A;
A a;
A b=a;   //拷贝构造函数调用
A b(a);   //拷贝构造函数调用

A a;
A b;
b = a;   //赋值运算符调用
```


5. String类的实现（考虑拷贝构造函数和=操作符重载）

```cpp
#include <iostream>
using namespace std;
class String
{
public:
    String(){}
    String(const char *str = NULL);     // 普通构造函数
    String(const String &lhs);     // 拷贝构造函数
    ~String(void);         // 析构函数
    String& operator=(const String &lhs); //赋值函数
private:
    char *string_;    //私有成员，保存字符串
};


String::~String(void)
{
    cout<<"Destructing"<<endl;
    delete[] string_;
}
String::String(const char *str)
{
    cout<<"Construcing"<<endl;
    if(str==NULL)
    {
        string_ = new char[1];
        *string_ = '\0';
    }
    else
    {
        int length = strlen(str);
        string_ = new char[length+1];
        strcpy(string_, str);
    }
}


String::String(const String &rhs)
{
cout<<"Constructing Copy"<<endl;
int length=strlen(rhs.string_);
string_ = new char[length+1];
strcpy(string_, rhs.string_);
}


String& String::operator=(const String &rhs)
{
    cout<<"Operate = Function"<<endl;
    //检查自赋值
    if(this == &other) //比较的是指针
        return *this;
    //释放原有的内存资源
    delete[] string_;
    //分配新的内存资源，并复制内容
    int length=strlen(rhs.string_);
    string_ = new char[length+1];
    strcpy(string_, rhs.string_);
    //返回本对象的引用
    return *this;
}
int main()
{
    String a("Hello");
    String b("Hi");
    String c(a);
    c=b;
}
```

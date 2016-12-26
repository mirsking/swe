# 字符串常量与const

char* p = "test"，声明了一个指针，而这个指针指向的是**全局的const**
```cpp
char* p1 = "anything";
char* p2 = "anything";
printf(“ p1=%x, p2=%x /n”, p1, p2);
```

取而代之的，应该是使用数组来做初始化声明。如：`char str[] = “hello world”; `

我们知道，双引号引起来的字符串是const的，所以，在的世界中，你应该进行如下的声明才比较稳妥：`const char *p = "test";`

可问题是，像这种对类型要求很严格的语言来说，为什么它在编译诸如char *p="test" 程序的时候不出错，甚至连个警告都没有（vc++7）？我想，这应该是对古老的的一个向下兼容。因为，在的世界中，这种用法太多了。
比如：函数的参数和异常的捕获都存在这种问题，如下所示：（因编译器而定，在gcc 3.4.3版中，下例中的异常示例不能被捕获，但VC++6中却可以被捕获）
```cpp
func( char* p) { }   // 以这种方式调用函数func(“abc”);
try { thow “exception”; } catch (char* p) { }
```
这些都是C++编译器默认了可以把const char* 转成 char* 的罪行，无疑会对大家是一个误导。甚至让人无所畏惧地走入其中，并自以为走入了正途。这样看来，这种向下兼容的标准，就显得有点误人不浅了
不过好在，标准委员会早已意识到了这一点。这个feature被定义为了“Deprecated Feature”，即“不被建议使用的特性”。意思就是，在将来，这种特性将被从中移出，于是，你目前的这种程序将无法在新的编译器上编译通过。对于程序的可移植性来说，我们今天所写的代码尤其要注意这些“Deprecated Feature
据我所知，目前中被列为“Deprecated Feature”如下所示（可能不准确，请大家指正）

下面的这些feature都已被C++标准委员会订为废除featrue了。
* 隐晦的字符串的const
```cpp
char *p = "test";
w_char *pw = L"test";
```
const的字符串类型转成non-const的。包括指针和数组。

* 隐晦的类型声明。
```cpp
func() {}   //函数的隐晦返回类型是
static num;    //变量的隐晦类型是
```
feature中还可以使用，但在中都被去除了。（gcc 3.4版本对于这种声明会给出编译错误，而VC++6.0会认为这是合法的程序）

* 布尔变量的累加操作。
bool isConn = false;
isConn++;          //这个操作会把isConn
就目前而言，几乎所有的编译器都认可这种操作，但这种用法也是不被建议的，终有一天会被取消。

* 更改父类成员的存取权限。
```cpp
class B
    protected:
        int i;
class D : public B
     public:
        B::i; //这种方式可能大家很少看到。
```
对于这种语法，子类重新暴露了父类的私有成员。这会带来很大的安全性问题。目前而言，feature对于所有的编译器来说应该都是可以编译通过的（连个Warning都没有）。但这个feature也是要被废除的。

* 文件中域的static
```cpp
static int i;
static void func()
```
据说，这种旧的在中的为了实现其作用域在本文件中的feature中也要被取消。

-------------------------------------
文章到这里应该结束了，在结束之前，让我再给大家共享一个有趣的关于const的例子（在网上看到的）
```cpp
const int a = 1;
    int *p = const_cast<int*>(&a);
    *p = 2;
    cout << “value a=”<< a << endl;
    cout << “value *p=” <<*p << endl;
    cout << “address a=” <<&a << endl;
    cout << “address p=” <<p << endl;
```
这段代码输出的结果如下：
```cpp 
value a=1
value *p=2
address a=0xbff1d48c
address p=0xbff1d48c
```

地址都是一样的，可值为什么不一样呢？呵呵。这个问题看起来有点“学术味”过浓，不过是个好例子，可以让你知道的一些用法和一些原理。有以下几个方面大家可以考虑一下：
const int a = 1是不是和宏有点像，会不会被编译器优化了？
去修改一个const的值，本来应该是不对的。这可能会是向旧的兼容。是否会让编译器产生未知行为？
所以，这个示例也告诉我们，我们应该遵循constnon-const的语义，任何想要破坏这个语义的事情都会给我们带来未知的结果。

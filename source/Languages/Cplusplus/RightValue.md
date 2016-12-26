# C++右值引用
C++ 11中引入的一个非常重要的概念就是右值引用。理解右值引用是学习“移动语义”（move semantics）的基础。而要理解右值引用，就必须先区分左值与右值。

对左值和右值的一个最常见的误解是：等号左边的就是左值，等号右边的就是右值。左值和右值都是针对表达式而言的，**左值是指表达式结束后依然存在的持久对象，右值是指表达式结束时就不再存在的临时对象**。一个**区分左值与右值的便捷方法是：看能不能对表达式取地址，如果能，则为左值，否则为右值**。下面给出一些例子来进行说明。

```cpp
a = 10;
b = 20;
*pFlag = &a;
vector<int> vctTemp;
vctTemp.push_back(1);
string str1 = "hello" ;
string str2 = "world";
const  &m = 1;
```

请问，a，b, a+b, a++, ++a, pFlag, *pFlag, vctTemp[0], 100, string("hello"), str1, str1+str2, m分别是左值还是右值？
* a和b都是持久对象（可以对其取地址），是左值；
* a+b是临时对象（不可以对其取地址），是右值；
* a++是先取出持久对象a的一份拷贝，再使持久对象a的值加1，最后返回那份拷贝，而那份拷贝是临时对象（不可以对其取地址），故其是右值；
* ++a则是使持久对象a的值加1，并返回那个持久对象a本身（可以对其取地址），故其是左值；
* pFlag和*pFlag都是持久对象（可以对其取地址），是左值；
* vctTemp[0]调用了重载的[]操作符，而[]操作符返回的是一个int &，为持久对象（可以对其取地址），是左值；
* 100和string("hello")是临时对象（不可以对其取地址），是右值；
* str1是持久对象（可以对其取地址），是左值；
* str1+str2是调用了+操作符，而+操作符返回的是一个string（不可以对其取地址），故其为右值；
* m是一个常量引用，引用到一个右值，但引用本身是一个持久对象（可以对其取地址），为左值。

区分清楚了左值与右值，我们再来看看左值引用。左值引用根据其修饰符的不同，可以分为非常量左值引用和常量左值引用。

非常量左值引用只能绑定到非常量左值，不能绑定到常量左值、非常量右值和常量右值。如果允许绑定到常量左值和常量右值，则非常量左值引用可以用于修改常量左值和常量右值，这明显违反了其常量的含义。如果允许绑定到非常量右值，则会导致非常危险的情况出现，因为非常量右值是一个临时对象，非常量左值引用可能会使用一个已经被销毁了的临时对象。

常量左值引用可以绑定到所有类型的值，包括非常量左值、常量左值、非常量右值和常量右值。

可以看出，使用左值引用时，我们无法区分出绑定的是否是非常量右值的情况。那么，为什么要对非常量右值进行区分呢，区分出来了又有什么好处呢？这就牵涉到C++中一个著名的性能问题——拷贝临时对象。考虑下面的代码：

```cpp
vector<int> GetAllScores()
{
vector<int> vctTemp;
vctTemp.push_back(90);
vctTemp.push_back(95);
return vctTemp;
}
```

当使用vector<int> vctScore = GetAllScores()进行初始化时，实际上调用了三次构造函数。尽管有些编译器可以采用RVO（Return Value Optimization）来进行优化，但优化工作只在某些特定条件下才能进行。可以看到，上面很普通的一个函数调用，由于存在临时对象的拷贝，导致了额外的两次拷贝构造函数和析构函数的开销。当然，我们也可以修改函数的形式为void GetAllScores(vector<int> &vctScore)，但这并不一定就是我们需要的形式。另外，考虑下面字符串的连接操作：

```cpp
string s1(hello);
string s = s1 +'a'  + 'b' + 'c' + 'd' + 'e';
```
在对s进行初始化时，会产生大量的临时对象，并涉及到大量字符串的拷贝操作，这显然会影响程序的效率和性能。怎么解决这个问题呢？如果我们能确定某个值是一个非常量右值（或者是一个以后不会再使用的左值），则我们在进行临时对象的拷贝时，可以不用拷贝实际的数据，而只是“窃取”指向实际数据的指针（类似于STL中的auto_ptr，会转移所有权）。C++ 11中引入的右值引用正好可用于标识一个非常量右值。C++ 11中用&表示左值引用，用&&表示右值引用，如：

```cpp
&&a = ;
```

右值引用根据其修饰符的不同，也可以分为非常量右值引用和常量右值引用。

非常量右值引用只能绑定到非常量右值，不能绑定到非常量左值、常量左值和常量右值（VS2010 beta版中可以绑定到非常量左值和常量左值，但正式版中为了安全起见，已不允许）。如果允许绑定到非常量左值，则可能会错误地窃取一个持久对象的数据，而这是非常危险的；如果允许绑定到常量左值和常量右值，则非常量右值引用可以用于修改常量左值和常量右值，这明显违反了其常量的含义。

常量右值引用可以绑定到非常量右值和常量右值，不能绑定到非常量左值和常量左值（理由同上）。

有了右值引用的概念，我们就可以用它来实现下面的CMyString类。

```cpp
class CMyString
{
public:
   // 构造函数
CMyString(const char *pszSrc = NULL)
{
 cout << "CMyString(const char *pszSrc = NULL)" << endl;
 if (pszSrc == NULL)
 {
  m_pData = new char[1];
  *m_pData = '\0';
 }
 else
 {
  m_pData = new char[strlen(pszSrc)+1];
  strcpy(m_pData, pszSrc);
 }
}

   // 拷贝构造函数
CMyString(const CMyString &s)
{
 cout << "CMyString(const CMyString &s)" << endl;
 m_pData = new char[strlen(s.m_pData)+1];
 strcpy(m_pData, s.m_pData);
}

   // move构造函数
CMyString(CMyString &&s)
{
 cout << "CMyString(CMyString &&s)" << endl;
 m_pData = s.m_pData;
 s.m_pData = NULL;
}

   // 析构函数
~CMyString()
{
 cout << "~CMyString()" << endl;
 delete [] m_pData;
 m_pData = NULL;
}

   // 拷贝赋值函数
CMyString &operator =(const CMyString &s)
{
 cout << "CMyString &operator =(const CMyString &s)" << endl;
 if (this != &s)
 {
  delete [] m_pData;
  m_pData = new char[strlen(s.m_pData)+1];
  strcpy(m_pData, s.m_pData);
 }
 return *this;
}

   // move赋值函数
CMyString &operator =(CMyString &&s)
{
 cout << "CMyString &operator =(CMyString &&s)" << endl;
 if (this != &s)
 {
  delete [] m_pData;
  m_pData = s.m_pData;
  s.m_pData = NULL;
 }
 return *this;
}

private:
char *m_pData;
};
```

可以看到，上面我们添加了move版本的构造函数和赋值函数。那么，添加了move版本后，对类的自动生成规则有什么影响呢？唯一的影响就是，如果提供了move版本的构造函数，则不会生成默认的构造函数。另外，编译器永远不会自动生成move版本的构造函数和赋值函数，它们需要你手动显式地添加。

当添加了move版本的构造函数和赋值函数的重载形式后，某一个函数调用应当使用哪一个重载版本呢？下面是按照判决的优先级列出的3条规则：
* 常量值只能绑定到常量引用上，不能绑定到非常量引用上。
* 左值优先绑定到左值引用上，右值优先绑定到右值引用上。
* 非常量值优先绑定到非常量引用上。

当给构造函数或赋值函数传入一个非常量右值时，依据上面给出的判决规则，可以得出会调用move版本的构造函数或赋值函数。而在move版本的构造函数或赋值函数内部，都是直接“移动”了其内部数据的指针（因为它是非常量右值，是一个临时对象，移动了其内部数据的指针不会导致任何问题，它马上就要被销毁了，我们只是重复利用了其内存），这样就省去了拷贝数据的大量开销。

一个需要注意的地方是，拷贝构造函数可以通过直接调用`*this = s`来实现，但move构造函数却不能。这是因为在move构造函数中，s虽然是一个非常量右值引用，但其本身却是一个左值（是持久对象，可以对其取地址），因此调用`*this = s`时，会使用拷贝赋值函数而不是move赋值函数，而这已与move构造函数的语义不相符。要使语义正确，我们需要将左值绑定到非常量右值引用上，C++ 11提供了move函数来实现这种转换，因此我们可以修改为`*this = move(s)`，这样move构造函数就会调用move赋值函数。

# Valarray

valarray类似vector，也是一个模板类，其主要被用来对一系列元素进行高速的数字计算，其与vector的主要区别在于以下两点

1. valarray定义了一组在两个相同长度和相同类型的valarray类对象之间的数字计算，例如xarr = cos(yarr) + sin(zarr)；

2. 通过重载operater[]，可以返回valarray的相关信息（valarray其中某个元素的引用、特定下标的值或者其某个子集）。

```cpp
// valarray_acos.cpp
// compile with: /EHsc
#include <valarray>
#include <iostream>
#include <iomanip>

int main( )
{
   using namespace std;
   double pi = 3.14159265359;
   int i;

valarray<double> va1 ( 9 );
   for ( i = 0 ; i < 9 ; i++ )
      va1 [ i ] =  0.25 * i - 1;
   valarray<double> va2 ( 9 );

cout << "The initial valarray is:";
   for (i = 0 ; i < 9 ; i++ )
      cout << " " << va1 [ i ];
   cout << endl;

va2 = acos ( va1 );   //对va1中所有元素求反余弦然后赋值给va2中的对应元素
   cout << "The arccosine of the initial valarray is:/n";
   for (i = 0 ; i < 9 ; i++ )
      cout << setw(10) << va2 [ i ]
         << "  radians, which is  "
         << setw(11) << (180/pi) * va2 [ i ]
         << "  degrees" << endl;
  return 0;
}

```

注意：vc6不支持va2 = acos (va1);操作！

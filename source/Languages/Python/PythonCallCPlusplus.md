# Python 调用 C++

## 推荐方法
[py++](http://www.cnblogs.com/rocketfan/archive/2010/11/30/1892429.html)

## 基础方法

在python中调用C++类成员函数, 如下调用TestFact类中的fact函数,
```cpp
#include <Python.h>

class TestFact{
    public:
    TestFact(){};
    ~TestFact(){};
    int fact(int n);
};

int TestFact::fact(int n)
{
  if (n <= 1)
    return 1;
  else
    return n * (n - 1);
}

int fact(int n)
{
    TestFact t;
    return t.fact(n);
}

PyObject* wrap_fact(PyObject* self, PyObject* args)
{
  int n, result;

  if (! PyArg_ParseTuple(args, "i:fact", &n))
    return NULL;
  result = fact(n);
  return Py_BuildValue("i", result);
}

static PyMethodDef exampleMethods[] =
{
  {"fact", wrap_fact, METH_VARARGS, "Caculate N!"},
  {NULL, NULL}
};

extern "C"              //不加会导致找不到initexample
void initexample()
{
  PyObject* m;
  m = Py_InitModule("example", exampleMethods);
}
```

把这段代码存为wrapper.cpp, 编成so库,

```bash
g++ -fPIC wrapper.cpp -o example.so -shared -I/usr/include/python2.6 -I/usr/lib/python2.6/config
```

然后在有此so库的目录, 进入python, 可以如下使用
```python
import example
example.fact(4)
```

## Boost.Python

Boost库是非常强大的库, 其中的python库可以用来封装c++被python调用, 功能比较强大, 不但可以封装函数还能封装类, 类成员.

http://dev.gameres.com/Program/Abstract/Building%20Hybrid%20Systems%20with%20Boost_Python.CHN.by.JERRY.htm

首先在ubuntu下安装boost.python, `apt-get install libboost-python-dev`
```cpp
#include <boost/python.hpp>
char const* greet()
{
   return "hello, world";
}

BOOST_PYTHON_MODULE(hello)
{
    using namespace boost::python;
    def("greet", greet);
}
```
把代码存为hello.cpp, 编译成so库
``` bash
g++ hello.cpp -o hello.so -shared -I/usr/include/python2.5 -I/usr/lib/python2.5/config -lboost_python-gcc42-mt-1_34_1
```

此处python路径设为你的python路径, 并且必须加-lboost_python-gcc42-mt-1_34_1, 这个库名不一定是这个, 去/user/lib查


然后在有此so库的目录, 进入python, 可以如下使用
```bash
>>> import hello
>>> hello.greet()
'hello, world'
```

## ctypes

ctypes is an advanced ffi (Foreign Function Interface) package for Python 2.3 and higher. In Python 2.5 it is already included.

ctypes allows to call functions in dlls/shared libraries and has extensive facilities to create, access and manipulate simple and complicated C data types in Python - in other words: wrap libraries in pure Python. It is even possible to implement C callback functions in pure Python.

http://python.net/crew/theller/ctypes/


```cpp
#include <Python.h>

class TestFact{
    public:
    TestFact(){};
    ~TestFact(){};
    int fact(int n);
};

int TestFact::fact(int n)
{
  if (n <= 1)
    return 1;
  else
    return n * (n - 1);
}

extern "C"
int fact(int n)
{
    TestFact t;
    return t.fact(n);
}
```

将代码存为wrapper.cpp不用写python接口封装, 直接编译成so库,

```bash
g++ -fPIC wrapper.cpp -o example.so -shared -I/usr/include/python2.6 -I/usr/lib/python2.6/config
```

进入python, 可以如下使用

```bash
>>> import ctypes
>>> pdll = ctypes.CDLL('/home/ubuntu/tmp/example.so')
>>> pdll.fact(4)
```

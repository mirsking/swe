# 嵌套函数与闭包

## 嵌套函数

Python的函数也是对象，Python支持将函数作为参数传入，当然也可以将函数作为返回值返回。

```python
def lazy_sum(*args):
    def sum():
        ax = 0
        for n in args:
            ax = ax + n
        return ax
    return sum
```
当lazy_sum返回函数sum时，相关参数和变量都保存在返回的函数中，这种称为“**闭包（Closure）**”的程序结构拥有极大的威力。


返回的是一个函数对象，
```python
>>> f = lazy_sum(1, 3, 5, 7, 9)
>>> f
<function lazy_sum.<locals>.sum at 0x101c6ed90>
```
只有在实际`f()`调用时，才会真正执行函数。


## 闭包

注意到返回的函数在其定义内部引用了局部变量args，所以，当一个函数返回了一个函数后，其内部的局部变量还被新函数引用，所以，闭包用起来简单，实现起来可不容易。

返回闭包时牢记的一点就是：返回函数不要引用任何循环变量，或者后续会发生变化的变量。

比如以下函数：
```python
def count():
    fs = []
    for i in range(1, 4):
        def f():
             return i*i
        fs.append(f)
    return fs

f1, f2, f3 = count()
```
f1、f2、放调用后，返回的是三个9，而不是`1,4,9`。因为函数在三个函数都计算后才会返回，而计算后i已经变为3。

如果一定要引用循环变量的话，则可以再创建一个函数，用该函数的参数绑定循环变量当前的值，无论该循环变量后续如何更改，已绑定到函数参数的值不变：
```python
def count():
    def f(j):
        def g():
            return j*j
        return g
    fs = []
    for i in range(1, 4):
        fs.append(f(i)) # f(i)立刻被执行，因此i的当前值被传入f()
    return fs
```

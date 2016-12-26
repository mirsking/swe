# Python 函数参数传递


## Python传对象引用

Python不允许程序员选择采用传值还是传址。Python参数传递采用的是“**传对象引用**”的方式。实际上，这种方式相当于传值和传址的一种综合。

如果函数收到的是一个可变对象（比如字典或者列表）的引用，就能修改对象的原始值——相当于传址。如果函数收到的是一个不可变对象（比如数字、字符或者元组）的引用（其实也是对象地址！！！），就不能直接修改原始对象——相当于传值。

所以python的传值和传址是根据传入参数的类型来选择的：

* 传值的参数类型：数字，字符串，元组（immutable）
* 传址的参数类型：列表，字典（mutable）


## 例子
1. 不可变对象
  ```python
  def change(val):
      val = 0
  num = 1
  change(num)
  print(num) #输出结果为1
  ```
2. 可变对象
  ```python
  def change(val):
    val.append(1)
  nums = [0]
  change(nums)
  print(nums) #输出结果是[0,1]
  ```

3. 注意引用的改变
```python
def change(val):
    val.append(100)
    val = ['T', 'Z', 'Y']
nums = [0, 1]
change(nums)
print(nums)#输出是[0,1,100], val赋值过程改变了val的引用，但是没有改变nums的引用
```

# Python Iterator Protocol

Python的Iterator协议定义了两个接口

```python
iterator.__iter__()
```
返回iterator自身。

```python
iterator.next()
```
返回下一个容器元素，如果已经到了容器末尾，则抛出StopIteration异常。

## 容器的`__iter__()`

```python
container.__iter__()
```
返回一个Iterator对象。

## Generator类型

generator有两种：
1. yield实现的函数
2. list generator的`[]`换成`()`

Python的Generator可以方便地实现Iterator协议。如果容器的`__iter__()`实现成了一个generator，就会自动返回一个Iterator对象（技术上来说是一个generator对象）。


## Iterator和Iterable对象
`list`, `dict`, `str`都是Iterable对象，支持for in。
Iterator对象可以用`next()`函数不断取得下一个值。

可以通过`iter()`将Iterable对象转换成一个Iterator对象。

Python的for循环，其实质是对Iterator对象，不断调用`next()`函数进行遍历的。

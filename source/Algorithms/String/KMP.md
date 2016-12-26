# 字符串匹配算法
字符串匹配算法要解决的是在source字符串中查找pattern字符串，并给出pattern在source中出现的index的问题。

## Brute-Force Algorithm
字符串匹配最简单的想法是将pattern对齐source，然后顺序比较是否相同，发生不匹配后将pattern后移一位进行匹配。

```
int search(const string& src, const string& pattern)
{
  if (pattern.empty())
    return -1;
  int n = src.size();
  int m = pattern.size();
  for (int i = 0; i <= n - m; i++)
  {
    int ii = i;
    int j = 0;
    while (j < m && src[ii] == pattern[j])
    {
      ii++;
      j++;
    }
    if (j == m)
    {
      return i;
    }
  }
  return -1;
}
```

## KMP算法
Brute-Force算法虽然写起来简单，但是由于在pattern的每次移位时抛弃了之前匹配的信息，所以效率很低（时间复杂度O(n*m)）。而KMP算法利用发生不匹配前以获取的匹配信息，最大化每次移位的步进，从而是算法时间复杂度达到了O(m+n) = O(n)。

下面是KMP算法容易理解的一种解释：

### 字符串匹配的一些基本概念
给定字符串$$x=x_0x_1...x_{k-1},  k\in N^+$$

1. 前缀(prefix)
$x$的子串：$u = x_0x_1...x_{b-1}, b\in{0,...,k}$

2. 后缀(suffix)
$x$的子串：$u = x_{k-b}x_{k-b+1}...x_{k-1}, b\in{0,...,k}$

3. 真前缀/真后缀(proper prefix/ proper suffix)
前缀或后缀中$u!=x$的那一部分

4. 边界(border)
相等的前缀和后缀：$r=x_0x_1...x_{b-1} =  x_{k-b}x_{k-b+1}...x_{k-1}, b\in{0,...,k}$

> 例子
对于$x = abacab$：
* 真前缀：$\epsilon, a, ab, aba, abac, abaca$
* 真后缀：$\epsilon, b, ab, cab, acab, bacab$
* 边界：$\epsilon, ab$
其中$\epsilon$代表空字符串


### 预处理
KMP算法的核心思想是记录一个数组，这个数组保存着当在pattern第$j$位发生不匹配时，应该将pattern向后移动的步进值相关的信息。这一部分也是算法中最难理解的一部分。

> **定理**：$r, s$都是$x$的边界，且$|r|<|s|$，那么$r$也是$s$的边界。
这个定理可以由下图推得：
![border_theorem](http://7xnluw.com1.z0.glb.clouddn.com/algorithm/kmp/border_theorem.gif)

> **定义**：$r$是字符串$x$的边界，$a$是一个字符。如果$ra$是$xa$的边界，那么我们说边界$r$被$a$扩展为$ra$。
如下图所示
![border_extension](http://7xnluw.com1.z0.glb.clouddn.com/algorithm/kmp/border_extension.gif)

#### 预处理过程
我们用b[m+1]这样一个数组来存储pattern步进信息（有些地方，这个数组叫做next数组），其中m是pattern字符串的长度。
b[i+1]的代表的意义是pattern的长度为$i+1$的前缀$p_0p_1...p_i$的边界。所以他可以由$p_0p_1...p_{i-1}$的边界是否可以有$p_i$扩展获得。
如果能够扩展也就意味着：$p[b[i]] == p[i]$（见下图）。
![border_extension_b_i+1](http://7xnluw.com1.z0.glb.clouddn.com/algorithm/kmp/border_extension_b_i%2B1.gif)

预处理的过程，在一个while循环中用j来存储当前边界的长度
* 如果$p[j] == p[i]$，意味着当前边界可以被$p[i]$扩展。
* 如果$p[j] != p[i]$，意味着当前边界无法被$p[i]$扩展。则通过$j = b[j]$，尝试当前边界的子边界（注意这里的描述）

```
void kmpBorder(const string& pattern, vector<int>& borders)
{
  borders.resize(pattern.size() + 1);
  int i = 0, j = -1;
  borders[0] = -1;

  while (i < pattern.size())
  {
    while (j >= 0 && pattern[i] != pattern[j])
      j = borders[j];
    i++;
    j++;
    borders[i] = j;
  }
}
```

### 搜索过程
搜索过程和Brute-Force类似，只是在发生不匹配时，向右滑动的步进是已经匹配的pattern长度j减去已匹配的pattern前缀的边界长度b[j]。
> 例子
如下图，0~4已经匹配，5处发生c和d的不匹配。而已匹配的部分$abcab$长度为5， 其边界是$ab$长度为2，所以要向右移5-2=3位。
![shift_example](
http://7xnluw.com1.z0.glb.clouddn.com/algorithm/kmp/shift_example.png)

上搜索过程的代码，详细过程见下图。

```
int search(const string& src, const string& pattern)
{
  if (pattern.empty())
    return -1;

  vector<int> borders;
  kmpBorder(pattern, borders);
  int i = 0, j = 0;
  while (i < src.size())
  {
    while (j >= 0 && src[i] != pattern[j])
      j = borders[j];
    i++;
    j++;
    if (j == pattern.size())
    {
      return i-j;
    }
  }

  return -1;
}
```
![search](http://7xnluw.com1.z0.glb.clouddn.com/algorithm/kmp/search.gif)



### 参考链接
[Knuth-Morris-Pratt algorithm](http://www.inf.fh-flensburg.de/lang/algorithmen/pattern/kmpen.htm)

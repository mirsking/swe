## dos和unix文件
DOS/Windows和Linux/Unix的文件换行回车格式不同。
基于 DOS/Windows 的文本文件在每一行末尾有一个 CR（回车）和 LF（换行），而 UNIX 文本只有一个换行。



## 文件格式转换


### 方法一


vim打开后
```
: set ff=unix
```


### 方法二


1. dos转unix

  ```
  sed -e 's/.$//' mydos.txt > myunix.txt
  ```

2. unix转dos

  ```
  sed -e 's/$/\r/' myunix.txt > mydos.txt
  ```

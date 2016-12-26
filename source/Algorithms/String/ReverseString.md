# 字符串翻转

## 翻转句子
`I am Mirs King` --> `King Mirs am I`

$O(n)$， 先每个单词翻转，然后整个字符串翻转。

## 循环右移

`abcedf` --> `edfabc`

找到旋转位置，同上进行翻转。

## 未知大小的文件，翻转整个文件

* 方法1： windows核心编程，内存映射，但是需要知道文件的大小
* 方法2： 每次取少量字节，如256字节，这256字节做反转，写入一个文件，命名为1，而后再读256，同样反转，命名为2，以此类推，直至文件末尾，然后遍历文件夹，找到值最大的文件，将其内容写入结果文件，而后其文件名减一，读到内容，继续写入结果文件末尾，直至将1文件写入，整个文件就反转了
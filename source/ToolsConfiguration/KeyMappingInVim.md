## Vim的模式

* Normal Mode
也就是最一般的普通模式，默认进入vim之后，处于这种模式。

* Visual Mode
一般译作可视模式，在这种模式下选定一些字符、行、多列。
在普通模式下，可以按v进入。

* Insert Mode
插入模式，其实就是指处在编辑输入的状态。普通模式下，可以按i进入。

* Select Mode
在gvim下常用的模式，可以叫作选择模式吧。用鼠标拖选区域的时候，就进入了选择模式。
和可视模式不同的是，在这个模式下，选择完了高亮区域后，敲任何按键就直接输入并替换选择的文本了。
和windows下的编辑器选定编辑的效果一致。普通模式下，可以按gh进入。

* Command-Line/Ex Mode
就叫命令行模式和Ex模式吧。两者略有不同，普通模式下按冒号(:)进入Command-Line模式，可以输入各种命令，
使用vim的各种强大功能。普通模式下按Q进入Ex模式，其实就是多行的Command-Line模式。

## Map，有几个基本的概念

### 命令的组合
同Vim下的其他命令一样，命令的名字往往由好几段组成。前缀作为命令本身的修饰符，微调命令的效果。
对于map而言，可能有这么几种前缀

* nore
表示非递归，见下面的介绍

* n
表示在普通模式下生效

* v
表示在可视模式下生效

* i
表示在插入模式下生效

* c
表示在命令行模式下生效

### Recursive Mapping
递归的映射。其实很好理解，也就是如果键a被映射成了b，c又被映射成了a，如果映射是递归的，那么c就被映射成了b。
```
:map a b
:map c a
```
对于c效果等同于
```
:map c b
```
默认的map就是递归的。如果遇到[nore]这种前缀，比如:noremap，就表示这种map是非递归的。

### unmap
unmap后面跟着一个按键组合，表示删除这个映射。
```
:unmap c
```
那么在map生效模式下，c不再被映射到a上。

同样，unmap可以加各种前缀，表示影响到的模式。

### mapclear
mapclear直接清除相关模式下的所有映射。
同样，mapclear可以加各种前缀，表示影响到的模式。

## map小结
这里列出常用的一些map命令，默认map命令影响到普通模式和可视模式。

| map  | noremap  | unmap  | mapclear  |
|------|----------|--------|-----------|
| nmap | nnoremap | nunmap | nmapclear |
| vmap | vnoremap | vunmap | vmapclear |
| imap | inoremap | iunmap | imapclear |
| cmap | cnoremap | cunmap | cmapclear |

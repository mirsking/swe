# 红黑树
参考链接[红黑树 维基百科](https://zh.wikipedia.org/wiki/%E7%BA%A2%E9%BB%91%E6%A0%91)
![Red-black_tree_example](http://7xnluw.com1.z0.glb.clouddn.com/data_structure/tree/Red-black_tree_example.svg.png)
1. 红黑树的五大规则
  * 节点非黑即红
  * 根是黑色的
  * 所有叶子都是黑色（叶子是NIL节点）
  * 每个红色节点必须有两个黑色的子节点（保证红色节点不相邻）
  * 从任一节点到其每个叶子的所有简单路径经过的黑色节点数目相同

2. 红黑树的插入
树节点的定义
```cpp
enum {
    BLACK,
    RED
};

struct node {
    int color;
    node *parent, *left, *right;
};
```
找到叔叔节点和祖父节点
```cpp
node* grandparent(node *n){
     return n->parent->parent;
}

node* uncle(node *n){
     if(n->parent == grandparent(n)->left)
         return grandparent (n)->right;
     else
         return grandparent (n)->left;
}
```

我们首先以二叉查找树的方法增加节点并标记它为**红色**（如果设为黑色，就会导致根到叶子的路径上有一条路上，多一个额外的黑节点，这个是很难调整的。但是设为红色节点后，可能会导致出现两个连续红色节点的冲突，那么可以通过颜色调换（color flips）和树旋转来调整。）。
  * 情形1  新节点N位于树的根上，没有父节点
  直接将父节点染黑
  ```cpp
  void insertCase1(node* n)
  {
    if(n->parent == NULL)
      n->color = BLACK;
    else
      insertCase2(n);
  }
  ```

  * 情形2 新节点的父节点是黑的
  直接返回
  ```cpp
  void insertCase2(node* n)
  {
    if(n->parent->color == BLACK)
      return;
    else
      insertCase3(n);
  }
  ```

  * 情形3 新节点的父节点是红的，新节点的叔叔节点是红的
  ![Red-black_tree_insert_case_3](http://7xnluw.com1.z0.glb.clouddn.com/data_structure/tree/Red-black_tree_insert_case_3.png)
  将父节点与叔叔节点染黑，祖父节点染红，递归更新祖父节点
  ```cpp
  void insertCase3(node* n)
  {
    if(uncle(n) != NULL && uncle(n)->color == RED)
    {
      n->parent->color = BLACK;
      uncle(n)->color = BLACK;
      grandparent(n)->color = RED;
      insertCase1(grandparent(n));
    }
    else
    {
      insertCase4_1(n);
    }
  }
  ```

  * 情形4 新节点的父节点是红的，新节点的叔叔节点是黑的
    * 情形4.1 新节点在父节点的左右方向和父节点在祖父节点的左右方向不同
    ![Red-black_tree_insert_case_4_1](http://7xnluw.com1.z0.glb.clouddn.com/data_structure/tree/Red-black_tree_insert_case_4_1.png)
     对父节点下的子树进行一次左旋或右旋，转换为情形4.2（注意这一步不需要变色）
      ```cpp
       void insertCase4_1(node* n)
       {
         if(n == n->parent->right && n->parent == grandparent(n)->left)
         {
            rotateLeft(n->parent);
            n = n->left;
         }
         else if( n == n->parent->left && n->parent == grandparent(n)->right)
          {
            rotateRight(n->parent);
            n = n->right;
          }
          insertCase4_2(n);
       }
      ```
    * 情形4.2 新节点在父节点的左右方向和父节点在祖父节点的左右方向相同
    ![Red-black_tree_insert_case_4_2](http://7xnluw.com1.z0.glb.clouddn.com/data_structure/tree/Red-black_tree_insert_case_4_2.png)
    将父节点染黑，祖父节点染红，对祖父节点进行一次左旋或右旋
    ```cpp
    void insertCase4_2(node* n)
    {
      n->parent->color = BLACK;
      grandparent(n)->color = RED:
      if(n== n->parent->left && n->parent == grandparent(n)->left)
      {
        rotateRight(grandparent(n));
      }
      else // if(n==n->parent->right && n->parent == grandparent(n)->right)
      {
        rotateLeft(grandparent(n));
      }
    }
    ```

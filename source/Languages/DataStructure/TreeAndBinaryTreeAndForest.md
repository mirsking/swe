# 树、二叉树和森林

## 二叉树转换为森林
假如一棵二叉树的根节点有右孩子，则这棵二叉树能够转换为森林，否则将转换为一棵树。
1. 从根节点开始，若右孩子存在，则把与右孩子结点的连线删除。再查看分离后的二叉树，若其根节点的右孩子存在，则连线删除…。直到所有这些根节点与右孩子的连线都删除为止。
2. 将每棵分离后的二叉树转换为树。

![BT2Forest](http://7xnluw.com1.z0.glb.clouddn.com/data_structure/tree/BT2Forest.jpg)


## 二叉树转换为树
是树转换为二叉树的逆过程。
1. 加线。若某结点X的左孩子结点存在，则将这个左孩子的右孩子结点、右孩子的右孩子结点、右孩子的右孩子的右孩子结点…，都作为结点X的孩子。将结点X与这些右孩子结点用线连接起来。
2. 去线。删除原二叉树中所有结点与其右孩子结点的连线。
3. 层次调整。
![BT2Tree]( http://7xnluw.com1.z0.glb.clouddn.com/data_structure/tree/BT2Tree.jpg)


## 算法
1. 二叉树是否存在一条由根到叶子的路径来保证路径节点之和等于给定值
    ```cpp
    bool existPath(TreeNode* root, int target)
    {
        if (!root)
            return target == 0 ? true : false;

        // important, ensure node that only has one child will not be handle as leaf node.
        if (target == root->val)
        {
            if (!root->left && !root->right)
                return true;
            else
                return false;
        }

        return existPath(root->left, target - root->val)
            || existPath(root->right, target - root->val);
    }
    ```

2. 二叉搜索树（BST）的构型种类
  设n个节点的二叉树有f(n)种。
  N个节点,其中1个为根节点,则剩下有n-1个节点,这n-1个节点可以：

    * 0个作为根节点的左子树（1种方法）,n-1个节点作为根节点的右子树（f(n-1)种方法）
    * 1个节点作为左子树（1种方法）,n-2个节点作为右子树（f(n-2)种方法）
    * 2个节点作为左子树（f(2)种方法）,n-3个节点作为右子树（f(n-3)种方法）

    以此类推，把这些情况全部加起来：
  $f(n) = f(0)*f(n-1) + f(1)*f(n-2) + ...+ f(n-2)*f(1) + f(n-1)*f(0)$
  其中$f(0) = f(1) = 1$
  这个数叫做卡特兰数

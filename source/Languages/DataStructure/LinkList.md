# 链表的各种操作

## 单链表
### 查找
### 删除
1. 只知道要删除节点的值
遍历找到该节点
2. 只知道要删除节点的地址
    * 对于非最后一个节点，直接把后边的内容拷贝到前边，然后删除下一个节点。
    * 对于最后一个节点，遍历链表，找到前一个节点，置NULL其next指针。
    ```cpp
    Node* removeNode(Node* head, Node* node)
    {
        if (head == NULL || node == NULL)
            return NULL;

        if (node->next != NULL)
        {
            Node* p1 = node->next;
            node->val = p1->val;
            node->next = p1->next;
            //delete p1;
            return head;
        }
        else
        {
            Node* parent = head;
            while (parent!= NULL && parent->next != node)
            {
                parent = parent->next;
            }

            if (parent == NULL)
                return NULL;

            parent->next = NULL;
            // delete node;
            return head;
        }
    }
    ```

3. 删除重复节点（重复节点保留一个）
    ```cpp
    ListNode* deleteDuplicates(ListNode* head) {
        if(!head || !head->next)
            return head;

        ListNode* p1= head;
        ListNode* p2 = head->next;

        while(!p2)
        {
            if(p2->val == p1->val)
            {
                p1->next = p2->next;
                ListNode* tmp = p2;
                p2 = p2->next;
                delete tmp;
            }
            else
            {
                p1 = p1->next;
                p2 = p2->next;
            }
        }
        return head;
    }
    ```

4. 删除重复节点（重复节点不保留）
    ```cpp
    ListNode* deleteDuplicates(ListNode* head) {
        if (!head || !head->next)
            return head;
        ListNode* p = head->next;
        if (head->val == p->val)
        {
            while (p && head->val == p->val)
            {
                ListNode* tmp = p;
                p = p->next;
                delete tmp;
            }
            delete head;
            return deleteDuplicates(p);
        }
        else
        {
            head->next = deleteDuplicates(head->next);
            return head;
        }
    }
    ```

### 插入
### 反转
```
Node* reverse(Node* head)
{
    if (head == NULL)
        return NULL;

    Node* p0 = NULL;
    Node* p1 = head;
    Node* p2 = head->next;

    while (p2 != NULL)
    {
        p1->next = p0;
        Node* tmp = p2->next;
        p2->next = p1;
        p0 = p1;
        p1 = p2;
        p2 = tmp;
    }

    return p1;
}
```

### 相关算法
1. 有序链表合并
2. 链表判断是否有环，环的入口
    * 是否有环：快慢指针，如果相遇则必有环
    * 环的入口：如下图
        + 链表头到环入口有K步，则慢指针在走到环的入口时，快指针在环中走了k步。所以慢指针落后了快指针k步，也可以说快指针落后了慢指针L-K步。
        + 因为快慢指针没走一次，快指针追上慢指针一步，所以在走L-K步，两指针相遇。所以两指针相遇的地方距环的入口有L-k步（慢指针走过的步数），也就是说慢指针再走k步就又到了环的入口。
        + 这时从链表头和两指针相遇的地方同时走，相遇的地方就是环的入口。
    ![circle_in_link_list](http://7xnluw.com1.z0.glb.clouddn.com/algorithm/circle_in_link_list.png)
    ![circle_in_link_list2](http://7xnluw.com1.z0.glb.clouddn.com/algorithm/circle_in_link_list2.png)



3. 两个链表是否相交（快慢指针）
    * [判断两个链表是否相交并找出交点](http://blog.csdn.net/jiqiren007/article/details/6572685)
    * 方法1：计算两个链表长度len1， len2。利用|len1 - len2|将两个链表对齐，然后遍历两个链表。
    * 方法2：将第一个链表的尾指向第一个链表的头，如果两个链表相交，则新链表一定有环。则转换为快慢指针判断链表是否有环的问题。
    ![link_list_cross](http://7xnluw.com1.z0.glb.clouddn.com/algorithm/link_list_cross.gif)


## 双向链表
### 节点的定义
### 插入
### 删除
### 查找

## 问题描述

Given an singly linked where elements are sorted in ascending order, convert it to a height balanced BST.</br>

For this problem, a height-balanced binary is defined as a binary tree in which the depth of the two subtrees of ***every*** node never differ by more than 1.

#### Example:<br>
<pre>Given the sorted linked list: [-10,-3,0,5,9],

One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:

      0
     / \
   -3   9
   /   /
 -10  5
</pre>
__Explanation:__<br>

给定一个单向有序链表，生成一颗平衡的二叉搜索树。

#### Solution

* 由于列表是有序的，将其分为左右两部分即可保证<code>BST</code>.

* 从中间位置切分列表，即可保证 ***balanced***。

* 对于左右两个部分，递归即可。

* 上述部分与[Convert Sorted Array to Binary Search Tree](https://github.com/myskety/leetcode/blob/master/108-Convert%20Sorted%20Array%20to%20Binary%20Search%20Tree.md)一样，但这里的数据结构是链表，需要寻找中间位置。

* 使用快慢指针寻找中间位置，慢指针一次走一步，快指针一次走两步。

#### AC code

```
class Solution {
public:
    TreeNode* sortedListToBST(ListNode* head) {
        if(head == NULL)
            return NULL;
        if(head->next == NULL)
            return new TreeNode(head->val);
        ListNode *slow = head, *fast = head, *pre = head;
        while(fast->next && fast->next->next){
            pre = slow;
            slow = slow->next;
            fast = fast->next->next;
        }
        fast = slow->next;
        pre->next = NULL;
        TreeNode *cur = new TreeNode(slow->val);
        if(slow != head)
            cur->left = sortedListToBST(head);
        cur->right = sortedListToBST(fast);
        return cur;
    }
};
```
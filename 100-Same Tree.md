## 问题描述

Given two binary trees, write a function to check if they are the same or not.</br>

Two binary trees are considered the same if they are structurally identical and the nodes have the same value.
#### Example 1
<pre><strong>Input:</strong>     1         1
          / \       / \
         2   3     2   3

        [1,2,3],   [1,2,3]

<strong>Output:</strong> true
</pre>
#### Example 2
<pre><strong>Input:</strong>     1         1
          /           \
         2             2

        [1,2],     [1,null,2]

<strong>Output:</strong> false
</pre>
#### Example 3
<pre><strong>Input:</strong>     1         1
          / \       / \
         2   1     1   2

        [1,2,1],   [1,1,2]

<strong>Output:</strong> false
</pre>

__Explanation:__<br>

判断两颗二叉树是否相同

#### Solution

* 递归即可

#### AC code

```
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if(p == NULL && q == NULL)
            return true;
        if(p != NULL && q != NULL)
            return p->val == q->val && isSameTree(p->left, q->left) && isSameTree(p->right, q->right);
        return false;
    }
};
```
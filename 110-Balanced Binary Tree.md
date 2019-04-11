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

判断二叉树是否是平衡二叉树。

#### Solution

* 最直观的想法，我们找出左右子树的深度，看差值是否小于等于1.

#### AC code

```
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        if(!root)
            return true;
        if(abs(getdepth(root->left) - getdepth(root->right)) > 1)
            return false;
        return isBalanced(root->left) && isBalanced(root->right);
    }
    int getdepth(TreeNode* node){
        if(!node)
            return 0;
        return 1 + max(getdepth(node->left), getdepth(node->right));
    }
};
```

#### Solution

* 上面那种解法存在结点被重复访问的情况，不高效。

* 优化角度可以从子树入手，当我们发现子树不平衡时直接返回结果就好了。

#### AC code

```
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        return helper(root) == -1 ? false : true;
    }
    int helper(TreeNode* node){
        if(!node)
            return 0;
        int left = helper(node->left);
        if(left == -1)
            return -1;
        int right = helper(node->right);
        if(right == -1)
            return -1;
        if(abs(left - right) > 1)
            return -1;
        return 1 + max(left, right);
    }
};
```
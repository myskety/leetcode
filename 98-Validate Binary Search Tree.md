## 问题描述

Given a binary tree, determine if it is a valid binary search tree (BST).</br>

Assume a BST is defined as follows:

* The left subtree of a node contains only nodes with keys ***less than*** the node's key.
* The right subtree of a node contains only nodes with keys ***greater than*** the node's key.
* Both the left and right subtrees must also be the binary search trees.

#### Example 1
<pre>
Input: 
    2
   / \
  1   3
Output: true
</pre>
#### Example 2
<pre>
Input: 
    5
   / \
  1   4
     / \
    3   6
Output: false
Explanation: The input is: [5,1,4,null,null,3,6]. The root node's value is 5 but its right child's value is 4.
</pre>
__Explanation:__<br>

判断一颗二叉树是不是二叉搜索树。

#### Solution

* 二叉搜索树的中序遍历是递增序列

* 以中序遍历方式遍历二叉树，同时记下前驱结点的值，遍历到某一结点时比较两者大小，若前驱结点的值大于或等于当前结点，则返回<code>false</code>。

#### AC code

```
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        TreeNode *cur = root, *pre = NULL;
        stack<TreeNode*> s;
        while(cur || !s.empty()){
            while(cur){
                s.push(cur);
                cur = cur->left;
            }
            cur = s.top();
            s.pop();
            if(pre && pre->val >= cur->val)
                return false;
            pre = cur;
            cur = cur->right;
        }
        return true;
    }
};
```

#### Recursive Solution

* 利用二叉搜索树的本身性质，左 < 根 < 右。

* 使用二个变量记录某一子树中结点值应属范围，初始化<code>left = LONG_MIN; right = LONG_MAX;</code>，在访问完根结点后，修改这一范围并递归左右子树即可。

#### AC code

```
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        return helper(root, LONG_MIN, LONG_MAX);
    }
    bool helper(TreeNode* root, long left, long right){
        if(!root)
            return true;
        if(root->val >= right || root->val <= left)
            return false;
        return helper(root->left, left, root->val) && helper(root->right, root->val, right);
    }
};
```
#### Morris Solution

* 既然中序遍历对于本题是work的，那么之前提到过的Morris traversal也可以使用。

* 思想依然是记录前驱结点，并与当前访问结点比较。

#### AC code

```
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        if(!root)
            return true;
        TreeNode *pre = NULL, *parent = NULL, *cur = root;
        bool ans = true;
        while(cur){
            if(!cur->left){
                if(parent && parent->val >= cur->val)
                    ans = false;
                parent = cur;
                cur = cur->right;
            }
            else{
                pre = cur->left;
                while(pre->right && pre->right != cur)
                    pre = pre->right;
                if(!pre->right){
                    pre->right = cur;
                    cur = cur->left;
                }else{
                    pre->right = NULL;
                    if(parent && parent->val >= cur->val)
                        ans = false;
                    parent = cur;
                    cur = cur->right;
                }
            }
        }
        return ans;
    }
  
};
```

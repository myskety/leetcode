## 问题描述

Given a binary tree, find its minimum depth.</br>

The minimum depth is the number of nodes alpng the shortest path from the root node down to the mearest leaf node.<br>

__Note:__ A leaf is a node with no children.

#### example:<br>
Given binary tree <code>[3,9,20,null,null,15,7]</code>,
<pre>    3
   / \
  9  20
    /  \
   15   7
</pre>
</pre>
return its depth = 2

__Explanation:__<br>

求二叉树的最小深度。

#### Non-recursive Solution

* 二叉树的层次遍历，每遍历一层就将深度加一，当碰到某一结点没有孩子时，停止遍历。

#### AC code

```
class Solution {
public:
    int minDepth(TreeNode* root) {
        if(!root)
            return 0;
        queue<TreeNode*> s;
        s.push(root);
        int ans = 0;
        while(!s.empty()){
            ans += 1;
            for(int i = s.size()-1; i >= 0; i--){
                TreeNode* tmp = s.front();
                s.pop();
                if(!tmp->left && !tmp->right)
                    return ans;
                if(tmp->left)
                    s.push(tmp->left);
                if(tmp->right)
                    s.push(tmp->right);
            }
        }
        return -1;
    }
};
```

#### Recursive Solution

* 这种类型的题目一般都少不了DFS递归解法。

* 结点为空时直接返回0.

* 左子树为空时，返回 <code>1 + depth of right child</code>。

* 右子树为空时，返回 <code>1 + depth of left child</code>。

* 左右子树都存在时，返回 <code>1 + min(depth of right child, depth of left child)</code>。

#### AC code

```
class Solution {
public:
    int minDepth(TreeNode* root) {
        if(!root)
            return 0;
        if(!root->left)
            return 1 + minDepth(root->right);
        if(!root->right)
            return 1 + minDepth(root->left);
        return 1 + min(minDepth(root->left), minDepth(root->right));
    }
};
```
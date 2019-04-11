## 问题描述

Given a binary tree, find its maximum depth.</br>

The maximum depth is the number of nodes alpng the longest path from the root node down to the farthest leaf node.<br>

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
return its depth = 3

__Explanation:__<br>

求二叉树的最大深度。

#### Non-recursive Solution

* 二叉树的层次遍历，每遍历一层就将深度加一。

#### AC code

```
class Solution {
public:
    int maxDepth(TreeNode* root) {
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
                if(tmp->left)
                    s.push(tmp->left);
                if(tmp->right)
                    s.push(tmp->right);
            }
        }
        return ans;
    }
};
```

#### Recursive Solution

* DFS递归解法。

#### AC code

```
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(!root)
            return 0;
        return 1 + max(maxDepth(root->elft), maxDepth(root->right));
    }
    
};
```
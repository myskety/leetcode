## 问题描述

Given a binary tree, return the ***level order*** traversal of its nodes' values. (ie, from left to right, level by level).</br>

For example:<br>
Given binary tree <code>[3,9,20,null,null,15,7]</code>,
<pre>    3
   / \
  9  20
    /  \
   15   7
</pre>
</pre>
return its level order traversal as:
<pre>[
  [3],
  [9,20],
  [15,7]
]
</pre>
__Explanation:__<br>

二叉树的层次遍历。

#### Solution

* 借助于队列，遍历一层时，将队列中的结点依次出队加入到<code>tmp</code>数组中，出队时若结点左孩子不为空，将其左孩子入队，若右孩子不为空，则右孩子入队。

#### AC code

```
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        if(!root)
            return ans;
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            int size = q.size();
            vector<int> tmp;
            for(int i = 0; i < size; i++){
                TreeNode *p = q.front();
                q.pop();
                tmp.push_back(p->val);
                if(p->left)
                    q.push(p->left);
                if(p->right)
                    q.push(p->right);
            }
            ans.push_back(tmp);
        }
        return ans;
    }
};
```
#### Recursive Solution

* 同其他遍历一样，层次遍历也有其递归写法。

* 递归使得我们会按照深度优先的策略处理左子结点，因此处理某个结点的时候需要变量<code>level</code>记录当前深度。

```
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        if(!root)
            return ans;
        helper(root, 0, ans);
        return ans;
    }
    void helper(TreeNode* node, int level, vector<vector<int>>& ans){
        if(!node)
            return;
        if(ans.size() == level)
            ans.push_back({});
        ans[level].push_back(node->val);
        if(node->left)
            helper(node->left, level+1, ans);
        if(node->right)
            helper(node->right, level+1, ans);
    }
};
```
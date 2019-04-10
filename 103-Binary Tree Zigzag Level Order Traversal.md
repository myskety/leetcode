## 问题描述

Given a binary tree, return the ***zigzag level order*** traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).</br>

For example:<br>
Given binary tree <code>[3,9,20,null,null,15,7]</code>,
<pre>    3
   / \
  9  20
    /  \
   15   7
</pre>
</pre>
return its zigzag level order traversal as:
<pre>[
  [3],
  [20,9],
  [15,7]
]
</pre>
</pre>
__Explanation:__<br>

二叉树的层次遍历。不过是 ***Z*** 字形的。

#### Solution

* 使用两个栈。
* 遍历一层时，根据层数的奇偶性选择将左右孩子进入栈1或者将右左孩子进入栈2。

#### AC code

```
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        int flag = 0;
        stack<TreeNode*> one;
        stack<TreeNode*> two;
        vector<vector<int>> ans;
        if(!root)
            return ans;
        one.push(root);
        while(!one.empty() || !two.empty()){
            vector<int> levelnum;
            if(flag % 2 == 0){
                for(int i = one.size(); i > 0; i--){
                    TreeNode *tmp = one.top();
                    one.pop();
                    levelnum.push_back(tmp->val);
                    if(tmp->left)
                        two.push(tmp->left);
                    if(tmp->right)
                        two.push(tmp->right);  
                }
            }else{
                for(int i = two.size(); i > 0; i--){
                    TreeNode *tmp = two.top();
                    two.pop();
                    levelnum.push_back(tmp->val);
                    if(tmp->right)
                        one.push(tmp->right);
                    if(tmp->left)
                        one.push(tmp->left);
                }
            }
            flag++;
            ans.push_back(levelnum);
        }
        return ans;
    }
};
```
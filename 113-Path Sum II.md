## 问题描述

Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.</br>

__Note:__ A leaf is a node with no children.

#### example:<br>
Given the below binary tree and <code>sum = 22</code>,
<pre>      <strong>5</strong>
     <strong>/</strong> \
    <strong>4</strong>   8
   <strong>/</strong>   / \
  <strong>11</strong>  13  4
 /  <strong>\</strong>      \
7    <strong>2</strong>      1
</pre>
return：
<pre>[
   [5,4,11,2],
   [5,8,4,5]
]
</pre>

__Explanation:__<br>

求二叉树中是否存在一条根到叶的路径上的结点之和等于给定的某个值，返回路径集合。

#### DFS Solution

* 简单的DFS，访问到叶节点时检查是否满足条件。

* 这里需要保存路径。

* 访问到非叶结点时，根据是否有左右子结点，递归子结点，左右子树处理完毕之后，将当前结点从列表中删除。

#### AC code

```
class Solution {
public:
    vector<vector<int>> pathSum(TreeNode* root, int sum) {
        if(!root)
            return {};
        vector<vector<int>> ans;
        helper(root, sum, {}, ans);
        return ans;
    }
    void helper(TreeNode* node, int sum, vector<int> tmp, vector<vector<int>>& ans){
        if(!node->left && !node->right){
            if(sum == node->val){
                tmp.push_back(node->val);
                ans.push_back(tmp);
            }
            return;
        }else{
            sum = sum - node->val;
            tmp.push_back(node->val);
            if(node->left)
                helper(node->left, sum, tmp, ans);
            if(node->right)
                helper(node->right, sum, tmp, ans);
            tmp.pop_back();
        }
    }
};
```

#### Non-recursive Solution

* 中序遍历，使用<code>vector</code>记录走过的每个结点。

* 先找到最左子结点，若其为叶结点，比较当前路径和是否满足条件：
  * 若满足条件，则将<code>vector</code>中的路径记作一个解
  * 若不满足条件：
    * 如果它有右孩子，并且右孩子不是前驱结点(这里的前驱结点不是指中序遍历中的前驱结点)，则指针指向右孩子
    * 否则的话，更新前驱结点，路径和减去当前结点值，路径中删除尾结点，当前结点置NULL，相当于网上回溯。

#### AC code

```
class Solution {
public:
    vector<vector<int>> pathSum(TreeNode* root, int sum) {
        if(!root)
            return {};
        vector<vector<int>> ans;
        vector<TreeNode*> s;
        int val = 0;
        TreeNode *pre = NULL, *cur = root;
        while(cur || !s.empty()){
            while(cur){
                s.push_back(cur);
                val += cur->val;
                cur = cur->left;
            }
            cur = s.back();
            if(!cur->left && !cur->right && val == sum){
                vector<int> tmp;
                for(auto node : s){
                    tmp.push_back(node->val);
                }
                ans.push_back(tmp);
            }
            if(cur->right && cur->right != pre)
                cur = cur->right;
            else{
                pre = cur;
                val -= cur->val;
                s.pop_back();
                cur = NULL;
            }
        }
        return ans;
    }
};
```
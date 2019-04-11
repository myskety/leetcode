## 问题描述

Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.</br>

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
return true, as there exist a root-to-leaf <code>5->4->11->2</code> which sum is <code>22</code>.

__Explanation:__<br>

求二叉树中是否存在一条根到叶的路径上的结点之和等于给定的某个值。

#### DFS Solution

* 简单的DFS，访问到叶节点时检查是否满足条件。

#### AC code

```
class Solution {
public:
    bool hasPathSum(TreeNode* root, int sum) {
        bool ans = false;
        if(!root)
            return false;
        helper(root, sum, ans);
        return ans;
    }
    void helper(TreeNode* node, int sum, bool& ans){
        if(!node->left && !node->right){
            if(sum == node->val){
                ans = true;
            }
            return;
        }else{
            sum = sum - node->val;
            if(node->left)
                helper(node->left, sum, ans);
            if(node->right)
                helper(node->right, sum, ans);
        }
    }
};
```

#### Non-recursive Solution

* 先序遍历，因为只有先序遍历是从根节点开始的。

* 每访问一个结点时，子结点要加上父节点的值，当遍历到叶结点时就可以判断是否存在要求的路径。

#### AC code

```
class Solution {
public:
    bool hasPathSum(TreeNode* root, int sum) {
        if(!root)
            return false;
        stack<TreeNode*> s;
        s.push(root);
        while(!s.empty()){
            # 访问结点，并判断它是不是叶结点
            TreeNode *cur = s.top();
            s.pop();
            if(!cur->left && !cur->right){
                if(cur->val == sum)
                    return true;
            }
            if(cur->right){
                cur->right->val += cur->val;
                s.push(cur->right);
            }
            if(cur->left){
                cur->left->val += cur->val;
                s.push(cur->left);
            }
        }
        return false;
    }
};
```
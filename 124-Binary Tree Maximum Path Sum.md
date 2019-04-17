## 问题描述

Given a ***non-empty*** binary tree, find the maximum path sum.</br>


For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain ***at least one node*** and does not need to go through the root.
#### Example 1:<br>
<pre><strong>Input:</strong> [1,2,3]

       <strong>1</strong>
      <strong>/ \</strong>
     <strong>2</strong>   <strong>3</strong>

<strong>Output:</strong> 6
</pre>
#### Example 2:<br>
<pre><strong>Input:</strong> [-10,9,20,null,null,15,7]

&nbsp;  -10
&nbsp; &nbsp;/ \
&nbsp; 9 &nbsp;<strong>20</strong>
&nbsp; &nbsp; <strong>/ &nbsp;\</strong>
&nbsp; &nbsp;<strong>15 &nbsp; 7</strong>

<strong>Output:</strong> 42
</pre>

__Explanation__:<br>
求二叉树中的最大路径和。

#### DFS Solution

* 对于每个结点需要知道经过其左子结点的路径和大还是经过右子结点的路径和大。

* 定义递归函数返回值为以当前结点为根结点的最大路径之和

#### AC code

```
class Solution {
public:
    int maxPathSum(TreeNode* root) {
        int ans = INT_MIN;
        helper(root, ans);
        return ans;
    }
    inr helper(TreeNode *node, int& ans){
        if(!node)
            return 0;
        int left = max(helper(node->left), 0);
        int right = max(helper(node->right), 0);
        ans = max(ans, left + right + node->val);
        return max(left, right) + node->val;
    }
};
```
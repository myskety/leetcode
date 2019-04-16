## 问题描述

Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.</br>

#### Example:<br>
Given the following triangle:
<pre>[
     [<strong>2</strong>],
    [<strong>3</strong>,4],
   [6,<strong>5</strong>,7],
  [4,<strong>1</strong>,8,3]
]
</pre>
The minimum path sum from top to bottom is <code>11 (i.e., 2 + 3 + 5 + 1 = 11)</code>.<br>

__Note:__
Bonus point if you are able to do this using only ***O(n)*** extra space, where ***n*** is the total number of rows in the triangle.<br>

__Explanation__:<br>
从上到下最短的路径和，只能走相邻的结点。

#### DFS Solution

* 对于每个结点需要直到讲过其左子结点的路径和大还是经过右子结点的路径和大。

* 定义递归函数返回值为以当前结点为根结点，到叶结点的最大路径之和

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
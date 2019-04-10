## 问题描述

Given an integer ***n***, generate all structurally unique ***BST's*** (binary search tree) that store values ***1...n***.</br>

#### Example
<pre><strong>Input:</strong> 3
<strong>Output:</strong>
[
&nbsp; [1,null,3,2],
&nbsp; [3,2,null,1],
&nbsp; [3,1,null,null,2],
&nbsp; [2,1,3],
&nbsp; [1,null,2,null,3]
]
<strong>Explanation:</strong>
The above output corresponds to the 5 unique BST's shown below:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
</pre>

__Explanation:__<br>

求由***1...n***组成的所有的二叉搜索树的集合。

#### Recursive Solution

* 老方法，关于求全部解的问题一般来说递归都能work。

* 切入点就是根节点的值，***start-end (initially 1-n)*** 每个值都可以当作根节点，确定根节点之后左子树和右子树的组成数字也随之确定，递归调用生成左右子树即可，然后合并答案。

#### AC code

```
class Solution {
public:
    vector<TreeNode*> generateTrees(int n){
        if(n == 0)
        return {};
        return helper(1, n);
    }
    vector<TreeNode*> helper(int start, int end){
        vector<TreeNode*> tmptree = new vector<TreeNode*>();
        if(start > end)
            tmptree.push_back(NULL);
        else{
            for(int i = start; i <= end; i++){
                vector<TreeNode*> left = helper(start, i-1);
                vector<TreeNode*> right = helper(i+1, end);
                for(int j = 0; j < left.size(); j++){
                    for(int k = 0; k < right.size(); k++){
                        TreeNode *root = new TreeNode(i);
                        root->left = left[j];
                        root->right = right[k];
                        tmptree.push_back(root);
                    }
                }
            }
            
        }
        return tmptree;
    }
};
```

#### Dp Solution

* 这道题也是有dp解法的。

* <code>vector<vector<TreeNode*>> ans(n+1, vector<TreeNode*>(NULL)</code>，使用<code>dp[i]</code>记录<code>1-i</code>这<code>i</code>个数字生成的二叉搜索树集合。
* <code>i</code>从<code>1-n</code>变化。<code>j</code>从<code>1-i</code>变化
* 需要注意的一点是，左子树可以直接连到根节点上，但右子树上的值都要先加上<code>j</code>才能连到根节点。因为<code>dp</code>说到底还是从个数上讲的，<code>1-3</code>与<code>4-6</code>在我们这里是相同的。所以在存入结果之前需要调整。

#### AC code

```
class Solution {
public:
    vector<TreeNode*> generateTrees(int n){
        vector<vector<TreeNode*>> dp(n+1, vector<TreeNode*>(NULL)；
        if(n == 0)
            return dp[0];
        dp[0].push_back(NULL);
        for(int i = 1; i <= n; i++){
            for(int j = 1; j <= i; j++){ // j索引的是根节点的值
                int left_length = dp[j-1].size();
                int right_length = dp[i-j].size();
                for(int m = 0; m < left_length; m++){
                    for(int k = 0; k < right_length; k++){
                        TreeNode *root = new ListNode(j);
                        root->left = dp[j-1][m];
                        root->right = clone(dp[i-j], j);
                        dp[i].push_back(root);
                    }
                }
            }
        }
        return dp[n];
    }
    TreeNode* clone(TreeNode* right, int move){
        if(!right)
            return NULL;
        TreeNode* cur = new TreeNode(right->val + move);
        cur->left = clone(right->left, move);
        cur->right = clone(right->right, move);
        return cur;
    }
};
```

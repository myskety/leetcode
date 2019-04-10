## 问题描述

Given an integer ***n***,  how many structurally unique ***BST's*** (binary search tree) that store values ***1...n***.</br>

#### Example
<pre><strong>Input:</strong> 3
<strong>Output: 5</strong>
<strong>Explanation:</strong>
Given n = 3, there are a total of 5 unique BST's:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
</pre>

__Explanation:__<br>

求由***1...n***组成的所有的二叉搜索树数目。

#### Recursive Solution

* 老方法，关于求全部解的问题一般来说递归都能work。

* 切入点就是根节点的值，***start-end (initially 1-n)*** 每个值都可以当作根节点，确定根节点之后左子树和右子树的组成数字也随之确定，递归调用生成左右子树即可，然后合并答案。

* 缺点是耗时。

#### AC code (Time Limit Exceeded when n = 19)

```
class Solution {
public:
    int numTrees(int n) {
        return helper(1, n);
    }
    int helper(int start, int end){
        int ans = 0;
        if(star > end)
            ans += 1;
        else{
            for(int i = start; i <= end; i++){
                int left = helper(start, i-1);
                int right = helper(i+1, right);
                ans += left * right;
            }
        }
        return ans;
    }
};
```

#### Dp Solution

* <code>vector<int> ans(n+1, 0)</code>，使用<code>dp[i]</code>记录<code>1-i</code>这<code>i</code>个数字生成的二叉搜索树数目。
* <code>i</code>从<code>1-n</code>变化。<code>j</code>从<code>1-i</code>变化
* <code>dp[i] += dp[j-1] * dp[i-j]</code>

#### AC code

```
class Solution {
public:
    int numTrees(int n) {
        vector<int> dp(n+1, 0);
        dp[0] = 1;
        for(int i = 1; i <= n; i++){
            for(int j = 1; j <= i; j++){
                dp[i] += dp[j-1] * dp[i-j]; 
            }
        }
        return dp[n];
    }
};
```

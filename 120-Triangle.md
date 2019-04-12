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

#### Dp Solution

* 先来看凡人的DP策略。

* <code>dp[i][j]</code>表示从第<code>i</code>行的第<code>j</code>个数到最下方的最短路径和，则最后的结果直接返回<code>dp[1][1]</code>即可。

* 更新方式很容易想到：
  * <code>dp[i][j] = min(dp[i-1][j], dp[i-1][j+1]) + triangle[i-1][j-1]</code>.

* 初始化最后一行的各个数字本身。

#### AC code

```
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int n = triangle.size();
        vector<vector<int>> dp(n+1, vector<int>(n+1, INT_MAX));
        for(int i = 0; i < triangle[n-1].size(); i++){
            dp[n][i+1] = triangle[n-1][i];
        }
        for(int i = n-1; i >= 1; i--){
            for(int j = 0; j < triangle[i-1].size(); j++){
                dp[i][j+1] = min(dp[i+1][j+1], dp[i+1][j+2]) + triangle[i-1][j];
            }
        }
        return dp[1][1];
    }
};
```

#### Dp Solution

* 上面那种解法可以通过OJ，但显然不是题目中鼓励的 ***O(n)*** 的空间使用。

* <code>dp[i]</code>表示从索引为<code>i</code>的数字的最优解。对于每个数字，和它之后的元素比较选择较小的再加上面一行相邻位置的元素做为新的元素，然后一层一层的向上扫描。

* 仔细想想跟上面的那个方法思想上是相通的，只不过把数组复用了。

#### AC code

```
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int n = triangle.size();
        if(n == 0)
            return 0;
        vector<int> dp(triangle.back());
        for(int i = n-2; i >= 0; i--){
           for(int j = 0; j <= i; j++){
               dp[j] = triangle[i][j] + min(dp[j], dp[j+1]);
           }
        }
        return dp[0];
    }
};
```
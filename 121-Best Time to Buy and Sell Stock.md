## 问题描述

Say you have an array for which the ***i^th*** element is the price of a given stock on day ***i***.</br>

If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Note that you cannot sell a stock before you buy one.

#### Example 1:<br>
<pre><strong>Input:</strong> [7,1,5,3,6,4]
<strong>Output:</strong> 5
<strong>Explanation:</strong> Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
&nbsp;            Not 7-1 = 6, as selling price needs to be larger than buying price.
</pre>
The minimum path sum from top to bottom is <code>11 (i.e., 2 + 3 + 5 + 1 = 11)</code>.<br>
#### Example 2:<br>
<pre><strong>Input:</strong> [7,6,4,3,1]
<strong>Output:</strong> 0
<strong>Explanation:</strong> In this case, no transaction is done, i.e. max profit = 0.
</pre>

__Explanation__:<br>
只允许交易一次，求最大利润。

#### Dp Solution (Time Limit Exceeded)

* 这种序列型的问题一上来就想着用动态规划，很遗憾，超时了。

* <code>dp[i]</code>表示第<code>i</code>天卖出股票能得到的最大收益。

* 更新方式：第<code>i</code>天卖出股票，由于只能交易依次，因此我们要遍历前面那些天里的收益，比较它们与取消那些交易变成第<code>i</code>天卖出所得收益的大小。
  * <code>dp[i] = max(dp[i], dp[j] + prices[i-1] - prices[j-1], for j < i</code>.
  * <code>ans = max(ans, dp[i])</code>.

#### code

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size(), ans = 0;
        vector<int> dp(n+1, 0);
        for(int i = 2; i <= n; i++){
            for(int j = 1; j < i; j++){
                dp[i] = max(dp[i], dp[j] + prices[i-1] - prices[j-1]);
            }
            ans = max(ans, dp[i]);
        }
        return ans;
    }
};
```

#### Solution (AC)

* 也是，这道题是<code>easy</code>级别的，何必搞这么复杂呢。

* 我们使用一个变量记录之前碰到的最低价格，每天计算得到的最大收益，跟已有值比较更新答案即可。

#### AC code

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size(), ans = 0;
        if(n == 0)
            return 0;
        int small = prices[0];
        for(int i = 1; i < n; i++){
            ans = max(ans, prices[i] - small);
            small = min(small, prices[i]);
        }
        return ans;
    }
};
```
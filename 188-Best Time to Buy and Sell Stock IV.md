## 问题描述

Say you have an array for which the ***i^th*** element is the price of a given stock on day ***i***.</br>

Design an algorithm to find the maximum profit. You may complete at most ***k*** transactions.

__Note:__ You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).

#### Example 1:<br>
<pre><strong>Input:</strong> [2,4,1], k = 2
<strong>Output:</strong> 2
<strong>Explanation:</strong> Buy on day 1 (price = 2) and sell on day 2 (price = 4), profit = 4-2 = 2.
</pre>
#### Example 2:<br>
<pre><strong>Input:</strong> [3,2,6,5,0,3], k = 2
<strong>Output:</strong> 7
<strong>Explanation:</strong> Buy on day 2 (price = 2) and sell on day 3 (price = 6), profit = 6-2 = 4.
&nbsp;            Then buy on day 5 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
</pre>

__Explanation__:<br>
最多交易k次，求最大利润。

#### Dp Solution

* <code>global[i][j]</code>表示截止到第<code>i</code>天，允许交易<code>j</code>次的最大利润。<code>local[i][j]</code>表示在第<code>i</code>天发生交易，允许交易<code>j</code>次的最大利润。

* 如何更新？先来看<code>local[i][j]</code>，第<code>i</code>天发生交易，则存在以下情况：
  * 第<code>i</code>天买的，并在当天交易：<code>global[i-1][j-1] + 0</code>
  * 第<code>i-1</code>天买的，第<code>i</code>天交易出去：<code>global[i-1][j-1] + prices[i-1] - prices[i-2]</code>
  * 更早的时间点买的：<code>local[i-1][j] + prices[i-1] - prices[i-2]</code>

* 更新<code>global[i][j]</code>：
  * <code>golbal[i][j] = max(global[i-1][j], local[i][j])</code>

* 除了上面的分析之外，当<code>K</code>非常大时，本问题就退化成不限制交易次数的解法，这样做比动规更有效率。

#### AC code

```
class Solution {
public:
    int helper(vector<int>& prices){
        int ans = 0;
        for(int i = 1; i < prices.size(); i++){
            if (prices[i] - prices[i - 1] > 0) {
                 ans += prices[i] - prices[i - 1];
             }
        }
        return ans;
    }
    int maxProfit(int k, vector<int>& prices) {
        int n = prices.size();
        if(k > n/2)
            return helper(prices);
        vector<vector<int>> global(n+1, vector<int>(k+1,0));
        vector<vector<int>> local(n+1, vector<int>(k+1,0));
        for(int i = 1; i < n; i++){
            int diff = prices[i] - prices[i-1];
            for(int j = 1; j <= k; j++){
                local[i+1][j] = max(local[i][j] + diff, global[i][j-1] + max(0, diff));
                global[i+1][j] = max(local[i+1][j], global[i][j]);
            }
        }
        return global[n][k];
    }

};
```

#### Dp Solution

* 本题自然也有更省空间的写法

* 这里有必要说一下为什么内层循环是从大到小。我们计算<code>local[i][2]</code>时，会用到第一天的<code>global[i-1][1]</code>，如果我们的<code>j</code>是从小到大变化的，那么计算的<code>global[i][1]</code>会将<code>global[i-1][1]</code>覆盖。

#### AC code

```
class Solution {
public:
    int helper(vector<int>& prices){
        int ans = 0;
        for(int i = 1; i < prices.size(); i++){
            if (prices[i] - prices[i - 1] > 0) {
                 ans += prices[i] - prices[i - 1];
             }
        }
        return ans;
    }
    int maxProfit(int k, vector<int>& prices) {
        int n = prices.size();
        if(k > n/2)
            return helper(prices);
        vector<int> local(k+1, 0);
        vector<int> global(k+1, 0);
        for(int i = 1; i < n; i++){
            int diff = prices[i] - prices[i-1];
            for(int j = k; j >= 1; j--){
                local[j] = max(global[j-1] + max(diff, 0), local[j] + diff);
                global[j] = max(global[j], local[j]);
            }
        }
        return global[k];
    }
};
```
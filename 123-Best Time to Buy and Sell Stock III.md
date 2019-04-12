## 问题描述

Say you have an array for which the ***i^th*** element is the price of a given stock on day ***i***.</br>

Design an algorithm to find the maximum profit. You may complete at most ***two*** transactions.

__Note:__ You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).

#### Example 1:<br>
<pre><strong>Input:</strong> [3,3,5,0,0,3,1,4]
<strong>Output:</strong> 6
<strong>Explanation:</strong> Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
&nbsp;            Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.</pre>
#### Example 2:<br>
<pre><strong>Input:</strong> [1,2,3,4,5]
<strong>Output:</strong> 4
<strong>Explanation:</strong> Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
&nbsp;            Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are
&nbsp;            engaging multiple transactions at the same time. You must sell before buying again.
</pre>
#### Example 3:<br>
<pre><strong>Input:</strong> [7,6,4,3,1]
<strong>Output:</strong> 0
<strong>Explanation:</strong> In this case, no transaction is done, i.e. max profit = 0.</pre>

__Explanation__:<br>
最多交易2次，求最大利润。

#### Dp Solution

* <code>global[i][j]</code>表示截止到第<code>i</code>天，允许交易<code>j</code>次的最大利润。<code>local[i][j]</code>表示在第<code>i</code>天发生交易，允许交易<code>j</code>次的最大利润。

* 如何更新？先来看<code>local[i][j]</code>，第<code>i</code>天发生交易，则存在以下情况：
  * 第<code>i</code>天买的，并在当天交易：<code>global[i-1][j-1] + 0</code>
  * 第<code>i-1</code>天买的，第<code>i</code>天交易出去：<code>global[i-1][j-1] + prices[i-1] - prices[i-2]</code>
  * 更早的时间点买的：<code>local[i-1][j] + prices[i-1] - prices[i-2]</code>

* 更新<code>global[i][j]</code>：
  * <code>golbal[i][j] = max(global[i-1][j], local[i][j])</code>

#### AC code

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        vector<vector<int>> global(n+1, vector<int>(3,0));
        vector<vector<int>> local(n+1, vector<int>(3,0));
        for(int i = 1; i < n; i++){
            int diff = prices[i] - prices[i-1];
            for(int j = 1; j <= 2; j++){
                local[i+1][j] = max(local[i][j] + diff, global[i][j-1] + max(0, diff));
                global[i+1][j] = max(local[i+1][j], global[i][j]);
            }
        }
        return global[n][2];
    }
};
```

#### Dp Solution

* 更省空间的写法

* 这里有必要说一下为什么内层循环是从大到小。我们计算<code>local[i][2]</code>时，会用到第一天的<code>global[i-1][1]</code>，如果我们的<code>j</code>是从小到大变化的，那么计算的<code>global[i][1]</code>会将<code>global[i-1][1]</code>覆盖。

#### AC code

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        vector<int> local(3, 0);
        vector<int> global(3, 0);
        for(int i = 1; i < n; i++){
            int diff = prices[i] - prices[i-1];
            for(int j = 2; j >= 1; j--){
                local[j] = max(global[j-1] + max(diff, 0), local[j] + diff);
                global[j] = max(global[j], local[j]);
            }
        }
        return global[2];
    }
};
```
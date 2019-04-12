## 问题描述

Say you have an array for which the ***i^th*** element is the price of a given stock on day ***i***.</br>

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).

__Note:__ You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).

#### Example 1:<br>
<pre><strong>Input:</strong> [7,1,5,3,6,4]
<strong>Output:</strong> 7
<strong>Explanation:</strong> Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
&nbsp;            Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
</pre>
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
不限交易次数，求最大利润。

#### Solution

* 都不限交易次数了，因此能交易就交易。

#### AC code

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size(), ans = 0;
        for(int i = 0; i < n-1; i++){
            if(prices[i] < prices[i+1])
                ans += prices[i+1] - prices[i];
        }
        return ans;
    }
};
```
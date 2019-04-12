## 问题描述

Say you have an array for which the ***i^th*** element is the price of a given stock on day ***i***.</br>

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times) with the following restrictions:<br>

* You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

* After you sell your stock, you cannot buy stock on next day. (ie, cooldown 1 day)


#### Example:<br>
<pre><strong>Input:</strong> [1,2,3,0,2]
<strong>Output: </strong>3 
<strong>Explanation:</strong> transactions = [buy, sell, cooldown, buy, sell]
</pre>
__Explanation__:<br>
交易完，有一天的冷却期，求最大利润。

#### Dp Solution

* <code>sells[i]</code>表示第<code>i</code>天卖出获得的最大利润。
  * 第<code>i-1</code>天可以选择买或者不卖：<code>sells[i] = max(buys[i-1] + prices[i-1], sells[i-1] + prices[i-1] - prices[i-2])</code>

* <code>buys[i]</code>表示第<code>i</code>天买入获得的最大利润。
  * 第<code>i-1</code>天可以选择不买或者第<code>i-2</code>天卖出：<code>buys[i] = max(sells[i-2] - prices[i-1], buys[i-1] - prices[i-1] + prices[i-2])</code>

#### AC code

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        if(n == 0)
            return 0;
        int ans = 0;
        vector<int> sells(n+1, 0);
        vector<int> buys(n+1, 0);
        buys[1] = 0 - prices[0];
        for(int i = 2; i <= n; i++){
            int diff = prices[i-1] - prices[i-2];
            buys[i] = max(sells[i-2] - prices[i-1], buys[i-1] - diff);
            sells[i] = max(sells[i-1] + diff, buys[i-1] + prices[i-1]);
            ans = max(ans, sells[i]);
        }
        return ans;
    }
};
```
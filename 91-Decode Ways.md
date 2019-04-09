## 问题描述

A message containing letters from ***A-Z*** is being encoded to numbers using the following mapping:</br>
<pre>
'A' -> 1
'B' -> 2
...
'Z' -> 26
</pre>
<br>

Given a ***non-empty*** string containing only digits, determine the total number of ways to decode it.<br>
<br>

#### Example 1
<pre>
Input: "12" </br>
Output: 2</br>Explanation:</br>
It could be decoded as "AB" (1 2) or "L" (12).
</pre>
#### Example 2
<pre>
Input: "226" </br>
Output: 3</br>Explanation:</br>
It could be decoded as "BZ" (2 26), "VF" (22, 6), or "BBF" (2 2 6).
</pre>
__Explanation:__<br>

求解码方式的个数。

#### Dp Solution

* 题目需要抽象一下：对数字的解码分为2种：
  * 1位数字对应一种解码方式
  * 2位数字，当这个二位数小于26时也有一种对应的解码方式

* 可以看出，需要分1/2位讨论，有些类似于斐波那契数列。

* 想到用动态规划。<code>vector<int> dp[i]</code>表示前<code>i</code>个数字可有的解码方式个数。

* 更新方式，<code>dp[i]</code>有两部分组成：
  * 将这<code>i</code>个字符划分为前<code>i-1</code>个字符和第<code>i</code>个字符，则<code>s[i-1] == '0'</code>时，<code>dp[i]</code>的第一部分为<code>0</code>，否则为<code>dp[i-1]</code>.
  * 将这<code>i</code>个字符划分为前<code>i-2</code>个字符和后<code>2</code>个字符，则后<code>2</code>个字符组成的数字小于<code>26</code>且<code>i-2 >= 0</code>时，第二部分的数量为<code>dp[i-2]</code>.
* 将以上两部分综合起来即可求解。
#### AC code

```
class Solution {
public:
    int numDecodings(string s) {
        int n = s.size();
        if(n == 0 || (n > 1 && s[0] == '0'))
            return 0;
        vector<int> dp(n+1, 0);
        dp[0] = 1;
        for(int i = 1; i <= n; i++){
            if(s[i-1] == '0')
                dp[i] = 0;
            else
                dp[i] = dp[i-1];
            if(i-2 >= 0 && ((s[i-2] == '1') || (s[i-2] == '2'&& s[i-1] <= '6')))
                dp[i] = dp[i] + dp[i-2];
        }
        return dp[n];
    }
};
```

#### O(1) Space Solution
* 思想是一样的，节省了空间使用。

* 使用<code>c1</code>和<code>c2</code>分别记录前<code>i-1</code>个字符和前<code>i-2</code>个字符对应的解码个数，初始化都为<code>1</code>。

* 当所访问字符为<code>0</code>时，<code>c1</code>置<code>0</code>，当可以划分出后两个字符时，<code>c1 = c1 + c2</code>.

* 最后无论如何都要把<code>c2</code>赋值为之前的<code>c1</code>.

#### AC code

```
class Solution {
public:
    int numDecodings(string s) {
        int n = s.size();
        if(n == 0 || (n > 1 && s[0] == '0'))
            return 0;
        int c1 = 1, c2 = 1;
        for(int i = 1; i <= n; i++){
            if(s[i-1] == '0')
                c1 = 0;
            int tmp = c1;
            if(i-2 >= 0 && (s[i-2] == '1' || (s[i-2] == '2' && s[i-1] <= '6')))
                c1 = c1 + c2;
            c2 = tmp;
        }
    }
};
```
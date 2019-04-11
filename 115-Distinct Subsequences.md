## 问题描述

Given a string ***S*** and a string ***T***, count the number of distinct subsequences of ***S*** which equals ***T***.</br>

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, <code>"ACE"</code> is a subsequence of <code>"ABCDE"</code> while <code>"AEC"</code> is not).

#### Example 1:<br>
<pre><strong>Input: </strong>S = <code>"rabbbit"</code>, T = <code>"rabbit"
<strong>Output:</strong>&nbsp;3
</code><strong>Explanation:
</strong>
As shown below, there are 3 ways you can generate "rabbit" from S.
(The caret symbol ^ means the chosen letters)

<code>rabbbit</code>
^^^^ ^^
<code>rabbbit</code>
^^ ^^^^
<code>rabbbit</code>
^^^ ^^^
</pre>

#### Example 2:<br>
<pre><strong>Input: </strong>S = <code>"babgbag"</code>, T = <code>"bag"
<strong>Output:</strong>&nbsp;5
</code><strong>Explanation:
</strong>
As shown below, there are 5 ways you can generate "bag" from S.
(The caret symbol ^ means the chosen letters)

<code>babgbag</code>
^^ ^
<code>babgbag</code>
^^    ^
<code>babgbag</code>
^    ^^
<code>babgbag</code>
  ^  ^^
<code>babgbag</code>
    ^^^
</pre>

__Explanation:__<br>

找出字符串 ***S*** 中有多少个跟字符串 ***T*** 相等的子串。

#### Dp Solution

* 字符串匹配，老规矩，dp。

* <code>dp[i][j]</code>表示<code>s</code>的前<code>i</code>个字符与<code>t</code>的前<code>j</code>的匹配个数。

* 更新方式：
  * 若<code>s[i-1] == t[j-1]</code>，则尾字符可以匹配：<code>dp[i][j] = dp[i-1][j-1] + dp[i-1][j]</code>；
  * 否则：<code>dp[i][j] = dp[i-1][j]</code>。

* <code>dp[i][j] = dp[i-1][j] + (s[i-1] == t[j-1] ? dp[i-1][j-1] : 0)</code>。 

#### AC code

```
class Solution {
public:
    int numDistinct(string s, string t) {
        int n = s.size();
        int m = t.size();
        vector<vector<long>> dp(n+1, vector<long>(m+1, 0));
        for(int i = 1; i <= n; i++){
            dp[i][1] = dp[i-1][1] + (s[i-1] == t[0] ? 1 : 0);
        }
        for(int i = 1; i <= n; i++){
            for(int j = 2; j <= m; j++){
                dp[i][j] = dp[i-1][j] + (s[i-1] == t[j-1] ? dp[i-1][j-1] : 0);
            }
        }
        return dp[n][m];
    }
};
```
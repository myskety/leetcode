## 问题描述


Given a string ***s1***, we may represent it as a binary tree by partitioning it to two non-empty substrings recursively.</br>
<br>
Below is one possible representation of ***s1*** = <code>"great"</code>
<pre>
    great
   /    \
  gr    eat
 / \    /  \
g   r  e   at
           / \
          a   t
</pre>
<br>
To scramble the string, we may choose any non-leaf node and swap its two children.<br>
For example, if we choose the node <code>"gr"</code> and swap its two children, it produces a scrambled string <code>"rgeat"</code>.
<pre>
    rgeat
   /    \
  rg    eat
 / \    /  \
r   g  e   at
           / \
          a   t
</pre><br>
We say that <code>"rgeat"</code> is a scrambled string of <code>"great"</code>.

#### Example 1
<pre>
Input: s1 = "great", s2 = "rgeat" </br>
Output: true</br>
</pre>
#### Example 2
<pre>
Input: s1 = "abcde", s2 = "caebd" </br>
Output: false</br>
</pre>
Explanation: </br>

将一个字符串当作某一二叉树的根，它的非空子串是它的子节点，减缓某子字符串的两个子节点，可以重新形成一个新的字符串。则这两个字符串互为 ***scrambled string***.


#### Dp Solution
```
1. 关于字符串的匹配问题，一般都可以使用Dynamic Programming解决。<br>
2. 定义dp[i][j][len]为以i和j分别为s1和s2的起始字符，长度为len的字符串是不是互为scramble.
3. 先初始化dp[i][j][1]:<br>
for(int i = 0; i < m; i++){
    for(int j = 0; j < m; j++){
        dp[i][j][1] = s1[i] == s2[j];
    }
}
4. len从2~m变化，对于长度为len的字符串sub1和sub2，我们有1~len-1种切分方式，对于每一种切分，分为左右两个部分，则sub1和sub2互为srambled的条件是sub1[i:i+k]与sub2[j:j+k]互为scrambled且sub1[i+k:len]与sub2[j+k:len]互为scrambled; 或者sub1[i+k:len]与sub2[j:j+len-k]互为scrambled且sub1[i:k]与sub2[j+len-k:len]互为scrambled. 由此可得dp[i][j][len]的更新公式：
dp[i][j][len] = (dp[i][j][k] && dp[i+k][j+k][len-k]) || (dp[i][j+len-k][k] && dp[i+k][j][len-k] for 1 <= k < len)
```
#### AC code

```
class Solution {
public:
    bool isScramble(string s1, string s2) {
        int m = s1.length();
        int n = s2.length();
        if(s1 == s2)
            return true;
        if(m != n)
            return false;
        vector<vector<vector<bool>>> dp(m, vector<vector<bool>>(m, vector<bool>(m+1, false)));
        for(int i = 0; i < m; i++){
            for(int j = 0; j < m; j++){
                dp[i][j][1] = s1[i] == s2[j];
            }
        }
        for(int len = 2; len <= m; len++){
            for(int i = 0; i <= m - len; i++){
                for(int j = 0; j <= m - len; j++){
                    for(int k = 1; k < len; k++){
                        if((dp[i][j][k] && dp[i+k][j+k][len-k]) || (dp[i][j+len-k][k] && dp[i+k][j][len-k]))
                            dp[i][j][len] = true;
                    }
                }
            }
        }
        return dp[0][0][m];
    }
};
```
#### Recursive Solution
```
s1和s2互为scrambled的话，必然存在一个长度k，k < len(s1)，将s1分为两段s11，s12，同时有s21，s22。则要么s11和s21互为scrambled并且s12和s22互为scrambled，要么s11和s22互为scrambled并且s12和s21互为scrambled。
```
```
 bool isScramble(string s1, string s2) {
        int m = s1.length();
        int n = s2.length();
        if(s1 == s2)
            return true;
        if(m != n)
            return false;
        string a = s1, b = s2;
        sort(a.begin(), a.end());
        sort(b.begin(), b.end());
        if(a != b)
            return false;
        for(int i = 1; i < m; i++){
            string s11 = s1.substr(0, i);
            string s12 = s1.substr(i);
            string s21 = s2.substr(0, i);
            string s22 = s2.substr(i);
            if(isScramble(s11, s21) && isScramble(s12, s22))
                return true;
            s21 = s2.substr(s1.size() - i);
            s22 = s2.substr(0, s1.size() - i);
            if(isScramble(s11, s21) && isScramble(s12, s22))
                return true;
        }
        return false;
    }

```


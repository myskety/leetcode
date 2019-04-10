## 问题描述

Given ***s1,s2,s3***, find whether ***s3*** is formed by the interleaving of ***s1*** and ***s2***.</br>

#### Example 1
<pre><strong>Input:</strong> s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"
<strong>Output: true</strong>
</pre>
#### Example 2
<pre><strong>Input:</strong> s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc"
<strong>Output: false</strong>
</pre>

__Explanation:__<br>

检测 ***s3*** 能否被 ***s2*** 和 ***s1*** 交错得到，所谓 ***交错*** 是指每次构成 ***s3*** 的一个字符的时候可以选择它来自 ***s1*** 或是 ***s2***.

#### Dp Solution

* 碰到字符串匹配，还是老套路直接上DP，肯定好使。

* <code>dp[i][j]</code>表示考虑<code>s1</code>的前<code>i</code>个字符和<code>s2</code>的前<code>j</code>个字符是否可以交错得到<code>s3</code>的前<code>i+j</code>个字符。

* 更新方式：
  * <code>dp[i][j] = (s1[i-1] == s3[i+j-1] && dp[i-1][j]) || (s2[j-1] == s3[i+j-1] && dp[i][j-1])</code>

#### AC code (Time Limit Exceeded when n = 19)

```
class Solution {
public:
    bool isInterleave(string s1, string s2, string s3) {
        int n = s1.size();
        int m = s2.size();
        if((n+m) != s3.size())
            return false;
        vector<vector<bool>> dp(n+1, vector<bool>(m+1, false));
        dp[0][0] = true;
        for(int i = 1; i <= n; i++)
            dp[i][0] = dp[i-1][0] && s1[i-1] == s3[i-1];
        for(int i = 1; i <= m; i++)
            dp[0][i] = dp[0][i-1] && s2[i-1] == s3[i-1];
        for(int i = 1; i <= n; i++){
            for(int j = 1; j <= m; j++){
                if(s1[i-1] == s3[i+j-1] && s2[j-1] != s3[i+j-1])
                    dp[i][j] = dp[i-1][j];
                else if(s1[i-1] != s3[i+j-1] && s2[j-1] == s3[i+j-1])
                    dp[i][j] = dp[i][j-1];
                else if(s1[i-1] == s3[i+j-1] && s2[j-1] == s3[i+j-1])
                    dp[i][j] = dp[i][j-1] || dp[i-1][j];
                else
                    dp[i][j] = false;
            }
        }
        return dp[n][m];
    }
};
```

#### Recursive  Solution

* 一般的DFS递归解法通常都是要<code>Time Limited Exceeded</code>的，因此需要做些优化。

* 采用哈希集合保存匹配失败的情况，<code>i</code>和<code>j</code>记录<code>s1</code>和<code>s2</code>匹配到的位置。


#### AC code

```
class Solution {
public:
    bool isInterleave(string s1, string s2, string s3) {
        if(s1.size() + s2.size() != s3.size())
            return false;
        unordered_set<int> hash;
        return helper(s1, s2, s3, 0, 0, hash);
    }
    bool helper(string& s1, string& s2, string& s3, int i, int j, unordered_set<int>& hash){
        if(i == s1.size() && j == s2.size())
            return true;
        if(hash.count(i*s3.size()+j))
            return false;
        if(i < s1.size() && s1[i] == s3[i+j] && helper(s1, s2, s3, i+1, j, hash))
            return true;
        if(j < s2.size() && s2[j] == s3[i+j] && helper(s1, s2, s3, i, j+1, hash))
            return true;
        hash.insert(i*s3.size()+j);
        return false;
    }
};
```

#### Recursive  Solution

* 不经常写BFS，感受一下BFS的解法。

* 每次循环将当前可行的解法加入队列，最后有解时队列必然不为空。

#### AC code

```
class Solution {
public:
    bool isInterleave(string s1, string s2, string s3) {
        if(s1.size() + s2.size() != s3.size())
            return false;
        int n1 = s1.size(), n2 = s2.size(), n3 = s3.size(), k = 0;
        unordered_set<int> hash;
        queue<int> q{{0}};
        while(!q.empty() && k < n3){
            int len = q.size();
            for(int t = 0; t < len; t++){
                int i = q.front() / n3;
                int j = q.front() % n3;
                q.pop();
                if(i < n1 && s1[i] == s3[k]){
                    int key = (i+1) * n3 + j; 
                    if(!hash.count(key)){
                        hash.insert(key);
                        q.push(key);
                    }
                }
                if(j < n2 && s2[j] == s3[k]){
                    int key = i * n3 + j + 1; 
                    if(!hash.count(key)){
                        hash.insert(key);
                        q.push(key);
                    }
                }
            }
            k++;
        }
        return !q.empty() && k == n3;
    }
};
```

## 问题描述

Given a string containing only digits, restore it by returning all possible valid IP address combinations.</br>

#### Example
<pre>
Input: "25525511135" </br>
Output: ["255.255.11.135", "255.255.111.35"]
</pre>
__Explanation:__<br>

给定一串数字，求所有能组成的ip地址的集合。

#### Recursive Solution

* 这种让求所有解决方案的题目，一般都是用递归来做的。

* IP地址是分四段的，因此按照DFS的思想我们就按照段数依次向下遍历，每深入一层段数减一，当段数为0时，若不剩余字符，则找到一个解。

* 在处理每一段时，其长度有1-3种选择，需要对所有选择进行遍历。当然，这是在判定当前选择有效的前提下。


#### AC code

```
class Solution {
public:
    vector<string> restoreIpAddresses(string s) {
        
    }
    void helper(vector<string>& ans, string tmp, int k, string s){
        if( k == 0){
            if(s.empty())
                ans.push_back(tmp);
        }else{
            for(int i = 1; i <= 3; i++){
                if(s.size() > = i && isValid(s.substr(0, i))){
                    if(k == 1)
                        helper(ans, tmp + s.substr(0, i), k-1, s.substr(i));
                    else
                        helper(ans, tmp + s.substr(0, i) + ".", k-1, s.substr(i));
                }
            }
        }
    }

    bool isValid(string s){
        if(s.empty() || s.size() > 3 || (s.size() > 1 && s[0] == '0'))
            return false;
        int num = atoi(s.c_str());
        return num >= 0 && num <= 255
    }
};
```


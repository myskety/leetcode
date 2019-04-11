## 问题描述

Given a non-negative integer numRows, generate the first numRows of Pascal's triangle.</br>

<p><img alt="" src="https://github.com/myskety/leetcode/blob/master/images/PascalTriangleAnimated2.gif" style="height:240px; width:260px"><br>
<small>In Pascal's triangle, each number is the sum of the two numbers directly above it.</small></p>

#### Example:<br>
<pre><strong>Input:</strong> 5
<strong>Output:</strong>
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
</pre>

__Explanation__:<br>
杨辉三角

#### Solution

* 一层层生成就行。

#### AC code

```
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> ans;
        if(numRows == 0)
            return ans;
        vector<int> first(1, 1);
        ans.push_back(first);
        for(int i = 2; i <= numRows; i++){
            vector<int> tmp;
            for(int j = 0; j < ans[i-2].size() - 1; j++){
                tmp.push_back(ans[i-2][j] + ans[i-2][j+1]);
            }
            tmp.insert(tmp.begin(), 1);
            tmp.push_back(1);
            ans.push_back(tmp);
        }
        return ans;
    }
};
```
## 问题描述

Given a non-negative index ***k*** where ***k*** <= 33, return the ***k^th*** index row of the Pascal's triangle.</br>

__Note:__ The row index starts from 0.
<p><img alt="" src="https://github.com/myskety/leetcode/blob/master/images/PascalTriangleAnimated2.gif" style="height:240px; width:260px"><br>
<small>In Pascal's triangle, each number is the sum of the two numbers directly above it.</small></p>

#### Example:<br>
<pre><strong>Input:</strong> 3
<strong>Output:</strong>[1,3,3,1]
</pre>

__Explanation__:<br>
求杨辉三角的某一行。

#### Solution

* 题目限制了 ***O(k)*** 的空间使用，因此必须要复用上一层的结果

* 原理还是不变，不过用上一层生成下一层的时候注意从后往前生成，不然会用新值覆盖掉旧值。

* 一层层生成就行。

#### AC code

```
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        vector<int> ans;
        for(int i = 1; i <= rowIndex+1; i++){
            ans.resize(i, 1);
            for(int j = ans.size() -2; j > 0; j--){
                ans[j] = ans[j-1] + ans[j];
            }
        }
        return ans;
    }
};
```
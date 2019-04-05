## 问题描述


<html>
Given a 2D binary matrix filled with 0's and 1's, find the largest rectangle containing only 1's and return its area.</br>

</html>


#### Example

Input: <br>[</br>
&ensp;&ensp;&ensp;&ensp;["1","0","1","0","0"],</br>
&ensp;&ensp;&ensp;&ensp;["1","0","1","1","1"],</br>
&ensp;&ensp;&ensp;&ensp;["1","1","1","1","1"],</br>
&ensp;&ensp;&ensp;&ensp;["1","0","0","1","0"]</br>

]</br>
Output: 6<br>
Explanation: </br>
在给定二值(0/1)矩阵中，找出最大的仅包含1的矩形。


#### Solution

这道题是之前[Largest Rectangle in Histogram](https://github.com/myskety/leetcode/blob/master/84-Largest%20Rectangle%20in%20Histogram.md)的扩展，二维矩阵的每一层向上，可以看作一个直方图，对每个直方图调用[Largest Rectangle in Histogram](https://github.com/myskety/leetcode/blob/master/84-Largest%20Rectangle%20in%20Histogram.md)的方法即可。主要工作在于将矩阵的每一层构成直方图即可。
#### AC code

```
class Solution {
public:
    int maximalRectangle(vector<vector<char>>& matrix) {
        int rows = matrix.size();
        if(rows == 0)
            return 0;
        int cols = matrix[0].size();
        int ans = 0;
        vector<int> heights(cols, 0);
        for(int i = 0; i < rows; i++){
            for(int j = 0; j < cols; j++){
                heights[j] = matrix[i][j] == '0' ? 0 : (1 + heights[j]);
            }
            ans = max(ans, largestRectangleArea(heights));
        }
        return ans;
    }
    int largestRectangleArea(vector<int>& heights){
        int ans = 0;
        heights.push_back(0);
        stack<int> s;
        for(int i = 0; i < heights.size(); i++){
            if(s.empty() || heights[i] > heights[s.top()])
                s.push(i);
            else{
                int cur = s.top();
                s.pop();
                int width = s.empty() ? i : i - s.top() - 1;
                ans = max(ans, heights[cur] * width);
                i--;
            }
        }
        return ans;
    }
};
```


```
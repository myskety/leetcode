## 问题描述



Given <code>n</code> non-negative integers representing the histogram's bar height where width of each bar is 1. find the area of largest rectangle in the histogram.</br>

![](https://github.com/myskety/leetcode/blob/master/images/histogram.png)

Above is a histogram where width if each bar is 1, given height = [2,1,5,6,2,3]

![](https://github.com/myskety/leetcode/blob/master/images/histogram_area.png)

The largest rectangle is shown in the shaded area, which has area = 10 unit.

#### Example 1

Input: 1->2->3->3->4->4->5 </br>
Output: 1->2->5</br></br>

#### Example 1

Input: 1->1->1->2->3 </br>
Output: 2->3</br></br>
Explanation: </br>




#### Solution
这道题有些难度，方法是维护一个高度递增的栈，依然是考虑访问到的某一位置，当该位置的高度大于栈顶元素的高度时，将所访问位置进栈(之所以不是将高度进栈时为了后面计算面积时方便地获取宽度)；反之，开始计算矩形面积并更新结果。<br>
计算面积过程：取出栈顶元素，高度即为对应高度，当栈为空时，宽度为<code>i</code>，<code>i</code>表示当前位置索引，当栈不为空时，宽度为<code>i-stack.top()-1</code>。然后更新结果，并将<code>i</code>减一，重复上述过程。<br>
值得注意的是计算面积仅发生在栈中元素的高度大于当前访问位置的高度时。比如对于输入[2,1,5,6,2,3]，面积更新依次是2->6->10。为了保证高度为1的位置处的面积也能得到计算，需要在列表末尾添加高度0。<br>
这样，保证了当计算高度为<code>height[stack.top()]</code>的面积时<code>stack.pop().top()+1~i-1</code>这个范围内(闭区间)的所有bar的高度都可以用来构成矩形。(核心思想)
#### AC code

```
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int ans = 0;
        stack<int> s;
        heights.push_back(0);
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


## 问题描述


Given a ***non-empty*** integer array, find the minimum number of moves required to make all array elements equal, where a move is incrementing a selected by 1 or decrementing a selected element by 1.</br>
<br>

#### Example 1
<pre>
Input: [1, 2, 3] </br>
Output: 2</br>
Explanation:<br>
Only two moves are needed (remeber each move increments ordecrements one element):<br>
[1,2,3] => [2,2,3] => [2,2,2]
</pre>

Explanation: </br>

每次对一个元素加一或减一，求使得数组中元素相同的最小操作次数。


#### Dp Solution

* 这题有点歧义的，使得数组中元素相同，那这个数应该是数组中本来就有的吗？如果是，那应该是中位数，如果不是那应该是平均数。从题目结果来看，应该是属于前者。

* 那就可以先对数组进行排序，找到中位数，计算次数即可。
#### AC code

```
class Solution {
public:
    int minMoves2(vector<int>& nums) {
        int ans = 0;
        int left = 0;
        int right = nums.size() - 1;
        while(left < right){
            ans += nums[right] - nums[left];
            right--;
            left++;
        }
        return ans;
    }
};



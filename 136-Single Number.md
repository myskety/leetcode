## 问题描述


Given a ***non-empty*** array of integers, every element appears ***twice*** except for one. Find that single one.</br>
<br>
__Node:__<br>

* Your algorithm should have a linear runtime complexity. Could you implement is without using extra memory?

#### Example 1
<pre>
Input: [2,2,1]<br> 
Output: 1</br>
</pre>
#### Example 1
<pre>
Input: [4,1,2,1,2]<br> 
Output: 4</br>
</pre>

找出列表中仅出现一次的元素，其余元素均出现两次。


#### Solution

* 使用位运算：异或。

* 将列表中所有元素进行异或运算，出现两次的元素异或结果为0，因此最后的结果即为仅出现一次的元素。
#### AC code

```
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int ans = nums[0];
        for(int i = 1; i < nums.size(); ++i){
            ans = ans ^ nums[i];
        }
        return ans;
    }
};

```

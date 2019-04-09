## 问题描述


Given two sorted integer arrays ***nums1*** and ***nums2***, merge ***nums2*** **into** ***nums1*** as one sorted array.</br>
<br>
__Node:__<br>

* The number of elements initialized in ***nums1*** and ***nums2*** are ***m*** and ***n*** respectively.

* You may assume that ***nums1*** has enough space (size that is greater or equal to ***m+n***) to hold additional elements from ***nums2***.
#### Example 1
<pre>
Input:<br> 
nums1 = [1,2,3,0,0,0], m = 3 </br>
nums2 = [2,5,6], n = 3<br>
Output: [1,2,2,3,5,6]</br>
</pre>

Explanation: </br>

将有序列表合并到另一个有序列表, ***in-place***.


#### Solution

* 从尾部开始合并，使用<code>i, j, k</code>分别索引<code>nums1, nums2, new nums1</code>的尾部。

* 依次在<code>nums1</code>尾部插入两个列表的较大值，直到<code>nums2</code>的全部元素被处理完毕。
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

```

## 问题描述


<html>
Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.(i.e.,[0,0,1,2,2,5,6]) might become [2,5,6,0,0,1,2].</br>
<br>
You are given a target value to search. If found in the array return <code>true</code>, otherwise return <code>false</code><br>
<br>
<b>Note</b>: The solution set must not contain duplicate subsets.
</html>


#### Example 1

Input: nums = [2,5,6,0,0,1,2], target = 0 </br>
Output: true</br></br>

#### Example 2

Input: nums = [2,5,6,0,0,1,2], target = 3 </br>
Output: false</br></br>

Explanation: </br>
给定数值，查找其它是否在一个某有序数组于某一位置旋转后得到的新数组中。



#### Binary Search Solution

二分查找，比较<code>target</code>与<code>left</code>处数值大小，相等则返回<code>true</code>,否则判断<code>target</code>位于数组的左半部分或右半部分，由于数组中可能存在相同数字，因此存在某一特殊情况无法判断，此时将<code>left</code>加一即可。
#### AC code

```
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        int left = 0, right = nums.size() - 1;
        while(left < right){
            if(target == nums[left])
                return true;
            else if(target > nums[left]){
                int mid = left + (right - left) / 2;
                if(nums[mid] == target)
                    return true;
                else if(nums[mid] > target){
                    right = mid - 1;
                }else{
                    if(nums[mid] > nums[left])
                        left = mid + 1;
                    else if(nums[mid] < nums[left])
                        right = mid - 1;
                    else{
                        left++;
                    }
                }
            }
            else{
                int mid = left + (right - left) / 2;
                if(nums[mid] == target)
                    return true;
                else if(nums[mid] > target){
                    
                    if(nums[mid] > nums[left])
                        left = mid + 1;
                    else if(nums[mid] < nums[left])
                        right = mid - 1;
                    else
                        left++;
                }else{
                    
                    left = mid + 1;
                }
            }
            
        }
        if(left == right)
            return nums[left] == target;
        return false;
    }
};
```


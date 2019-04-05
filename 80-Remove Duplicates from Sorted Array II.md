## 问题描述


<html>
Given a sorted array nums, remove the duplicated <b>in-place</b> such that duplicates appeared at most twice and return the new length.</br>
<b>Note</b>: Do not allocate extra space for another array, you must do this by <b>modifying the input array</b> in-place.
</html>


#### Example

Given nums = [1,1,1,2,2,3] </br>
<br>
Your function should return length = 5, with the first five elements of nums being 1, 1, 2, 2 and 3 respectively.<br>
<br>
It doesn't matter what you leave beyond the returned length.
Explanation: </br>
给定排好序的数组，删除重复数字，使得每个数字出现的次数不多于两次，不能开辟新的数组，要在原数组上修改，返回新的数组长度，且不关心该长度后面是哪些数字。



#### Solution One

正常做就可以了，使用**cnt**记录数字出现次数，**previous**记录前驱数字，当访问到的数字与**previous**相等时，**cnt**加一，**cnt**大于2时，将访问到的数字从数组中删去，同时cnt减一，访问下标减一，数组长度减一。最后返回数组长度。
#### AC code

```
class Solution {
public:
    int removeDuplicates2(vector<int>& nums) {
        int length = nums.size();
        int i = 0;
        int cnt = 0;
        int previous = 0;
        for(i = 0; i < length; i++)
        {
            if(i == 0)
            {
             previous = nums[i];
                cnt = 1;
            }
            
            
            else
            {
                if(nums[i] == previous)
                {
                    cnt++;
                    if(cnt > 2)
                    {
                        nums.erase(nums.begin() + i);
                        cnt--;
                        i--;
                        length--;
                    }
                }else
                {
                    previous = nums[i];
                    cnt = 1;
                }
            }
        }
        return nums.size();
    }
};
```

#### Solution Two
使用快慢指针，起初**slow**指向第一个元素，**fast**指向第二个元素，使用**cnt**表示**slow**指向的数字还允许重复几次，当**fast**与**slow**指向相同数字且**cnt**已经变为0时，**fast**迁移，否则，判断**fast**与**slow**所指数字是否相同，相同的话，**cnt**减一，不同的话，cnt重新赋值为1.<br>
<br>
**cnt**初始赋值为1.
<br>
最后返回slow+1。
#### AC code

```
class Solution {
public:

    int removeDuplicates(vector<int>& nums) {
        int length = nums.size();
        int i = 0;
        int cnt = 1;
        int slow = 0, fast = 1;
        if(length <= 2)
            return length;
        while(fast < length){
            if(nums[fast] == nums[slow] && cnt == 0)
                fast++;
            else{
                if(nums[fast] == nums[slow])
                    cnt--;
                else
                    cnt = 1;
                slow++;
                nums[slow] = nums[fast];
                fast++;
            }
        }
        return slow+1;
    }
    
};
```
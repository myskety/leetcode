## 问题描述


Given an array of numbers ***nums***, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once.</br>
<br>
__Node:__<br>
* The order of the result is not important.

* Your algorithm should have a linear runtime complexity. Could you implement is without using extra memory?

#### Example 1
<pre>
Input: [1,2,1,3,2,5]<br> 
Output: [3,5]</br>
</pre>

__Explanation:__<br>
找出列表中仅出现一次的元素，其余元素均出现两次，这样的元素有两个。


#### Solution

* 首先我们已经知道如何解决仅有一个元素出现1次的情况，因此，可以把原始数组划分为两个数组，每个数组包含一个仅出现一次的元素。

* 怎么做有效的划分呢？按照以前的思想，我们把所有数字异或得到一个结果，那这个值应该是要求的两个元素的异或结果。找到该值第一个为1的位置，则该位置处两者的值必然不一样，因此按照在该位置的值是0/1将原始数组划分为两个数组。这样相同的元素必然在同一个数组，而要求的两个元素也必然在不同数组。

* 对划分得到的每个数组使用 [Single Number](https://github.com/myskety/leetcode/blob/master/136-Single%20Number.md) 的方法即可。
#### AC code

```
class Solution {
public:
    vector<int> singleNumber(vector<int>& nums) {
        int tmpresult = 0;
        for(int i = 0; i < nums.size(); i++){
            tmpresult = tmpresult ^ nums[i];
        }
        int index = findfirst1index(tmpresult);
        vector<int> ans(2, 0);
        for(int i = 0; i < nums.size(); i++){
            if(is1(nums[i], index))
                ans[0] = ans[0] ^ nums[i];
            else
                ans[1] = ans[1] ^ nums[i];
        }
        return ans;

    }

    int findfirst1index(int num){
        int ans = 0;
        while((num & 1) == 0){
            ans++;
            num = num >> 1;
        }
        return ans;
    }

    int is1(int num, int index){
        num = num >> index;
        return (num & 1);
    }
};
```

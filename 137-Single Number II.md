## 问题描述


Given a ***non-empty*** array of integers, every element appears ***three times*** except for one. Find that single one.</br>
<br>
__Node:__<br>

* Your algorithm should have a linear runtime complexity. Could you implement is without using extra memory?

#### Example 1
<pre>
Input: [2,2,3,2]<br> 
Output: 2</br>
</pre>
#### Example 1
<pre>
Input: [0,1,0,1,0,1,99]<br> 
Output: 99</br>
</pre>

找出列表中仅出现一次的元素，其余元素均出现三次。


#### Solution

* 使用位运算：如果一个数字出现三次，那么它的二进制表示的每一位都出现三次，将它们起来，必然能被3整除。

* 我们将列表中所有数字的二进制表示都加起来，如果某一位置的和不能被3整除，则那个仅出现一次的数字的二进制表示对应位置是1，否则是0。
#### AC code

```
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        vector<int> bitsum(32, 0);//记录每个位置的数字之和
        for(int i = 0; i <nums.size(); i++){
            int bitmask = 1;//用来取得对应位置的数字
            for(int j = 31; j >= 0; j--){
                int bit = nums[i] & bitmask;
                if(bit != 0)
                    bitsum[j] += 1;
                if(j > 0)//防止溢出
                    bitmask = bitmask << 1;
            }
        }
        int ans = 0;
        for(int i = 0; i < 32; i++){
            ans = ans << 1;
            ans += bitsum[i] % 3;
        }
        return ans;
    }
};

```

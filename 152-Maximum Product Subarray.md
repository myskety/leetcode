## 问题描述


Given an integer array ***nums*** , find the contiguous subarray within an array (containing at least one number) which has the largest product.</br>
<br>

#### Example 1
<pre>
Input: [2,3,-2,4] </br>
Output: 6</br>
Explanation:<br>
[2,3] has the largest product 6.
</pre>

#### Example 2
<pre>
Input: [-2,0,-1] </br>
Output: 0</br>
Explanation:<br>
The result cannot be 2, because [-2,-1] is not a subarray.
</pre>


Explanation: </br>

求数组的一个连续子数组，使得它的元素的乘积是所有子数组乘积的最大值。

#### Dp Solution

* 典型的动态规划可解的题。类似题目我们一般都是维护两个变量：局部最优和全局最优。但题目求的是乘积的最大值，有可能前面得到的是一个极小的负值，这样遇到下一个元素为负数时，就有可能产生一个极大的值。
  * <code>vector<int> local(n+1, 0);</code>
  * <code>vector<int> global(n+1, 0);</code>
* 因此需要额外维护一个变量用于记录局部最小值。
  * <code>vector<int> local_min(n+1, 0);</code>

* 初始化：<code>local[1] = nums[0]; global[1] = nums[0]; local_min[1] = nums[0];</code>
* 更新方式：
  * <code>local[i] = max(local[i-1] * nums[i-1], max(local_min[i-1] * nums[i-1], nums[i-1]));</code>
  * <code>local_min[i] = min(local[i-1] * nums[i-1], min(local_min[i-1] * nums[i-1], nums[i-1]));</code>
  * <code>global[i] = max(global[i-1], local[i];</code>
#### AC code

```
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int n = nums.size();
        if(n == 1)
            return nums[0];
        vector<int> global(n+1, 0);
        vector<int> local(n+1, 0);
        vector<int> local_min(n+1, 0);
        global[1] = nums[0];
        local[1] = nums[0];
        local_min[1] = nums[0];
        for(int i = 2; i <= n; i++){
            local[i] = max(local[i-1] * nums[i-1], max(local_min[i-1] * nums[i-1], nums[i-1]));
            locla_min[i] = min(local[i-1] * nums[i-1], min(local_min[i-1] * nums[i-1], nums[i-1]));
            global[i] = max(global[i-1], local[i]);
        }
        return global[n];
    }
};
```

#### 一种更节省空间的写法

```
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int n = nums.size();
        if(n == 1)
            return nums[0];
        int maxPro = nums[0]; //当前最大值
        int minPro = nums[0]; //当前最小值
        int ans = nums[0]; //全局最大值
        for(int i = 2; i <= n; i++){
            int t1 = maxPro * nums[i-1];
            int t2 = minPro * nums[i-1];
            maxPro = max(max(t1, t2), nums[i-1]);
            minPro = min(min(t1, t2), nums[i-1]);
            ans = max(maxPro, ans);
        }
        return ans;
    }
};
```
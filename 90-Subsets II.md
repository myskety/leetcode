## 问题描述


<html>
Given a collection of integers that might contain duplicates, ***nums***, return all possible subsets (the power set).</br>
<br>
<b>Note</b>: The solution set must not contain duplicate subsets.
</html>


#### Example

Input: nums = [1,2,2] </br>
Output: </br>[</br>
&ensp;&ensp;&ensp;&ensp;[2],</br>
&ensp;&ensp;&ensp;&ensp;[1],</br>
&ensp;&ensp;&ensp;&ensp;[1,2,2],</br>
&ensp;&ensp;&ensp;&ensp;[2,2],</br>
&ensp;&ensp;&ensp;&ensp;[1,2],</br>
&ensp;&ensp;&ensp;&ensp;[]</br>
]</br>
Explanation: </br>
就是求集合的全部子集, 与之前不同的是可能含有重复元素。

#### Non-recursion Solution

* 先回忆一下之前[Subsets](https://github.com/myskety/leetcode/blob/master/78-Subsets.md)的做法，我们将访问到的数字依次添加到现有的每个子集中，并将新的到的子集依次加入到子集集合中。

* 这里由于可能存在重复元素，因此要做一些改变。

* 首先是对数组进行排序，使得重复的元素变得相邻。

* 其次，我们在添加元素之前不能是从头添加了，即<code>for(int j = 0; j < len; j++)</code>，这里的<code>j</code>的初始值需要根据当前访问的元素与前一个访问的元素是否相同变化。
* 自然地，我们想到，当二者不同时，<code>j</code>的初始值依然是<code>0</code>，二者不同时<code>j = ans.size() - original_size</code>，这里的<code>original_size</code>应该是访问到重复元素以前，子集集合的大小。
#### AC code

```
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> ans(1);//默认有一个空集
        sort(nums.begin(), nums.end());
        int original_size = 1;//初始化保证访问第一个元素时j必然从0开始
        int last = nums[0];//初始化上一个访问的元素
        for(int i = 0; i < nums.size(); i++){
            if(nums[i] != last){
                last = nums[i];
                original_size = ans.size();//j从0开始
            }
            int size = ans.size();
            for(int j = size - original_size; j < size; j++){
                ans.push_back(ans[j]);
                ans.back().push_back(nums[i]);
            }
        }
        return ans;
    }
};
```

#### Recursive Solution
* 同样是DFS的解法。核心思想依然是考虑每个数字在构成子集的时候的两种可选状态：选择或不选择。因此可以构造一颗二叉树，每个数字对应树的一层，左子树表示不选择这个数字，右子树表示选择。最终所有的叶子节点就是所有的子集。

* 不同之处，排序

* 不同之处，在右子树阶段，处理完选择结点之后，跳过重复结点。注意这里可以直接跳过是因为它们位于同一层。

#### AC code

```
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> ans;
        vector<int> tmp;
        sort(nums.begin(), nums.end());
        dfs(ans, 0, tmp, nums);
        return ans;
        
    }
    void dfs(vector<vector<int>>& ans, int start, vector<int>& tmp, vector<int>& nums){
        ans.push_back(tmp);//这里表示左子树，不选择start指示的路径
        for(int i = start; i < nums.size(); i++){
            tmp.push_back(nums[i]);//右子树，选择start指示的路径
            dfs(ans, i+1, tmp, nums);//深度遍历
            tmp.pop_back();//返回父节点
            while(i+1 < nums.size() && nums[i] == nums[i+1])
                i++;
        }
    }
};
```
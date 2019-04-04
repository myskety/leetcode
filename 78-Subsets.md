## 问题描述


<html>
Given a set of distinct integers, nums, return all possible subsets(the power set).</br>
<b>Note</b>: The solution set must not contain duplicate subsets.
</html>


#### Example

Input: nums = [1,2,3] </br>
Output: </br>[</br>
&ensp;&ensp;&ensp;&ensp;[3],</br>
&ensp;&ensp;&ensp;&ensp;[1],</br>
&ensp;&ensp;&ensp;&ensp;[2],</br>
&ensp;&ensp;&ensp;&ensp;[1,2,3],</br>
&ensp;&ensp;&ensp;&ensp;[1,3],</br>
&ensp;&ensp;&ensp;&ensp;[2,3],</br>
&ensp;&ensp;&ensp;&ensp;[1,2],</br>
&ensp;&ensp;&ensp;&ensp;[]</br>
]</br>
Explanation: </br>
就是求集合的全部子集。



#### Non-recursion Solution

顺序考虑集合中的每个数字，初始一个仅含空集的子集集合，将访问到的数字依次添加到现有的每个子集中，并将新的到的子集依次加入到子集集合中。比如<br>[]->[1]; <br>[], [1] ->[2], [1,2];<br>[],[1],[2],[1,2]->[3],[1,3], [2,3], [1,2,3]
#### AC code

```
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> ans(1);//默认有一个空集
        for(int i = 0; i < nums.size(); i++){
            int len = ans.size();
            for(int j = 0; j < len; j++){
                ans.push_back(ans[j]);
                ans.back().push_back(nums[i]);
            }
        }
        return ans;
    }
};
```

#### Recursive Solution
同样是DFS的解法。核心思想依然是考虑每个数字在构成子集的时候的两种可选状态：选择或不选择。因此可以构造一颗二叉树，每个数字对应树的一层，左子树表示选择这个数字，右子树表示不选择。最终所有的叶子节点就是所有的子集。<br>

#### AC code

```
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> ans;
        vector<int> tmp;
        dfs(ans, 0, tmp, nums);
        return ans;
        
    }
    void dfs(vector<vector<int>>& ans, int start, vector<int>& tmp, vector<int>& nums){
        ans.push_back(tmp);//这里表示左子树，不选择start指示的路径
        for(int i = start; i < nums.size(); i++){
            tmp.push_back(nums[i]);//右子树，选择start指示的路径
            dfs(ans, i+1, tmp, nums);//深度遍历
            tmp.pop_back();//返回父节点
        }
    }
};
```
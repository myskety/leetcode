## 问题描述


<html>
Given two integers n and k, return all possible combinations of k numbers out of 1...n.
</html>


#### Example

Input: n = 4, k = 2 </br>
Output: </br>[</br>
&ensp;&ensp;&ensp;&ensp;[2,4],</br>
&ensp;&ensp;&ensp;&ensp;[3,4],</br>
&ensp;&ensp;&ensp;&ensp;[2,3],</br>
&ensp;&ensp;&ensp;&ensp;[1,2],</br>
&ensp;&ensp;&ensp;&ensp;[1,3],</br>
&ensp;&ensp;&ensp;&ensp;[1,4]</br>
]</br>
Explanation: </br>
求1-n共n个数字里的组合情况，每个组合包含k个数字。自然地想到使用dfs求解。



#### Solution

1. 初始化一个vector<vector<int>>记录所有的组合结果，vector<int>记录某一个组合结果，start记录从哪个数字开始组合.
```
  vector<vector<int>> ans;
  vector<int> tmp;
  int start = 1;
```
2. 当tmp中的元素个数等于k时，说明已经找到了一个组合，将其加入ans后返回；否则，从start开始，先将start加入tmp，递归调用dfs,然后将start移除。
```
  for(int i = start; i <= n; i++){
      tmp.push_back(i);
      dfs(ans, i+1, k, n, tmp);
      tmp.pop_back();
  }
  
```





#### AC code

```
class Solution {
public:
    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>> ans;
        vector<int> tmp;
        dfs(ans, 1, k, n, tmp);
        return ans;
        
    }
    void dfs(vector<vector<int>>& ans, int start, int k, int n, vector<int>& tmp){
        if(tmp.size() == k){
            ans.push_back(tmp);
            return;
        }
            
        for(int i = start; i <= n; i++){
            tmp.push_back(i);
            dfs(ans, i+1, k, n, tmp);
            tmp.pop_back();
        }
    }
};
```




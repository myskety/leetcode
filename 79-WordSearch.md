## 问题描述


<html>
Given a 2D board and a word, find if the word exists in the grid.</br>
</br>
The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or verically neighboring. The sanme letter cell may not be used more than once.</br>

</html>


#### Example

board = <br>[<br>&ensp;&ensp;['A', 'B', 'C', 'E'],</br>
&ensp;&ensp;['S', 'F', 'C', 'S'],</br>
&ensp;&ensp;['A', 'D', 'E', 'E']</br>
]
</br>
Given word = **"ABCCED**, return **true**. <br>
Given word = **"SEE**, return **true**. <br>
Given word = **"ABCB**, return **false**. <br>
Explanation: </br>
在给定字符组成的grid中判断能否找出给定word,查找方向是所访问字符的上下左右，每个字符只能使用一次。



#### Backtracking Solution

backtracking，我们从grid里的每一个字符处开始考虑，若存在一次正确的匹配，则返回**true**，否则返回false。<br>
访问到某一字符时，递归调用它的上下左右四个邻居，存在一个正确匹配，则返回**true**。
<br>
使用**visited**来标识某一字符是否被访问过。

#### AC code

```
class Solution {
public:
    bool exist(vector<vector<char>>& board, string word) {
        if(board.empty() || board[0].empty())
            return false;
        int rows = board.size();
        int cols = board[0].size();
        vector<vector<int>> visited(rows, vector<int>(cols, 0));
        for(int i = 0; i < rows; i++){
            for(int j = 0; j < cols; j++){
                if(backtracking(board, word, 0, i, j, visited))
                    return true;
            }
        }
        return false;
        
    }
    bool backtracking(vector<vector<char>>& board, string word, int cur, int i, int j, vector<vector<int>>& visited){
        if(cur == word.size())
            return true;
        if(i < 0 || j < 0 || i >= board.size() || j >= board[0].size() || visited[i][j] == 1 || board[i][j] != word[cur])
            return false;
        visited[i][j] = 1;
        bool ans = backtracking(board, word, cur+1, i, j-1, visited) 
                || backtracking(board, word, cur+1, i-1, j, visited) 
                || backtracking(board, word, cur+1, i, j+1, visited) 
                || backtracking(board, word, cur+1, i+1, j, visited);
        visited[i][j] = 0;
        return ans;
    }
};
```


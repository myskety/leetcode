## 问题描述

Given preorder and inorder traversal of a tree, construct the binary tree.</br>

__Note:__ You may assume that duplicates do not exist in the tree.

#### example:<br>
<pre> 
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]  
</pre>
return the following binary tree:
<pre>    3
   / \
  9  20
    /  \
   15   7</pre>
__Explanation:__<br>

从中序遍历和前序遍历复原二叉树。

#### Recursive Solution

* 思想很简单，先找到根节点，然后划分出左右子树。

* 接下来对左右子树分别递归即可。

#### AC code

```
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
           return helper(preorder, inorder, 0, preorder.size()-1, 0, inorder.size()-1);
    }

    TreeNode* helper(vector<int>& preorder, vector<int>& inorder, int pleft, int pright, int ileft, int iright){
        if(pleft > pright || ileft > iright)
            return NULL;
        int i = 0;
        for(i = ileft; i <= iright; i++){
            if(inorder[i] == preorder[pleft])
                break;
        }
        TreeNode *root = new TreeNode(preorder[pleft]);
        root->left = helper(preorder, inorder, pleft+1, pleft+i-ileft, ileft, i-1);
        root->right = helper(preorder, inorder, pleft+i-ileft+1, pright, i+1, iright);
        return root;
    }
};
```
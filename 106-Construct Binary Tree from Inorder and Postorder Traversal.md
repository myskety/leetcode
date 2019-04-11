## 问题描述

Given postorder and inorder traversal of a tree, construct the binary tree.</br>

__Note:__ You may assume that duplicates do not exist in the tree.

#### example:<br>
<pre> 
preorder = [9,3,15,20,7]
inorder = [9,15,7,20,3]  
</pre>
return the following binary tree:
<pre>    3
   / \
  9  20
    /  \
   15   7</pre>
__Explanation:__<br>

从中序遍历和后序遍历复原二叉树。

#### Recursive Solution

* 跟[Construct Binary Tree from Preorder and Inorder Traversal](https://github.com/myskety/leetcode/blob/master/105-Construct%20Binary%20Tree%20from%20Preorder%20and%20Inorder%20Traversal.md)一样，思想很简单，先找到根节点，然后划分出左右子树。

* 接下来对左右子树分别递归即可。

#### AC code

```
class Solution {
public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
           return helper(inorder, postorder, 0, inorder.size()-1, 0, postorder.size()-1);
    }

    TreeNode* helper(vector<int>& inorder, vector<int>& postorder, int ileft, int iright, int pleft, int pright){
        if(pleft > pright || ileft > iright)
            return NULL;
        int i = 0;
        for(i = ileft; i <= iright; i++){
            if(inorder[i] == postorder[pright])
                break;
        }
        TreeNode *root = new TreeNode(postorder[pright]);
        root->left = helper(inorder, postorder, ileft, i-1, pleft, pleft+i-ileft-1);
        root->right = helper(inorder, postorder, i+1, iright, pleft+i-ileft, pright-1);
        return root;
    }
};
```
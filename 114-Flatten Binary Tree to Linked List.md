## 问题描述

Given a binary tree, flatten it to a linked list in-place.</br>


#### example:<br>
Given the following tree:
<pre>    1
   / \
  2   5
 / \   \
3   4   6
</pre>
The flattened tree should look like:
<pre>1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6
</pre>

__Explanation:__<br>

平铺二叉树。

#### Recursive Solution

* 对左右子树调用<code>flatten</code>函数之后

* 将左子树接到根结点的右孩子位置，并保存之前的右子树。

* 找到根结点的最右子结点，将保存的右子树接到最右子结点的右孩子处。

#### AC code

```
class Solution {
public:
    void flatten(TreeNode* root) {
        if(!root)
            return;
        if(root->left)
            flatten(root->left);
        if(root->right)
            flatten(root->right);
        TreeNode *tmp = root->right;
        root->right = root->left;
        root->left = NULL;
        while(root->right)
            root = root->right;
        root->right = tmp;
    }
};
```

#### Non-recursive Solution

* 若当前结点存在左子树，则将当前结点的右子树接到左子树的最优子结点的右孩子处。

* 将左子树接到当前结点的右孩子处，当前结点的左子树置空。

* 将当前结点指向其右子树，重复上述步骤直到当前结点为空。

#### AC code

```
class Solution {
public:
    void flatten(TreeNode* root) {
        TreeNode *cur = root;
        while(cur){
            if(cur->left){
                TreeNode *tmp = cur->left;
                while(tmp->right)
                    tmp = tmp->right;
                p->right = cur->right;
                cur->right = cur->left;
                cur->left = NULL;
            }
            cur = cur->right;
        }
    }
};
```
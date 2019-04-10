## 问题描述

given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).</br>

For example, this binary tree <code>[1,2,2,3,4,4,3]</code> is symmetric:
<pre>    1
   / \
  2   2
 / \ / \
3  4 4  3
</pre>
But the following <code>[1,2,2,null,3,null,3]</code> is not:
<pre>    1
   / \
  2   2
   \   \
   3    3
</pre>
__Explanation:__<br>

判断一颗二叉树是不是镜像的

#### Solution

* 比如有两个结点<code>a</code>和<code>b</code>，我们需要判断<code>a</code>的左孩子和<code>b</code>的右孩子是否相等以及<code>a</code>的右孩子和<code>b</code>的左孩子是否相等。

#### AC code

```
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        return preorder(root, root);  
    }
    bool preorder(TreeNode *p1, TreeNode *p2){
        if(p1 == NULL && p2 == NULL)
            return true;
        if(p1 == NULL || p2 == NULL)
            return false;
        if(p1->val != p2->val)
            return false;
        return preorder(p1->left, p2->right) && preorder(p1->right, p2->left);
    }
};
```
#### Non-recursive Solution

* 队列每次记录一对需要比较的的对象。比较完按照固定顺序扩展队列。

```
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if(!root)
            return true;
        queue<TreeNode*> q;
        q.push(root->left);
        q.push(root->right);
        while(!q.empty()){
            TreeNode *left = q.front();
            q.pop();
            TreeNode *right = q.front();
            q.pop();
            if(left == NULL && right == NULL)
                continue;
            if((left == NULL && right != NULL) || (left != NULL && right == NULL))
                return false;
            if(left->val != right->val)
                return false;
            q.push(left->left);
            q.push(right->right);
            q.push(left->right);
            q.push(right->left);
        }
        return true;
    }
};
```
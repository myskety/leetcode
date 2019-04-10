## 问题描述

Given a binary tree, return the ***inorder traversal*** of its nodes' value.</br>

#### Example
<pre>
<strong>Input:</strong>
 [1,null,2,3]
   1
    \
     2
    /
   3
<strong>Output:</strong>
[1,3,2]
</pre>
__Explanation:__<br>

二叉树的中序遍历

#### Recursive Solution

* 递归写法很简单，直接写就行了。

#### AC code

```
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> ans;
        helper(root, ans);
        return ans;
    }
    void helper(TreeNode* root, vector<int>& ans){
        if(root == NULL)
            return;
        else{
            helper(root->left, ans);
            ans.push_back(root->val);
            helper(root->right, ans);
        }
    }
};
```
#### Non Recursive Solution (stack)

* 树的遍历只会递归写法是找不到工作的，下面是基于栈的非递归写法。

* 思路：从根节点卡斯hi，根节点入栈，然后将其所有左子结点入栈，然后取出栈顶元素(算是访问结点一次)，然后将指针指向右子结点，若存在右子结点，则下次循环又将其所有子结点入栈。如此保证了左-根-右的访问顺序。

#### AC code

```
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> ans;
        stack<TreeNode*> s;
        TreeNode *p = root;
        while(p || !s.empty()){
            while(p){
                s.push(p);
                p = p->left;
            }
            p = s.top();
            s.pop();
            ans.push_back(p->val);
            p = p->right;
        }
        return ans;
    }
    
};
```
#### Non Recursive Solution (Morris)

* 树的遍历只会前两种也是找不到工作的，下面是一种常数空间复杂度的遍历方式，非递归且不用栈--Morris Traversal.

* 首先引入一种新型树--Threaded binary tree. 它将所有原本为空的右子结点指向中序遍历顺序之后的那个结点，把所有原本为空的左子结点指向中序遍历之前的那个结点。

* 本题则需要构建一个Treaded binary tree。
  * 初始化指针<code>cur</code>指向<code>root</code>。
  * 当<code>cur</code>不为空时：
    * 如果<code>cur</code>没有左子结点：
      * 打印<code>cur</code>的值，将<code>cur</code>指向其右子结点
    * 如果<code>cur</code>有左子结点：
      * 将<code>pre</code>指向<code>cur</code>的左子树的最右子结点
        * 若<code>pre</code>不存在右子结点：
          * 将其右子结点指回<code>cur</code>
          * <code>cur</code>指向其左子结点
        * 反之
          * 将<code>pre</code>右子结点置空
          * 打印<code>cur</code>的值
          * 将<code>cur</code>指针指向其右子结点
* 这是什么神仙算法
#### AC code

```
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> ans;
        if(!root)
            return ans;
        TreeNode *cur, *pre;
        cur = root;
        while(cur){
            if(!cur->left){
                res.push_back(cur->val);
                cur = cur->right;
            }else{
                pre = cur->left;
                while(pre->right && pre->right != cur)
                    pre = pre->right;
                if(!pre->right){
                    pre->right = cur;
                    cur = cur->left;
                }else{
                    pre->right = NULL;
                    ans.push_back(cur->val);
                    cur = cur->right;
                }
            }
        }
        return ans;
    }
    
};
```
#### Morris Traversal 中序遍历

这里重新总结一下算法流程：<br>

* 如果当前结点的左孩子为空，则输出当前结点并将其右孩子作为当前结点。

* 如果当前结点的左孩子不为空，在当前结点的左子树中找到当前结点在中序遍历下的前驱结点。
  * 如果前驱结点的右孩子为空，将它的右孩子设置为当前结点，当前结点更新为当前结点的左孩子
  * 如果前驱结点的右孩子为当前结点，将它的右孩子重新设为空，输出当前结点。当前结点更新为当前结点的右孩子

* 重复上述两个步骤直到当前结点为空。

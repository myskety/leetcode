## 问题描述

Two elements of a binary search tree (BST) are swapped by mistake. </br>

Recover the tree without changing its structure.

#### Example 1
<pre><strong>Input:</strong> [1,3,null,null,2]
&nbsp;  1
&nbsp; /
&nbsp;3
&nbsp; \
&nbsp;  2
<strong>Output:</strong> [3,1,null,null,2]
&nbsp;  3
&nbsp; /
&nbsp;1
&nbsp; \
&nbsp;  2
</pre>
#### Example 2

<pre><strong>Input:</strong> [3,1,4,null,null,2]
  3
 / \
1   4
&nbsp;  /
&nbsp; 2
<strong>Output:</strong> [2,1,4,null,null,3]
  2
 / \
1   4
&nbsp;  /
 &nbsp;3
</pre>

__Follow up:__<br>

* A solution using ***O(n)*** space is pretty straight forward.

* Could you devise a constant space solution?

#### Solution

* 依然依赖中序遍历。

* 不过既然提到了最好使用 ***O(1)*** 的空间复杂度，那就必须要上Morris Traversal了。这玩意儿用过两遍了，直接上代码吧。

#### AC code

```
class Solution {
public:
    void recoverTree(TreeNode* root) {
        TreeNode *cur = root, *pre = NULL, *parent = NULL; //Morris 标准套装
        TreeNode *first = NULL, *second = NULL; //为了记录哪两个元素的值被交换了
        while(cur){
            if(!cur->left){
                if(parent && parent->val >= cur->val){
                    if(!first)
                        first = parent;
                    second = cur;
                }
                parent = cur;
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
                    if(parent && parent->val >= cur->val){
                        if(!first)
                            first = parent;
                        second = cur;
                    }
                    parent = cur;
                    cur = cur->right;
                }
            }
        }
        swap(first->val, second->val);
    }
};
```
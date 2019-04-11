## 问题描述

Given an array where elements are sorted in ascending order, convert it to a height balanced BST.</br>

For this problem, a height-balanced binary is defined as a binary tree in which the depth of the two subtrees of ***every*** node never differ by more than 1.

#### Example:<br>
<pre>Given the sorted array: [-10,-3,0,5,9],

One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:

      0
     / \
   -3   9
   /   /
 -10  5
</pre>
__Explanation:__<br>

给定一个有序列表，生成一颗平衡的二叉搜索树。

#### Solution

* 由于列表是有序的，将其分为左右两部分即可保证<code>BST</code>.

* 从中间位置切分列表，即可保证 ***balanced***。

* 对于左右两个部分，递归即可。

#### AC code

```
class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return helper(nums, 0, nums.size()-1);
    }
    TreeNode* helper(vector<int>& nums, int left, int right){
        if(left > right)
            return NULL;
        else{
            int mid = left + (right - left) / 2;
            TreeNode *root = new TreeNode(nums[mid]);
            root->left = helper(nums, left, mid-1);
            root->right = helper(nums, mid+1, right);
            return root;
        }
    }
};
```
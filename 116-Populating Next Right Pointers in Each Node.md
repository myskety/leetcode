## 问题描述

You are given a ***perfect binary tree*** where all leaves are on the same level, and every parent has two children. The binary tree has the following definition:</br>

<pre>struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
</pre>

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to <code>NULL</code>.<br>

Initially, all next pointers are set to <code>NULL</code>.

#### Example:<br>
<img alt="" src="https://github.com/myskety/leetcode/blob/master/images/116_sample.png" style="width: 640px; height: 218px;">

<pre><strong>Input: </strong><span>{"$id":"1","left":{"$id":"2","left":{"$id":"3","left":null,"next":null,"right":null,"val":4},"next":null,"right":{"$id":"4","left":null,"next":null,"right":null,"val":5},"val":2},"next":null,"right":{"$id":"5","left":{"$id":"6","left":null,"next":null,"right":null,"val":6},"next":null,"right":{"$id":"7","left":null,"next":null,"right":null,"val":7},"val":3},"val":1}</span>

<strong>Output: </strong><span>{"$id":"1","left":{"$id":"2","left":{"$id":"3","left":null,"next":{"$id":"4","left":null,"next":{"$id":"5","left":null,"next":{"$id":"6","left":null,"next":null,"right":null,"val":7},"right":null,"val":6},"right":null,"val":5},"right":null,"val":4},"next":{"$id":"7","left":{"$ref":"5"},"next":null,"right":{"$ref":"6"},"val":3},"right":{"$ref":"4"},"val":2},"next":null,"right":{"$ref":"7"},"val":1}</span>

<strong>Explanation: </strong>Given the above perfect binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B.
</pre>

__Note:__

* You may only use constant extra space.

* Recursive approach is fine, implicit stack space does not count as extra space for this problem.

#### Solution

* 层次遍历，边遍历边添加指针。

#### AC code

```
class Solution {
public:
    Node* connect(Node* root) {
        if(root == NULL)
            return NULL;
        queue<Node*> q;
        q.push(root);
        while(!q.empty()){
            int n = q.size();
            for(int i = 0; i < n; i++){
                Node *tmp = q.front();
                q.pop();
                if(i < n-1)
                    tmp->next = q.front();
                if(tmp->left)
                    q.push(tmp->left);
                if(tmp->right)
                    q.push(tmp->right);
            }
        }
        return root;
    }
};
```

#### Solution

* 递归层次遍历。

#### AC code

```
class Solution {
public:
    Node* connect(Node* root) {
        if(root == NULL)
            return NULL;
        if(root->left)
            root->left->next = root->right;
        if(root->right)
            root->right->next = root->next ? root->next->left : NULL;
        connect(root->left);
        connect(root->right);
        return root;
    }
};
```
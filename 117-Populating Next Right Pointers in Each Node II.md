## 问题描述

You are given a binary tree</br>

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
<img alt="" src="https://github.com/myskety/leetcode/blob/master/images/117_sample.png" style="width: 640px; height: 218px;">

<pre><strong>Input: </strong><span>{"$id":"1","left":{"$id":"2","left":{"$id":"3","left":null,"next":null,"right":null,"val":4},"next":null,"right":{"$id":"4","left":null,"next":null,"right":null,"val":5},"val":2},"next":null,"right":{"$id":"5","left":null,"next":null,"right":{"$id":"6","left":null,"next":null,"right":null,"val":7},"val":3},"val":1}</span>

<strong>Output: </strong><span>{"$id":"1","left":{"$id":"2","left":{"$id":"3","left":null,"next":{"$id":"4","left":null,"next":{"$id":"5","left":null,"next":null,"right":null,"val":7},"right":null,"val":5},"right":null,"val":4},"next":{"$id":"6","left":null,"next":null,"right":{"$ref":"5"},"val":3},"right":{"$ref":"4"},"val":2},"next":null,"right":{"$ref":"6"},"val":1}</span>

<strong>Explanation: </strong>Given the above binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B.
</pre>

__Note:__

* You may only use constant extra space.

* Recursive approach is fine, implicit stack space does not count as extra space for this problem.

#### Solution

* [Populating Next Right Pointers in Each Node](https://github.com/myskety/leetcode/blob/master/116-Populating%20Next%20Right%20Pointers%20in%20Each%20Node.md)的解法同样适用于本题

#### AC code

```
class Solution {
public:
    Node* connect(Node* root) {
        if(!root)
            return root;
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

* 当然题目里也说了，建议使用常数级别的空间消耗，这就需要点trick了。

* 建立一个虚拟结点指向每层的首结点，使用<code>cur</code>遍历该层结点。

* 从根节点开始，如果左子结点存在，则<code>cur->next = root->left; cur = cur->next</code>；如果右子结点存在，则<code>cur->next = root->right; cur= cur->next</code>。

* 平移根结点<code>root = root->next</code>。

* 若平移后根结点为空，则该层遍历完毕，将<code>cur</code>重置为<code>dummy</code>，而根结点应该赋值为本层的首结点<code>root = dummy->next</code>。同时需要将虚拟结点断开与本层的连接。

* 问题来了，为什么要将虚拟结点断开呢？考率到最后一层之前，<code>root = dummy->next</code>，<code>root</code>现在指向最后一层的首结点，势必没有左子结点和右子结点，经过一系列的<code>next</code>平移，<code>root</code>指向最后一层的最后一个结点，这样<code>root</code>又会回到该层的首结点，造成死循环。

#### AC code

```
class Solution {
public:
    Node* connect(Node* root) {
        Node *dummy = new Node(-1, NULL, NULL, NULL);
        Node *cur = dummy, *head = root;
        while(root){
            if(root->left){
                cur->next = root->left;
                cur = cur->next;
            }
            if(root->right){
                cur->next = root->right;
                cur = cur->next;
            }
            root = root->next;
            if(!root){
                cur = dummy;
                root = dummy->next;
                dummy->next = NULL;
            }
        }
        return head;
    }
};
```
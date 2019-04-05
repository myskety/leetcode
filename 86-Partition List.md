## 问题描述


<html>
Given a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.</br>
<b>Note</b>: You should preserve the original relative order of the nodes in each of the two partitions.
</html>


#### Example

Input: head = 1->4->3->2->5->2, x = 3 </br>
Output: 1->2->2->4->3->5</br>
Explanation: </br>
对于给定链表和x,使得小于x的结点位于大于或等于x的结点前面。<br>保证结点的原始相对位置。



#### Non-recursion Solution

找到最后一个值小于<code>x</code>的结点<code>cur</code>，记<code>pre = cur</code>，然后往后遍历，每当碰到一个值小于<code>x</code>的结点，就把他插入到<code>pre</code>的后面。
#### AC code

```
class Solution {
public:
    ListNode* partition(ListNode* head, int x) {
        ListNode *dummy = new ListNode(-1);
        ListNode *pre = dummy, *cur;
        dummy->next = head;
        while(pre->next && pre->next->val < x)
            pre = pre->next;
        cur = pre;
        while(cur->next){
            if(cur->next->val < x){
                ListNode *tmp = cur->next;
                cur->next = tmp->next;
                tmp->next = pre->next;
                pre->next = tmp;
                pre = pre->next;
            }else{
                cur = cur->next;
            }
        }
        return dummy->next;
    }
};
```


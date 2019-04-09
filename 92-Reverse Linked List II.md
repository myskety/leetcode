## 问题描述

Reverse a linked list from position ***m*** to ***n***. Do it in one-pass.</br>

<br>

__Note:__
1 <= m <= n <= length of list.
<br>

#### Example 1
<pre>
Input: 1->2->3->4->5->NULL, m = 2, n = 4 </br>
Output: 1->4->3->2->5->NULL</br>
</pre>
__Explanation:__<br>

固定位置翻转链表。

#### Solution

* 首先使用快慢指针找到翻转链表的起始位置。

* 然后依次翻转即可。

#### AC code

```
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        ListNode *slow = head, *fast = head;
        ListNode *dummy = new ListNode(-1);
        ListNode *pre = dummy;
        dummy->next = head;
        int diff = n - m;
        while(diff > 0 && fast->next){
            fast = fast->next;
            diff--;
        }
        while(m > 1){
            pre = slow;
            slow = slow->next;
            fast = fast->next;
            m--;
        }
        ListNode *tmp = slow;
        while(slow->next != fast){
            ListNode *ne = slow->next;
            pre->next = ne;
            slow->next = ne->next;
            ne->next = tmp;
            tmp = ne;
        }
        //移动fast
        pre->next = fast;
        slow->next = fast->next;
        fast->next = tmp;
        return dummy->next;
    }
};
```


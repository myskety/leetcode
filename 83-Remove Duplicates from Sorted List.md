## 问题描述



Given a sorted linked list, delete all duplicates such that each element appear only once.</br>



#### Example 1

Input: 1->1->2 </br>
Output: 1->2</br></br>

#### Example 1

Input: 1->1->2->3->3 </br>
Output: 1->2->3</br></br>
Explanation: </br>
删去有序链表里的重复元素，使得每个元素仅出现一次。



#### Solution
这道题很简单，直接贴代码吧。

#### AC code

```
class Solution {
public:
  
    ListNode* deleteDuplicates(ListNode* head) {
        if(head == NULL)
            return NULL;
        ListNode *cur = head->next;
        ListNode *pre = head;
        while(cur){
            if(cur->val != pre->val){
                pre->next = cur;
                pre = cur;
            }
            cur = cur->next;
        }
        pre->next = NULL;
        return head;
    }
};
```


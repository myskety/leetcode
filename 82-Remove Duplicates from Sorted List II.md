## 问题描述



Given a sorted linked list, delete all nodes that have duplicate number, leaving only ***distinct*** numbers from the original list.</br>
<b>Note</b>: The solution set must not contain duplicate subsets.



#### Example 1

Input: 1->2->3->3->4->4->5 </br>
Output: 1->2->5</br></br>

#### Example 1

Input: 1->1->1->2->3 </br>
Output: 2->3</br></br>
Explanation: </br>
删去有序链表中出现次数大于1的所有结点。



#### Solution
<code>pre</code>指向前驱结点，<code>cur</code>指向当前访问结点，检测当前结点是否是重复结点，如果是，忽略，继续遍历，如果不是，更新<code>pre</code>和<code>cur</code>，继续遍历。最后将<code>pre</code>的<code>next</code>置<code>null</code>.

#### AC code

```
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if(head == NULL)
            return NULL;
        ListNode *cur = head;
        ListNode *dummy = new ListNode(-1);
        ListNode *pre = dummy;
        dummy->next = head;
        while(cur){
            int flag = 0;
            if(cur->next && cur->next->val == cur->val){
                flag = 1;
                cur = cur->next;
            }
            if(flag)
                cur = cur->next;
            if(!flag){
                pre->next = cur;
                pre = cur;
                cur = cur->next;
            }
        }
        pre->next = NULL;
        return dummy->next;
    }
};
```


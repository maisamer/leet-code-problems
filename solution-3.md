## Medium problems section two

### 142. Linked List Cycle II
Problem Link: https://leetcode.com/problems/linked-list-cycle-ii/

#### - CPP Solution
```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if(head == nullptr || head->next == nullptr)
            return nullptr;
        ListNode *slow = head;
        ListNode *fast = head;
        while(fast != nullptr && fast->next != nullptr){
            fast = fast->next->next;
            slow = slow->next;
            if(fast == slow)
                break;
        }
        if (slow != fast)
            return nullptr;
        slow = head;
        while(slow != fast){
            fast = fast->next;
            slow = slow->next;
        }
        return slow;
    }
};
```

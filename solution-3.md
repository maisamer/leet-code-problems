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

### 2. Add Two Numbers
Problem Link: https://leetcode.com/problems/add-two-numbers/

#### - CPP Solution
```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* res = new ListNode();
        ListNode* curr = res;
        int carry=0;
        while(l1 != nullptr or l2 != nullptr or carry){
            int sum = carry;
            if(l1 != nullptr){
                sum += l1->val; 
                l1 = l1->next;
            }
            if(l2 != nullptr){
                sum += l2->val; 
                l2 = l2->next;
            }
            carry = sum > 9 ? 1 : 0; 
            curr->next = new ListNode(sum%10);
            curr = curr->next;
        }
        return res->next;
    }
};
```
### 19. Remove Nth Node From End of List
Problem Link: https://leetcode.com/problems/remove-nth-node-from-end-of-list/

#### - CPP Solution
```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        int size = 1;
        ListNode * curr = head;
        while(curr->next != nullptr){
            size++;
            curr = curr->next;
        }
        int removed = size - n + 1 ;
        if(removed == 1){
            head = head->next;
        }else{
            removed-=2;
            curr = head;
            while(removed--){
                curr=curr->next;
            }
            curr->next = curr->next->next;
        }
        return head;
    }
};
```
#### - optimize solution with two pointer ticnique CPP Solution
```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode * dummy = new ListNode(0,head);
        ListNode * left = dummy;
        ListNode * right = head;
        while(n-- and right != nullptr){
            right = right->next;
        }
        while(right != nullptr){
            right = right->next;
            left = left->next;
        }
        left->next = left->next->next;
        return dummy->next;
    }
};
```

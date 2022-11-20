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
#### - optimize solution with two pointer technique CPP Solution
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
### 148. Sort List
Problem Link: https://leetcode.com/problems/sort-list/

#### - CPP Solution
```cpp
class Solution {
public:
    ListNode* getMidle(ListNode* node){
        cout<<node->val<<endl;
       ListNode* slow = node , *fast = node->next; 
       while(fast != nullptr and fast->next != nullptr){
           slow = slow->next;
           fast = fast->next->next;
       }
       return slow;
    }
    ListNode* merge(ListNode* l1,ListNode* l2){
        ListNode* dummy = new ListNode();
        ListNode* root = dummy;
        while (l1 != nullptr and l2 != nullptr){
            if(l1->val < l2->val){
                root->next = l1;
                l1 = l1->next;
            }else{
                root->next = l2;
                l2 = l2->next;
            }
            root = root->next;
        }
        if(l1 != nullptr)
            root->next = l1;
        if(l2 != nullptr)
            root->next = l2;
        return dummy->next;
    }
    ListNode* sortList(ListNode* head) {
        if(head == nullptr or head->next == nullptr)
            return head;
        ListNode* left = head;
        ListNode* right = getMidle(head);
        ListNode* temp = right->next;
        right->next = nullptr;
        right = temp;
        left = sortList(left);
        right = sortList(right);
        return merge(left,right);  
    }
};
```
### 143. Reorder List
Problem Link: https://leetcode.com/problems/reorder-list/

#### - CPP Solution
```cpp
class Solution {
public:
    void reorderList(ListNode* head) {
        // get midle 
        ListNode* slow = head, *fast = head->next;
        while(fast != nullptr and fast->next != nullptr){
            slow = slow->next;
            fast = fast->next->next;
        }
        ListNode* second = slow->next;   
        ListNode* prev = slow->next = nullptr;
        // reverse linked list
        while(second != nullptr){
            ListNode* temp = second->next;
            second->next = prev;
            prev = second; 
            second = temp;
        }
        ListNode* first = head,*last = prev;
        while(last != nullptr){
            ListNode* temp1 = first->next,*temp2 = last->next;
            first->next = last;
            last->next = temp1;
            first = temp1;
            last = temp2;
        }
    }
};
```
### 133. Clone Graph
Problem Link: https://leetcode.com/problems/clone-graph/

#### - CPP Solution
```cpp
class Solution {
    Node* vis[105];
    Node* dfs(Node* node){
        if(vis[node->val]!= nullptr)
            return vis[node->val];
        Node *temp = new Node(node->val);
        vis[node->val] = temp;
        for(int i=0;i<node->neighbors.size();i++){
            temp->neighbors.push_back(dfs(node->neighbors[i]));
        }
        return temp;
    }
public:
    Node* cloneGraph(Node* node) {
        if(node == nullptr)
            return node;
        return dfs(node);    
    }
};
```
### 
Problem Link: 

#### - CPP Solution
```cpp
```

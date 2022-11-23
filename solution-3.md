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
### 417. Pacific Atlantic Water Flow
Problem Link: https://leetcode.com/problems/pacific-atlantic-water-flow/

#### - CPP Solution
```cpp
class Solution {
public:
    int rows,cols;
    int dx[4] = {0,0,-1,1};
    int dy[4] = {1,-1,0,0};

    bool isOutBound(int r,int c){
        return r<0 or r>=rows or c<0 or c>=cols ;
    }
    void dfs(vector<vector<int>>& heights,int i,int j,map<pair<int,int>,bool>& vis,int prevHieght){
        if(isOutBound(i,j) or vis[{i,j}] or heights[i][j]<prevHieght) return;
        vis[{i,j}] = true;
        for(int it=0;it<4;it++)
            dfs(heights,i+dx[it],j+dy[it],vis,heights[i][j]);
    }
    vector<vector<int>> pacificAtlantic(vector<vector<int>>& heights) {
        vector<vector<int>> res;
        rows = heights.size();
        cols = heights[0].size();
        map<pair<int,int>,bool> pac,atl;
        for(int c=0;c<cols;c++){
            dfs(heights,0,c,pac,heights[0][c]);
            dfs(heights,rows-1,c,atl,heights[rows-1][c]);
        }
        for(int r=0;r<rows;r++){
            dfs(heights,r,0,pac,heights[r][0]);
            dfs(heights,r,cols-1,atl,heights[r][cols-1]);
        }
        for(int i=0;i<rows;i++)
            for(int j=0;j<cols;j++)
                if(pac[{i,j}] and atl[{i,j}])
                    res.push_back({i,j});
        return res;
    }
};
```
### 200. Number of Islands
Problem Link: https://leetcode.com/problems/number-of-islands/

#### - CPP Solution
```cpp
class Solution {
    bool vis[301][301];
    int dx[4] = {0,0,-1,1};
    int dy[4] = {1,-1,0,0};
    int rows,cols;

    bool isOutBound(int r,int c){
        return r<0 or r>=rows or c<0 or c>=cols ;
    }
    void dfs(vector<vector<char>>& grid,int r,int c){
        if(isOutBound(r,c) or vis[r][c] or grid[r][c]=='0') return;
        vis[r][c] = true;
        for(int it=0;it<4;it++)
            dfs(grid,r+dx[it],c+dy[it]);
    }
public:
    int numIslands(vector<vector<char>>& grid) {
        int ans = 0;
        rows = grid.size(),cols = grid[0].size();
        for(int r=0;r<rows;r++)
            for(int c=0;c<cols;c++)
                if(!vis[r][c] && grid[r][c] == '1'){
                    dfs(grid,r,c);
                    ans++;
                }
        return ans;
    }
};
```
### 92. Reverse Linked List II
Problem Link: https://leetcode.com/problems/reverse-linked-list-ii/

#### - CPP Solution
```cpp
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int left, int right) {
        if(left == right)
            return head;
        int cnt = 1;
        ListNode* leftNode ,*rightNode = head,*node;
        while(cnt < right){
            if(cnt == left -1)
                node = rightNode;
            if(cnt == left)
                leftNode = rightNode;
            rightNode = rightNode->next;
            cnt++;      
        }
        ListNode* prev = rightNode->next;
        int steps = right - left + 1;
        while(steps --){
            ListNode* temp = leftNode->next;
            leftNode->next = prev;
            prev = leftNode;
            leftNode = temp;
        }
        if(node != nullptr)
            node->next = rightNode;
        if(left == 1) 
            head = rightNode;
        return head;
    }
};
```

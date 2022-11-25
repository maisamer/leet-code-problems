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
### 61. Rotate List
Problem Link: https://leetcode.com/problems/rotate-list/

#### - CPP Solution
```cpp
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if(head == nullptr || head->next == nullptr) return head;
        int length = 1;
        ListNode* right = head;
        while(right->next != nullptr){
            right = right->next;
            length ++;
        }
        k = k % length;
        if(!k)   return head;
        ListNode* left = head;
        int steps = length - k;
        ListNode * tail;
        while(steps){
            if(steps == 1)
                tail = left;
            left = left->next;
            steps--;
        }
        if(tail != nullptr)
            tail->next = nullptr;
        ListNode* ans = left;
        right->next = head;
        return ans;

    }
};
```
### 328. Odd Even Linked List
Problem Link: https://leetcode.com/problems/odd-even-linked-list/

#### - CPP Solution
```cpp
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        if(head == nullptr || head-> next == nullptr || head->next->next == nullptr) return head;
        ListNode* odd = head;
        ListNode*even = head->next,*even_head=head->next;
        while(even != nullptr and even->next != nullptr){
            odd->next = odd->next->next;
            even->next = even->next->next;
            even = even->next;
            odd = odd->next;
        }
        odd->next = even_head;
        return head;
    }
};
```
### 24. Swap Nodes in Pairs
Problem Link: https://leetcode.com/problems/swap-nodes-in-pairs/

#### - CPP Solution
```cpp
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if(head == nullptr || head->next == nullptr) return head;
        ListNode*dummy = new ListNode(0);
        ListNode*curr = head;
        ListNode* ans = dummy;
        while(curr != nullptr){
            if(curr->next != nullptr){
                ans->next = new ListNode(curr->next->val);
                ans = ans->next;
            }
            ans->next = new ListNode(curr->val);
            ans = ans->next;
            curr = curr->next;
            if(curr != nullptr)
                curr = curr->next;
        }
        return dummy->next;
    }
};
```
### 378. Kth Smallest Element in a Sorted Matrix
Problem Link: https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/

#### - CPP Solution
```cpp
class Solution {
    vector<int> heap;
    int left(int node){
        int left = 2*node + 1;
        return left > size() ? -1 : left;
    }
    int right(int node){
        int right = 2*node + 2;
        return right > size() ? -1 : right;
    }
    int parent(int node){ return node == 0 ? -1 : (node - 1)/2; }
    void reheapUp(int pos){
        if(pos == 0 or heap[parent(pos)] > heap[pos])
            return;
        swap(heap[pos],heap[parent(pos)]);
        reheapUp(parent(pos));
    }
    void reheapDown(int pos){
        int selchild = left(pos);
        if(selchild == -1) return;
        int rightPos = right(pos);
        if(rightPos != -1 && heap[selchild]<heap[rightPos])
            selchild = rightPos;
        if(heap[pos] < heap[selchild]){
            swap(heap[pos],heap[selchild]);
            reheapDown(selchild);
        }
    }
    int top(){ return heap.front(); }
    void push(int node){
        heap.push_back(node);
        reheapUp(size()-1);
    }
    void pop(){
        if(size()){
            heap[0] = heap.back();
            heap.pop_back();
            reheapDown(0);
        }
    }
    int size(){return heap.size();}

public:
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        int n = matrix.size();
        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                push(matrix[i][j]);
                if(size()>k)
                    pop();
            }
        }
        return top();
    }
};
```
#### - CPP Solution using build in max heap(priority queue)
```cpp
class Solution {
public:
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        int n = matrix.size();
        priority_queue<int> maxHeap;
        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                maxHeap.push(matrix[i][j]);
                if(maxHeap.size() > k)
                    maxHeap.pop();
            }
        }
        return maxHeap.top();
    }
};
 ```
### 373. Find K Pairs with Smallest Sums
Problem Link: https://leetcode.com/problems/find-k-pairs-with-smallest-sums/

#### - CPP Solution
```cpp
typedef pair<int, int> pd;

class Solution {
    struct myComp {
        constexpr bool operator()(
            pair<int, int> const& a,
            pair<int, int> const& b)
            const noexcept
        {
            return a.first + a.second < b.first + b.second;
        }
    };
public:
    vector<vector<int>> kSmallestPairs(vector<int>& nums1, vector<int>& nums2, int k) {
        vector<vector<int>> result;
        priority_queue<pd, vector<pd>, myComp> maxHeap;
        for(int i=0;i<nums1.size();i++){
            for(int j=0;j<nums2.size();j++){
                if(maxHeap.size() < k)
                    maxHeap.push({nums1[i],nums2[j]});
                else if(maxHeap.size() == k && nums1[i] + nums2[j] < maxHeap.top().first + maxHeap.top().second){
                    maxHeap.pop();
                    maxHeap.push({nums1[i],nums2[j]});
                }else if(maxHeap.size() == k && nums1[i] + nums2[j] > maxHeap.top().first + maxHeap.top().second){
                    break;
                }
            }
        }
        while(!maxHeap.empty()){
            result.push_back({maxHeap.top().first ,maxHeap.top().second});
            maxHeap.pop();
        }
        return result;
    }
};
```
### 56. Merge Intervals
Problem Link: https://leetcode.com/problems/merge-intervals/

#### - CPP Solution
```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        vector<vector<int>> ans;
        sort(intervals.begin(),intervals.end());
        for(int i = 0;i < intervals.size();i++){
            int first = intervals[i][0];
            int second = intervals[i][1];
            while(i<intervals.size()){
                if(i+1 < intervals.size() and second>=intervals[i+1][0]){
                    second = max(second,intervals[i+1][1]);
                }else{
                    break;
                }
                i++;
            }
            ans.push_back({first,second});
        }
        return ans;
    }
};
```
### 986. Interval List Intersections
Problem Link: https://leetcode.com/problems/interval-list-intersections/

#### - CPP Solution
```cpp
class Solution {
public:
    vector<vector<int>> intervalIntersection(vector<vector<int>>& firstList, vector<vector<int>>& secondList) {
        vector<vector<int>> ans;
        int i=0,j=0;
        while(i < firstList.size() and j < secondList.size()){
            int mn = min(firstList[i][1],secondList[j][1]);
            int mx = max(firstList[i][0],secondList[j][0]);
            if(mx <= mn) ans.push_back({mx,mn});
            firstList[i][1]<secondList[j][1] ? i++ : j++;
        }
        return ans;
    }
};
```

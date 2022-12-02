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
### 435. Non-overlapping Intervals
Problem Link: https://leetcode.com/problems/non-overlapping-intervals/

#### - CPP Solution
```cpp
class Solution {
public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        int ans = 0;
        sort(intervals.begin(),intervals.end());
        int prev = -5e5;
        for(int i=0;i<intervals.size();i++){
            if(intervals[i][0]<prev){
                ans++;
                if(intervals[i][1]<prev)
                    prev = intervals[i][1]; 
            }else
             prev = intervals[i][1];
        }
        return ans;
    }
};
```
### 621. Task Scheduler
Problem Link: https://leetcode.com/problems/task-scheduler/

#### - CPP Solution
```cpp
class Solution {
public:
    int leastInterval(vector<char>& tasks, int n) {
        map<char,int> m;
        int ans = tasks.size();
        int mx = 0; 
        for(int i=0;i<tasks.size();i++){
            m[tasks[i]]++;
            mx = max(mx,m[tasks[i]]);
        }
        ans+=(mx-1)*n;
        bool flag = true;
        for (auto const& [key, val] : m){
            if(ans <= tasks.size()) break;
            if(val == mx && flag){
                flag = false;
                continue;
            } 
            if(val == mx)
                ans -=val-1;
            else
                ans -=val;
        }
        return ans <= tasks.size() ? tasks.size() : ans;
    }
};
```
### 452. Minimum Number of Arrows to Burst Balloons
Problem Link: https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/

#### - CPP Solution
```cpp
class Solution {
public:
    int findMinArrowShots(vector<vector<int>>& points) {
        sort(points.begin(),points.end(),[](auto &left, auto &right) {
             return left[1] < right[1];
        });
        int ans = 1;
        int prev = points[0][1];
        for(int i=0;i<points.size();i++){
            cout<< points[i][0] << points[i][1] <<endl;
        }
        for(int i=1;i<points.size();i++){
            if(prev <= points[i][1] and prev >= points[i][0]){
                continue;
            }
            ans ++;
            prev = points[i][1];
        }
        return ans;
    }
};
```
### 57. Insert Interval
Problem Link: https://leetcode.com/problems/insert-interval/

#### - CPP Solution
```cpp
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        vector<vector<int>> ans;
        int index = -1;
        for(int i=0;i<intervals.size();i++){
            if(intervals[i][0]<=newInterval[0]&&intervals[i][1]>=newInterval[0]
                or intervals[i][0]<=newInterval[1]&&intervals[i][1]>=newInterval[1]){
                intervals[i][0] = min(intervals[i][0],newInterval[0]);
                intervals[i][1] = max(intervals[i][1],newInterval[1]); 
                index = i;
                break;
            }else if(intervals[i][0]>newInterval[0]){
                intervals.insert(intervals.begin()+i,newInterval);
                index = i;
                break;
            }
        }
        if(index == -1){
            intervals.push_back(newInterval);
            return intervals;
        }
        int prev = intervals[0][1];
        for(int i=0;i<intervals.size();i++){
            int a = intervals[i][0];
            int b = intervals[i][1];
            while(i+1<intervals.size() && b>=intervals[i+1][0]){
                b = max(intervals[i+1][1],b); 
                i++;
            }
            prev = b;
            ans.push_back({a,b});
        }
        return ans;
    }
};
```
### 162. Find Peak Element
Problem Link: https://leetcode.com/problems/find-peak-element/

#### - CPP Solution
```cpp
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int l=0,r=nums.size()-1;
        while(l<=r){
            int mid = (l+r)/2;
            if((mid-1 < 0 or nums[mid]>nums[mid-1]) and (mid+1 >= nums.size() or nums[mid]>nums[mid+1]))
                return mid;
            else if(mid-1 >=0 and nums[mid-1]>nums[mid])  r = mid-1;
            else l = mid+1;
        }
        return r;
    }
};
```
### 33. Search in Rotated Sorted Array
Problem Link: https://leetcode.com/problems/search-in-rotated-sorted-array/

#### - CPP Solution
```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int l=0,r=nums.size()-1;
        while(l<=r){
            int mid = (l+r)/2;
            if(nums[mid]==target)
                return mid;
             // left sorting part
            if(nums[l]<=nums[mid]){
                if(target > nums[mid]) l = mid + 1;
                else if(target >= nums[l]) r = mid - 1;
                else l = mid + 1;
            }else{ // right sorting part
                if(target < nums[mid]) r = mid - 1;
                else if(target>nums[r]) r = mid - 1;
                else l = mid+1;

            }
        }
        return -1;
    }
};
```
### 81. Search in Rotated Sorted Array II
Problem Link: https://leetcode.com/problems/search-in-rotated-sorted-array-ii/

#### - CPP Solution
```cpp
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        int l=0,r=nums.size()-1;
        while(l<=r){
            int mid = (l+r)/2;
            if(nums[mid]==target)
                return true;
            if(nums[l] == nums[mid] and nums[r] == nums[mid]){
                l++;
                r--;
            }
             // left sorting part
            else if(nums[l]<=nums[mid]){
                if(target > nums[mid]) l = mid + 1;
                else if(target >= nums[l]) r = mid - 1;
                else l = mid + 1;
            }else{ // right sorting part
                if(target < nums[mid]) r = mid - 1;
                else if(target>nums[r]) r = mid - 1;
                else l = mid+1;

            }
        }
        return false;
    }
};
```
### 74. Search a 2D Matrix
Problem Link: https://leetcode.com/problems/search-a-2d-matrix/solutions/

#### - CPP Solution
```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int rows = matrix.size(),cols=matrix[0].size();
        int l=0,r =rows*cols - 1;
        while(l<=r){
            int mid = (l+r)/2;
            int i = mid/cols;
            int j = mid%cols;
            if(matrix[i][j] == target)
                return true;
            if(matrix[i][j] > target)
                r = mid-1;
            else
                l = mid+1;
        }
        return false;
    }
};
```
### 240. Search a 2D Matrix II
Problem Link: https://leetcode.com/problems/search-a-2d-matrix-ii/

#### - CPP Solution
```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int rows = matrix.size(),cols=matrix[0].size();
        for(int i=0;i<matrix.size();i++){
            int l=0,r=cols-1;
            while(l<=r){
                int mid = (l+r)/2;
                if(matrix[i][mid] == target)
                    return true;
                if(matrix[i][mid] > target)
                    r = mid-1;
                else
                    l = mid+1;
            }
        }
        return false;
    }
};
```
### 658. Find K Closest Elements
Problem Link: https://leetcode.com/problems/find-k-closest-elements/

#### - java Solution
```java
class Solution {
    public List<Integer> findClosestElements(int[] arr, int k, int x) {
        int l=0,r=arr.length-k;
        List<Integer> ans = new ArrayList();
        while(l<r){
            int mid = (l+r)/2;
            if(arr[mid]-x<x-arr[mid+k]) l = mid+1;
            else r = mid;
        }
        for(int i=l;i<l+k;i++){
            ans.add(arr[i]);
        }
        return ans;
    }
}
```
### 209. Minimum Size Subarray Sum
Problem Link: https://leetcode.com/problems/minimum-size-subarray-sum/

#### - CPP Solution
```cpp
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int l=0,sum=0;
        int res = Integer.MAX_VALUE;
        for(int r=0;r<nums.length;r++){
            sum+=nums[r];
            while(sum>=target){
                res = Math.min(res,r-l+1);
                sum-=nums[l];
                l++;
            }
        }
        return res == Integer.MAX_VALUE ? 0 : res;
    }
}
```
### 904. Fruit Into Baskets
Problem Link: https://leetcode.com/problems/fruit-into-baskets/

#### - java Solution
```java
class Solution {
    public int totalFruit(int[] fruits) {
        int l=0,key1=-1,key2=-1,count1=0,count2=0,res=0;
        for(int r=0;r<fruits.length;r++){
            if(fruits[r] != key1 && fruits[r] != key2){
                while(count1>0 && count2>0){
                    if(fruits[l]==key1) {count1--; if(count1 == 0) key1=-1;}
                    else {count2--; if(count2 == 0)key2=-1;}
                    l++;
                }
            }
            if(key1 == -1 || fruits[r] == key1){
                count1++;
                key1=fruits[r];
            }else if(key2 == -1 || fruits[r] == key2){
                count2++;
                key2=fruits[r];
            }
            res=Math.max(res,count1+count2);
        }
        return res;
    }
}
```
### 567. Permutation in String
Problem Link: https://leetcode.com/problems/permutation-in-string/

#### - CPP Solution
```cpp
class Solution {
public:
    bool checkInclusion(string s1, string s2) {
        int l=0,target=s1.size();
        vector<int> v(28,-1);
        for(int i=0;i<s1.size();i++) {
            int index = s1[i]-'a';
            if(v[index] == -1) v[index]+=2;
            else v[index]+=1;
        }
        for(int r=0;r<s2.size();r++){
            int index = s2[r] - 'a';
            if(v[index]<=0){
                if(v[index] == -1){
                    while(target < s1.size()){
                        int lci = s2[l]-'a';
                        target++;
                        v[lci]++;
                        l++;
                    }
                }else{
                    while(v[index] <= 0){
                        int lci = s2[l]-'a';
                        target++;
                        v[lci]++;
                        l++;
                    }
                }
            }
            if(v[index]>0){
                v[index]--;
                target--;
            }else{
                l++;
            }
            if(target <= 0) return true;

        }
        return false;
    }
};
```
### 424. Longest Repeating Character Replacement
Problem Link: https://leetcode.com/problems/longest-repeating-character-replacement/

#### - CPP Solution
```cpp
class Solution {
public:
    int characterReplacement(string s, int k) {
        int l = 0,res=0,rem=0,mxfreq=0;
        map<char,int> m;
        for(int r=0;r<s.size();r++){
            m[s[r]]++;
            mxfreq = max(mxfreq,m[s[r]]);
            rem = k-(r-l+1-mxfreq);
            while(rem < 0){
                m[s[l]]--;
                l++;
                rem = r-l+1-mxfreq;
            }
            res=max(res,r-l+1);
        }
        return res;
    }
};

```
### 3. Longest Substring Without Repeating Characters
Problem Link: https://leetcode.com/problems/longest-substring-without-repeating-characters

#### - CPP Solution
```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        map<char,int>m;
        int pnt =0,longSequence =0;
        for(int i=0;i<s.size();i++){
            if(m[s[i]]>=pnt+1){
                pnt = m[s[i]];
                m[s[i]] = i+1;
            }else{
                m[s[i]]=i+1;
                longSequence=max(longSequence,i-pnt+1);
            }
        }
        return longSequence;
    }
};
```
### 230. Kth Smallest Element in a BST
Problem Link: https://leetcode.com/problems/kth-smallest-element-in-a-bst

#### - CPP Solution using Inorder traverse
```cpp
class Solution {
    vector<int> v;
    void inOrder(TreeNode* root) {
        if(root == nullptr)
            return;
        inOrder(root->left);
        v.push_back(root->val);
        inOrder(root->right);   
    }
public:
    int kthSmallest(TreeNode* root, int k) {
        inOrder(root);
        return v[k-1];
    }
};
```
#### - CPP Solution using Stack
```cpp
class Solution {
public:
    int kthSmallest(TreeNode* root, int k) {
        stack<TreeNode*> s;
        TreeNode* curr = root;
        while(!s.empty() or curr != nullptr){
            // go left first
            while(curr != nullptr){
                s.push(curr);
                curr = curr->left;
            }
            curr = s.top();
            s.pop();
            k --;
            if(!k)
                return curr->val;
            curr = curr->right;
        }
        return -1;

    }
};
```
### 973. K Closest Points to Origin
Problem Link: https://leetcode.com/problems/k-closest-points-to-origin

#### - CPP Solution using sort approach
```cpp
class Solution {
public:
    vector<vector<int>> kClosest(vector<vector<int>>& points, int k) {
        sort(points.begin(),points.end(), [](vector<int> const& a, vector<int> const& b)
        {
             return a[0]*a[0]+a[1]*a[1] < b[0]*b[0]+b[1]*b[1];
        });
        vector<vector<int>> newVec(points.begin(), points.begin()+k);
        return newVec;
    }
};
```
#### - CPP Solution using min heap approach
```cpp
struct myComp {
    constexpr bool operator()(
        vector<int> const& a,
        vector<int> const& b)
        const noexcept
    {
        return a[0]*a[0]+a[1]*a[1] > b[0]*b[0]+b[1]*b[1];
    }
 };
class Solution {
public:
    vector<vector<int>> kClosest(vector<vector<int>>& points, int k) {
        priority_queue<vector<int>,vector<vector<int>>,myComp> q;
        vector<vector<int>> ans;
        for(int i=0;i<points.size();i++){
            q.push(points[i]);
        }
        while(k--){
            ans.push_back(q.top());
            q.pop();
        }
        return ans;
    }
};
```
### 347. Top K Frequent Elements
Problem Link: https://leetcode.com/problems/top-k-frequent-elements

#### - CPP Solution using max heap
```cpp
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        map<int,int>m;
        priority_queue<pair<int,int>> q;
        vector<int> ans;
        for(int i=0;i<nums.size();i++)
            m[nums[i]]++;
        for (auto const& [key, val] : m)
            q.push({val,key});
        while(k--){
            ans.push_back(q.top().second);
            q.pop();
        }
        return ans;
    }
};
```
#### - CPP Solution using buket sort
```cpp
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        map<int,int>m;
        vector<vector<int>> freq(nums.size()+1);
        vector<int> ans;
        for(int i=0;i<nums.size();i++)
            m[nums[i]]++;
        for (auto const& [key, val] : m)
            freq[val].push_back(key);
        for(int i=nums.size();i>=0;i--){
            for(int j=0;j<freq[i].size();j++){
                ans.push_back(freq[i][j]);
                k--;
                if(!k)
                    return ans;
            }
        }
        return ans;
    }
};
```
### 451. Sort Characters By Frequency
Problem Link: https://leetcode.com/problems/sort-characters-by-frequency/

#### - CPP Solution
```cpp
class Solution {
public:
    string frequencySort(string s) {
        string ans = "";
        vector<pair<int,char>> freq(123,{0,'_'});
        for(int i=0;i<s.size();i++){
            int index = (int)s[i];
            freq[index].first+=1;
            freq[index].second = s[i];
        }
        sort(freq.begin(),freq.end());
        int sz = 0;
        for(int i=freq.size()-1;i>=0;i--){
            char c = freq[i].second;
            int cnt = freq[i].first;
            while(cnt--){
                sz++;
                ans+=c;
            }
            if(sz == s.size())
                break;
        }
        return ans;
    }
};
```
### 215. Kth Largest Element in an Array
Problem Link: https://leetcode.com/problems/kth-largest-element-in-an-array

#### - CPP Solution using max heap
```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        int ans = 0;
        make_heap(nums.begin(), nums.end());
        while(k--){
            ans = nums.front();
            pop_heap(nums.begin(), nums.end());
            nums.pop_back();
        }
        return ans;
    }
};
```
#### - CPP Solution using Quick select algorithm
```cpp
class Solution {
    int target = 0;
    int quickSelect(vector<int>& nums,int l,int r){
        int pivot = nums[r] , pt = l;
        for(int i=l;i<r;i++){
            if(nums[i]<=pivot){
                swap(nums[i],nums[pt]);
                pt++;
            }
        }
        swap(nums[r],nums[pt]);
        if(target == pt)    return nums[pt];
        if(target > pt)     return quickSelect(nums,pt+1,r);
        else    return quickSelect(nums,l,pt-1);
    }
public:
    int findKthLargest(vector<int>& nums, int k) {
        target = nums.size() - k;
        return quickSelect(nums,0,nums.size()-1);
    }
};
```
### 767. Reorganize String
Problem Link: https://leetcode.com/problems/reorganize-string/

#### - CPP Solution
```cpp
class Solution {
public:
    string reorganizeString(string s) {
        map<char,int> m;
        int mxFreq = 0;
        char c ;
        for(int i=0;i<s.size();i++){
            m[s[i]]++;
            mxFreq = max(mxFreq,m[s[i]]);
            if(m[s[i]]==mxFreq)
                c = s[i];
        }
        int i = 0;
        string ans = "";
        if(mxFreq-1 > s.size()-mxFreq)
            return ans;
        while(i<s.size()){
            ans+=c;
            i++;
            mxFreq--;
            for(auto const&[key,val] : m){
                if(mxFreq>0 and mxFreq-1 >= s.size()-i-mxFreq)
                    break;
                if(key != c and val){
                    ans += key;
                    m[key]--;
                    i++;
                }
            }
        }
        return ans;
    }
};

```
### 207. Course Schedule
Problem Link: https://leetcode.com/problems/course-schedule

#### - CPP Solution
```cpp
class Solution {
    bool vis[2002];
    vector<int> adj[2003];
    bool isCycle(int node,int numCourses){
        if(vis[node])
            return true;
        if(adj[node].size()==0)
            return false;
        vis[node] = true;
        for(int i=0;i<adj[node].size();i++){
            if(isCycle(adj[node][i],numCourses))
                return true;
        }
        vis[node] = false;
        adj[node] = {};
        return false;
    }
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        for(int i=0;i<prerequisites.size();i++){
            adj[prerequisites[i][0]].push_back(prerequisites[i][1]);
        }
        for(int i=0;i<numCourses;i++){
            if(isCycle(i,numCourses))
                return false;
        }
        return true;
    }
};

```
### 210. Course Schedule II
Problem Link: https://leetcode.com/problems/course-schedule-ii

#### - CPP Solution
```cpp
class Solution {
    bool vis[2002];
    vector<int> adj[2003];
    vector<int> ans;
    set<int> exist;
    bool isCycle(int node,int numCourses){
        if(vis[node])
            return true;

        if(exist.find(node) != exist.end())
            return false;
        
        vis[node] = true;
        for(int i=0;i<adj[node].size();i++){
            if(isCycle(adj[node][i],numCourses))
                return true;
        }
        ans.push_back(node);
        exist.insert(node);
        vis[node] = false;
        return false;
    }
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        for(int i=0;i<prerequisites.size();i++){
            adj[prerequisites[i][0]].push_back(prerequisites[i][1]);
        }
        for(int i=0;i<numCourses;i++){
            if(isCycle(i,numCourses)){
                return {};
            }
        }
        return ans;
    }
};
```
### 
Problem Link: 

#### - CPP Solution
```cpp

```
### 
Problem Link: 

#### - CPP Solution
```cpp

```

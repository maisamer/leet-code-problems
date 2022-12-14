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
### 310. Minimum Height Trees
Problem Link: https://leetcode.com/problems/minimum-height-trees/

#### - CPP Solution
```cpp
class Solution {
public:
    vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
        vector<int> leaves;
        if(n <= 2){
            for(int i=0;i<n;i++)
                leaves.push_back(i);
        }else{
            vector<int> adj[20000+2];
            vector<int> degree(n, 0);
            for(int i=0;i<edges.size();i++){
                adj[edges[i][0]].push_back(edges[i][1]);
                adj[edges[i][1]].push_back(edges[i][0]);
                degree[edges[i][0]]++;
                degree[edges[i][1]]++;
            }
            for(int i=0;i<n;i++)
                if(degree[i]==1)
                    leaves.push_back(i);
            while(n > 2){
                n-=leaves.size();
                vector<int> temp;
                for(auto leaf : leaves){
                    for(auto node : adj[leaf]){
                        degree[node]--;
                        if(degree[node]==1)
                            temp.push_back(node);
                    }
                }
                leaves = temp;
            }
        }
        return leaves;
    }
};
```
### 107. Binary Tree Level Order Traversal II
Problem Link: https://leetcode.com/problems/binary-tree-level-order-traversal-ii/

#### - CPP Solution
```cpp
class Solution {
public:
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        if(root == nullptr) return {};
        vector<vector<int>> ans;
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            int sz = q.size();
            vector<int> level;
            while(sz --){
                TreeNode* curr = q.front();
                q.pop();
                level.push_back(curr->val);
                if(curr->left != nullptr)   q.push(curr->left);
                if(curr->right != nullptr)   q.push(curr->right);
            }
            ans.push_back(level);
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```
### 102. Binary Tree Level Order Traversal
Problem Link: https://leetcode.com/problems/binary-tree-level-order-traversal/

#### - CPP Solution
```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        if(root == nullptr) return {};
        vector<vector<int>> ans;
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            int sz = q.size();
            vector<int> level;
            while(sz --){
                TreeNode* curr = q.front();
                q.pop();
                level.push_back(curr->val);
                if(curr->left != nullptr)   q.push(curr->left);
                if(curr->right != nullptr)   q.push(curr->right);
            }
            ans.push_back(level);
        }
        return ans;
    }
};
```
### 103. Binary Tree Zigzag Level Order Traversal
Problem Link: https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/

#### - CPP Solution using stack
```cpp
class Solution {
public:
vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
    std::stack<TreeNode*>s1,s2 ;
    vector<vector<int>>ans;
    if(root != NULL) s1.push(root);
    int sz =0;
    vector<int> v;
    while(!s1.empty()||!s2.empty()){
        while(!s1.empty()){
            TreeNode* curr = s1.top();
            s1.pop();
            v.push_back(curr->val);
            if(curr->left != nullptr) s2.push(curr->left);
            if(curr->right != nullptr) s2.push(curr->right);
            if(s1.empty()){
                ans.push_back(v);
                v.clear();
            }
        }
        while(!s2.empty()){
            TreeNode* curr = s2.top();
            s2.pop();
            v.push_back(curr->val);
            if(curr->right != nullptr) s1.push(curr->right);
            if(curr->left != nullptr) s1.push(curr->left);
            if(s2.empty()){
                ans.push_back(v);
                v.clear();
            }
        }
    }
    return ans;
}
};

```

#### - CPP Solution using bfs
```cpp
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        if(root == nullptr) return {};
        vector<vector<int>> ans;
        queue<TreeNode*> q;
        q.push(root);
        bool flag = false;
        while(!q.empty()){
            int sz = q.size();
            vector<int> level;
            while(sz --){
                TreeNode* curr = q.front();
                q.pop();
                level.push_back(curr->val);
                if(curr->left != nullptr)   q.push(curr->left);
                if(curr->right != nullptr)   q.push(curr->right);
            }
            if(flag)
                reverse(level.begin(),level.end());
            flag = !flag;
            ans.push_back(level);
        }
        return ans;
    }
};
```
### 116. Populating Next Right Pointers in Each Node
Problem Link: https://leetcode.com/problems/populating-next-right-pointers-in-each-node/

#### - CPP Solution
```cpp
class Solution {
public:
    Node* connect(Node* root) {
        queue<Node*> q;
        if(root != nullptr) q.push(root);
        while(!q.empty()){
            Node* next = nullptr;
            int sz = q.size();
            while(sz--){
               Node* curr=q.front();
               q.pop();
               curr->next = next;
               next = curr;
               if(curr->right != nullptr) q.push(curr->right); 
               if(curr->left != nullptr) q.push(curr->left);
            }
        }
        return root;
    }
};
```
### 116. Populating Next Right Pointers in Each Node
Problem Link: https://leetcode.com/problems/populating-next-right-pointers-in-each-node

#### - CPP Solution
```cpp
class Solution {
public:
    Node* connect(Node* root) {
        Node* curr = root;
        Node* nxt = nullptr;
        if(curr != nullptr)
            nxt = curr->left;
        while(curr != nullptr and nxt != nullptr){
            curr->left->next = curr->right;
            if(curr->next != nullptr)
                curr->right->next = curr->next->left;
            curr = curr->next;
            if(curr == nullptr){
                curr = nxt;
                nxt = curr->left;
            }

        }
        return root;
    }
};
```
### 199. Binary Tree Right Side View
Problem Link: https://leetcode.com/problems/binary-tree-right-side-view

#### - CPP Solution
```cpp
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        if(root == nullptr) return {};
        vector<int> level;
        queue<TreeNode*> q;
        q.push(root);
        bool flag = true;
        while(!q.empty()){
            int sz = q.size();
            while(sz --){
                TreeNode* curr = q.front();
                q.pop();
                if(flag){
                    level.push_back(curr->val);
                    flag = false;
                }
                if(curr->right != nullptr)   q.push(curr->right);
                if(curr->left != nullptr)   q.push(curr->left);
            }
            flag = true;
        }
        return level;
    }
};
```
### 863. All Nodes Distance K in Binary Tree
Problem Link: https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree/

#### - CPP Solution
```cpp
class Solution {
    void buildGraph(vector<vector<int>>& adj,TreeNode* node){
        if(node == nullptr)
            return ;
        if(node->left != nullptr){
            adj[node->val].push_back(node->left->val);
            adj[node->left->val].push_back(node->val);
        }
        if(node->right != nullptr){
            adj[node->val].push_back(node->right->val);
            adj[node->right->val].push_back(node->val);
        }
        buildGraph(adj,node->left);
        buildGraph(adj,node->right);
    }
public:
    vector<int> distanceK(TreeNode* root, TreeNode* target, int k) {
        vector<vector<int>> adj(510);
        vector<bool> vis(505,false);
        buildGraph(adj,root);
        queue<int>q;
        q.push(target->val);
        vector<int> ans;
        while(!q.empty() and k){
            int sz = q.size();
            while(sz--){
                int curr = q.front();
                q.pop();
                vis[curr] = true;
                for(int i=0;i<adj[curr].size();i++){
                    if(!vis[adj[curr][i]]){
                        q.push(adj[curr][i]);
                    }
                }
            }
            k--;
        }
        while(!q.empty()){
            ans.push_back(q.front());
            q.pop();
        }
        return ans;
    }
};
```
### 113. Path Sum II
Problem Link: https://leetcode.com/problems/path-sum-ii/

#### - CPP Solution
```cpp
class Solution {
    vector<vector<int>> ans;
    void dfs(TreeNode* node,int targetSum,int sum,vector<int> path){
        if(node == nullptr)
            return;
        sum += node->val;
        path.push_back(node->val);
        if(sum == targetSum && node->left == nullptr && node->right == nullptr){
            ans.push_back(path);
            return;
        }
        dfs(node->left,targetSum,sum,path);
        dfs(node->right,targetSum,sum,path);
    }
public:
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        dfs(root,targetSum,0,{});
        return ans;
    }
};
```
### 437. Path Sum III
Problem Link: https://leetcode.com/problems/path-sum-iii/

#### - JAVA Solution
```java
class Solution {
    Map<Long,Integer> freq;
    int dfs(TreeNode node,Long prevSum,int targetSum){
        if(node == null)
            return 0;
        Long currentSum = prevSum + node.val;
        Long x = currentSum - targetSum;
        int sol = 0;
        if(freq.containsKey(x)){
            sol = freq.get(x);
        }
        if(freq.containsKey(currentSum))
            freq.put(currentSum,1+freq.get(currentSum));
        else
            freq.put(currentSum,1);
        int ans = sol + dfs(node.left,currentSum,targetSum) + dfs(node.right,currentSum,targetSum);
        freq.put(currentSum,freq.get(currentSum)-1);
        return ans;
    }
    public int pathSum(TreeNode root, int targetSum) {
        freq = new HashMap<>();
        freq.put(0L,1);
        return dfs(root,0L,targetSum);
    }
}
```
### 236. Lowest Common Ancestor of a Binary Tree
Problem Link: https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/

#### - JAVA Solution
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null)
            return null;
        if(root.val == p.val || root.val == q.val)
            return root;
        TreeNode left = lowestCommonAncestor(root.left,p,q);
        TreeNode right = lowestCommonAncestor(root.right,p,q);
        if(left != null && right != null)
            return root;
        return left == null ? right : left;
        
    }
}
```
### 654. Maximum Binary Tree
Problem Link: https://leetcode.com/problems/maximum-binary-tree/

#### - CPP Solution
```cpp
class Solution {
    private TreeNode buildTree(int[] nums,int l,int r){
        if(r-l+1<=0)
            return null;
        int mxInd = l;
        for(int i=l+1;i<=r;i++)
            if(nums[mxInd]<nums[i])
                mxInd = i;
        TreeNode root = new TreeNode(nums[mxInd]);
        root.left = buildTree(nums,l,mxInd-1);
        root.right = buildTree(nums,mxInd+1,r);
        return root;
    }
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        return buildTree(nums,0,nums.length-1);
    }
}
```
### 662. Maximum Width of Binary Tree
Problem Link: https://leetcode.com/problems/maximum-width-of-binary-tree/

#### - CPP Solution
```cpp

class Solution {
public:
    int widthOfBinaryTree(TreeNode* root) {
       queue<pair<TreeNode*,long long>> q;
       int res = 1;
       q.push({root,0});
       while(!q.empty()){
           int sz = q.size();
           int ptr = q.front().second;
           while(sz--){
               auto [curr,width] = q.front();
               if(curr->left)
                    q.push({curr->left,width*2-2*ptr});
                if(curr->right)
                    q.push({curr->right,width*2+1-2*ptr});
               q.pop();
           }
           if(!q.empty()){
               int level_width = q.back().second - q.front().second + 1;
               res = max(res,level_width);
           }
       }
       return res; 
    }
};
```
### 105. Construct Binary Tree from Preorder and Inorder Traversal
Problem Link: https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal

#### - CPP Solution
```cpp
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        if(!preorder.size() or !inorder.size())
            return nullptr;
        TreeNode* root = new TreeNode(preorder[0]);
        int mid = find(inorder.begin(),inorder.end(), preorder[0]) - inorder.begin();
        vector<int> left_inorder(inorder.begin(), inorder.begin()+mid);
        vector<int> right_inorder(inorder.begin()+mid+1, inorder.end());
        vector<int> left_preorder(preorder.begin()+1, preorder.begin()+mid+1);
        vector<int> right_preorder(preorder.begin()+mid+1, preorder.end());
        root->left = buildTree(left_preorder,left_inorder);
        root->right = buildTree(right_preorder,right_inorder);
        return root;
    }
};
```
### 98. Validate Binary Search Tree
Problem Link: https://leetcode.com/problems/validate-binary-search-tree/

#### - CPP Solution
```cpp
class Solution {
public:

    bool isValidBST(TreeNode* root,long left,long right) {
        if(root == nullptr)
            return true;
        if(root->val > right or root->val < left)
            return false;
        return isValidBST(root->left,left,root->val-1LL) and isValidBST(root->right,root->val+1LL,right);
    }
    bool isValidBST(TreeNode* root) {
        return isValidBST(root,-long(1LL<<31), long(1LL<<31));
    }
};
```
### 208. Implement Trie (Prefix Tree)
Problem Link: https://leetcode.com/problems/implement-trie-prefix-tree

#### - CPP Solution
```cpp
class TrieNode{
public:
    map<char,TrieNode*> children;
    bool endOfWords;
    TrieNode(){
        endOfWords = false;
    }
};
class Trie {
public:
    TrieNode* root;
    Trie() {
        root = new TrieNode();
    }
    
    void insert(string word) {
        TrieNode* curr = root;
        for(auto c : word){
            if(curr->children[c]==NULL)
                curr->children[c] = new TrieNode();
            curr = curr->children[c];
        }
        curr->endOfWords = true;
    }
    
    bool search(string word) {
        TrieNode* curr = root;
        for(auto c : word){
            if(curr->children[c] == NULL)
                return false;
            curr = curr->children[c];
        }   
        return curr->endOfWords;
    }
    
    bool startsWith(string prefix) {
        TrieNode* curr = root;
        for(auto c : prefix){
            if(curr->children[c] == NULL)
                return false;
            curr = curr->children[c];
        }
        return true;
    }
};
```
### 15. 3Sum
Problem Link: https://leetcode.com/problems/3sum/

#### - CPP Solution
```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> res;
        sort(nums.begin(),nums.end());
        for(int i=0;i<nums.size();i++){
            if(i > 0 and nums[i] == nums[i-1]) 
                continue;
            int l = i+1 , r=nums.size()-1;
            while(l < r){
                int sum = nums[i] + nums[l] + nums[r];
                if(sum == 0){
                    res.push_back({nums[i],nums[l],nums[r]});
                    l++;
                    while(nums[l] == nums[l-1] and l<r) l++;
                }
                else if(sum > 0)
                    r--;
                else
                    l++;
            }
        }
        return res;
    }
};
```
### 16. 3Sum Closest
Problem Link: https://leetcode.com/problems/3sum-closest/

#### - CPP Solution
```cpp
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        int res = INT_MAX;
        int cloestSum = 0;
        sort(nums.begin(),nums.end());
        for(int i=0;i<nums.size();i++){
            if(i>0 and nums[i]==nums[i-1])
                continue;
            int l=i+1,r=nums.size()-1;
            while(l < r){
                int sum = nums[i] + nums[l] + nums[r];
                int diff = sum-target;
                if(diff == 0)
                    return sum;
                if(abs(diff) < res){
                    res = abs(diff);
                    cloestSum = sum;
                }
                if(sum > target)
                    r--;
                else
                    l++;
            }
        }  
        return cloestSum;
    }
};
```
### 713. Subarray Product Less Than K
Problem Link: https://leetcode.com/problems/subarray-product-less-than-k/

#### - CPP Solution
```cpp
class Solution {
public:
    int numSubarrayProductLessThanK(vector<int>& nums, int k) {
        if(k<=1)
            return 0;
        int ans = 0,l=0,r=0 , prod = 1;
        while(r < nums.size()){
            prod*=nums[r];
            while(prod >= k){
                prod/=nums[l];
                l++;
            }
            ans+= r-l+1;
            r++;
        }
        return ans;
    }
};
```
### 75. Sort Colors
Problem Link: https://leetcode.com/problems/sort-colors/

#### - CPP Solution using bucket sort algorithm
```cpp
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int count[3]={0,0,0};
        for(int i = 0;i < nums.size();i++)
            count[nums[i]]++;
        int l = 0;
        for(int i=0;i<3;i++)
            for(int j=0;j<count[i];j++)
                nums[l++] = i;
    }
};
```

#### - CPP Solution using two pointer approach
```cpp
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int l = 0,r=nums.size()-1,i=0;
        while(i<=r){
            if(nums[i] == 0){
                swap(nums[i],nums[l]);
                l++;
            }else if(nums[i] == 2){
                swap(nums[i],nums[r]);
                r--;
                i--;
            }
            i++;
        }
    }
};
```
### 11. Container With Most Water
Problem Link: https://leetcode.com/problems/container-with-most-water/

#### - CPP Solution
```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        int l=0,r=height.size()-1,res=0;
        while(l<r){
            res = max(res,min(height[l],height[r]) * (r-l));
            if(height[l]<height[r])
                l++;
            else if(height[l]>height[r])
                r--;
            else{
                if(l+1 < r and r-1>l and height[l+1]>height[r-1])
                    l++;
                else
                    r--;
            }
        }
        return res;
    }
};
```
### 720. Longest Word in Dictionary
Problem Link: https://leetcode.com/problems/longest-word-in-dictionary/description/

#### - CPP Solution
```cpp
class Trie{
    class TrieNode{
    public :
        map<char,TrieNode*> children;
        bool endWord;
        TrieNode(){
            endWord = false;
        }
    };
    TrieNode* root;
public:
    Trie(){
        root = new TrieNode();
    }
    void insert(string word){
        TrieNode* curr = root;
        for(auto c:word){
            if(curr->children[c] == NULL)
                curr->children[c] = new TrieNode();
            curr = curr->children[c];
        }
        curr->endWord = true;
    }
    bool checkWord(string word){
        TrieNode* curr = root;
        for(auto c: word){
            if(curr->children[c] == NULL)
                return false;
            curr = curr->children[c];
            if(!curr->endWord)
                return false;
        }
        return true;
    }
};
class Solution {
public:
    string longestWord(vector<string>& words) {
        string ans="";
        Trie trie;
        for(auto w : words)
            trie.insert(w);
        for(auto w : words){
            if (size(w) < size(ans) or size(w) == size(ans) and w >= ans)
                continue;
            if(trie.checkWord(w))
                    ans = w;
        }
        return ans;
    }
};
```
### 421. Maximum XOR of Two Numbers in an Array
Problem Link: https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/

#### - CPP Solution
```cpp
class Solution {
public:
    int findMaximumXOR(vector<int>& nums) {
        int mask=0,ans=0;
        for(int i=31;i>=0;i--){
            set<int> found;
            mask = mask | 1<<i;
            for(auto num : nums)
                found.insert(num & mask);
            int temp = ans | 1<<i;
            for (auto& f : found){
                if(found.find(f^temp) != found.end()){
                    ans = temp;
                } 
            }
        }
        return ans;
    }
};
```

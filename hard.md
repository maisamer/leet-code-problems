## Hard problem

### 41. First Missing Positive
Problem Link: https://leetcode.com/problems/first-missing-positive/

#### - CPP Solution
```cpp
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        for(int i=0;i<nums.size();i++){
            if(nums[i] <= 0)
                nums[i] = 0;
        }
        for(int i=0;i<nums.size();i++){
            if(nums[i] >= 0 && nums[i] > nums.size() || nums[i] == 0)
                continue;
            int temp = abs(nums[i]) - 1;
            if(nums[temp] > 0)
                nums[temp] = -1 * nums[temp];
            if(nums[temp] == 0)
                nums[temp] = -1 * (temp+1);
        }
        for(int i=0;i<nums.size();i++){
            if(nums[i] >= 0){
                return i+1;
            }
        }
        return nums.size()+1;
    }
};
```
### 37. Sudoku Solver
Problem Link: https://leetcode.com/problems/sudoku-solver/

#### - CPP Solution
```cpp
class Solution {
    bool valid(vector<vector<char>>& board,int num,int i,int j){
        char ch = num + '0';
        for(int t=0;t<9;t++){
            if(board[i][t] == ch||board[t][j] == ch)
                return false;
        }
        i = i/3*3;
        j = j/3*3;
        for(int r = i;r<i+3;r++){
            for(int c = j;c<j+3;c++){
                if(board[r][c] == ch)
                    return false;
            }
        }
        return true;
    }
    bool backtrack(vector<vector<char>>& board,int i,int j){
        if (j == 9) {
            i += 1;
            j = 0;
        }
        if (i == 9)
            return true;
        if(board[i][j] != '.')
            return backtrack(board,i,j+1);
        for(int choice=1;choice<10;choice++){
            char c = choice + '0';
            if(valid(board,choice,i,j)){
                board[i][j] = c;
                if(backtrack(board,i,j+1))
                    return true;
            }
            board[i][j] = '.'; 
        }
        return false;
    }
public:
    void solveSudoku(vector<vector<char>>& board) {
         backtrack(board,0,0);
    }
};
```
### 51. N-Queens
Problem Link: https://leetcode.com/problems/n-queens/

#### - CPP Solution
```cpp
class Solution {
    vector<vector<string>> ans;
    bool vaild(vector<string> grid,int n,int r,int c){
        for(int t=0;t<n;t++)
            if(grid[r][t] == 'Q')
                return false;
        for(int i=r,j=c;i<n&&j<n;i++,j++)
            if(grid[i][j] == 'Q')
                return false;
        for(int i=r,j=c;i>=0&&j>=0;i--,j--)
            if(grid[i][j] == 'Q')
                return false;
        for(int i=r,j=c;i>=0&&j<n;i--,j++)
            if(grid[i][j] == 'Q')
                return false;
        return true;
    }
    void backtrack(vector<string> grid,int n,int row){
        if(row == n){
            ans.push_back(grid);
        }else{
           for(int col=0;col<n;col++){
                if(vaild(grid,n,row,col)){
                    grid[row][col] = 'Q';
                    backtrack(grid,n,row+1);
                }   
                grid[row][col] = '.';
           }
        }
    }
public:
    vector<vector<string>> solveNQueens(int n) {
        vector<string> grid;
        for(int i=0;i<n;i++){
            string init = "";
            for(int j=0;j<n;j++){
                init+='.';
            }
            grid.push_back(init);
        }
        backtrack(grid,n,0);
        return ans;
    }
};
```
### 25. Reverse Nodes in k-Group
Problem Link: https://leetcode.com/problems/reverse-nodes-in-k-group/

#### - CPP Solution
```cpp
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
         ListNode *res = new ListNode(0, head);
        ListNode* prevGroup = res;
        while(1){
            ListNode* end = getKthNode(prevGroup,k);
            if(end == nullptr)
                break;
            ListNode* prev = end->next, *groupNext = end->next,*curr = prevGroup->next;
            while(curr != groupNext){
                ListNode* temp = curr->next;
                curr->next = prev;
                prev = curr;
                curr = temp;
            }
            ListNode* temp = prevGroup->next;
            prevGroup->next = end;
            prevGroup = temp;
        }
        return res->next;
    }
    ListNode* getKthNode(ListNode* start,int k){
        while(k > 0 and start != nullptr){
            start = start->next;
            k--;
        }
        return start;
    }
};
```
### 23. Merge k Sorted Lists
Problem Link: https://leetcode.com/problems/merge-k-sorted-lists/

#### - CPP Solution
```cpp
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        if(lists.size()==0) return nullptr;
        while(lists.size()>1){
            vector<ListNode*> temp;
            for(int i=0;i<lists.size();i+=2){
                ListNode* l2 = i+1 < lists.size() ? lists[i+1] : nullptr;
                temp.push_back(mergeLists(lists[i],l2));
            }
            lists = temp;
        }
        return lists[0];
    }
    ListNode* mergeLists(ListNode* l1,ListNode* l2){
        ListNode* dummy = new ListNode();
        ListNode* curr = dummy;
        while(l1 != nullptr and l2 != nullptr){
            if(l1->val < l2->val){
                curr->next = l1;
                l1 = l1->next;
            }else{
                curr->next = l2;
                l2 = l2->next;
            }
            curr = curr->next;
        }
        if(l1 != nullptr)
            curr->next = l1;
        if(l2 != nullptr)
            curr->next = l2;
        return dummy->next;
    }
};
```
### 632. Smallest Range Covering Elements from K Lists
Problem Link: https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/

#### - CPP Solution
```cpp
class Item{
public:
    int num,i,j;
    Item(int num,int i,int j){
        this->num = num;
        this->i = i;
        this->j = j;
    }
};
class Solution {
public:
    vector<int> smallestRange(vector<vector<int>>& nums) {
       auto comp = [](const Item &x, const Item &y) {
            return x.num > y.num;
        };
        priority_queue<Item, vector<Item>, decltype(comp)> min_heap(comp);
        int mx = -1e6;
        for(int i=0;i<nums.size();i++){
            min_heap.push(Item(nums[i][0],i,0));
            mx = max(mx,nums[i][0]);
        }
        Item it = min_heap.top();
        min_heap.pop();
        vector<int> ans {it.num,mx};
        while(1){
            if(it.j >= nums[it.i].size()-1)
                break;
            min_heap.push(Item(nums[it.i][it.j+1],it.i,it.j+1));
            mx = max(mx,nums[it.i][it.j+1]);
            it = min_heap.top();
            min_heap.pop();
            if(mx - it.num < ans[1] - ans[0]){
                ans[0] = it.num;
                ans[1] = mx;
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

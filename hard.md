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
        ListNode* dummy = new ListNode();
        priority_queue<int,vector<int>,greater<int>> q;
        for(int i=0;i<lists.size();i++){
            ListNode* curr = lists[i];
            while(curr != nullptr){
                q.push(curr->val);
                curr = curr->next;
            }
        }
        ListNode* curr = dummy;
        while(!q.empty()){
            int val = q.top();
            q.pop();
            curr->next = new ListNode(val);
            curr = curr->next;
        }
        return dummy->next;
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
### 
Problem Link: 

#### - CPP Solution
```cpp

```

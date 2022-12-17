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

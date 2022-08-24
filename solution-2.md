## Medium problems

### 238. Product of Array Except Self:
Problem Link: https://leetcode.com/problems/product-of-array-except-self/

#### - CPP Solution
```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int> res(n,1);
        int temp = 1;
        for(int i=0;i<n;i++){
            res[i]= temp;
            temp *= nums[i];
        }
        temp = 1;
        for(int i=n-1;i>=0;i--){
            res[i]=res[i]*temp;
            temp *= nums[i];
        }
        return res;
    }
};
```
### 287. Find the Duplicate Number:
Problem Link: https://leetcode.com/problems/find-the-duplicate-number/

#### - CPP Solution
```cpp
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int p1=0,p2=0;
        for(int i=0;i<nums.size();i++){
            p1 = nums[p1];
            p2 = nums[nums[p2]];
            if(p1==p2)
             break;
        }
        p2 = 0;
        for(int i=0;i<nums.size();i++){
            p1 = nums[p1];
            p2 = nums[p2];
            if(p1==p2)
             break;
        }
        return p1;
    }
};
```
### 852. Peak Index in a Mountain Array
Problem Link: https://leetcode.com/problems/find-the-duplicate-number/

#### - CPP Solution
```cpp
class Solution {
public:
    int peakIndexInMountainArray(vector<int>& arr) {
        int mid,l=0,r=arr.size()-1;
        while(l<=r){
            mid = (l+r)/2;
            if(mid == 0)
                return mid+1;
            if(mid == arr.size()-1)
                return mid-1;
            if(arr[mid]>arr[mid-1]&&arr[mid]>arr[mid+1])
                return mid;
            if(arr[mid]<arr[mid+1]&&arr[mid]>arr[mid-1])
                l = mid+1;  
            else if(arr[mid]<arr[mid-1]&&arr[mid]>arr[mid+1])
                r= mid-1;
        }
        return -1;
    }
};
```
### 442. Find All Duplicates in an Array
Problem Link: https://leetcode.com/problems/find-all-duplicates-in-an-array/

#### - CPP Solution
```cpp
class Solution {
public:
    vector<int> findDuplicates(vector<int>& nums) {
        vector<int> res;
        for(int i=0;i<nums.size();i++){
            int ind = abs(nums[i]) - 1;
            if(nums[ind]<0)
                res.push_back(ind + 1);
            else
                nums[ind] *=-1;
        }
        return res;
    }
};
```
### 73. Set Matrix Zeroes
Problem Link: https://leetcode.com/problems/set-matrix-zeroes/

#### - CPP Solution
```cpp
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        int rowZero=2;
        int r=matrix.size(),c=matrix[0].size();
        for(int i=0;i<r;i++)
            for(int j=0;j<c;j++)
                if(matrix[i][j]==0){
                    matrix[0][j]=0;
                    if(i==0)
                       rowZero = 0;
                    else
                        matrix[i][0] = 0;
                }
        for (int i=1;i<r; i++)
            for(int j=1;j<c;j++)
                if(matrix[i][0]==0 || matrix[0][j]==0)
                    matrix[i][j] = 0;
        if(matrix[0][0]==0)
            for (int i=0;i<r; i++)
                matrix[i][0] = 0;
        if(rowZero == 0)
            for (int i=0;i<c; i++)
                matrix[0][i] = 0;
        
    }
};
```
### 128. Longest Consecutive Sequence
Problem Link: https://leetcode.com/problems/longest-consecutive-sequence/

#### - CPP Solution
```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        if(nums.size()<=1)
            return nums.size();
        set<int> s;
        int mx = 0;
        for(int i=0;i<nums.size();i++)
           s.insert(nums[i]);
        int counter = 1;
        set<int>::iterator bf = s.begin();
        set<int>::iterator itr = s.begin();
        itr ++ ;
        for (;itr != s.end(); itr++){
            if(*itr - *bf == 1){
                counter++;
            }else{
                counter = 1;
            }
            bf ++;
            mx = max(counter,mx);
        }
        return mx == 0 ? 1 : mx ;
    }
};
```
### 784. Letter Case Permutation
Problem Link: https://leetcode.com/problems/letter-case-permutation/

#### - CPP Solution
```cpp
class Solution {
public:
    void backtracking(string s,int it,vector<string>&ans,string curr){
        if(curr.size()==s.size()){
            ans.push_back(curr);
            return;
        }
        if(s[it]>='0' && s[it]<='9')
            backtracking(s,it+1,ans,curr+s[it]);
        else{
            backtracking(s,it+1,ans,curr+(char)toupper(s[it]));
            backtracking(s,it+1,ans,curr+(char)tolower(s[it]));
        }
        
    }

    vector<string> letterCasePermutation(string s) {
        vector<string> ans;
        backtracking(s,0,ans,"");
        return ans;
    }
};
```

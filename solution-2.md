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
### 78. Subsets
Problem Link: https://leetcode.com/problems/subsets/

#### - CPP Solution
```cpp
class Solution {
public:
    vector<vector<int>> ans;
    void backtrack(vector<int>nums,int i,vector<int>v){
        if(i==nums.size()){
            ans.push_back(v);
            return;
        }
        backtrack(nums,i+1,v);
        v.push_back(nums[i]);
        backtrack(nums,i+1,v);
    }
    vector<vector<int>> subsets(vector<int>& nums) {
        backtrack(nums,0,{});
        return ans;
    }
};
```
### 90. Subsets II
Problem Link: https://leetcode.com/problems/subsets-ii/

#### - CPP Solution
```cpp
class Solution {
public:
    vector<vector<int>> ans;
    void backtracking(vector<int>nums,int i,vector<int> v){
        if(i==nums.size()){
            ans.push_back(v);
            return;
        }
        int j = i;
        while(j+1<nums.size()&&nums[j]==nums[j+1])
            j++;
        backtracking(nums,j+1,v);
        v.push_back(nums[i]);
        backtracking(nums,i+1,v);
    }
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        backtracking(nums,0,{});
        return ans;
    }
};
```
### 46. Permutations
Problem Link: https://leetcode.com/problems/permutations/

#### - CPP Solution
```cpp
class Solution {
public:
    vector<vector<int>> ans;
    void backtrack(vector<int> nums,int l){
        if(l==nums.size()){
            ans.push_back(nums);
            return;
        }
        for(int i=l;i<nums.size();i++){
            swap(nums[l],nums[i]);
            backtrack(nums,l+1);
            swap(nums[l],nums[i]);
        }
        
    }
    vector<vector<int>> permute(vector<int>& nums) {
        backtrack(nums,0);
        return ans;
    }
};
```
### 47. Permutations II
Problem Link: https://leetcode.com/problems/permutations-ii/submissions/

#### - CPP Solution
```cpp
class Solution {
public:
    vector<vector<int>> ans;
    void permute(map<int,int>&m,int len,vector<int> v){
        if(len==v.size()){
            ans.push_back(v);
            return;
        }
        for (auto[key, val] : m)
        {
            if(val>0){
               m[key]--;
               v.push_back(key);
               permute(m,len,v);
               m[key]++;
               v.pop_back();
            }
        }
    }
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        map<int,int>m;
        for(int i:nums)
            m[i]++;
        permute(m,nums.size(),{});
        return ans;
    }
};
```
### 77. Combinations
Problem Link: https://leetcode.com/problems/combinations/submissions/

#### - CPP Solution
```cpp
class Solution {
public:
    vector<vector<int>> ans;
    void combination(int n,int k,int start,vector<int> v){
        if(k == v.size()){
            ans.push_back(v);
            return;
        }
        for(int i = start;i<=n;i++){
            v.push_back(i);
            combination(n,k,i+1,v);
            v.pop_back();
        }
        
    }
    vector<vector<int>> combine(int n, int k) {
        combination(n,k,1,{});
        return ans;
    }
};
```
### 39. Combination Sum
Problem Link: https://leetcode.com/problems/combination-sum/

#### - CPP Solution
```cpp
class Solution {
    vector<vector<int>> ans;
public:
    void combination(vector<int> candidates, int target,int sum,vector<int> v,int start){
        if(sum == target){
            ans.push_back(v);
            return;
        }
        if(sum > target)
            return;
        for(int i=start;i<candidates.size();i++){
            v.push_back(candidates[i]);
            sum+=candidates[i];
            combination(candidates,target,sum,v,i);
            sum-=candidates[i];
            v.pop_back();
        }
        
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        combination(candidates,target,0,{},0);
        return ans;
    }
};
```
### 40. Combination Sum II
Problem Link: https://leetcode.com/problems/combination-sum-ii/

#### - CPP Solution
```cpp
class Solution {
    vector<vector<int>> ans;
    void backtracking(vector<int> candidates, int target,vector<int> v,int sum,int i){
        if(sum == target){
            ans.push_back(v);
            return;
        }
        if(i == candidates.size() || sum > target)
            return;

        v.push_back(candidates[i]);
        backtracking(candidates,target,v,sum+candidates[i],i+1);
        v.pop_back(); 
        while(i+1<candidates.size() && candidates[i]==candidates[i+1]) i++;
        backtracking(candidates,target,v,sum,i+1);
    }
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(),candidates.end());
        backtracking(candidates,target,{},0,0);
        return ans;
    }
};

```
### 216. Combination Sum III
Problem Link: https://leetcode.com/problems/combination-sum-iii/

#### - CPP Solution
```cpp
class Solution {
    vector<vector<int>> ans;
    void backtracking(vector<int> sub,int k,int n,int sum,int i){
        if(sum == n && sub.size() == k){
            ans.push_back(sub);
            return;
        }
        if(sum > n || i > 9)
            return;
        sub.push_back(i);
        backtracking(sub,k,n,sum +i,i+1);
        sub.pop_back();
        backtracking(sub,k,n,sum,i+1);
    }
public:
    vector<vector<int>> combinationSum3(int k, int n) {
       backtracking({},k,n,0,1);
       return ans;
    }
};
```
### 22. Generate Parentheses
Problem Link: https://leetcode.com/problems/generate-parentheses/

#### - CPP Solution
```cpp
class Solution {
    vector<string> ans;
    void backtrack(int o,int c,int n,string curr){
        if(curr.size()==n*2){
            ans.push_back(curr);
            return;
        }
        if(o < n)
            backtrack(o+1,c,n,curr+"(");
        if(c < n && c < o)
            backtrack(o,c+1,n,curr+")");
    }
public:
    vector<string> generateParenthesis(int n) {
        backtrack(0,0,n,"");
        return ans;
    }
};
```
### 494. Target Sum
Problem Link: https://leetcode.com/problems/target-sum/

#### - CPP Solution
```cpp
class Solution {
public:
    int counter = 0;
    void backtrack(vector<int> nums,int target,int sum,int i){
        if(nums.size() == i){
            if(sum ==target)
                counter ++;
            return;
        }
        backtrack(nums,target,sum+nums[i],i+1);
        backtrack(nums,target,sum-nums[i],i+1);   
    }
    int findTargetSumWays(vector<int>& nums, int target) {
       int mem[21][4001];
       for(int i=0;i<21;i++)
           for(int j=0;j<4001;j++)
               mem[i][j] = MIN_INT;
       backtrack(nums,target,0,0); 
       return counter;
    }
};
```
### 54. Spiral Matrix
Problem Link: https://leetcode.com/problems/spiral-matrix/submissions/

#### - CPP Solution
```cpp
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        int l=0,r=matrix[0].size(),t=0,b=matrix.size();
        vector<int> ans;
        while(l<r && t<b){
            for(int i=l;i<r;i++) ans.push_back(matrix[t][i]);
            t++;
            for(int i=t;i<b;i++) ans.push_back(matrix[i][r-1]);
            r--;
            if(!(l<r && t<b))
                break;
            for(int i=r-1;i>=l;i--) ans.push_back(matrix[b-1][i]);
            b--;
            for(int i=b-1;i>=t;i--) ans.push_back(matrix[i][l]);
            l++;
        }
        return ans;
    }
};
```

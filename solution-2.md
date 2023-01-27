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
Problem Link: https://leetcode.com/problems/peak-index-in-a-mountain-array/

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
### 131. Palindrome Partitioning
Problem Link: https://leetcode.com/problems/palindrome-partitioning/

#### - CPP Solution
```cpp
class Solution {
    vector<vector<string>> ans;
    bool isPalindrome(string S)
    {
        for (int i = 0; i < S.length() / 2; i++) {
            if (S[i] != S[S.length() - i - 1]) {
                return false;
            }
        }
        return true;
    }
    string substr(string s,int l,int r){
        string str="";
        for(int i=l;i<r;i++){
            str+=s[i];
        }
        return str;
    }
    void backtrack(string s,int start,vector<string> subAns){
        if(start >= s.size()){
            ans.push_back(subAns);
            return;
        }
        for(int i=start;i<s.size();i++){
            string curr = substr(s,start, i+1);
            if(isPalindrome(curr)){
                subAns.push_back(curr);
                backtrack(s,i+1,subAns);
                subAns.pop_back();
            }
        }
    }
public:
    vector<vector<string>> partition(string s) {
        backtrack(s,0,{});
        return ans;
    }
};
```
### 48. Rotate Image
Problem Link: https://leetcode.com/problems/rotate-image/

#### - CPP Solution
```cpp
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int L=0,R=matrix.size()-1;
        while(L<R){
            for(int i=L,j=R;i<R;i++,j--){
                int temp=matrix[j][L];
                matrix[j][L]=matrix[R][j];
                matrix[R][j]=matrix[i][R];
                matrix[i][R]=matrix[L][i];
                matrix[L][i]=temp;
            }
            L++,R--;
        }
    }
};
```
### 79. Word Search
Problem Link: https://leetcode.com/problems/word-search/

#### - CPP Solution
```cpp
class Solution {
    int dx[4]={1,-1,0,0};
    int dy[4]={0,0,1,-1};
    int m,n;
    bool ans = false;
    bool vis[7][7];
public:
    void backtrack(int curr,int x,int y,vector<vector<char>>& board, string word){
        if(curr == word.size()){
            ans = true;
            return ;
        }
        if(x >= m || y >= n || x<0 || y<0 || word[curr]!=board[x][y] || vis[x][y])
            return;
        vis[x][y] = true;
        for(int i=0;i<4;i++){
            int newX=x+dx[i];
            int newY=y+dy[i];
            backtrack(curr+1,newX,newY,board,word);
            
        }
        vis[x][y] = false;
    }
    void initVis(){
        for(int i=0;i<6;i++)
           for(int j=0;j<6;j++)
               vis[i][j] = false;
    }
    bool exist(vector<vector<char>>& board, string word) {
       m = board.size();
       n = board[0].size();
       initVis();
       for(int i=0;i<m;i++){
           for(int j=0;j<n;j++){
               backtrack(0,i,j,board,word);
               if(ans)
                   return ans;
           }
       }
       return ans;
    }
};
```
### 17. Letter Combinations of a Phone Number
Problem Link: https://leetcode.com/problems/letter-combinations-of-a-phone-number/

#### - CPP Solution
```cpp
class Solution {
    map<int,string>dic;
    vector<string>ans;
    void backtrack(string digits,int it,string curr){
        if(it==digits.size()){
            ans.push_back(curr);
            return;
        }
        string com = dic[digits[it]-'0'];
        for(int i=0;i<com.size();i++){
           backtrack(digits,it+1,curr+com[i]); 
        }
    }
public:
    vector<string> letterCombinations(string digits) {
        if(digits.size()>0){
            int asci = 97;
            for(int i=2;i<10;i++){
                string s="";
                int len = i == 7 || i==9 ? 4 : 3;
                for(int j=0;j<len;j++){
                    char c = asci;
                    s+=c;
                    asci++;
                }
                dic[i]=s;
            }
            backtrack(digits,0,"");
        }
        return ans;
    }
};
```
### 198. House Robber
Problem Link: https://leetcode.com/problems/house-robber/description/

#### - java Solution
```java
class Solution {
    public int rob(int[] nums) {
       int rob1=0,rob2=0;
       for(int i=0;i<nums.length;i++){
           int temp = Math.max(rob1+nums[i],rob2);
           rob1 = rob2;
           rob2 = temp;
       } 
       return rob2;
    }
}
```
### 213. House Robber II
Problem Link: https://leetcode.com/problems/house-robber-ii/description/

#### - cpp Solution
```cpp
class Solution {
public:
    int rob(vector<int>& nums,int start,int end) {
        if(nums.size()==1)
            return nums[0];
        int rob1=0,rob2=0;
        for(int i=start;i<end;i++){
            int temp = max(rob1+nums[i],rob2);
            rob1 = rob2;
            rob2 = temp;
        }
        return rob2;
    }
    int rob(vector<int>& nums) {
        return max(rob(nums,0,nums.size()-1),rob(nums,1,nums.size())); 
    }
};
```
### 62. Unique Paths
Problem Link: https://leetcode.com/problems/unique-paths/

#### - cpp Solution
```cpp
class Solution {
public:
    int mem[105][105];
    bool outOfBound(int m,int n,int x,int y){
        return x>m||y>n;
    }
    int uniquePaths(int m,int n,int x,int y){
        if(x==m&&y==n)
            return 1;
        if(outOfBound(m,n,x,y))
            return 0;
        if(mem[x][y])
            return mem[x][y];
        mem[x][y] = uniquePaths(m,n,x+1,y)+uniquePaths(m,n,x,y+1);
        return mem[x][y];
    }
    int uniquePaths(int m, int n) {
        for(int i=1;i<105;i++)
            for(int j=1;j<105;j++)
                mem[i][j] = 0;
        return uniquePaths(m,n,1,1);
    }
};
```
### 152. Maximum Product Subarray
Problem Link: https://leetcode.com/problems/maximum-product-subarray/

#### - cpp Solution
```cpp
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int res = *max_element(nums.begin(), nums.end());
        int mn=1,mx=1;
        for(int i=0;i<nums.size();i++){
            int temp = mn;
            mn = min(mn*nums[i],min(mx*nums[i],nums[i]));
            mx = max(temp*nums[i],max(mx*nums[i],nums[i]));
            res = max(res,mx);
        }
        return res;
    }
};
```
### 139. Word Break
Problem Link: https://leetcode.com/problems/word-break

#### - cpp Solution
```cpp
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        vector<int>dp(301,-1);
        dp[s.size()]=1;
        int n = s.size();
        for(int i=n-1;i>=0;i--)
            for(int j=0;j<wordDict.size();j++)
                if(i+wordDict[j].size()<=n){
                    string curr = s.substr(i,wordDict[j].size());
                    if(curr == wordDict[j]&&dp[i+wordDict[j].size()]>0){
                        dp[i]=1;
                    }
                }
        return dp[0]==1;
    }
};
```
### 377. Combination Sum IV
Problem Link: https://leetcode.com/problems/combination-sum-iv/

#### - cpp Solution
```cpp
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        vector<unsigned int>dp(1e3+5,0);
        dp[0]=1;
        for(int i=1;i<=target;i++){
            for(int j=0;j<nums.size();j++){
                if(i-nums[j]>=0)
                    dp[i]+=dp[i-nums[j]];
            }
        }
        return dp[target];
    }
};
```
### 91. Decode Ways
Problem Link: https://leetcode.com/problems/decode-ways/

#### - cpp Solution
```cpp
class Solution {
public:
    int numDecodings(string s) {
        int choice1=1,choice2=1,res=0;
        for(int i=s.size()-1;i>=0;i--){
            res=1;
            if(s[i]=='0') res=0;
            else res += choice1;
            if(i+1<s.size()&&
            (s[i+1]-'0'==1||(s[i+1]-'0'==2&&s[i]-'0'<7))){
                res+=choice2;
            }
            int temp = choice1;
            choice1 = res;
            choice2 = temp;

        }
        return res;
    
    }
};
```
### 322. Coin Change
Problem Link: https://leetcode.com/problems/coin-change/

#### - cpp Solution
```cpp
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        int MAX_SIZE = 1e4+5;
        vector<int>dp(MAX_SIZE,MAX_SIZE);
        dp[0] = 0;
        for(int i=1;i<=amount;i++)
            for(int j=0;j<coins.size();j++)
                if(i-coins[j]>=0)
                    dp[i] = min(dp[i],1+dp[i-coins[j]]);
        if(dp[amount] == MAX_SIZE)
            return -1;
        return dp[amount];
    }
};
```
### 5. Longest Palindromic Substring
Problem Link: https://leetcode.com/problems/longest-palindromic-substring/

#### - cpp Solution
```cpp
class Solution {
     string longestPalindrome(string s,int sz) {
         string res = "";
         int res_len = 0;
        for(int i=0;i<s.size();i++){
            int l=i,r=i+sz;
            while (l >= 0 and r < size(s) and s[l] == s[r]) {
                if (res_len < r-l+1) {
                    res = s.substr(l, r-l+1);
                    res_len = r-l+1;
                }
                l --;
                r ++;
            }    
        }
        return res;
     }
public:
    string longestPalindrome(string s) {
        string res1 = longestPalindrome(s,0);
        string res2 = longestPalindrome(s,1);
        return (size(res1) > size(res2))? res1 : res2;
    }
};
```
### 55. Jump Game
Problem Link: https://leetcode.com/problems/jump-game/

#### - cpp Solution
```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int steps = 0;
        for(int i=0;i<nums.size()-1;i++){
            if(steps==0&&nums[i]==0)
                return false;
            steps = max(steps,nums[i]);
            steps --;
        }
        return true;
    }
};
```
### 647. Palindromic Substrings
Problem Link: https://leetcode.com/problems/palindromic-substrings/

#### - cpp Solution
```cpp
class Solution {
public:
    int countSubstrings(string s,int size_type) {
        int cnt = 0;
        for(int i=0;i<s.size();i++){
            int l=i,r=i+size_type;
            while(l>=0 && r<s.size() && s[l]==s[r]){
                l--;
                r++;
                cnt++;
            }
        }
        return cnt;
    }
    int countSubstrings(string s) {
        return countSubstrings(s,0)+countSubstrings(s,1);
    }
};
```
### 300. Longest Increasing Subsequence
Problem Link: https://leetcode.com/problems/longest-increasing-subsequence/

#### - cpp Solution
```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        vector<int>dp(2e5+3,1);
        for(int i=nums.size()-1;i>=0;i--){
            for(int j=i+1;j<nums.size();j++){
                if(nums[i]<nums[j])
                    dp[i] = max(dp[i],1+dp[j]);
            }
        }
        return *max_element(dp.begin(), dp.end()); 
    }
};
```
### 416. Partition Equal Subset Sum
Problem Link: https://leetcode.com/problems/partition-equal-subset-sum/

#### - cpp Solution
```cpp
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int sum = accumulate(nums.begin(), nums.end(), 0);
        if(sum % 2 != 0)
            return false;
        set<int>dp;
        dp.insert(0);
        sum/=2;
        for(int j=0;j<nums.size();j++){
            set<int>temp;
            for(int item:dp){
                if(item+nums[j] == sum)
                    return true;
                temp.insert(item+nums[j]);
                temp.insert(item);
            }
            dp = temp;
        }
        return false;
    }
};
```
### 698. Partition to K Equal Sum Subsets
Problem Link: https://leetcode.com/problems/partition-to-k-equal-sum-subsets/

#### - cpp Solution
```cpp
class Solution {
    int sum = 0;
public:
    bool canPartitionKSubsets(vector<int>& a, int k) {
        sort(a.begin(), a.end(), greater <>());
        sum = accumulate(a.begin(), a.end(), 0);
        if(sum%k)
            return false;
        sum/=k;
        vector<bool>used(18);
        return backtrack(a,used,k,sum,0);
    }
    bool backtrack(vector<int>& a,vector<bool>& used, int k,int target,int i){
        if(i<a.size()&&a[i]>sum)
            return false;
        if(k==1)
            return true;
        if(target==0)
            return backtrack(a,used,k-1,sum,0);
        if(target < 0)
            return false;
        for(int j = i;j<a.size();j++){
            if(used[j] or target-a[j] < 0)
                continue;
            used[j]=true;
            if(backtrack(a,used,k,target-a[j],j+1))
                return true;
            used[j]=false;
        }
        return false;
    }
};
```
### 309. Best Time to Buy and Sell Stock with Cooldown
Problem Link: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/

#### - cpp Solution
```cpp
class Solution {
    int memo[5003][2];
public:
    int maxProfit(vector<int>& prices) {
        memset(memo, -1, sizeof memo);
        return backtrack(prices,1,0);
    }
    int backtrack(vector<int>& prices,int buy,int i){
        if(i>=prices.size())
            return 0;
        if(memo[i][buy]!=-1)
            return memo[i][buy];
        int cooldown = backtrack(prices,buy,i+1); 
        if(buy)
            memo[i][buy] = max(backtrack(prices,0,i+1) - prices[i],cooldown);//buy
        else
            memo[i][buy] = max(backtrack(prices,1,i+2) + prices[i],cooldown);//sell
        return memo[i][buy];        
    }
};
```

## LeetCode OJ - Phase 4 Interviews Questions

## Easy problems

### 217. Contains Duplicate:
Problem Link: https://leetcode.com/problems/contains-duplicate

#### - CPP Solution
```cpp
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        map<int,bool> m;
        for(int i=0;i<nums.size();i++){
            if(m[nums[i]])
                return true;
            m[nums[i]] = true;
        }
        return false;
    }
};
```

### 268. Missing Number:
Problem Link: https://leetcode.com/problems/missing-number/

#### - CPP Solution
```cpp
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int expectedSum = nums.size()*(nums.size()+1)/2;
        return expectedSum - accumulate(nums.begin(),nums.end(),0);
    }
};
```
### 448. Find All Numbers Disappeared in an Array:
Problem Link: https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/

#### - CPP Solution
```cpp
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums) {
        int n = nums.size();
        vector<int> ans;
        for(int i=0;i<n;i++){
            int idx = abs(nums[i]) -1 ;
            if(nums[idx] >0){
                nums[idx] = nums[idx] * -1;
            }
        }
        for(int i=0;i<n;i++){
            if(nums[i]>0)
                ans.push_back(i+1);
        }
        return ans;
    }
};
```
### 136. Single Number:
Problem Link: https://leetcode.com/problems/single-number/

#### - CPP Solution
```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        for(int i=0;i<nums.size()-1;i++){
            nums[i+1]=nums[i]^nums[i+1];
        }
        return nums[nums.size()-1];
    }
};
```
### 70. Climbing Stairs :
Problem Link: https://leetcode.com/problems/climbing-stairs/

#### - CPP Solution
```cpp
class Solution {
public:
    int climbStairs(int n) {
        if(n<3){
            return n;
        }
        int oneStep = 1,twoStep=2;
        for(int i=3;i<=n;i++){
            int temp = twoStep;
            twoStep = oneStep + twoStep;
            oneStep = temp;
        }
        return twoStep;
    }
};
```
### 121. Best Time to Buy and Sell Stock :
Problem Link: https://leetcode.com/problems/best-time-to-buy-and-sell-stock/

#### - CPP Solution
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int profit = 0;
        int mn = prices[0];
        for(int i=1;i<prices.size();i++){
            profit = max(profit,prices[i]-mn);
            mn = min(mn,prices[i]);
        }
        return profit;
    }
};
```
### 53. Maximum Subarray :
Problem Link: https://leetcode.com/problems/maximum-subarray/

#### - CPP Solution
```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int maxSum = nums[0],currSum = nums[0];
        for(int i=1;i<nums.size();i++){
            if(currSum < 0)
                currSum = 0;
            currSum += nums[i];
            maxSum = max(maxSum,currSum);
        }
        return maxSum;
    }
};
```
### 303. Range Sum Query - Immutable
Problem Link: https://leetcode.com/problems/range-sum-query-immutable/

#### - CPP Solution
```cpp
class NumArray {
public:
    vector<int> sumArray;
    NumArray(vector<int>& nums) {
        sumArray.clear();
        sumArray.push_back(nums[0]);
        for(int i=1;i<nums.size();i++){
            sumArray.push_back(sumArray[i-1]+nums[i]);
        }
    }
    
    int sumRange(int left, int right) {
        if(left==0) return sumArray[right];
        return sumArray[right]-sumArray[left-1];
    }
};
```
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
        for(int i=0;i<nums.size();i++){
            int ind = abs(nums[i])-1;
            if(nums[ind]>0)
                nums[ind]*=-1;
            else
                return ind+1;
            
        }
         return -1;
    }
};
```

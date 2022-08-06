## LeetCode OJ - Phase 4 Interviews Questions

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

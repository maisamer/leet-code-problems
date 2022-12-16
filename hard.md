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

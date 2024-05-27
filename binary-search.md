## Binary search problems

### 34. Find First and Last Position of Element in Sorted Array

Problem Link: https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/

#### - JAVA Solution
```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        return new int[]{binarySearch(nums,target,true),binarySearch(nums,target,false)};
    }
    public int binarySearch(int[] nums, int target,boolean leftPointer){
        int l = 0,r = nums.length-1;
        int ans = -1;
        while(l<=r){
            int mid = (l+r) / 2;
            if(nums[mid] > target){
                r = mid-1;
            }else if(nums[mid] < target){
                l = mid+1;
            }else{
                ans = mid;
               if(leftPointer)
                   r = mid -1;
                else
                    l = mid +1;
            }
        }
        return ans;
    }
}
```

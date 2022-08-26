### 869. Reordered Power of 2
Problem Link: https://leetcode.com/problems/reordered-power-of-2/

#### - CPP Solution
```cpp
class Solution {
public:
    bool reorderedPowerOf2(int n) {
       vector<int> v = count(n);
       for(int i=0;i<31;i++)
            if (v == count(1 << i))
                return true;
        return false; 
    }
    vector<int> count(int n){
        vector<int> v(10,0);
        while(n>0){
            v[n%10]++;
            n/=10;
        }
        return v;
    }
};
```

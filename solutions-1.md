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
### 141. Linked List Cycle
Problem Link: https://leetcode.com/problems/linked-list-cycle/

#### - CPP Solution
```cpp
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode *curr = head;
        while(curr!= nullptr){
            if(curr->val == INT_MAX)
                return true;
            curr->val = INT_MAX;
            curr = curr->next;
        }
        return false;   
    }
};
```
#### - java Solution using floyd detction cycle algorithm
```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while(fast != null && fast.next != null){
            fast = fast.next.next;
            slow = slow.next;
            if(fast == slow)
                return true;
        }
        return false;
    }
}
```
### 876. Middle of the Linked List
Problem Link: https://leetcode.com/problems/middle-of-the-linked-list/

#### - java Solution
```java
class Solution {
    public ListNode middleNode(ListNode head) {
        ListNode curr = head;
        int length = 0;
        while(curr != null){
            curr = curr.next;
            length++;
        }
        curr = head;
        int mid = length/2;
        while(mid>0){
            mid--;
            curr = curr.next;
        }
        return curr;
    }
}
```
### 234. Palindrome Linked List
Problem Link: https://leetcode.com/problems/palindrome-linked-list/

#### - cpp Solution
```cpp
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        ListNode* slow = head,*fast=head;
        while(fast != nullptr && fast->next != nullptr){
            fast = fast->next->next;
            slow = slow->next;
        }
        ListNode* prev = nullptr;
        // reverse linked list
        while(slow != nullptr){
            ListNode* temp = slow->next;
            slow->next = prev;
            prev = slow; 
            slow = temp;
        }
        ListNode* first = head,*last = prev;
        while(last != nullptr){
            if(first->val != last->val)
                return false;
            first = first->next;
            last = last->next;
        }
        return true;
    }
};
```
### 203. Remove Linked List Elements
Problem Link: https://leetcode.com/problems/remove-linked-list-elements/

#### - cpp Solution
```cpp
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        while(head != nullptr && head->val == val)
            head = head->next;
        ListNode* curr = head;
        while(curr != nullptr && curr->next != nullptr){
            if(curr->next->val == val)
                curr->next = curr->next->next;
            else
                curr = curr->next;

        }
        return head;
    }
};
```
### 83. Remove Duplicates from Sorted List
Problem Link: https://leetcode.com/problems/remove-duplicates-from-sorted-list/

#### - cpp Solution
```cpp
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        ListNode* curr = head;
        while(curr != nullptr && curr->next != nullptr){
            if(curr->next->val == curr->val)
                curr->next = curr->next->next;
            else
                curr = curr->next;
        }
        return head;
    }
};
```
### 206. Reverse Linked List
Problem Link: https://leetcode.com/problems/reverse-linked-list/

#### - cpp Solution
```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* curr = head,*prev=nullptr;
        while(curr != nullptr){
            ListNode* temp = curr->next;
            curr->next = prev;
            prev = curr;
            curr = temp;
        } 
        return prev;
    }
};
```
#### - cpp recursive Solution
```cpp
class Solution {
    ListNode* reverseList(ListNode* curr,ListNode* prev) {
        if(curr == nullptr)
            return prev;
        ListNode* temp = curr->next;
        curr->next = prev;
        prev = curr;
        curr = temp;
        return reverseList(curr,prev);
    }
public:
    ListNode* reverseList(ListNode* head) {
        return reverseList(head,nullptr);
    }
};
```
### 21. Merge Two Sorted Lists
Problem Link: https://leetcode.com/problems/merge-two-sorted-lists/

#### - cpp Solution
```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode* resHead = new ListNode();
        ListNode* ptr = resHead;
        while(list1 != nullptr && list2 != nullptr){
            if(list1->val <= list2->val){
                ptr->next = list1;
                list1 = list1->next;
            }else{
               ptr->next = list2; 
               list2 = list2->next;
            }
            ptr = ptr->next;
        }
        if(list1 != nullptr){
            ptr->next = list1;
        }
        if(list2 != nullptr){
            ptr->next = list2;
        }
        return resHead->next;
    }
};
```
### 704. Binary Search
Problem Link: https://leetcode.com/problems/binary-search/

#### - cpp Solution
```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
       int l=0,r=nums.size()-1,mid,res=-1;
       while(l<=r){
          mid = (r+l)/2;
          if(nums[mid]==target)
            return mid;
          else if(nums[mid]<target)
            l = mid +1;
          else
            r = mid-1;    
       }
       return res;
    }
};
```
### 744. Find Smallest Letter Greater Than Target
Problem Link: https://leetcode.com/problems/find-smallest-letter-greater-than-target/

#### - cpp Solution
```cpp
class Solution {
public:
    char nextGreatestLetter(vector<char>& letters, char target) {
       int l=0,r=letters.size()-1,mid;
       while(l<=r){
          mid = (r+l)/2;
          if(letters[mid]<=target)
            l = mid +1;
          else
            r = mid - 1;    
       }
        l = l>=letters.size()?0:l;
       return letters[l];
    }
};
```
### 637. Average of Levels in Binary Tree
Problem Link: https://leetcode.com/problems/average-of-levels-in-binary-tree/

#### - cpp Solution
```cpp
class Solution {
public:
    vector<double> averageOfLevels(TreeNode* root) {
        vector<double> res;
        queue<TreeNode*>q;
        q.push(root);
        while(!q.empty()){
            double sz = q.size();
            double sum = 0;
            for(int i=0;i<sz;i++){
                TreeNode* curr =q.front();
                sum+=curr->val;
                if(curr->left!= nullptr) q.push(curr->left);
                if(curr->right!= nullptr) q.push(curr->right);
                q.pop();
            }
            res.push_back(ceil(sum/sz * 100000.0) / 100000.0);
        }
        return res;
    }
};
```
### 111. Minimum Depth of Binary Tree
Problem Link: https://leetcode.com/problems/minimum-depth-of-binary-tree/

#### - cpp Solution
```cpp
class Solution {
public:
    int minDepth(TreeNode* root) {
        int depth = 0;
        queue<TreeNode*>q;
        if(root!=nullptr)
            q.push(root);
        while(!q.empty()){
            double sz = q.size();
            depth++;
            while(sz--){
                TreeNode* curr =q.front();
                if(curr->left!= nullptr) q.push(curr->left);
                if(curr->right!= nullptr) q.push(curr->right);
                if(curr->left== nullptr &&curr->right== nullptr)
                    return depth;
                q.pop();
            }
        }
        return depth;
    }
};
```
### 100. Same Tree
Problem Link: https://leetcode.com/problems/same-tree/

#### - cpp Solution
```cpp
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if(p==nullptr && q==nullptr)
            return true;
        if(p==nullptr || q==nullptr)
            return false;
        if(p->val != q->val)
            return false;
        return isSameTree(p->left,q->left) && isSameTree(p->right,q->right);
    }
};
```
### 112. Path Sum
Problem Link: https://leetcode.com/problems/path-sum/

#### - cpp Solution
```cpp
class Solution {
    bool hasPathSum(TreeNode* p, int sum,int targetSum) {
        if(p==nullptr)
            return false;
        if(p!=nullptr && p->left == nullptr && p->right == nullptr)
            return sum+p->val == targetSum;
        return hasPathSum(p->left,sum+p->val,targetSum) || hasPathSum(p->right,sum+p->val,targetSum);
    }
public:
    bool hasPathSum(TreeNode* root, int targetSum) {
        if(root == nullptr) return false;
        return hasPathSum(root,0,targetSum);
    }
};
```
### 104. Maximum Depth of Binary Tree
Problem Link: https://leetcode.com/problems/maximum-depth-of-binary-tree/

#### - cpp Solution
```cpp
class Solution {
public:
    int maxDepth(TreeNode* root) {
        int mxDepth = 0;
        queue<TreeNode*> q;
        if(root != nullptr)
            q.push(root);
        while(!q.empty()){
            int sz = q.size();
            mxDepth++;
            while(sz--){
                TreeNode* curr = q.front();
                if(curr->left!= nullptr)
                    q.push(curr->left);
                if(curr->right != nullptr)
                    q.push(curr->right);
                q.pop();
            }
        }
        return mxDepth;
    }
};
```
### 543. Diameter of Binary Tree
Problem Link: https://leetcode.com/problems/diameter-of-binary-tree/

#### - cpp Solution
```cpp
class Solution {
public:
    int ans=0;
    int maxHieght(TreeNode* node){
        if(node == nullptr)
            return 0;
        int l = maxHieght(node->left);
        int r = maxHieght(node->right);
        ans=max(ans,l+r);
        return max(l,r)+1;
    }
    int diameterOfBinaryTree(TreeNode* root) {
        maxHieght(root);
        return ans;
    }
};
```

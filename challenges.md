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
### 146. LRU Cache
Problem Link: https://leetcode.com/problems/lru-cache/

#### - JAVA Solution
```java
class LRUCache {
    private int capacity;
    private Map<Integer,Node> cache;
    Node left , right;
    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.cache = new HashMap<>();
        left = new Node(0,0);
        right = new Node(0,0);
        left.next = right;
        right.previous = left;
    }
    
    private void remove(Node node){
        Node prev = node.previous,next = node.next;
        prev.next = next;
        next.previous = prev;
    }
    
    private void insert(Node node){
        Node prev = right.previous,next = right;
        next.previous = prev.next = node;
        node.next = next;
        node.previous = prev;
    }
    
    public int get(int key) {
        if(cache.containsKey(key)){
            remove(cache.get(key));
            insert(cache.get(key));
            return cache.get(key).val;
        }
        return -1;  
    }
    
    public void put(int key, int value) {
        if(cache.containsKey(key)){
            remove(cache.get(key));
            cache.remove(key);
        }
        Node node = new Node(key,value);
        insert(node);
        cache.put(key,node);
        if(this.cache.size() > this.capacity){
            cache.remove(this.left.next.key);
            remove(this.left.next);
        }
    }
    
    public class Node{
        int val;
        int key;
        Node previous;
        Node next;
        Node(int key,int val){
            this.key = key;
            this.val = val;
            previous = next = null;
        }
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```

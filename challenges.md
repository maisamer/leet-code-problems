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
### 460. LFU Cache
Problem Link: https://leetcode.com/problems/lfu-cache/

#### - JAVA Solution
```java
class LFUCache {

    int capacity;
    int lfuCnt;
    Map<Integer,Item> items;

    Map<Integer,LinkedList> listMap;

    public LFUCache(int capacity) {
        this.capacity = capacity;
        this.items = new HashMap<>();
        this.listMap = new HashMap<>();
        this.lfuCnt = 0;
    }

    public int get(int key) {
        if(items.containsKey(key)){
            counter(key);
            return items.get(key).val;
        }
        return -1;
    }

    public void put(int key, int value) {
        if(!items.containsKey(key) && capacity == items.size()){
            int removedItem = listMap.get(lfuCnt).removeLeft();
            items.remove(removedItem);
        }
        items.put(key,new Item(value,items.getOrDefault(key,new Item(value)).cnt));

        counter(key);
        lfuCnt = Math.min(lfuCnt,items.get(key).cnt);
    }

    private void counter(int key){
        int cnt = items.get(key).cnt;
        getLinkedList(cnt).remove(key);
        getLinkedList(cnt + 1).insertRight(key);
        if(cnt == lfuCnt && this.listMap.get(cnt).length() == 0)
            lfuCnt = cnt +1;
        items.put(key,new Item(items.get(key).val,cnt+1));
    }

    private LinkedList getLinkedList(int cnt){
        if(!listMap.containsKey(cnt))
            listMap.put(cnt,new LinkedList());
        return listMap.get(cnt);
    }

    public class Item{
        int val;
        int cnt;

        public Item(int val) {
            this.val = val;
            this.cnt = 0;
        }
        public Item(int val,int cnt) {
            this.val = val;
            this.cnt = cnt;
        }
    }

    public class Node{
        int val;
        Node previous;
        Node next;
        Node(int val,Node previous,Node next){
            this.val = val;
            this.previous = previous;
            this.next = next;
        }
    }

    public class LinkedList{
        Node left,right;
        private Map<Integer,Node> cache;
        LinkedList(){
            left = new Node(0,null,null);
            right = new Node(0,left,null);
            left.next = right;
            this.cache = new HashMap<>();
        }

        public void insertRight(int val){
            Node node = new Node(val,right.previous,right);
            cache.put(val,node);
            right.previous = node;
            node.previous.next = node;
        }

        public void remove(int val){
            if(cache.containsKey(val)) {
                Node node = cache.get(val);
                Node prev = node.previous, next = node.next;
                prev.next = next;
                next.previous = prev;
                cache.remove(val);
            }
        }

        public int removeLeft(){
            int val = left.next.val;
            remove(val);
            return val;
        }

        public void update(int val){
            remove(val);
            insertRight(val);
        }

        public int length(){
            return cache.size();
        }
    }
}

/**
 * Your LFUCache object will be instantiated and called as such:
 * LFUCache obj = new LFUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```

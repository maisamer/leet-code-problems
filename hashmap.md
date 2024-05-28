## Hashmap problems

### 205. Isomorphic Strings

Problem Link: https://leetcode.com/problems/isomorphic-strings/

#### - JAVA Solution
```java
class Solution {
    public boolean isIsomorphic(String s, String t) {
        Map<Character,Character> sdic = new HashMap<>();
        Map<Character,Character> tdic = new HashMap<>();
        for(int i=0;i<s.length();i++){
            char sc = s.charAt(i);
            char tc = t.charAt(i);
            if(sdic.containsKey(sc)){
                char c = sdic.get(sc);
                if(c != tc)
                    return false;
            }if(tdic.containsKey(tc)){
                char c = tdic.get(tc);
                if(sc != c)
                    return false; 
            }else{
                sdic.put(sc,tc);
                tdic.put(tc,sc);
            }
        }
        return true;
    }
}
```

### 49. Group Anagrams

Problem Link: https://leetcode.com/problems/group-anagrams/

#### - JAVA Solution
```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        List<List<String>> res = new ArrayList<>();
        Map<String,ArrayList<String>> map = new HashMap<>();
        for(int i=0;i<strs.length;i++){
            String sorted = sort(strs[i]);
            if(map.containsKey(sorted)){
                map.get(sorted).add(strs[i]);
            }else{
                ArrayList<String> list = new ArrayList<>();
                list.add(strs[i]);
                map.put(sorted,list);
            }
        }
        
        for (String key : map.keySet()) {
            res.add(map.get(key));
        }
        return res;
    }
    
    private String sort(String str){
        char[] charArray = str.toCharArray();
        Arrays.sort(charArray);
        return new String(charArray);     
    }
}
```

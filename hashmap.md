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

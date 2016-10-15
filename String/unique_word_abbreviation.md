# Unique Word Abbreviation

[Leetcode](https://leetcode.com/problems/unique-word-abbreviation/)

題意：

An abbreviation of a word follows the form <first letter><number><last letter>. Below are some examples of word abbreviations:

a) it                      --> it    (no abbreviation)

     1
b) d|o|g                   --> d1g

              1    1  1
     1---5----0----5--8
c) i|nternationalizatio|n  --> i18n

              1
     1---5----0
d) l|ocalizatio|n          --> l10n
Assume you have a dictionary and given a word, find whether its abbreviation is unique in the dictionary. A word's abbreviation is unique if no other word from the dictionary has the same abbreviation.

Example: 
Given dictionary = [ "deer", "door", "cake", "card" ]

isUnique("dear") -> false

isUnique("cart") -> true

isUnique("cane") -> false

isUnique("make") -> true

解題思路：

updated on 2015.12.6

```java
public class ValidWordAbbr {

    HashMap<String, Set<String>> map = new HashMap<String, Set<String>>();
    public ValidWordAbbr(String[] dictionary) {
        for (String str : dictionary) {
            String key = genKey(str);
            if (!map.containsKey(key)) {
                map.put(key, new HashSet<String>());
            }
            map.get(key).add(str);
        }
    }
    
    private String genKey(String str) {
        if (str.length() <= 2) {
            return str;
        }
        
        String res = str.charAt(0) + Integer.toString(str.length() - 2) + str.charAt(str.length() - 1);
        return res;
    }

    public boolean isUnique(String word) {
        String key = genKey(word);
        return !map.containsKey(key) || (map.get(key).size() == 1 && map.get(key).contains(word));
    }
}
```

2015.11.5

```java
public class ValidWordAbbr {
    
    HashMap<String, HashSet<String>> map = new HashMap<String, HashSet<String>>();
    HashSet<String> set = new HashSet<String>();
    public ValidWordAbbr(String[] dictionary) {
        
        for (int i = 0; i < dictionary.length; i++) {
            String cur = dictionary[i];
            set.add(cur);
            if (cur.length() < 3) {
                if (!map.containsKey(cur)) {
                    map.put(cur, new HashSet<String>());
                    
                }
                map.get(cur).add(cur);
            } else {
                StringBuilder sb = new StringBuilder();
                sb.append(cur.charAt(0));
                sb.append(cur.length() - 2);
                sb.append(cur.charAt(cur.length() - 1));
                String key = sb.toString();
                if (!map.containsKey(key)) {
                    map.put(key, new HashSet<String>());
                }
                
                map.get(key).add(cur);
            }
        }
    }

    public boolean isUnique(String word) {
        if (word.length() < 3) {
            if (!map.containsKey(word)) {
                return true;
            }
            return map.get(word).size() == 1 && set.contains(word);
        } else {
            StringBuilder sb = new StringBuilder();
            sb.append(word.charAt(0));
            sb.append(word.length() - 2);
            sb.append(word.charAt(word.length() - 1));
            String key = sb.toString();
            if (!map.containsKey(key)) {
                return true;
            }
            return map.get(key).size() == 1 && set.contains(word);
        }
        
    }
}


// Your ValidWordAbbr object will be instantiated and called as such:
// ValidWordAbbr vwa = new ValidWordAbbr(dictionary);
// vwa.isUnique("Word");
// vwa.isUnique("anotherWord");
```

網友提供的版本，較精簡

```java
public class ValidWordAbbr {

    public ValidWordAbbr(String[] dictionary) {
        for(String s: dictionary) {
            String key = s.charAt(0) + Integer.toString(s.length() - 2) + s.charAt(s.length() - 1);
            if(d.containsKey(key)) {
                d.get(key).add(s);
            } else {
                List<String> l = new ArrayList<>();
                l.add(s);
                d.put(key, l);
            }
        }
    }

    public boolean isUnique(String word) {
        String key = word.charAt(0) + Integer.toString(word.length() - 2) + word.charAt(word.length() - 1);
        if(!d.containsKey(key))
            return true;
        else if(d.get(key).size() < 2  && d.get(key).get(0).equals(word))
            return true;
        return false;
    }
    
    private HashMap<String, List<String>> d = new HashMap<>();
}


// Your ValidWordAbbr object will be instantiated and called as such:
// ValidWordAbbr vwa = new ValidWordAbbr(dictionary);
// vwa.isUnique("Word");
// vwa.isUnique("anotherWord");
```
---
###Reference
1. http://blog.csdn.net/pointbreak1/article/details/48858599
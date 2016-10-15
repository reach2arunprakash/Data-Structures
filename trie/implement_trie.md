# Implement Trie

[Lintcode](http://www.lintcode.com/en/problem/implement-trie/#)

題意：

Implement a trie with insert, search, and startsWith methods.

Note
You may assume that all inputs are consist of lowercase letters a-z.

![Trie](http://www.programcreek.com/wp-content/uploads/2014/05/implement-trie.png)

解題思路：


```java
/**
 * Your Trie object will be instantiated and called as such:
 * Trie trie = new Trie();
 * trie.insert("lintcode");
 * trie.search("lint"); will return false
 * trie.startsWith("lint"); will return true
 */
class TrieNode {
    // Initialize your data structure here.
    char c;
    HashMap<Character, TrieNode> children = new HashMap<Character, TrieNode>();
    boolean isLeaf;
    
    public TrieNode() {
        
    }
    
    public TrieNode(char c) {
        this.c = c;
    }
}

public class Solution {
    private TrieNode root;

    public Solution() {
        root = new TrieNode();
    }

    // Inserts a word into the trie.
    public void insert(String word) {
        HashMap<Character, TrieNode> children = root.children;
        
        for (int i = 0; i < word.length(); i++) {
            char c = word.charAt(i);
            
            
            if (!children.containsKey(c)) {
                children.put(c, new TrieNode(c));
            } 
            
            TrieNode t = children.get(c);
            children = t.children;
            
            // 到最後一個詞了，設為leaf
            if (i == word.length() - 1) {
                t.isLeaf = true;
            }
        }
    }

    // Returns if the word is in the trie.
    public boolean search(String word) {
        TrieNode t = searchNode(word);
        
        if (t != null && t.isLeaf) {
            return true;
        } else {
            return false;
        }
    }

    // Returns if there is any word in the trie
    // that starts with the given prefix.
    public boolean startsWith(String prefix) {
        if (searchNode(prefix) == null) {
            return false;
        } else {
            return true;
        }
    }
    
    public TrieNode searchNode(String str) {
        Map<Character, TrieNode> children = root.children;
        TrieNode t = null;
        for (int i = 0; i < str.length(); i++) {
            char c = str.charAt(i);
            if (children.containsKey(c)) {
                t = children.get(c);
                children = t.children;
            } else {
                return null;
            }
        }
        
        return t;
    }
}

```

---
###Reference
1. http://www.programcreek.com/2014/05/leetcode-implement-trie-prefix-tree-java/
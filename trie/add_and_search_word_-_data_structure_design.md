# Add and Search Word - Data structure design

[Leetcode](https://leetcode.com/problems/add-and-search-word-data-structure-design/)


題意：

Design a data structure that supports the following two operations:
```
void addWord(word)
bool search(word)
```
search(word) can search a literal word or a regular expression string containing only letters a-z or .. A . means it can represent any one letter.

For example:
```
addWord("bad")
addWord("dad")
addWord("mad")
search("pad") -> false
search("bad") -> true
search(".ad") -> true
search("b..") -> true
```
Note:
You may assume that all words are consist of lowercase letters a-z.

解題思路：

主要使用 trie與dfs來作，其程式碼如下(此解法會超時)：

```java
public class WordDictionary {
    
    public class TrieNode {
        char c;
        HashMap<Character, TrieNode> children = new HashMap<Character, TrieNode>();
        boolean isLeaf = false;
        public TrieNode() {
            
        }
        
        public TrieNode(char c) {
            this.c = c;
        }
    }
    
    TrieNode root = new TrieNode();
    // Adds a word into the data structure.
    public void addWord(String word) {
        HashMap<Character, TrieNode> children = root.children;
        for (int i = 0; i < word.length(); i++) {
            char c = word.charAt(i);
            if (!children.containsKey(c)) {
                children.put(c, new TrieNode());
            }
            TrieNode t = children.get(c);
            children = t.children;
            
            if (i == word.length() - 1) {
                t.isLeaf = true;
            }
        }
    }

    // Returns if the word is in the data structure. A word could
    // contain the dot character '.' to represent any one letter.
    public boolean search(String word) {
        return searchHelper(root, word);
    }
    
    public boolean searchHelper(TrieNode root, String word) {
        if (root == null) {
            return false;
        }
        if (word.length() == 0) {
            return root.isLeaf;
        }
        
        Map<Character, TrieNode> children = root.children;
        
        char c = word.charAt(0);
        if (c == '.') {
            for (char key : children.keySet()) {
                if (searchHelper(children.get(key), word.substring(1))) {
                    return true;
                }
            }
            return false;
        } else if (!children.containsKey(c)) {
            return false;
        } else {
            return searchHelper(children.get(c), word.substring(1));
        }
    }
}

// Your WordDictionary object will be instantiated and called as such:
// WordDictionary wordDictionary = new WordDictionary();
// wordDictionary.addWord("word");
// wordDictionary.search("pattern");
```

網友使用另一解法，其程式碼如下：

```java
public class WordDictionary {
    class TrieNode {
        TrieNode[] children;
        boolean isEndOfWord;
        public TrieNode() {
            this.children = new TrieNode[26];
            this.isEndOfWord = false;
        }
    }

    TrieNode root = new TrieNode();

    // Adds a word into the data structure.
    public void addWord(String word) {
        TrieNode runner = root;
        for (char c : word.toCharArray()) {
            if (runner.children[c - 'a'] == null) {
                runner.children[c - 'a'] = new TrieNode();
            }
            runner = runner.children[c - 'a'];
        }
        runner.isEndOfWord = true;
    }

    // Returns if the word is in the data structure. A word could
    // contain the dot character '.' to represent any one letter.
    public boolean search(String word) {
        return match(word.toCharArray(), 0, root);
    }

    public boolean match(char[] word, int k, TrieNode node) {
        if (k == word.length) return node.isEndOfWord;
        if (word[k] != '.') {
            return node.children[word[k] - 'a'] != null && match(word, k+1, node.children[word[k] - 'a']);
        }
        else {
            for (int i = 0; i < node.children.length; i++) {
                if (node.children[i] != null) {
                    if (match(word, k+1, node.children[i])) return true;
                }
            }
        }
        return false;
    }
}
```
---
###Reference
1. https://leetcode.com/discuss/57653/simple-ac-java-code
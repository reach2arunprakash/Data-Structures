# Word Search II

[Lintcode](http://www.lintcode.com/en/problem/word-search-ii/)

題意：

[Word Search](string/word_search.md) 的 followup，找出所有可能的結果

解題思路：

類似第一題 用 dfs 的方式，把每個字丟到board去作dfs，若返回true則把答案加上去，但這題複雜度太高，我另外可以利用 trie 來幫助我們解決這題，首先把所有要找的字全插入trie中，接著不斷的把從board中目前字元的上下左右來搜尋 trie，若為true，則把答案加入 res中。



網友[programcreek](http://www.programcreek.com/2014/06/leetcode-word-search-ii-java/) 提供以下思路，其程式碼如下，高達近150行，可能面試時跟面試官說明一下 trie怎麼執行即可。

```java
public class Solution {
    /**
     * @param board: A list of lists of character
     * @param words: A list of string
     * @return: A list of string
     */
    Set<String> result = new HashSet<String>(); 
 
    public ArrayList<String> wordSearchII(char[][] board, ArrayList<String> words) {
        //HashSet<String> result = new HashSet<String>();
 
        Trie trie = new Trie();
        for(String word: words){
            trie.insert(word);
        }
 
        int m=board.length;
        int n=board[0].length;
 
        boolean[][] visited = new boolean[m][n];
 
        for(int i=0; i<m; i++){
            for(int j=0; j<n; j++){
               dfs(board, visited, "", i, j, trie);
            }
        }
 
        return new ArrayList<String>(result);
    }
 
    public void dfs(char[][] board, boolean[][] visited, String str, int i, int j, Trie trie){
        int m=board.length;
        int n=board[0].length;
 
        if(i<0 || j<0||i>=m||j>=n){
            return;
        }
 
        if(visited[i][j])
            return;
 
        str = str + board[i][j];
 
        if(!trie.startsWith(str))
            return;
 
        if(trie.search(str)){
            result.add(str);
        }
 
        visited[i][j]=true;
        dfs(board, visited, str, i-1, j, trie);
        dfs(board, visited, str, i+1, j, trie);
        dfs(board, visited, str, i, j-1, trie);
        dfs(board, visited, str, i, j+1, trie);
        visited[i][j]=false;
    }
    
    
    
    // 直接拿implemented Trie那題的程式碼來用
    class TrieNode {
        // Initialize your data structure here.
        HashMap<Character, TrieNode> children = new HashMap<Character, TrieNode>();
        char c;
        boolean isLeaf;
        public TrieNode() {
            
        }
        
        public TrieNode(char c) {
            this.c = c;
        }
    }
    
    public class Trie {
        private TrieNode root;
    
        public Trie() {
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
                if (i == word.length() - 1) {
                    t.isLeaf = true;
                }
            }
            
        }
    
        // Returns if the word is in the trie.
        public boolean search(String word) {
            TrieNode res = searchNode(word);
            if (res != null && res.isLeaf) {
                return true;
            } else {
                return false;
            }
        }
    
        // Returns if there is any word in the trie
        // that starts with the given prefix.
        public boolean startsWith(String prefix) {
            TrieNode res = searchNode(prefix);
            if (res != null) {
                return true;
            } else {
                return false;
            }
        }
        
        public TrieNode searchNode(String word) {
            TrieNode t = null;
            HashMap<Character, TrieNode> children = root.children;
            for (int i = 0; i < word.length(); i++) {
                char c = word.charAt(i);
                if (!children.containsKey(c)) {
                    return null;
                }
                t = children.get(c);
                children = t.children;
            }
            return t;
        }
    }
}

```

---
###Reference
1. http://www.programcreek.com/2014/06/leetcode-word-search-ii-java/
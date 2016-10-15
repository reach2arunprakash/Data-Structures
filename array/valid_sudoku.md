#Valid Sudoku

[Lintcode](http://www.lintcode.com/en/problem/valid-sudoku/)

題意：

Determine whether a Sudoku is valid.

The Sudoku board could be partially filled, where empty cells are filled with the character ```.```.

**Note**

A valid Sudoku board (partially filled) is not necessarily solvable. Only the filled cells need to be validated.

解題思路：

檢查每行以及子九宮格區塊是否有相同的數字，若有則返回 false，若無則返回 true。

行與列很簡單，只要各用一個hashset 即可搞定。

子九宮格區塊需要把 9x9的格子切成 3x3，因此需要計算index，把相同區塊的元素分到同一區塊，再搭配hashmap來檢查，公式如下

> index[i][j] = i / 3 + (j / 3) * 3

程式碼如下：

class Solution {
    /**
      * @param board: the board
        @return: wether the Sudoku is valid
      */
    public boolean isValidSudoku(char[][] board) {
        
        if (board == null || board.length == 0) {
            return true;
        }
        
        for (int i = 0; i < 9; i++) {
            HashSet<Character> set = new HashSet<Character>();
            for (int j = 0; j < 9; j++) {
                if (board[i][j] == '.') {
                    continue;
                }
                if (set.contains(board[i][j])) {
                    return false;
                } else{
                    set.add(board[i][j]);
                }
            }
        }
        
        for (int i = 0; i < 9; i++) {
            HashSet<Character> set = new HashSet<Character>();
            for (int j = 0; j < 9; j++) {
                if (board[j][i] == '.') {
                    continue;
                }
                if (set.contains(board[j][i])) {
                    return false;
                } else {
                    set.add(board[j][i]);
                }
                
            }
        }
        
        HashMap<Integer, List<Character>> map = new HashMap<Integer, List<Character>>();
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                if (board[i][j] == '.') {
                    continue;
                }
                int index = i / 3 + 3 * (j / 3);
                if (!map.containsKey(index)) {
                    map.put(index, new ArrayList<Character>());
                    map.get(index).add(board[i][j]);
                } else {
                    if (map.get(index).contains(board[i][j])) {
                        return false;
                    }
                    map.get(index).add(board[i][j]);
                }
            }
        }
        
        return true;
        
        
    }
};

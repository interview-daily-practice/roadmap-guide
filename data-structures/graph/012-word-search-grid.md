# Word Search In Grid


```java
class Solution {
    public boolean exist(char[][] board, String word) {
        int m = board.length;
        int n = board[0].length;
        visited = new boolean[m][n];

        for (int row = 0; row < m; row++) {
            for (int col = 0; col < n; col++) {
                int wordIndex = 0;
                boolean flag = dfs(board, row, col, m, n, word, wordIndex, "");
                if (flag) {
                    return flag;
                }
            }
        }

        return false;

    }

    private boolean[][] visited = null;

    public boolean dfs(char[][] board, int row, int col, int m, int n, String word, int wordIndex, String target) {
        if (target.equals(word)) {
            return true;
        }
        boolean valid = row >= 0 && row < m
                && col >= 0 && col < n
                
                && wordIndex < word.length()
                && word.charAt(wordIndex) == board[row][col]
                && !visited[row][col];

        if (!valid) {
            return false;
        }

        visited[row][col] = true; 

        char curr = board[row][col];

        boolean found = dfs(board, row + 1, col, m, n, word, pos + 1, target + curr)
                || dfs(board, row - 1, col, m, n, word, pos + 1, target + curr)
                || dfs(board, row, col + 1, m, n, word, pos + 1, target + curr)
                || dfs(board, row, col - 1, m, n, word, pos + 1, target + curr);

        visited[row][col] = false;

        return found;
    }
}
```

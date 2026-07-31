# Number of Islands

Approach: Count Connected Components

```java
class Solution {

    int count = 0;

    public int numIslands(char[][] grid) {
        int rowMax = grid.length;
        int colMax = grid[0].length;

        for (int row = 0; row < rowMax; row++) {
            for (int col = 0; col < colMax; col++) {

                char current = grid[row][col];
                if (current == '0') {
                    continue;
                }

                String key = row + ":" + col;

                if (visited.containsKey(key)) {
                    continue;
                }

                count++;

                dfs(grid, row, col, rowMax, colMax);

            }

        }
        return count;
    }

    private Map<String, Boolean> visited = new HashMap<>();

    private void dfs(char[][] grid, int row, int col, int rowMax, int colMax) {

        String key = row + ":" + col;

        if (visited.containsKey(key)) {
            return;
        }

        if (!isValid(row, col, rowMax, colMax)) {
            return;
        }

        char current = grid[row][col];
        if (current == '0') {
            return;
        }

        visited.put(key, true);

        dfs(grid, row + 1, col, rowMax, colMax); // down
        dfs(grid, row, col + 1, rowMax, colMax); // right
        dfs(grid, row - 1, col, rowMax, colMax); // up
        dfs(grid, row, col - 1, rowMax, colMax); // left
    }

    private boolean isValid(int row, int col, int rowMax, int colMax) {

        return row >= 0 && row < rowMax
                && col >= 0 && col < colMax;
    }

}
```

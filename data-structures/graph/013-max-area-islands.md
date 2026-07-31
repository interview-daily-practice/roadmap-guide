# Max Area Island

Approach: largest connected component

```java
class Solution {
    public int maxAreaOfIsland(int[][] grid) {
        int maxRow = grid.length;
        int maxCol = grid[0].length;
        boolean[][] v = new boolean[maxRow][maxCol];
        int result = 0;
        for (int i = 0; i < maxRow; i++) {
            for (int j = 0; j < maxCol; j++) {
                if (v[i][j] || grid[i][j] == 0) {
                    continue;
                }
                //   count = 0;
                int temp = dfs(grid, v, i, j, maxRow, maxCol);
                result = Math.max(result, temp);
            }
        }

        return result;
    }

    // private int count = 0;

    private int dfs(int[][] g, boolean[][] v, int i, int j, int maxRow, int maxCol) {

        boolean isValid = i >= 0 && i < maxRow
                && j >= 0 && j < maxCol
                && v[i][j] == false
                && g[i][j] == 1;

        if (!isValid) {
            return 0;
        }
        v[i][j] = true;
        // count++;

        int count = 1;
        int dirs[][] = {
                { 1, 0 }, { -1, 0 }, { 0, 1 }, { 0, -1 }
        };

        for (int[] dir : dirs) {
            int nextRow = i + dir[0];
            int nextCol = j + dir[1];
            count += dfs(g, v, nextRow, nextCol, maxRow, maxCol);
        }

        return count;

    }
}
```

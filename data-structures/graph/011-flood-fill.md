# 733 Flood Fill

## DFS

```java
class Solution {
    public int[][] floodFill(int[][] image, int sr, int sc, int color) {
        int startColor = image[sr][sc];
        if(startColor == color) return image;
        dfs(image, sr, sc, startColor, color);
        return image;
    }

    static int[][] dirs = {
                { 1, 0 }, { -1, 0 },
                { 0, 1 }, { 0, -1 }
        };
    private void dfs(int[][] grid, int row, int col, int startColor, int targetColor) {

        grid[row][col] = targetColor;

        for (int[] dir : dirs) {

            int nr = row + dir[0];
            int nc = col + dir[1];
            boolean isValid = isValid(nr, nc, grid.length, grid[0].length)
                    && grid[nr][nc] == startColor;

            if (isValid) { // delegate to dfs
                dfs(grid, nr, nc, startColor, targetColor);
            }

        }
    }

    private boolean isValid(int row, int col, int maxRow, int maxCol) {
        return row >= 0 && row < maxRow && col >= 0 && col < maxCol;
    }
}
```

## BFS

class Solution {
    public int[][] floodFill(int[][] image, int sr, int sc, int color) {
        int startColor = image[sr][sc];
        if(startColor == color) return image;
        dfs(image, sr, sc, startColor, color);
        return image;
    }

    static int[][] dirs = {
                { 1, 0 }, { -1, 0 },
                { 0, 1 }, { 0, -1 }
        };
    private void dfs(int[][] grid, int row, int col, int startColor, int targetColor) {

        Queue<int[]> q = new LinkedList<>();
        grid[row][col] = targetColor;
        q.add(new int[]{row, col});

        while(!q.isEmpty()){
          int len = q.size();
          for(int i =0; i < len; i++){
            int[] item = q.poll();
            for(int[] dir: dirs){
                int nr = item[0] + dir[0];
                int nc = item[1] + dir[1];
                boolean isValid = isValid(nr,nc, grid.length, grid[0].length) &&
                grid[nr][nc] == startColor;
                if(isValid){
                    grid[nr][nc] = targetColor;
                    q.add(new int[]{nr,nc});
                }
            }
          }
        }
        
    }

    private boolean isValid(int row, int col, int maxRow, int maxCol) {
        return row >= 0 && row < maxRow && col >= 0 && col < maxCol;
    }
}

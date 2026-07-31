# Rotten Oranges Using BFS (Min time)

```java
class Solution {
    public int orangesRotting(int[][] grid) {
        int rowMax = grid.length;
        int colMax = grid[0].length;

        int fresh = 0;
        Queue<int[]> queue = new LinkedList<>();

        int minutes = 0;

        for (int row = 0; row < rowMax; row++) {
            for (int col = 0; col < colMax; col++) {
                int value = grid[row][col];
                if (value == 0) { // empty cell
                    continue;
                }
                if (value == 2) { // rotten
                    queue.add(new int[] { row, col });
                    continue;
                }
                if (value == 1) { // fresh
                    fresh++;
                }
            }

        }

        int[][] directions = {
                { 1, 0 },
                { -1, 0 },
                { 0, 1 },
                { 0, -1 }
        };

        while (!queue.isEmpty() && fresh > 0) {

            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int[] elem = queue.poll();
                int row = elem[0];
                int col = elem[1];

                for (int[] dir : directions) {
                    int newRow = row + dir[0];
                    int newCol = col + dir[1];

                    boolean valid = newRow >= 0 && newRow < rowMax 
                            && newCol >= 0 && newCol < colMax
                            && grid[newRow][newCol] == 1; // fresh
                    if (!valid) {
                        continue;
                    }

                    // make it rotten
                    grid[newRow][newCol] = 2;
                    // rotten should go to queue
                    queue.add(new int[] { newRow, newCol });
                    // reduce fresh count
                    fresh--;
                }
            }
            minutes++;
        }

        return fresh == 0 ? minutes : -1;
    }
}
```

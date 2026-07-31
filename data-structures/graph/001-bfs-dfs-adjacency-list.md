# BFS / DFS Matrix

## Matrix Traversal using DFS and BFS

A matrix can be treated like a graph.

Example:

```text
1 2 3
4 5 6
7 8 9
```

Each cell has neighbors:

```
        up
        |
left -- cell -- right
        |
       down
```

We use:

```java
int[][] dirs = {
    {-1,0}, // up
    {1,0},  // down
    {0,-1}, // left
    {0,1}   // right
};
```

---

# 1. DFS Traversal (Recursion)

### Idea

DFS goes **deep first**.

Example:

Start from `5`.

```
5
|
2
|
1
|
4
...
```

It keeps going until no more cells are available, then comes back.

Steps:

1. Check boundary
2. Check already visited
3. Mark current cell visited
4. Visit all 4 neighbors recursively

### Java Code

```java
class DFSMatix {

    int[][] dirs = {
        {-1,0},
        {1,0},
        {0,-1},
        {0,1}
    };

    void dfs(int[][] matrix, int r, int c, boolean[][] visited) {

        int rows = matrix.length;
        int cols = matrix[0].length;

        // invalid cell
        if(r < 0 || c < 0 || r >= rows || c >= cols)
            return;

        // already visited
        if(visited[r][c])
            return;


        // mark visited
        visited[r][c] = true;

        System.out.println(matrix[r][c]);


        // visit neighbours
        for(int[] dir : dirs) {

            int nr = r + dir[0];
            int nc = c + dir[1];

            dfs(matrix, nr, nc, visited);
        }
    }
}
```

---

### DFS Execution

Matrix:

```
1 2 3
4 5 6
7 8 9
```

Start:

```
dfs(1,1)
```

means:

```
5
```

Call stack:

```
dfs(5)

   dfs(2)

      dfs(1)

          dfs(4)

             ...
```

When a path finishes, recursion returns and explores the next direction.

---

# 2. BFS Traversal (Queue)

### Idea

BFS goes **level by level**.

It uses a Queue.

Example:

Start from `5`.

First visit:

```
    2
4   5   6
    8
```

Then next level:

```
1 3 7 9
```

---

### Java Code

```java
import java.util.*;

class BFSMatrix {

    int[][] dirs = {
        {-1,0},
        {1,0},
        {0,-1},
        {0,1}
    };


    void bfs(int[][] matrix, int sr, int sc) {

        int rows = matrix.length;
        int cols = matrix[0].length;

        boolean[][] visited = new boolean[rows][cols];

        Queue<int[]> queue = new LinkedList<>();


        queue.offer(new int[]{sr, sc});
        visited[sr][sc] = true;


        while(!queue.isEmpty()) {

            int[] cell = queue.poll();

            int r = cell[0];
            int c = cell[1];


            System.out.println(matrix[r][c]);


            for(int[] dir : dirs) {

                int nr = r + dir[0];
                int nc = c + dir[1];


                if(nr < 0 || nc < 0 ||
                   nr >= rows || nc >= cols)
                    continue;


                if(visited[nr][nc])
                    continue;


                visited[nr][nc] = true;

                queue.offer(new int[]{nr,nc});
            }
        }
    }
}
```

---

# DFS vs BFS Quick Difference

| DFS                           | BFS                      |
| ----------------------------- | ------------------------ |
| Uses recursion/stack          | Uses queue               |
| Goes deep first               | Goes level by level      |
| Good for connected components | Good for shortest path   |
| Example: Number of Islands    | Example: Rotting Oranges |

---

# Interview Template to Remember

For matrix problems:

```
1. Create visited array

2. Define directions

3. Check boundary

4. Mark visited

5. Traverse neighbours
```

Most matrix problems are just this template with different processing logic.


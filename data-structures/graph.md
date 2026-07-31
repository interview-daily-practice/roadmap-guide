# Contents

## Part 1

- BFS / DFS matrix
- BFS / DFS adjacency list
- 133 Clone A Graph BFS / DFS
- Cycle in UnDirected Graph BFS / DFS
- Cycle in directed graph DFS (state based)
- Graph is a valid tree
- Topological Sort
- 207 Course Schedule 1 (Cycle UD Graph DFS)
- 210 Course Schedule 2 (TopoSort)
- 269 Alien Dictionary Order
- 79 Word Search in a Grid
- 695 Max Area Od Islands (Largest connected component)
- 200 Number Of Islands (count connected components)
- 994 Rotten Oranges
- 127 Word Ladder 1 Char Diff (Count min length)
- Word Ladder 2 (Find shortest path)
- 684 Redundant Connection (DSU)
- Account Merge
- Reconstruct Itenary
- Network delay time
- Min Cost to connect all points
- Swim In Rising Water
- Cheapest Flight In k Stops
- Pacific Atalantic Water flow

### BFS / DFS Matrix

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

If vertices are given as **characters (A, B, C...)** instead of numbers, we cannot directly use an array index. We usually use a **Map<Character, List<Character>>** for adjacency list.

Example graph:

```
      A
     / \
    B   C
        |
        D
```

Edges:

```
A-B
A-C
C-D
```

Adjacency List:

```
A -> B,C
B -> A
C -> A,D
D -> C
```

---

## Build Adjacency List

```java
Map<Character, List<Character>> graph = new HashMap<>();

void addEdge(char u, char v) {

    graph.computeIfAbsent(u, k -> new ArrayList<>()).add(v);
    graph.computeIfAbsent(v, k -> new ArrayList<>()).add(u);
}
```

Why two additions?

Because this is an **undirected graph**.

```
A-B
```

means:

```
A can go to B

and

B can go to A
```

---

# DFS using Character Vertices

### Code

```java
import java.util.*;

class GraphDFS {

    Map<Character, List<Character>> graph = new HashMap<>();

    void dfs(char node, Set<Character> visited) {

        // mark visited
        visited.add(node);

        // process node
        System.out.println(node);


        // visit neighbours
        for(char neighbour : graph.get(node)) {

            if(!visited.contains(neighbour)) {
                dfs(neighbour, visited);
            }
        }
    }
}
```

---

## DFS Execution

Start:

```java
dfs('A')
```

Flow:

```
A

visit B

return

visit C

visit D
```

Output:

```
A B C D
```

---

# BFS using Character Vertices

BFS needs a Queue.

```java
import java.util.*;

class GraphBFS {


    void bfs(char start,
             Map<Character,List<Character>> graph) {


        Set<Character> visited = new HashSet<>();

        Queue<Character> queue = new LinkedList<>();


        queue.offer(start);
        visited.add(start);


        while(!queue.isEmpty()) {


            char node = queue.poll();


            System.out.println(node);



            for(char neighbour : graph.get(node)) {


                if(!visited.contains(neighbour)) {

                    visited.add(neighbour);

                    queue.offer(neighbour);
                }
            }
        }
    }
}
```

---

# Complete Example

```java
public class Main {

    public static void main(String[] args) {

        Map<Character,List<Character>> graph = new HashMap<>();

        addEdge(graph,'A','B');
        addEdge(graph,'A','C');
        addEdge(graph,'C','D');


        Set<Character> visited = new HashSet<>();

        dfs(graph,'A',visited);


        System.out.println("BFS");

        bfs(graph,'A');
    }



    static void addEdge(Map<Character,List<Character>> graph,
                        char u,
                        char v) {

        graph.computeIfAbsent(u,k->new ArrayList<>()).add(v);
        graph.computeIfAbsent(v,k->new ArrayList<>()).add(u);
    }



    static void dfs(Map<Character,List<Character>> graph,
                    char node,
                    Set<Character> visited) {


        visited.add(node);

        System.out.println(node);


        for(char nbr : graph.get(node)) {

            if(!visited.contains(nbr))
                dfs(graph,nbr,visited);
        }
    }



    static void bfs(char start,
                    Map<Character,List<Character>> graph) {


        Queue<Character> q = new LinkedList<>();
        Set<Character> visited = new HashSet<>();

        q.offer(start);
        visited.add(start);


        while(!q.isEmpty()) {

            char node = q.poll();

            System.out.println(node);


            for(char nbr : graph.get(node)) {

                if(!visited.contains(nbr)) {

                    visited.add(nbr);
                    q.offer(nbr);
                }
            }
        }
    }
}
```

Output:

```
DFS
A
B
C
D

BFS
A
B
C
D
```

---

## Interview Note

When input is:

```
1 2
2 3
3 4
```

Use:

```
List<List<Integer>>
```

When input is:

```
A B
B C
C D
```

Use:

```
Map<Character,List<Character>>
```

The traversal logic is exactly the same. Only the **vertex storage changes**.

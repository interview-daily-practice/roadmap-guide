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

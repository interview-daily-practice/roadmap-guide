# Topological Sort

## Definition

Topological sort is a **linear ordering of vertices** in a **Directed Acyclic Graph (DAG)** such that:

For every directed edge:

```text
A → B
```

A must come before B.

Example:

```text
A → C
B → C
C → D
```

Valid topological orders:

```text
A B C D
```

or

```text
B A C D
```

Invalid:

```text
C A B D
```

because C depends on A and B.

---

# Where it is used?

Common examples:

* Course schedule (prerequisite problems)
* Build systems (Maven dependencies)
* Task scheduling
* Package dependency resolution

Example:

```text
Java
 |
Spring Boot
 |
Application
```

Java must be built before Spring Boot.

---

# Approach 1: Kahn's Algorithm (BFS)

Most commonly used.

### Idea

A node can be processed only when it has **no incoming edges**.

Incoming edges are called **indegree**.

Example:

```
A → C
B → C
C → D
```

Indegree:

```
A = 0
B = 0
C = 2
D = 1
```

Start with nodes having:

```
indegree = 0
```

Queue:

```
[A, B]
```

---

## Steps

### Step 1: Build graph

```
A -> C
B -> C
C -> D
```

### Step 2: Calculate indegree

```
A 0
B 0
C 2
D 1
```

### Step 3: Add zero indegree nodes to queue

```
Queue = [A,B]
```

### Step 4: Remove node

Remove A:

```
Order = A
```

Reduce neighbors:

```
C: 2 -> 1
```

Remove B:

```
Order = A B
```

Reduce:

```
C: 1 -> 0
```

Add C.

Continue.

Final:

```
A B C D
```

---

# Java Code (Kahn's Algorithm)

```java
import java.util.*;

class TopologicalSort {


    public List<Integer> topoSort(int n, int[][] edges) {


        List<Integer>[] graph = new ArrayList[n];


        for(int i=0;i<n;i++)
            graph[i] = new ArrayList<>();


        int[] indegree = new int[n];


        // build graph
        for(int[] edge : edges) {

            int u = edge[0];
            int v = edge[1];

            graph[u].add(v);

            // incoming edge count
            indegree[v]++;
        }



        Queue<Integer> queue = new LinkedList<>();


        // nodes with no dependency
        for(int i=0;i<n;i++) {

            if(indegree[i] == 0)
                queue.offer(i);
        }



        List<Integer> result = new ArrayList<>();


        while(!queue.isEmpty()) {


            int node = queue.poll();

            result.add(node);



            for(int neighbour : graph[node]) {


                indegree[neighbor]--;


                if(indegree[neighbor] == 0)
                    queue.offer(neighbour);
            }
        }



        // cycle detection
        if(result.size() != n)
            return new ArrayList<>();


        return result;
    }
}
```

---

# Cycle Detection in Topological Sort

If graph has a cycle:

```
A → B
↑   |
|___|
```

No node has indegree 0.

Queue becomes empty.

Example:

```
A indegree = 1
B indegree = 1
```

Nothing can start.

So:

```java
result.size() != numberOfNodes
```

means cycle exists.

---

# Approach 2: DFS Topological Sort

Idea:

Go deep first.

When a node finishes processing, put it in stack.

Example:

```
A → B → C
```

DFS:

```
A
 |
 B
 |
 C
```

Finish order:

```
C
B
A
```

Reverse:

```
A B C
```

---

## DFS Code

```java
class TopologicalDFS {


    void dfs(int node,
             List<Integer>[] graph,
             boolean[] visited,
             Stack<Integer> stack) {


        visited[node] = true;


        for(int nbr : graph[node]) {

            if(!visited[nbr]) {
                dfs(nbr, graph, visited, stack);
            }
        }


        // after all children processed
        stack.push(node);
    }
}
```

---

# BFS vs DFS Topological Sort

| Kahn BFS                 | DFS                         |
| ------------------------ | --------------------------- |
| Uses indegree            | Uses recursion              |
| Easier cycle detection   | Uses recursion stack        |
| Good for course schedule | Common theoretical approach |
| Produces order directly  | Need reverse stack          |

---

# Important Interview Pattern

For **Course Schedule**:

```
Prerequisite:
B -> A
```

means:

```
B must complete before A
```

Build:

```
B → A
```

Then run topological sort.

If:

```
topological order size == number of courses
```

then possible.

Otherwise cycle exists.

---

Remember:

```
Topological Sort = Ordering of dependencies

Condition:
Only DAG can have topological ordering
```

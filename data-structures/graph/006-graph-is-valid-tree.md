
# Graph Is Valid Tree

Yes, your approach is correct. This is the **Graph Valid Tree** problem (LeetCode 261).

A graph is a valid tree if:

1. **No cycle exists**
2. **All nodes are connected** (we visited all nodes)

So your logic:

```
DFS/BFS from any node

↓

If cycle found → Not a tree

↓

After traversal, if visited count == total nodes

↓

Valid tree
```

is correct.

---

## Example

Edges:

```
0 - 1
1 - 2
2 - 3
```

Graph:

```
0
|
1
|
2
|
3
```

DFS from 0:

```
0 visited
 |
1 visited
 |
2 visited
 |
3 visited
```

No cycle.

Visited count = 4

Total nodes = 4

✅ Valid Tree

---

## Cycle Detection Logic

For an **undirected graph**, when visiting a neighbor:

* If neighbor is not visited → go deeper
* If neighbor is already visited AND it is not parent → cycle exists

Example:

```
0
|
1
|
2
 \
 0
```

DFS:

```
0
 |
1
 |
2
```

From 2:

Neighbor = 0

0 is already visited.

But parent of 2 is 1.

So:

```
visited neighbor != parent
```

Cycle found.

---

## Java Code (DFS)

```java
class Solution {

    public boolean validTree(int n, int[][] edges) {

        List<Integer>[] graph = new ArrayList[n];

        for(int i=0;i<n;i++) {
            graph[i] = new ArrayList<>();
        }


        // build graph
        for(int[] edge : edges) {

            int u = edge[0];
            int v = edge[1];

            graph[u].add(v);
            graph[v].add(u);
        }


        boolean[] visited = new boolean[n];


        // check cycle
        if(hasCycle(0, -1, graph, visited))
            return false;


        // connectivity check
        for(boolean v : visited) {
            if(!v)
                return false;
        }


        return true;
    }



    private boolean hasCycle(int node,
                             int parent,
                             List<Integer>[] graph,
                             boolean[] visited) {


        visited[node] = true;


        for(int neighbour : graph[node]) {


            // ignore the edge we came from
            if(neighbour == parent)
                continue;


            // already visited means cycle
            if(visited[neighbour])
                return true;


            if(hasCycle(neighbour,
                        node,
                        graph,
                        visited))
                return true;
        }


        return false;
    }
}
```

---

## Why parent is needed?

Consider:

```
0 ---- 1
```

DFS:

```
visit 0

go to 1

from 1 see 0
```

0 is already visited.

But this is **not a cycle**.

It is the same edge we came from.

Therefore:

```java
if(neighbour == parent)
    continue;
```

ignores the backward edge.

---

## BFS version idea

Same logic, but store parent in queue:

```java
Queue<int[]> queue;

[node, parent]
```

When processing:

```
neighbor already visited
AND
neighbor != parent
```

means cycle.

---

## Important Interview Observation

For an undirected graph:

```
Valid Tree = Connected + No Cycle
```

Another mathematical shortcut:

A tree with `n` nodes always has:

```
edges = n - 1
```

So many solutions first check:

```java
if(edges.length != n-1)
    return false;
```

Then only check connectivity.

Because if edges = n-1 and no cycle, it is automatically a tree.

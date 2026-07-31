
# Cycle In Undirected Graph

[Try in GFG](https://www.geeksforgeeks.org/problems/detect-cycle-in-an-undirected-graph/1)

When item is visited we found then return true. But pre-check if item is parent then continue.

```java
    
 import java.util.*;

class Solution {
    public boolean isCycle(int V, int[][] edges) {
        mappings.clear();
        buildGraph(edges);
        Set<Integer> visited = new HashSet<>();
        
        // BFS
        for (int node : mappings.keySet()) {
            if (!visited.contains(node) && hasCycle(visited, node)) return true;
        } // BFS END
        
        // DFS
        visited.clear();
        for (int node : mappings.keySet()) {
            if (!visited.contains(node) && hasCycle(visited, node, -1)) return true;
        } // DFS END
        
        return false;
    }
    

// bfs
private boolean hasCycle(Set<Integer> visited, int start) {
    Queue<int[]> q = new LinkedList<>();
    q.add(new int[]{start, -1});
    visited.add(start);

    while (!q.isEmpty()) {
        int[] current = q.poll();
        int val = current[0];
        int parent = current[1];

        for (int child : mappings.getOrDefault(val, new ArrayList<>())) {
            if (child == parent) continue;
            if (visited.contains(child)) return true;
            visited.add(child);
            q.add(new int[]{child, val});
        }
    }

    return false;
}

// dfs
private boolean hasCycle(Set<Integer> visited, int node, int parent){
      visited.add(node);
      
      for(int neighbor: mappings.getOrDefault(node, new ArrayList<>())){
         if(neighbor==parent) continue;
         if(visited.contains(neighbor)) return true;
        // visited.add(neighbor);
         boolean flag = hasCycle(visited,neighbor, node);
         if(flag) return true;
      }
      
      return false;
 }

private Map<Integer, List<Integer>> mappings = new HashMap<>();

private void buildGraph(int[][] edges) {
    for (int[] edge : edges) {
        int u = edge[0], v = edge[1];
        mappings.computeIfAbsent(u, k -> new ArrayList<>()).add(v);
        mappings.computeIfAbsent(v, k -> new ArrayList<>()).add(u);
    }
  }


} // end of class
```

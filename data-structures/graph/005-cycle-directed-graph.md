# Cycle Detection In Directed Graph

Approach: State and Recursion

visiting->visited

- if we found state is visiting then cycle exist so return true
- if we found state is visited then cycle does not exists so return false
- mark as visiting
- explore neighbors and recurse. if cycle found return true
- mark as visited
- return false

```java
import java.util.*;
class Solution {
    public boolean isCyclic(int V, int[][] edges) {
        // code here
        graph.clear();
        Map<Integer,State> states = new HashMap<>();
        buildGraph(edges);
        // for each edge start and check the cycle exists
        for(int key: graph.keySet()){
          if(states.get(key) == null && hasCycle(states,key)){
              return true;
          }    
        }
        
        return false;
    }
    
    private boolean hasCycle(Map<Integer,State> states, int node){
       if(states.get(node) == State.V) return true; // cycle found
       if(states.get(node) == State.C) return false; // no cycle found
       
       states.put(node, State.V); // mark the node visiting. if you see it again then it has a cycle
       
       for(int neighbor: graph.getOrDefault(node, new ArrayList<>())){
          boolean flag = hasCycle(states, neighbor);
          if(flag){
             return true;
          }
       }
       
       states.put(node, State.C);
       
       return false;
    }
    
    private static enum State {
        U, // unvisisted: thought not used, we assume it as null
        V, // visisting: 
        C; // visisted
    }
    
    private Map<Integer, List<Integer>> graph = new HashMap<>();
    
    private void buildGraph(int[][] edges){
        for(int[] edge: edges){
          int u = edge[0];
          int v = edge[1];
          graph.computeIfAbsent(u, k-> new ArrayList<>()).add(v);
        //   graph.computeIfAbsent(v, k-> new ArrayList<>()).add(u);
          
        }
    }
    
}
```

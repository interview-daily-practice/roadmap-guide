# Course Schedule 1

Approach: Check Cycle Exists in a DAG.

```java
class Solution {
    public boolean canFinish(int count, int[][] courses) {
        for (int[] course : courses) {
            graph.computeIfAbsent(course[1], k -> new ArrayList<>()).add(course[0]);
        }

        Map<Integer, State> map = new HashMap<>();
        for (int key : graph.keySet()) {
            if (map.get(key) == null) {
                boolean flag = dfs(key, map);
                if (flag)
                    return false;
            }
        }

        return true;
    }

    public boolean dfs(int node, Map<Integer, State> map) {

        if (map.get(node) == State.VISITING)
            return true;
        if (map.get(node) == State.VISITED)
            return false;

        map.put(node, State.VISITING);

        for (int nbr : graph.getOrDefault(node, new ArrayList<>())) {
           // if (!map.containsKey(nbr)) {
                boolean flag = dfs(nbr, map);
                if (flag)
                    return true;
          //  }
        }

        map.put(node, State.VISITED);

        return false;

    }

    private static enum State {
        VISITING,
        VISITED
    }

    private Map<Integer, List<Integer>> graph = new LinkedHashMap<>();

}
```

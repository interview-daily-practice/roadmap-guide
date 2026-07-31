# Course Schedule 2

Approach: TopoSort And Cycle Check in DAG

```java
class Solution {

    private Map<Integer, List<Integer>> graph = new LinkedHashMap<>();

    public int[] findOrder(int numCourses, int[][] courses) {
        for (int[] course : courses) {
            graph.computeIfAbsent(course[1], k -> new ArrayList<>()).add(course[0]);
        }

        Map<Integer, State> map = new HashMap<>();
        boolean flag = false;
        for (int i = 0; i < numCourses; i++) { // not on graph keys but on all courses
            if (map.get(i) == null) {
                flag = dfs(i, map);
                if (flag) {
                    System.out.println(flag);
                    return new int[0];
                }
            }
        }

        Collections.reverse(list);

        int[] result = list.stream().mapToInt(Integer::intValue).toArray();

        return result;

    }

    private List<Integer> list = new ArrayList<>();

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

        list.add(node);// this is extra line

        return false;

    }

    private static enum State {
        VISITING,
        VISITED
    }
}
```

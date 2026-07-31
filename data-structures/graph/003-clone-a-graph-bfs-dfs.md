# 133 Clone A Graph

## DFS Approach

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> neighbors;
    public Node() {
        val = 0;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val) {
        val = _val;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val, ArrayList<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}
*/

class Solution {
    public Node cloneGraph(Node node) {
      return dfs(node);  
    }

    private Map<Integer, Node> map = new HashMap<>();

    private Node dfs(Node node){
      if(node == null){
        return null;
      }  
      if(map.containsKey(node.val)){
        return map.get(node.val);
      }

      Node clone = new Node(node.val);
      clone.neighbors = new ArrayList<>();
      map.put(node.val, clone);
      for(Node nbr: node.neighbors){
        clone.neighbors.add(dfs(nbr));
      }
      return clone;   
    }
}
```

## BFS Approach

/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> neighbors;
    public Node() {
        val = 0;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val) {
        val = _val;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val, ArrayList<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}
*/

class Solution {
    public Node cloneGraph(Node node) {
        return node == null ? node : bfs(node);
    }

    private Map<Integer, Node> map = new HashMap<>();

    public Node bfs(Node node) {
        Queue<Node> q = new LinkedList<>();
        q.add(node);
        map.putIfAbsent(node.val, new Node(node.val));

        while (!q.isEmpty()) {
            int len = q.size();
            for (int i = 0; i < len; i++) {
                Node curr = q.poll();

                for (Node nbr : curr.neighbors) {
                    if (!map.containsKey(nbr.val)) {
                        map.putIfAbsent(nbr.val, new Node(nbr.val));
                        q.add(nbr);
                    }

                    Node clone = map.get(curr.val);
                    clone.neighbors.add(map.get(nbr.val));
                }
            }
        }

        return map.get(node.val);
    }
}



